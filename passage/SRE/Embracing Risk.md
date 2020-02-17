## Embracing Risk

Site Reliability Engineering seeks to balance the risk of unavailability with the goals of rapid innovation and efficient ser‐ vice operations, so that users’ overall happiness—with features, service, and perfor‐ mance—is optimized.

### Managing Risk

Unreliable systems can quickly erode users’ confidence, so we want to reduce the chance of system failure.

The costliness has two dimensions:

- The cost of redundant machine/compute resources

- The opportunity cost

In SRE, we manage service reliability largely by managing risk. We conceptualize risk as a continuum. 

### Measuring Service Risk

As standard practice at Google, we are often best served by identifying an objective metric to represent the property of a system we want to optimize. By setting a target, we can assess our current performance and track improvements or degradations over time. For service risk, it is not immediately clear how to reduce all of the potential factors into a single metric. Service failures can have many potential effects, including user dissatisfaction, harm, or loss of trust; direct or indirect revenue loss; brand or reputational impact; and undesirable press coverage. Clearly, some of these factors are very hard to measure. To make this problem tractable and consistent across many types of systems we run, we focus on unplanned downtime.

Unplanned downtime is captured by the desired level of service availability

Time-based availability:

***availability = uptime /(uptime + downtime)***

For example, a system with an availability target of 99.99% can be down for up to 52.56 minutes in a year and stay within its availability target.

At Google, however, a time-based metric for availability is usually not meaningful because we are looking across globally distributed services. Our approach to fault iso‐ lation makes it very likely that we are serving at least a subset of traffic for a given service somewhere in the world at any given time

Aggregate availability:

***availability = successful requests / total requests***

For example, a system that serves 2.5M requests in a day with a daily availability tar‐ get of 99.99% can serve up to 250 errors and still hit its target for that given day.

Quantifying unplanned downtime as a request success rate also makes this availabil‐ ity metric more amenable for use in systems that do not typically serve end users directly.


### Risk Tolerance of Services

To identify the risk tolerance of a service, SREs must work with the product owners to turn a set of business goals into explicit objectives to which we can engineer. 

#### Identifying the Risk Tolerance of Consumer Services

There are many factors to consider when assessing the risk tolerance of services, such as the following:
- What level of availability is required?
- Do different types of failures have different effects on the service?
- How can we use the service cost to help locate a service on the risk continuum?
- What other service metrics are important to take into account?

 Target level of availability:

- What level of service will the users expect?
- Does this service tie directly to revenue (either our revenue, or our customers’ revenue)?
- Is this a paid service, or is it free?
- If there are competitors in the marketplace, what level of service do those com‐ petitors provide?
- Is this service targeted at consumers, or at enterprises?

The expected shape of failures for a given service is another important consideration. How resilient is our business to service downtime? Which is worse for the service: a constant low rate of failures, or an occasional full-site outage? Both types of failure may result in the same absolute number of errors, but may have vastly different impacts on the business.'

Cost is often the key factor in determining the appropriate availability target for a ser‐ vice. Ads is in a particularly good position to make this trade-off because request suc‐ cesses and failures can be directly translated into revenue gained or lost.

- If we were to build and operate these systems at one more nine of availability, what would our incremental increase in revenue be?
- Does this additional revenue offset the cost of reaching that level of reliability?

Examining the risk tolerance of services in relation to metrics besides availability is often fruitful. Understanding which metrics are important and which metrics aren’t important provides us with degrees of freedom when attempting to take thoughtful risks.

### Identifying the Risk Tolerance of Infrastructure Services

To understand the different needs of the different types of users, you can look at the desired state of the request queue for each type of Bigtable user.

The user concerned with offline analysis is more interested in system throughput, so that user wants request queues to never be empty. To optimize for throughput, the Bigtable system should never need to idle while waiting for its next request.

Note that we can run multiple classes of services using identical hardware and soft‐ ware. We can provide vastly different service guarantees by adjusting a variety of ser‐ vice characteristics, such as the quantities of resources, the degree of redundancy, the geographical provisioning constraints, and, critically, the infrastructure software configuration.


### Motivation for Error Budgets


Product development performance is largely evaluated on product velocity, which creates an incentive to push new code as quickly as possible. Meanwhile, SRE performance is (unsurprisingly) evaluated based upon reliability of a service, which implies an incentive to push back against a high rate of change. 


The product devel‐ opers have more visibility into the time and effort involved in writing and releasing their code, while the SREs have more visibility into the service’s reliability (and the state of production in general).


- Software fault tolerance

How hardened do we make the software to unexpected events? Too little, and we have a brittle, unusable product. Too much, and we have a product no one wants to use (but that runs very stably).

- Testing

Again, not enough testing and you have embarrassing outages, privacy data leaks, or a number of other press-worthy events. Too much testing, and you might lose your market.


- Push frequency

Every push is risky. How much should we work on reducing that risk, versus doing other work?


- Canary duration and size

It’s a best practice to test a new release on some small subset of a typical work‐ load, a practice often called canarying. How long do we wait, and how big is the canary?


#### Forming Your Error Budget

- Product Management defines an SLO, which sets an expectation of how much uptime the service should have per quarter.

- The actual uptime is measured by a neutral third party: our monitoring system.

- The difference between these two numbers is the “budget” of how much “unreli‐ ability” is remaining for the quarter.

- As long as the uptime measured is above the SLO—in other words, as long as there is error budget remaining—new releases can be pushed.


#### Benefits

The main benefit of an error budget is that it provides a common incentive that allows both product development and SRE to focus on finding the right balance between innovation and reliability.


Slowing down releases or rolling them back when the SLO-violation error budget is close to being used up.


### Key Insights

- Managing service reliability is largely about managing risk, and managing risk can be costly.

- 100% is probably never the right reliability target: not only is it impossible to achieve, it’s typically more reliability than a service’s users want or notice. Match the profile of the service to the risk the business is willing to take.

- An error budget aligns incentives and emphasizes joint ownership between SRE and product development. Error budgets make it easier to decide the rate of releases and to effectively defuse discussions about outages with stakeholders, and allows multiple teams to reach the same conclusion about production risk without rancor.





