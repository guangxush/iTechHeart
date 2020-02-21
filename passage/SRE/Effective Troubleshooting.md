## Effective Troubleshooting


Troubleshooting is a critical skill for anyone who operates distributed computing sys‐ tems—especially SREs—but it’s often viewed as an innate skill that some people have and others don’t.


Novices are often tripped up when troubleshooting because the exercise ideally depends upon two factors: an understanding of how to troubleshoot generically (i.e., without any particular system knowledge) and a solid knowledge of the system. While you can investigate a problem using only the generic process and derivation from first principles, we usually find this approach to be less efficient and less effective than understanding how things are supposed to work. 


### Theory

Formally, we can think of the troubleshooting process as an application of the hypothetico-deductive method: given a set of observations about a system and a the‐ oretical basis for understanding system behavior, we iteratively hypothesize potential causes for the failure and try to test those hypotheses.


![](../../image/SRE/troubleshooting.png)



Ineffective troubleshooting sessions are plagued by problems at the Triage, Examine, and Diagnose steps, often because of a lack of deep system understanding. The fol‐ lowing are common pitfalls to avoid:

- Looking at symptoms that aren’t relevant or misunderstanding the meaning of system metrics. Wild goose chases often result.
- Misunderstanding how to change the system, its inputs, or its environment, so as to safely and effectively test hypotheses.
- Coming up with wildly improbable theories about what’s wrong, or latching on to causes of past problems, reasoning that since it happened once, it must be hap‐ pening again.
- Hunting down spurious correlations that are actually coincidences or are correla‐ ted with shared causes.


Understanding failures in our reasoning process is the first step to avoiding them and becoming more effective in solving problems. A methodical approach to knowing what we do know, what we don’t know, and what we need to know, makes it simpler and more straightforward to figure out what’s gone wrong and how to fix it.



### In Practice

In practice, of course, troubleshooting is never as clean as our idealized model sug‐ gests it should be. There are some steps that can make the process less painful and more productive for both those experiencing system problems and those responding to them.

#### Problem Report

Every problem starts with a problem report, which might be an automated alert or one of your colleagues saying, “The system is slow.” An effective report should tell you the expected behavior, the actual behavior, and, if possible, how to reproduce the behavior.


#### Triage

Instead, your course of action should be to make the system work as well as it can under the circumstances.


#### Examine


Logging is another invaluable tool. Exporting information about each operation and about system state makes it possible to understand exactly what a process was doing at a given point in time. You may need to analyze system logs across one or many processes. Tracing requests through the whole stack using tools such as Dapper provides a very powerful way to understand how a distributed system is working, though varying use cases imply significantly different tracing designs.

Text logs are very helpful for reactive debugging in real time, while storing logs in a structured binary format can make it possible to build tools to conduct retrospective analysis with much more information.


It’s really useful to have multiple verbosity levels available, along with a way to increase these levels on the fly. This functionality enables you to examine any or all operations in incredible detail without having to restart your process, while still allowing you to dial back the verbosity levels when your service is operating normally. Depending of the volume of traffic your service receives, it might be better to use stat‐ istical sampling; for example, you might show one out of every 1,000 operations.

### Diagnose


#### Simplify and reduce


Ideally, components in a system have well-defined interfaces and perform known transformations from their input to their output (in our example, given an input search text, a component might return output containing possible matches). 

Dividing and conquering is a very useful general-purpose solution technique. In a multilayer system where work happens throughout a stack of components, it’s often best to start systematically from one end of the stack and work toward the other end, examining each component in turn.


#### Ask “what,” “where,” and “why”

A malfunctioning system is often still trying to do something—just not the thing you want it to be doing. Finding out what it’s doing, then asking why it’s doing that and where its resources are being used or where its output is going can help you under‐ stand how things have gone wrong.

Unpacking the Causes of a Symptom:

