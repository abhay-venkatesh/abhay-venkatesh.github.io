---
layout: post
title: "On Software Systems Design"
categories: misc
---

> We are then in the position of beings who are sane and sober when engaged in trivial business and who gamble like madmen when confronted with serious issues—retail sanity and wholesale madness (Strauss, 1965)

There are two pieces to designing software systems:
- details
- the big picture

To get the details, one simply must read excellent resources such as [System Design Primer, Martin D.](https://github.com/donnemartin/system-design-primer) or [Google Technology Stack](http://michaelnielsen.org/blog/lecture-course-the-google-technology-stack/). 

But my focus in this article is on the big picture. For whatever reason, it seems like we refrain from general and aggrandizing theorization, and accordingly, we do not make general theories about software systems design. My goal here is to get at a basic starting point for a general software systems design theory. Perhaps the theory would apply to the design of all systems really, so I use the terms "software systems" and "systems" interchangeably throughout this piece.

It is fair to say that the design of systems is not a purely technical endeavor. Many get to this conclusion, but then they seem to make the jump to the other conclusion that the design of systems is some sort of purely aesthetic and creative endeavor. They are, to some extent, aesthetic. But I wonder if they are at least as philosophic, in the sense that there is a truth about the design of systems. The idea is that there are some general and universally applicable principles that are always true about designing systems, and that we do not need to concede arguments about the design of systems on purely aesthetic or creative grounds.

There has been some work on getting at these general principles about software design such as [Clean Architecture, Martin B.](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164), but people critique this book as [too opinionated](https://twitter.com/etscrivner/status/1260584368407445504?s=20), a critique which I sympathize with. In any case, Bob Martin makes a lot of progress with respect to general theorization, for example with the [Single-Responsibiltiy Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle), which I do admire and adhere to.

#### System Design and People

I want to take a different cut at this problem. I want to look at system design not as a set of rules about systems, but rather a set of rules about people. More generally, I think software engineering is really misunderstood because people do not understand that software engineering is at least as much about people as it is about software. Software engineering is a social activity, and thus, to understand the design of software systems, we need to understand people.

I have not fully developed a coherent reason for why listing things are nefarious, e.g. 5 ways to get rich, 7 ways to lose fat, 10 ways to get great ay systems design, but such listings have gained a well-earned notoriety. Hence, I take a different approach to get at understanding how software must be designed by understanding people. There will still be an implicit listing, but I will try to develop a story or a larger theme that make them work together. 

#### A Listing Anyway
- API Design
- Minimalism
- Pessimism
- Principles
    - Law of Demeter
    - Single-Responsibility Principle
    - Open-Closed Principle

#### References
- [1] Natural Right and History, 1965, Strauss L.
- [2] [System Design Primer, Martin D.](https://github.com/donnemartin/system-design-primer)
- [3] [On System Design, Waldo J.](https://scholar.harvard.edu/files/waldo/files/ps-2006-6.pdf)