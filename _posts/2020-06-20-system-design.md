---
layout: post
title: "On Software Systems Design"
categories: misc
---

#### Topics
- [Introduction](#Introduction)
- [Aesthetics vs. Philosophy](#aesthetics-vs.-philosophy)
- [Existing Work](#existing-work)
- [System Design and People](#system-design-and-people)
- [Boundaries due to Differences](#boundaries-due-to-differences)
- [Pessimism due to Entropy](#pessimism-due-to-entropy)
- [The Design and Iteration Dialectic](#the-design-and-iteration-dialectic)

#### Introduction

> We are then in the position of beings who are sane and sober when engaged in trivial business and who gamble like madmen when confronted with serious issues—retail sanity and wholesale madness [(Strauss, 1965)](https://contemporarythinkers.org/leo-strauss/book/natural-right-and-history/)

There are two pieces to designing software systems:
- details
- the big picture

To get the details, one simply must read excellent resources such as [System Design Primer, Martin D.](https://github.com/donnemartin/system-design-primer) or [Google Technology Stack](http://michaelnielsen.org/blog/lecture-course-the-google-technology-stack/). Part of becoming a good system designer is just to have experience with a good selection of system design problems. I suspect there is no way to circumvent that.

But my focus in this article is on the big picture. For whatever reason, it seems like we refrain from general and aggrandizing theorization, and accordingly, we do not make general theories about software systems design. My goal here is to get at a basic starting point for a general software systems design theory. Perhaps the theory would apply to the design of all systems really, so I use the terms "software systems" and "systems" interchangeably throughout this piece.

#### Aesthetics vs. Philosophy

It is fair to say that the design of systems is not a purely technical endeavor. Many get to this conclusion, but then they seem to make the jump to the other conclusion that the design of systems is some sort of purely aesthetic and creative endeavor. They are, to some extent, aesthetic. But I wonder if they are at least as philosophic, in the sense that there is a truth about the design of systems. The idea is that there are some general and universally applicable principles that are always true about designing systems, and that we do not need to concede arguments about the design of systems on purely aesthetic or creative grounds.

#### Existing Work

There has been some work on getting at these general principles about software design such as [Clean Architecture, Martin B.](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164), but people critique this book as [too opinionated](https://twitter.com/etscrivner/status/1260584368407445504?s=20), a critique which I sympathize with, at least with respect to the [specific design] Uncle Bob touts. In any case, Bob Martin makes a lot of progress with respect to general theorization, and provides an excellent listing of principles that seem to have become fairly canonical and undisputed.

There are more technical treatments on this problem with respect to scalability and performance. A good resource for that is [Designing Data-Intensive Applications, Kleppmann, M.](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321). The text comes up with solutions to the fact that modern applications are data-constrainted and not-compute constrained, and comes up with a listing of ideas around how to make applications scale with respect to data.

There is also a [good sociological treatment on the problem of system design by Jim Waldo](https://scholar.harvard.edu/files/waldo/files/ps-2006-6.pdf). The focus is so much on principles of software design but rather why it the art of system design is being lost.

#### System Design and People

I want to take a different cut at this problem. I want to look at system design not as a set of rules about systems, but rather a set of rules about people. More generally, I think software engineering is really misunderstood because people do not understand that software engineering is at least as much about people as it is about software. Software engineering is a social activity, and thus, to understand the design of software systems, we need to understand people.

I have not fully developed a coherent reason for why listing things are nefarious, e.g. 5 ways to get rich, 7 ways to lose fat, 10 ways to get great ay systems design, but such listings have gained a well-earned notoriety. Hence, I take a different approach to get at understanding how software must be designed by understanding people. There will still be an implicit listing, but I will try to develop a story or a larger theme that make them work together. 

#### Boundaries due to Differences

Since everyone at your company is unlikely to be great at the same things, in fact they should not be since you want to hire people who are uniquely good at what they do as much as possible, one of the important considerations when building software systems is accounting for these differences in skillsets.

A classic divide I have encountered is the engineer vs. mathematician dichotomy. Engineers are good at writing elegant APIs, maintainable code, and high-performance systems, and bad at solving hard math problems and tend to have somewhat quixotic preferences for tools and practices. Mathematicians are good at solving hard math problems, but have little patience about building software systems.

The conclusion here is basically that one of the most important aspects of system design is the skill of drawing boundaries. A separation of mathematical from engineering concerns is critical for organizations with mathematical and engineering problems. If people strong at X thing spend a lot of time doing Y thing that they are not good at, then it is natural that you could expect suboptimal outcomes. 

Furthermore, you can extend this argument not just to different people but to the same person. The same person changes over time, and your thoughts one day might not be the same the other. If you expose your entire system to all your thoughts all the time, then you will naturally end up with a bad system design. For that reason, a good design requires excellent and well-thought boundaries.

Applied to engineering organizations, this leads to [Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law).

#### Pessimism due to Entropy

A general principle about organizations (or systems) is that there is this notion of entropy. Sans a regulating function, things tend to disorder. This applies both to software engineering organizations, and codebases. The conclusion here is that one needs to be extremely pessimistic about the future of any system you are building. You can come up with an excellent design on day one, but as time passes, the system can only get worse. 

A good conclusion here is that the system can only get worse, because you can write a system from scratch only once, but you will have to extend it indefinitely. Hence, APIs must be extremely-well thought of, and the initial architecture must be as good as possible. There are of course resource constraints that define how you can manage this, maybe you need to just prove something out and get it to work. That might be a fine investment choice, but it does not break the principle. Great systems designs require great starting points.

#### The Design and Iteration Dialectic

If you spend all your time on design, or all your time on iteration, then you are doing the wrong thing. The correct solution is a dialectical resolution between the needs of design and iteration (https://www.quora.com/Joe-Lonsdale-what-are-dialectics-and-why-are-they-important-useful). That is, we need to do both, a very high standard of design, and simultaneously a very strong discipline of moving fast and getting things done. 

[Patrick Collison notes that many great things were built very fast](https://patrickcollison.com/fast). There is something about momentum that is very important to building systems. The opposite of momentum is maybe something like total friction, and it is naturally a bad outcome for any system if it gets burnt out by friction before it even hits production. 

The goal is to break the false dichotomy of design and iteration - you want both in extremes. Of course at first sight this is fairly contradictory, but there are ways to resolve it. One is the 80/20 rule: you can get 80% of the benefits of design (or iteration) by doing it 20% of the time. So there is a pareto efficiency curve that applies. Furthermore, you can break the dichotomy by hiring talented engineers who can both produce top notch designs and iterate fast. Differences in engineering talent is well-documented, and to get more of design and iteration simultaneously, focus on hiring only the very best.

#### Conclusion

I have produced three philosophic critiques:
- boundaries due to differences
- pessimism due to entropy
- design and iteration dialectic

These are fairly generally applicable to systems (software or people or generally), and meditating on some of these ideas have improved my engineering outputs a lot. Curious to hear what you think, or have any ideas about furthering some of the thought here. Please let me know! 