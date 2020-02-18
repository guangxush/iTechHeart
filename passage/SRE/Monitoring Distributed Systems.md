## Monitoring Distributed Systems

### Definitions

Even within Google, usage of the following terms varies, but the most common interpretations are listed here.

Monitoring, White-box monitoring, Black-box monitoring, Dashboard, Alert, Root cause, Node and machine, Push


### Why Monitor?

There are many reasons to monitor a system, including:

- Analyzing long-term trends
- Comparing over time or experiment groups
- Alerting
- Building dashboards
- Conducting ad hoc retrospective analysis (i.e., debugging)

Monitoring and alerting enables a system to tell us when it’s broken, or perhaps to tell us what’s about to break.

### Setting Reasonable Expectations for Monitoring

That being said, while it can be fun to have access to traffic graph dashboards and the like, SRE teams carefully avoid any situation that requires someone to “stare at a screen to watch for problems.”

We seldom use rules such as, “If I know the database is slow, alert for a slow database; otherwise, alert for the website being generally slow.”

### Symptoms Versus Causes

Your monitoring system should address two questions: what’s broken, and why?

### Black-Box Versus White-Box

We combine heavy use of white-box monitoring with modest but critical uses of black-box monitoring. The simplest way to think about black-box monitoring versus white-box monitoring is that black-box monitoring is symptom-oriented and represents active—not predicted—problems: “The system isn’t working correctly, right now.” White-box monitoring depends on the ability to inspect the innards of the system, such as logs or HTTP endpoints, with instrumentation. White-box monitoring therefore allows detection of imminent problems, failures masked by retries, and so forth.

For paging, black-box monitoring has the key benefit of forcing discipline to only nag a human when a problem is both already ongoing and contributing to real symptoms.


### The Four Golden Signals

- Latency: The time it takes to service a request. 
- Traffic: A measure of how much demand is being placed on your system, measured in a high-level system-specific metric. 
- Errors： The rate of requests that fail, either explicitly (e.g., HTTP 500s), implicitly (for example, an HTTP 200 success response, but coupled with the wrong content), or by policy (for example, “If you committed to one-second response times, any request over one second is an error”).
- Saturation: How “full” your service is. A measure of your system fraction, emphasizing the resources that are most constrained (e.g., in a memory-constrained system, show memory; in an I/O-constrained system, show I/O). Note that many systems degrade in performance before they achieve 100% utilization, so having a utilization target is essential.


### Worrying About Your Tail (or, Instrumentation and Performance)

The simplest way to differentiate between a slow average and a very slow “tail” of requests is to collect request counts bucketed by latencies (suitable for rendering a histogram), rather than actual latencies: how many requests did I serve that took between 0 ms and 10 ms, between 10 ms and 30 ms, between 30 ms and 100 ms, between 100 ms and 300 ms, and so on? Distributing the histogram boundaries approximately exponentially (in this case by factors of roughly 3) is often an easy way to visualize the distribution of your requests.


### Choosing an Appropriate Resolution for Measurements

1. Record the current CPU utilization each second.
2. Using buckets of 5% granularity, increment the appropriate CPU utilization bucket each second.
3. Aggregate those values every minute.

### As Simple as Possible, No Simpler

- Alerts on different latency thresholds, at different percentiles, on all kinds of different metrics
- Extra code to detect and expose possible causes
- Associated dashboards for each of these possible causes


In choosing what to monitor, keep the following guidelines in mind:
- The rules that catch real incidents most often should be as simple, predictable, and reliable as possible.
- Data collection, aggregation, and alerting configuration that is rarely exercised (e.g., less than once a quarter for some SRE teams) should be up for removal.
- Signals that are collected, but not exposed in any prebaked dashboard nor used by any alert, are candidates for removal.

### Tying These Principles Together


Tying These Principles Together:

- Every time the pager goes off, I should be able to react with a sense of urgency. I can only react with a sense of urgency a few times a day before I become fatigued.
- Every page should be actionable.
- Every page response should require intelligence. If a page merely merits a robotic response, it shouldn’t be a page.
- Pages should be about a novel problem or an event that hasn’t been seen before.


### Monitoring for the Long Term

It’s important that decisions about monitoring be made with long-term goals in mind. Every page that happens today distracts a human from improving the system for tomorrow, so there is often a case for taking a short-term hit to availability or performance in order to improve the long-term outlook for the system. 

#### Bigtable SRE: A Tale of Over-Alerting

Email alerts were triggered as the SLO approached, and paging alerts were triggered when the SLO was exceeded. Both types of alerts were firing voluminously, consum‐ ing unacceptable amounts of engineering time: the team spent significant amounts of time triaging the alerts to find the few that were really actionable, and we often missed the problems that actually affected users, because so few of them did. Many of the pages were non-urgent, due to well-understood problems in the infrastructure, and had either rote responses or received no response.

This strategy gave us enough breathing room to actually fix the longer-term problems in Bigtable and the lower layers of the storage stack, rather than constantly fixing tac‐ tical problems. On-call engineers could actually accomplish work when they weren’t being kept up by pages at all hours. Ultimately, temporarily backing off on our alerts allowed us to make faster progress toward a better service.


#### Gmail: Predictable, Scriptable Responses from Humans

At that time, the Gmail monitoring was structured such that alerts fired when indi‐ vidual tasks were “de-scheduled” by Workqueue. This setup was less than ideal because even at that time, Gmail had many, many thousands of tasks, each task repre‐ senting a fraction of a percent of our users. We cared deeply about providing a good user experience for Gmail users, but such an alerting setup was unmaintainable.

Pages with rote, algorithmic responses should be a red flag. Unwillingness on the part of your team to automate such pages implies that the team lacks confidence that they can clean up their technical debt. This is a major problem worth escalating.

#### The Long Run

A common theme connects the previous examples of Bigtable and Gmail: a tension between short-term and long-term availability. Often, sheer force of effort can help a rickety system achieve high availability, but this path is usually short-lived and fraught with burnout and dependence on a small number of heroic team members. Taking a controlled, short-term decrease in availability is often a painful, but strategic trade for the long-run stability of the system. It’s important not to think of every page as an event in isolation, but to consider whether the overall level of paging leads toward a healthy, appropriately available system with a healthy, viable team and long- term outlook. We review statistics about page frequency (usually expressed as inci‐ dents per shift, where an incident might be composed of a few related pages) in quarterly reports with management, ensuring that decision makers are kept up to date on the pager load and overall health of their teams.

### Conclusion

A healthy monitoring and alerting pipeline is simple and easy to reason about. It focuses primarily on symptoms for paging, reserving cause-oriented heuristics to serve as aids to debugging problems. Monitoring symptoms is easier the further “up” your stack you monitor, though monitoring saturation and performance of subsys‐ tems such as databases often must be performed directly on the subsystem itself. Email alerts are of very limited value and tend to easily become overrun with noise; instead, you should favor a dashboard that monitors all ongoing subcritical problems for the sort of information that typically ends up in email alerts. A dashboard might also be paired with a log, in order to analyze historical correlations.


Over the long haul, achieving a successful on-call rotation and product includes choosing to alert on symptoms or imminent real problems, adapting your targets to goals that are actually achievable, and making sure that your monitoring supports rapid diagnosis.