1. Symptom: A Spanner cluster has high latency and RPCs to its servers are timing out. 
2. Why? The Spanner server tasks are using all their CPU time and can’t make progress
on all the requests the clients send.
3. Where in the server is the CPU time being used? Profiling the server shows it’s sort‐ ing entries in logs checkpointed to disk.
4. Where in the log-sorting code is it being used? When evaluating a regular expression against paths to log files.
5. Solutions: Rewrite the regular expression to avoid backtracking. Look in the code‐ base for similar patterns. Consider using RE2, which does not backtrack and guaran‐ tees linear runtime growth with input size.11

#### What touched it last

Well-designed systems should have extensive production logging to track new ver‐ sion deployments and configuration changes at all layers of the stack, from the server binaries handling user traffic down to the packages installed on individual nodes in the cluster. Correlating changes in a system’s performance and behavior with other events in the system and environment can also be helpful in constructing monitoring dashboards.

#### Specific diagnoses

While the generic tools described previously are helpful across a broad range of prob‐ lem domains, you will likely find it helpful to build tools and systems to help with diagnosing your particular services. Google SREs spend much of their time building such tools. While many of these tools are necessarily specific to a given system, be sure to look for commonalities between services and teams to avoid duplicating effort.


### Test and Treat

There are a number of considerations to keep in mind when designing tests (which may be as simple as sending a ping or as complicated as removing traffic from a clus‐ ter and injecting specially formed requests to find a race condition):
- An ideal test should have mutually exclusive alternatives, so that it can rule one group of hypotheses in and rule another set out. In practice, this may be difficult to achieve.
- Consider the obvious first: perform the tests in decreasing order of likelihood, considering possible risks to the system from the test. It probably makes more sense to test for network connectivity problems between two machines before looking into whether a recent configuration change removed a user’s access to the second machine.
- An experiment may provide misleading results due to confounding factors. For example, a firewall rule might permit access only from a specific IP address, which might make pinging the database from your workstation fail, even if ping‐ ing from the application logic server’s machine would have succeeded.
- Active tests may have side effects that change future test results. For instance, allowing a process to use more CPUs may make operations faster, but might increase the likelihood of encountering data races. Similarly, turning on verbose logging might make a latency problem even worse and confuse your results: is the problem getting worse on its own, or because of the logging?
- Some tests may not be definitive, only suggestive. It can be very difficult to make race conditions or deadlocks happen in a timely and reproducible manner, so you may have to settle for less certain evidence that these are the causes.

### Negative Results Are Magic

#### Negative results should not be ignored or discounted. 
#### Experiments with negative results are conclusive.
#### Tools and methods can outlive the experiment and inform future work. 
#### Publishing negative results improves our industry’s data-driven culture.
#### Publish your results. 


### Cure

We can only find probable causal factors, for the following reasons:

- Systems are complex. It’s quite likely that there are multiple factors, each of which individually is not the cause, but which taken jointly are causes.15 Real systems are also often path-dependent, so that they must be in a specific state before a failure occurs.
- Reproducing the problem in a live production system may not be an option, either because of the complexity of getting the system into a state where the failure can be triggered, or because further downtime may be unacceptable. Having a non‐
production environment can mitigate these challenges, though at the cost of hav‐ ing another copy of the system to run.

Typically a sudden increase in latency and resource usage indicates either an increase in traffic sent to the system or a change in system configuration. 

### Making Troubleshooting Easier

There are many ways to simplify and speed troubleshooting. Perhaps the most funda‐ mental are:
- Building observability—with both white-box metrics and structured logs—into each component from the ground up.
- Designing systems with well-understood and observable interfaces between components.

Ensuring that information is available in a consistent way throughout a system—for instance, using a unique request identifier throughout the span of RPCs generated by various components—reduces the need to figure out which log entry on an upstream component matches a log entry on a downstream component, speeding the time to diagnosis and recovery.

Problems in correctly representing the state of reality in a code change or an environ‐ ment change often lead to a need to troubleshoot. Simplifying, controlling, and log‐ ging such changes can reduce the need for troubleshooting, and make it easier when it happens.











