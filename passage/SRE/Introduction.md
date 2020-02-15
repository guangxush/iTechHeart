## Introduction

The ops team attempts to safeguard the running system against the risk of change by introducing launch and change gates. For example, launch reviews may contain an explicit check for every problem that has ever caused an outage in the past—that could be an arbitrarily long list, with not all elements providing equal value. The dev team quickly learns how to respond. They have fewer “launches” and more “flag flips,” “incremental updates,” or “cherry‐ picks.” They adopt tactics such as sharding the product so that fewer features are sub‐ ject to the launch review.

### Site Reliability Engineering

Conflict isn’t an inevitable part of offering a software service. Google has chosen to run our systems with a different approach: our Site Reliability Engineering teams focus on hiring software engineers to run our products and to create systems to accomplish the work that would otherwise be performed, often manually, by sysadmins.


### Google’s Approach to Service Management: Site Reliability Engineering

We’ve found that Google SRE’s approach to running large-scale systems has many advantages. Because SREs are directly modifying code in their pursuit of making Google’s systems run themselves, SRE teams are characterized by both rapid innova‐ tion and a large acceptance of change. Such teams are relatively inexpensive—sup‐ porting the same service with an ops-oriented team would require a significantly larger number of people. Instead, the number of SREs needed to run, maintain, and improve a system scales sublinearly with the size of the system. Finally, not only does SRE circumvent the dysfunctionality of the dev/ops split, but this structure also improves our product development teams: easy transfers between product develop‐ ment and SRE teams cross-train the entire group, and improve skills of developers who otherwise may have difficulty learning how to build a million-core distributed system.


### Tenets of SRE

While the nuances of workflows, priorities, and day-to-day operations vary from SRE team to SRE team, all share a set of basic responsibilities for the service(s) they sup‐ port, and adhere to the same core tenets. In general, an SRE team is responsible for the availability, latency, performance, efficiency, change management, monitoring, emergency response, and capacity planning of their service(s). We have codified rules of engagement and principles for how SRE teams interact with their environment— not only the production environment, but also the product development teams, the testing teams, the users, and so on. Those rules and work practices help us to main‐ tain our focus on engineering work, as opposed to operations work.

An outage is no longer a “bad” thing—it is an expected part of the process of innovation, and an occurrence that both development and SRE teams manage rather than fear.


### Monitoring

Monitoring is one of the primary means by which service owners keep track of a sys‐ tem’s health and availability. 

Monitoring should never require a human to interpret any part of the alerting domain. Instead, software should do the interpreting, and humans should be notified only when they need to take action.

### Emergency Response

Reliability is a function of mean time to failure (MTTF) and mean time to repair (MTTR) 
The most relevant metric in evaluating the effectiveness of emer‐ gency response is how quickly the response team can bring the system back to health —that is, the MTTR.

### Change Management

SRE has found that roughly 70% of outages are due to changes in a live system. Best practices in this domain use automation to accomplish the following:

- Implementing progressive rollouts
- Quickly and accurately detecting problems
- Rolling back changes safely when problems arise

 
### Demand Forecasting and Capacity Planning

Demand forecasting and capacity planning can be viewed as ensuring that there is sufficient capacity and redundancy to serve projected future demand with the required availability. 

Several steps are mandatory in capacity planning:
- An accurate organic demand forecast, which extends beyond the lead time required for acquiring capacity
- An accurate incorporation of inorganic demand sources into the demand forecast
- Regular load testing of the system to correlate raw capacity (servers, disks, and so on) to service capacity


### Provisioning

Provisioning combines both change management and capacity planning. In our experience, provisioning must be conducted quickly and only when necessary, as capacity is expensive. This exercise must also be done correctly or capacity doesn’t work when needed. 


### Efficiency and Performance

Resource use is a function of demand (load), capacity, and software efficiency. SREs predict demand, provision capacity, and can modify the software. These three factors are a large part (though not the entirety) of a service’s efficiency.






