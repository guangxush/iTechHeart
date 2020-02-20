## Being On-Call

Being on-call is a critical duty that many operations and engineering teams must undertake in order to keep their services reliable and available. 

### Introduction

Several professions require employees to perform some sort of on-call duty, which entails being available for calls during both working and nonworking hours. In the IT context, on-call activities have historically been performed by dedicated Ops teams tasked with the primary responsibility of keeping the service(s) for which they are responsible in good health.


The SRE teams are quite different from purely operational teams in that they place heavy emphasis on the use of engineering to approach problems. 


We cap the amount of time SREs spend on purely operational work at 50%; at minimum, 50% of an SRE’s time should be allocated to engineering projects that further scale the impact of the team through automation, in addition to improving the service.


### Life of an On-Call Engineer


As soon as a page is received and acknowledged, the on-call engineer is expected to triage the problem and work toward its resolution, possibly involving other team members and escalating as needed.

Nonpaging production events, such as lower priority alerts or software releases, can also be handled and/or vetted by the on-call engineer during business hours. These activities are less urgent than paging events, which take priority over almost every other task, including project work. 


Many teams have both a primary and a secondary on-call rotation. The distribution of duties between the primary and the secondary varies from team to team. One team might employ the secondary as a fall-through for the pages the primary on-call misses. Another team might specify that the primary on-call handles only pages, while the secondary handles all other non-urgent production activities.


### Balanced On-Call

SRE teams have specific constraints on the quantity and quality of on-call shifts. The quantity of on-call can be calculated by the percent of time spent by engineers on on- call duties. The quality of on-call can be calculated by the number of incidents that occur during an on-call shift.

SRE managers have the responsibility of keeping the on-call workload balanced and sustainable across these two axes.

#### Balance in Quantity

We strongly believe that the “E” in “SRE” is a defining characteristic of our organiza‐ tion, so we strive to invest at least 50% of SRE time into engineering: of the remain‐ der, no more than 25% can be spent on-call, leaving up to another 25% on other types of operational, nonproject work.

If a service entails enough work to justify growing a single-site team, we prefer to cre‐ ate a multi-site team. A multi-site team is advantageous for two reasons:
- Night shifts have detrimental effects on people’s health, and a multi-site “follow the sun” rotation allows teams to avoid night shifts altogether.
- Limiting the number of engineers in the on-call rotation ensures that engineers do not lose touch with the production systems


#### Balance in Quality

Let’s define an incident as a sequence of events and alerts that are related to the same root cause and would be discussed as part of the same postmortem. We’ve found that on average, dealing with the tasks involved in an on-call incident—root-cause analysis, remedia‐ tion, and follow-up activities like writing a postmortem and fixing bugs—takes 6 hours. It follows that the maximum number of incidents per day is 2 per 12-hour on- call shift. In order to stay within this upper bound, the distribution of paging events should be very flat over time, with a likely median value of 0: if a given component or issue causes pages every day (median incidents/day > 1), it is likely that something else will break at some point, thus causing more incidents than should be permitted.

#### Compensation

This compensation structure ensures incen‐ tivization to be involved in on-call duties as required by the team, but also promotes a balanced on-call work distribution and limits potential drawbacks of excessive on-call work, such as burnout or inadequate time for project work.


### Feeling Safe

As mentioned earlier, SRE teams support Google’s most critical systems. Being an SRE on-call typically means assuming responsibility for user-facing, revenue-critical systems or for the infrastructure required to keep these systems up and running. SRE methodology for thinking about and tackling problems is vital for the appropriate operation of services.

Modern research identifies two distinct ways of thinking that an individual may, con‐ sciously or subconsciously, choose when faced with challenges:

- Intuitive, automatic, and rapid action
- Rational, focused, and deliberate cognitive functions


It’s important that on-call SREs understand that they can rely on several resources that make the experience of being on-call less daunting than it may seem. The most important on-call resources are:
- Clear escalation paths
- Well-defined incident-management procedures
- A blameless postmortem culture

Finally, when an incident occurs, it’s important to evaluate what went wrong, recog‐ nize what went well, and take action to prevent the same errors from recurring in the future. SRE teams must write postmortems after significant incidents and detail a full timeline of the events that occurred. By focusing on events rather than the people, these postmortems provide significant value. Rather than placing blame on individu‐ als, they derive value from the systematic analysis of production incidents. Mistakes happen, and software should make sure that we make as few mistakes as possible. Recognizing automation opportunities is one of the best ways to prevent human errors.


### Avoiding Inappropriate Operational Load


#### Operational Overload

The SRE team and leadership are responsible for including concrete objectives in quarterly work planning in order to make sure that the workload returns to sustaina‐ ble levels. Temporarily loaning an experienced SRE to an overloaded team.

Misconfigured monitoring is a common cause of operational overload. 

In extreme cases, SRE teams may have the option to “give back the pager”—SRE can ask the developer team to be exclusively on-call for the system until it meets the standards of the SRE team in question. Giving back the pager doesn’t happen very frequently, because it’s almost always possible to work with the developer team to reduce the operational load and make a given system more reliable. In some cases, though, complex or architectural changes spanning multiple quarters might be required to make a system sustainable from an operational point of view. In such cases, the SRE team should not be subject to an excessive operational load. Instead, it is appropriate to negotiate the reorganization of on-call responsibilities with the development team, possibly routing some or all paging alerts to the developer on-call. Such a solution is typically a temporary measure, during which time the SRE and developer teams work together to get the service in shape to be on-boarded by the SRE team again.

#### A Treacherous Enemy: Operational Underload

To counteract this eventuality, SRE teams should be sized to allow every engineer to be on-call at least once or twice a quarter, thus ensuring that each team member is sufficiently exposed to production. “Wheel of Misfortune” exercises (discussed in Chapter 28) are also useful team activities that can help to hone and improve trouble‐ shooting skills and knowledge of the service. Google also has a company-wide annual disaster recovery event called DiRT (Disaster Recovery Training) that combines theo‐ retical and practical drills to perform multiday testing of infrastructure systems and individual services.

### Conclusions

The approach to on-call described in this chapter serves as a guideline for all SRE teams in Google and is key to fostering a sustainable and manageable work environ‐ ment. Google’s approach to on-call has enabled us to use engineering work as the pri‐ mary means to scale production responsibilities and maintain high reliability and availability despite the increasing complexity and number of systems and services for which SREs are responsible.

While this approach might not be immediately applicable to all contexts in which engineers need to be on-call for IT services, we believe it represents a solid model that organizations can adopt in scaling to meet a growing volume of on-call work.


















