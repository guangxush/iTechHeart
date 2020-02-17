## Service Level Objectives

Ultimately, choosing appropriate metrics helps to drive the right action if something goes wrong, and also gives an SRE team confidence that a service is healthy.

### Service Level Terminology

#### Indicators


An SLI is a service level indicator—a carefully defined quantitative measure of some aspect of the level of service that is provided.

The SLI directly measures a service level of interest, but sometimes only a proxy is available because the desired measure may be hard to obtain or interpret. 

Another kind of SLI important to SREs is availability, or the fraction of the time that a service is usable.


#### Objectives

An SLO is a service level objective: a target value or range of values for a service level that is measured by an SLI. A natural structure for SLOs is thus SLI ≤ target or lower bound ≤ SLI ≤ upper bound. 


QPS and latency—might be connected behind the scenes: higher QPS often leads to larger latencies, and it’s common for services to have a performance cliff beyond some load threshold.


#### Agreements

Finally, SLAs are service level agreements: an explicit or implicit contract with your users that includes consequences of meeting (or missing) the SLOs they contain.

SRE doesn’t typically get involved in constructing SLAs, because SLAs are closely tied to business and product decisions. SRE does, however, get involved in helping to avoid triggering the consequences of missed SLOs. They can also help to define the SLIs: there obviously needs to be an objective way to measure the SLOs in the agree‐ ment, or disagreements will arise.

### Indicators in Practice


#### What Do You and Your Users Care About?

We typically find that a handful of representative indicators are enough to evaluate and reason about a system’s health.

An understanding of what your users want from the system will inform the judicious selection of a few indicators. Choosing too many indicators makes it hard to pay the right level of attention to the indicators that matter, while choosing too few may leave significant behaviors of your system unexamined. 

- User-facing serving systems: availability, latency, and throughput
- Storage systems often emphasize latency, availability, and durability. 
- Big data systems, such as data processing pipelines, tend to care about throughput and end-to-end latency.
- All systems should care about correctness


### Collecting Indicators

Many indicator metrics are most naturally gathered on the server side, using a moni‐ toring system such as Borgmon (see Chapter 10) or Prometheus, or with periodic log analysis—for instance, HTTP 500 responses as a fraction of all requests. 


### Aggregation

- the number of requests per second served,
- a latency SLI, some requests will be serviced quickly, while others will invariably take longer—sometimes much longer. 


The higher the variance in response times, the more the typical user experience is affected by long-tail behavior, an effect exacerba‐ ted at high load by queuing effects. User studies have shown that people typically pre‐ fer a slightly slower system to one with high variance in response time, so some SRE teams focus only on high percentile values, on the grounds that if the 99.9th percen‐ tile behavior is good, then the typical experience is certainly going to be.


### Objectives in Practice

#### Defining Objectives

SLOs should specify how they’re measured and the conditions under which they’re valid. 


#### Choosing Targets

Choosing targets (SLOs) is not a purely technical activity because of the product and business implications, which should be reflected in both the SLIs and SLOs (and maybe SLAs) that are selected. Similarly, it may be necessary to trade off certain prod‐ uct attributes against others within the constraints posed by staffing, time to market, hardware availability, and funding. While SRE should be part of this conversation, and advise on the risks and viability of different options, we’ve learned a few lessons that can help make this a more productive discussion:


- Don’t pick a target based on current performance
- Keep it simple
- Avoid absolutes
- Have as few SLOs as possible
- Perfection can wait


SLOs can—and should—be a major driver in prioritizing work for SREs and product developers, because they reflect what users care about. A good SLO is a helpful, legiti‐ mate forcing function for a development team.


#### Control Measures

SLIs and SLOs are crucial elements in the control loops used to manage systems:

1. Monitor and measure the system’s SLIs.
2. Compare the SLIs to the SLOs, and decide whether or not action is needed.
3. If action is needed, figure out what needs to happen in order to meet the target.
4. Take that action.

#### SLOs Set Expectations

In order to set realistic expectations for your users, you might consider using one or both of the following tactics:

- Keep a safety margin
- Don’t overachieve

Alternatively, if the service is doing fine, perhaps staff time should be spent on other priorities, such as paying off technical debt, adding new features, or introducing other products.

### Agreements in Practice

Crafting an SLA requires business and legal teams to pick appropriate consequences and penalties for a breach. SRE’s role is to help them understand the likelihood and difficulty of meeting the SLOs contained in the SLA. Much of the advice on SLO con‐ struction is also applicable for SLAs. It is wise to be conservative in what you adver‐ tise to users, as the broader the constituency, the harder it is to change or delete SLAs that prove to be unwise or difficult to work with.





