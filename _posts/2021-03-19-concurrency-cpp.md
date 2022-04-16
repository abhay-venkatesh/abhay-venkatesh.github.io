---
title: "How-To: Concurrency with C++ STL"
date: 2021-03-19
layout: post
---

### Overview

An opinionated guide for how to do concurrency in C++ focusing on more robotics applications (and less servers like most C++ concurrency guidance is based on). Furthermore, we try to make the best use of the standard library rather than use primitives such as for e.g. Facebook has developed inside [Folly](https://github.com/facebook/folly).

### General Guidance

* minimize number of threads in application due to the increased factoring complexity introduced by them
    * threads are also [expensive relative to Goroutines or Green Threads](https://eli.thegreenplace.net/2018/measuring-context-switching-and-memory-overheads-for-linux-threads/)
    * keep thing simple and linear to avoid having to do crazy multi-threaded synchronization
* for asynchronous tasks, prefer `std::async` 
* for long-running loops, prefer `std::thread
    `
    * no benefit produced by using `std::async`
    * `std::async` has a lot of compiler-defined semantics such as 
        * underlying thread pool implementation
        * whether the task is launched right away or not
    * can exhaust the thread pool/starve other tasks
* avoid lock-free concurrency
    * some CPU architectures like [ARM provide no read/write ordering guarantees](https://randomascii.wordpress.com/2020/11/29/arm-and-lock-free-programming/)
    * C++ compilers might reorder reads/writes
* use the “[non-realtime interleaved execution model](https://cs.lmu.edu/~ray/notes/introconcurrency/)” to analyze concurrency issues
    * code is correct if it does the right thing for each possible interleaving
    * **Example**: Suppose thread-1 has instruction sequence [A,B,C] and thread-2 has sequence [x,y]. Then we have to consider:
    *    [A B C x y]
           [A B x C y]
           [A B x y C]
           [A x B C y]
           [A x B y C]
           [A x y B C]
           [x A B C y]
           [x A B y C]
           [x A y B C]
           [x y A B C]
* use the LLVM [ThreadSanitizer](https://clang.llvm.org/docs/ThreadSanitizer.html), ideally in your CI jobs to guard against race conditions
* for reference on how to do concurrency in C++
    * [Java Concurrency in Practice](https://www.amazon.com/Java-Concurrency-Practice-Brian-Goetz/dp/0321349601) for how to structure concurrent programs
        * in general prefer Java references for advice on structuring concurrent programs since C++ references rarely provide high quality general advice for factoring concurrent code
    * [C++ Concurrency in Action](https://www.amazon.com/C-Concurrency-Action-Anthony-Williams/dp/1617294691/ref=sr_1_5?dchild=1&keywords=concurrency+in+C%2B%2B&qid=1616389621&sr=8-5) for C++ specific information 

### Loops with Non-Blocking Work

```cpp
class TaskInterface {
public:
   virtual void start() = 0;
   virtual void shutdown() = 0;
}

class Task : public TaskInterface {
   std::atomic_bool mShutdown;
    std::thread mThread;
    
public:
   Task() : mShutdown{false} {}

   void start () {
       mThread = std::thread([this](){
            while(!mShutdown) {
                // do non-blocking work
            }
       });
   }
   
   void shutdown() {
        mShutdown = true;
        if(mThread.joinable()) {
            mThread.join();
        }
   }
}
```

### Loops with Blocking Work

```cpp
class Task : public TaskInterface {
   std::atomic_bool mShutdown;
   std::thread mThread;
   
   BlockingReader mBlockingReader;
    
public:
   Task() : mShutdown{false} {}

   void start () {
       mThread = std::thread([this]{
            while(!mShutdown && mBlockingReader.blockingRead()) {}
       });
   }
   
   void shutdown() {
        mShutdown = true;
        mBlockingReader.shutdown();       
        if(mThread.joinable()) {
            mThread.join();
        }
   }
}
```

Prefer `while` to do `do-while` since the `while` applies the same invariant to each iteration whereas the `do-while` applies a different invariant for the first - since this simplifies analysis.

### Fast Shutdown

Use futures for an “interruptable wait” for fast shutdown (uses [CV internally](https://stackoverflow.com/questions/56490044/does-stdpromise-internally-use-stdcondition-variable-to-notify-the-associate/56491223#56491223)):

```cpp
class Task : public TaskInterface {
   std::promise<void> mRequestShutdown;
   std::future<void> mShutdownReceived;
   
   std::thread mThread;
    
public:
   Task() : 
        mRequestShutdown{std::promise<void>()}, 
        mShutdownReceived{mRequestShutdown.get_future()} 
   {
   }

   void start () {
       mThread = std::thread([this](){
            while(mShutdownReceived.wait(4s) == std::future_status::timeout) {
                // do work
            }
       });
   }
   
   void shutdown() {
        mRequestShutdown.set_value();
        if(mThread.joinable()) {
            mThread.join();
        }
   }
}
```

### Producer-Consumer

Use a blocking queue to solve producer-consumer:

```cpp
class Consumer: public TaskInterface {
   SynchronousQueue<void> mReady;
   std::thread mThread;

public:
   Consumer() : mReady{1} {}

   void start () {
       mThread = std::thread([this](){
            // Blocking pop
            while(mReady.pop()) {
                // do work
            }
       });
   }
   
   void shutdown() {
        mReady.clearAndShutdown();
        if(mThread.joinable()) {
            mThread.join();
        }
   }
   
   bool giveWork() {
        return mReady.push(true); // returns false if shutdown
   }
}

class Producer: public TaskInterface {
   std::thread mThread;
   std::shared_ptr<Consumer> mConsumer;
    
public:
   Producer(std::shared_ptr<Consumer> consumer) : mConsumer{consumer} {}

   void start () {
       mThread = std::thread([this](){
            // Blocking operation
            while(mConsumer.giveWork()) {}
       });
   }
   
   void shutdown() {
        mConsumer.shutdown();
        if(mThread.joinable()) {
            mThread.join();
        }
   }
}
```

In the above example, the producer produces every 4 seconds and wakes up the consumer on producing. 

### Avoid Waiting on Multiple Things

A fundamental limitation of the thread-based concurrency model in C++/Java is that it is very difficult to “wait on multiple things” like one can easily do in GoLang due to the powerful `select` statement. The only alternative is to use something like an event queue which delivers events to the various waiters on it, but then you need to use and pass around this event queue. External libraries like Folly support `collectAny` on futures, but we discuss Folly in a future “Advanced Concurrency Patterns” guide. 

Instead, for simplicity and to only use the standard library, far better is to just avoid having to wait on multiple things. If you find yourself having to do so, then consider a refactor. For example, instead of having the following, where a thread is calling the receive:

```cpp
template<typename T>
class Subscription {
   ConcurrentQueue<T> mQ;

public:
   std::optional<T> receive(std::shared_future<void> sf) {
        while(mQ.size() == 0 && sf.wait_for(std::chrono::milliseconds(512))) {}
        ...
   }
}
```

prefer instead to not have the thread calling the receive at all, and instead do:

```cpp
template<typename T>
class Subscription {
   std::mutex mMu;
   std::vector<std::function<void(const T&)>> mFs;

public:
   void registerReceiver(const std::function<void(const T&)>& f) {
        std::scoped_lock l(mMu);
        mFs.push_back(f);
   }
}
```

where you register a receiver instead, where the receiver is probably a class function on the class where you’d like to store the state from the receive. This way we avoid the expensive spin wait. A general pattern from this can be formed that we want to minimize the number of threads in our application to simplify the code in it. By going from two threads to one, we avoided the need for a spin wait. 
