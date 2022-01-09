---
title: "How-To: Design your Code"
date: 2021-06-15
layout: post
---

**Design is the art of managing complexity.**

In software design, the way we manage complexity is by making our code testable.
All the good stuff you might have heard of - modularity, encapsulation, extensibility, reliability - is downstream from testability.

The main way to make your software testable is to make it loosely coupled. There are broadly two important ways in which you can make your code loosely coupled:
1. minimizing the surface areas for interaction, and 
2. depending on interfaces rather than implementations.

### Law of Demeter

The first way is the Law of Demeter, or the “Principle of Least Knowledge”, which basically means that units should know as little as possible about other units. When crafting your code, this means that class surface areas should be as small ([classes should be deep](http://alex-ii.github.io/notes/2018/10/07/philosophy_of_software_design.html)) as possible, and functions should accept as little as possible in their parameters. 

For example, 

```golang
type bar struct {}

func (b *bar) baz() {}

type foo struct {
   b *bar
}

func (f *foo) getBar() *bar {}

func doBar(f *foo) {
   f.getBar().baz()
}
```

```cpp
struct Bar {
   void baz() {}
}

class Foo {
   std::shared_ptr<bar> mBar;
public:
   std::shared_ptr<bar> getBar() {
        return mBar;
   }
}

void doBar(std::shared_ptr<foo> f) {
   f->getBar()->baz();
}
```

would be a violation of the Law of Demeter because even though we only really needed `baz`, we supplied `foo` to the `doBar` function, which also gave us access to `bar`, which we did not need. 

The above code is just an illustration, and in this case remains relatively understandable even with the violation. But over time these violations can compound and lead to extremely confusing code with tight coupling which will become hard to test effectively.

### Dependency Inversion Principle

The second way to make your code loosely coupled is the Dependency Inversion Principle, which can be summarized as:

* A→ B
* Instead of A depending on B, A can choose to depend on an interface C.
* A → C ← B

This essentially decouples A’s implementation from B, which in turn makes it easier to test A. One can now inject “mocked out implementations” of C to test A independently from B. For example,

```golang
type C interface {
   foo()
}

var _ C = &B{}

type B struct {}

func (b *B) foo() { // real implementation }

type A struct {
   c C
}

func newA(c C) *A {
   return &A{
        c: c,
   }
}

var _ C = &B{}

type mockB struct {}

func (b *mockB) foo() { // fake implementation }

func TestA(t *testing.T) {
   a := A(&mockB{})
   // Test A
}
```

```cpp
class C { 
   virtual void foo() = 0;
};

class B : public C {
   void foo() { // real implementation }
};

class A {
   std::shared_ptr<C> mC;
   
public:
   A(std::shared_ptr<C> c) : mC{c} {}
};

class MockB : public C {
   void foo() { // fake implementation }
};

BOOST_AUTO_TEST_CASE(TestA) {
   std::shared_ptr<C> b = std::make_shared<MockB>();
   A a(b);
   // Test A
}
```

Without the dependency inversion principle, testing can require instantiating the entire dependency graph, making testing in isolation and debugging nearly impossible. 

The dependency inversion principle becomes extremely critical especially in cases where you have external dependencies such as a program written in another language. If you could not depend on interfaces instead of the other program, [you’d have to rely on e2e testing rather than the far more desirable unit tests](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html). 