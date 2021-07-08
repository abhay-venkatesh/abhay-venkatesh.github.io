---
title: "How-To: Design your Code"
date: 2021-06-15
layout: post
---

Design is the art of managing complexity.
In software design, the way we manage complexity is by making our code testable.
All the good stuff you might have heard of - modularity, encapsulation, extensibility, reliability - is downstream from testability.
The main way to make your software testable is to make it loosely coupled. There are broadly two important ways in which you can make your code loosely coupled:
minimizing the surface areas for interaction, and depending on interfaces rather than implementations.

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