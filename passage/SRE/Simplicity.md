## Simplicity

Software systems are inherently dynamic and unstable.

A software system can only be perfectly stable if it exists in a vacuum. If we stop changing the codebase, we stop introducing bugs. If the underlying hardware or libraries never change, neither of these components will introduce bugs. If we freeze the current user base, we’ll never have to scale the system. In fact, a good summary of the SRE approach to managing systems is: “At the end of the day, our job is to keep agility and stability in balance in the system.”

### System Stability Versus Agility

For the majority of production software systems, we want a balanced mix of stability and agility. SREs work to create procedures, practices, and tools that render software more reliable. At the same time, SREs ensure that this work has as little impact on developer agility as possible. In fact, SRE’s experience has found that reliable pro‐ cesses tend to actually increase developer agility: rapid, reliable production rollouts make changes in production easier to see. As a result, once a bug surfaces, it takes less time to find and fix that bug. Building reliability into development allows developers to focus their attention on what we really do care about—the functionality and per‐ formance of their software and systems.


### The Virtue of Boring

It is very important to consider the difference between essential complexity and accidental complexity. Essential complexity is the complexity inherent in a given situation that cannot be removed from a problem definition, whereas accidental complexity is more fluid and can be resolved with engineering effort. For example, writing a web server entails dealing with the essential complexity of serving web pages quickly. However, if we write a web server in Java, we may introduce accidental complexity when trying to minimize the performance impact of garbage collection.

With an eye towards minimizing accidental complexity, SRE teams should:
- Push back when accidental complexity is introduced into the systems for which they are responsible
- Constantly strive to eliminate complexity in systems they onboard and for which they assume operational responsibility

### I Won’t Give Up My Code!


At the risk of sounding extreme, when you consider a web service that’s expected to be available 24/7, to some extent, every new line of code written is a liability. SRE pro‐ motes practices that make it more likely that all code has an essential purpose, such as scrutinizing code to make sure that it actually drives business goals, routinely remov‐ ing dead code, and building bloat detection into all levels of testing.


### The “Negative Lines of Code” Metric

The term “software bloat” was coined to describe the tendency of software to become slower and bigger over time as a result of a constant stream of additional features. While bloated software seems intuitively undesirable, its negative aspects become even more clear when considered from the SRE perspective: every line of code changed or added to a project creates the potential for introducing new defects and bugs.

### Minimal APIs

Writing clear, minimal APIs is an essential aspect of managing simplicity in a soft‐ ware system. The fewer methods and arguments we provide to consumers of the API, the easier that API will be to understand, and the more effort we can devote to mak‐ ing those methods as good as they can possibly be. Again, a recurring theme appears: the conscious decision to not take on certain problems allows us to focus on our core problem and make the solutions we explicitly set out to create substantially better. In software, less is more! A small, simple API is usually also a hallmark of a well- understood problem.


### Modularity
Expanding outward from APIs and single binaries, many of the rules of thumb that apply to object-oriented programming also apply to the design of distributed systems. The ability to make changes to parts of the system in isolation is essential to creating a supportable system. Specifically, loose coupling between binaries, or between binar‐ ies and configuration, is a simplicity pattern that simultaneously promotes developer agility and system stability. If a bug is discovered in one program that is a component of a larger system, that bug can be fixed and pushed to production independent of the rest of the system.

### Release Simplicity
Simple releases are generally better than complicated releases. 


### A Simple Conclusion

Software simplicity is a prerequisite to reliability. We are not being lazy when we consider how we might simplify each step of a given task. Instead, we are clarifying what it is we actually want to accomplish and how we might most easily do so. Every time we say “no” to a feature, we are not restricting innovation; we are keeping the environment uncluttered of distrac‐ tions so that focus remains squarely on innovation, and real engineering can proceed.





