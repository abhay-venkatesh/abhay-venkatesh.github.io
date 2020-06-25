---
layout: post
title: "On Systems Thinking"
categories: misc
---

![](http://pilot-projects.org/images/banner/CofP_sketch_banner2.jpg)

### Introduction
> We are then in the position of beings who are sane and sober when engaged in trivial business and who gamble like madmen when confronted with serious issues—retail sanity and wholesale madness [(Strauss, 1965)](https://contemporarythinkers.org/leo-strauss/book/natural-right-and-history/)

There are two pieces to thinking about systems:

- details
- the big picture

To get the details, one simply must consume a lot of [discrete systems design knowledge](#specific-systems-knowledge) such as the classic Google systems research papers or resources like the [System Design Primer by Donne Martin](https://github.com/donnemartin/system-design-primer). Part of becoming a good system designer is just to get experience with a good selection of system design problems and background knowledge. I suspect there is no way to circumvent that.

However, my focus in this article is to try to get at the problem of system design from a more big picture perspective. While there has been a lot of work with respect to finding [lists of general system design principles](#listings-of-architectural-principles), I wonder if there is something more fundamental that can be said about systems. Typically, systems thinking is posed in opposition to pure technicality as a more aesthetic endeavor, but my claim is that it at least as philosophic, in the sense that there is a fundamental truth about systems thinking, and that debates about systems thinking do not have to be conceded on aesthetic grounds. 


> Software engineering is what happens to programming when you add time and other programmers. — Russ Cox 

> ...and perhaps its just about time, you are a different person depending on the time.

Additionally, it is worthwhile to approach systems thinking as not merely a problem about systems *tout court* but as a problem of people interacting with systems. System complexity would be irrelevant if people could instantly understand any arbitrarily complex system. The problem is that complex systems without good design are hard to understand, and therefore we need to impose discipline onto systems. Therefore, **a shift in first principles about people can powerfully inform the way we approach the construction of systems.**

### Boundaries due to Differences

Since everyone at your company is unlikely to be great at exactly the same things, in fact they should not be since you want to hire people who are uniquely good at what they do as much as possible, one of the important considerations when building software systems is accounting for these differences.

For example, a classic divide is the engineer vs. mathematician dichotomy. Engineers are good at writing elegant APIs, maintainable code, and high-performance systems, but bad at solving hard math problems and tend to have somewhat quixotic preferences for tools and practices. Mathematicians are good at solving hard math problems while having little patience about building and maintaining software systems.

The conclusion here is basically that **one of the most important aspects of system design is the skill of drawing boundaries**. A separation of mathematical from engineering concerns is critical for organizations that have both mathematical and engineering problems. If people strong at X thing spend a lot of time doing Y thing that they are not good at, then it is natural that you could expect suboptimal outcomes. For that reason, good system design requires excellent and well-thought boundaries. 

> Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure. — Melvin E. Conway

Applied to engineering organizations, this leads to [Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law), which also suggests a way in which boundaries feed back into themselves, reflecting the outcomes they produce. Issues due to differences are not limited to differences in engineer skills. Differences can also emanate in a variety of situations, such as when differences between customers and a company requires a service-level agreement, or within a technical system and its various parts (which leads to rules such as [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter), [Single-Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle), or [Principle of Least Privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)). The solution is always defining boundaries since **boundaries help regulate differences in system components through well-defined contracts**. 

### Pessimism due to Entropy

**Systems are entropic, and sans a regulating function, systems tend to disorder.** In the people domain, this is because people make mistakes, and people loose discipline. Thus, you need to be extremely pessimistic about the future of any system one you are building. You can come up with an excellent design on day one, but as time passes, the system can only get worse because you can write a system from scratch only once, but you will have to extend it indefinitely. 

Hence, boundaries must be extremely well thought of, and the initial architecture must be as good as possible. There are of course resource constraints that define how you can manage this, maybe you need to just prove something out and get it to work. That might be a fine investment choice, but it does not break the principle. **Great system designs require great starting points.**

Pessimism is in a way minimalism since doing anything more than necessary is just a seed for some sort of disaster or bad decision in the future. Fewer opportunities for entropic disorder is always desirable. 

### People over Process

**Building systems should be a people responsibility not a process responsibility.** While there might not be a total absolution of all process, coming up with the right processes to your organization is key. Blind application of process will not lead to greatness because processes have little value outside of the appropriate context. And the choice of context must be made by individuals. Hence, it is far more important to focus on getting the kind of people who are able to [develop the minimally necessary processes from first principles](https://ericscrivner.me/2017/06/software-process-first-principles/). 

In addition to the problem of defining process context, there is a non-linearity in decision making that is difficult to capture in any given process. Some decisions matter much more than others, and in fact, these non-linear decisions might ultimately be the only decisions that matter. For example, the decision to include a critical system feature that might change the course of a company dramatically can only be made by individuals. This is why you cannot replace Founders with a career CEO or worse, a committee. This ultimately leads to a rejection of thinking in decision-making mechanisms, and an acceptance of putting talented people in charge. 

### The Design and Iteration Dialectic

If you spend all your time on design, or all your time on iteration, then you are doing the wrong thing. The correct solution is a [dialectical resolution](https://www.quora.com/Joe-Lonsdale-what-are-dialectics-and-why-are-they-important-useful) between the needs of design and iteration. That is, **we need to have both, a very high standard of design, and simultaneously a very strong discipline of moving fast and getting things done.**

[Patrick Collison notes that many great things were built very fast](https://patrickcollison.com/fast). There is something about momentum that is very important to building systems. The opposite of momentum is maybe something like total friction, and it is naturally a bad outcome for any system if it gets burnt out by friction before it even hits production.

The goal is to break the false dichotomy of design and iteration - you want both in extremes. Of course at first sight this is fairly contradictory, but there are ways to resolve it. One is the 80/20 rule: you can get 80 percent of the benefits of design (or iteration) by doing it 20 percent of the time. So there is a Pareto efficiency curve that applies. Furthermore, you can break the dichotomy by hiring talented engineers who can both produce top notch designs and iterate fast. Differences in engineering talent is well-documented, and to get more of design and iteration simultaneously, focus on hiring only the very best.

### Conclusion

The ideas I develop here are fairly generally applicable to systems (software or people or generally). These ideas also apply onto each other in a higher order fashion. For example, you might want to apply the design and iteration dialectic onto the defining of boundaries. Or might want to have someone take responsibility of the design and iteration process rather than resorting to some off-the-shelf method like agile. I suspect there might be more ideas like these that would be useful to develop. Curious to hear what you think, or have any ideas about furthering some of the thought here. Please let me know! 

Thanks to [Eric Scrivner](https://twitter.com/etscrivner), [Jared Newman](https://www.linkedin.com/in/jared-newman-54a57b50/), and [Stephen Pimentel](https://twitter.com/StephenPiment) for their feedback. 

### Related Work

#### Specific Systems Knowledge

- [Designing Data-Intensive Applications by Martin Kleppmann](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321)
- [Jeff Dean Numbers](http://brenocon.com/dean_perf.html)
- [System Design Primer by Donne Martin](https://github.com/donnemartin/system-design-primer)
- [The Google Technology Stack by Michael Nielsen](http://michaelnielsen.org/blog/lecture-course-the-google-technology-stack/)

#### Listings of Architectural Principles

- [Documenting Software Architectures: Views and Beyond](https://www.oreilly.com/library/view/documenting-software-architectures/9780132488617/)
- [Software Architecture in Practice](https://www.amazon.com/Software-Architecture-Practice-3rd-Engineering/dp/0321815734)
- Michael Jackson's [Software Requirements and Specifications](https://www.amazon.com/Software-Requirements-Specifications-Principles-Prejudices/dp/0201877120#:~:text=Software%20Reqiuirements%20and%20Specifications%20is,requirements%20analysis%2C%20specification%20and%20design.) as well as his general [Problem Frames](https://people.csail.mit.edu/dnj/teaching/6898/lecture-notes/session8/slides/mj-problem-frames.pdf) approach to solution modeling
- [Watts Humphrey's work at the SEI](https://resources.sei.cmu.edu/news-events/events/watts/watts.cfm) (namely on the PSP and TSP)
- [Object-Oriented Software Engineering by Jacobson](https://www.amazon.com/Object-Oriented-Software-Engineering-Approach/dp/0201544350)
- [Clean Architecture, Martin B.](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164)

#### Miscellaneous

- [On System Design, Waldo J.](https://scholar.harvard.edu/files/waldo/files/ps-2006-6.pdf)
    - a good sociological treatment on why system design knowledge is being lost
- Software Engineering for the Apollo Space Program
- Thinking in Systems: A Primer by Donella Meadows
- [Pilot Projects](http://pilot-projects.org/blog/entry/communities-of-practice-using-systems-thinking-to-co-create-a-better-world) for the cover photo