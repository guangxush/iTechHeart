## Communication and Collaboration in SRE

The two preceding facts have steered the SRE organization in certain directions when it comes to two crucial dimensions of how our teams operate—communications and collaboration. Data flow would be an apt computing metaphor for our communica‐ tions: just like data must flow around production, data also has to flow around an SRE team—data about projects, the state of the services, production, and the state of the individuals. For maximum effectiveness of a team, the data has to flow in reliable ways from one interested party to another. One way to think of this flow is to think of the interface that an SRE team must present to other teams, such as an API. Just like an API, a good design is crucial for effective operation, and if the API is wrong, it can be painful to correct later on.

### Communications: Production Meetings

However, there’s one kind of meeting that we have that is more useful than the aver‐ age, called a production meeting. Production meetings are a special kind of meeting where an SRE team carefully articulates to itself—and to its invitees—the state of the service(s) in their charge, so as to increase general awareness among everyone who cares, and to improve the operation of the service(s). In general, these meetings are service-oriented; they are not directly about the status updates of individuals. The goal is for everyone to leave the meeting with an idea of what’s going on—the same idea. The other major goal of production meetings is to improve our services by bringing the wisdom of production to bear on our services. That means we talk in detail about the operational performance of the service, and relate that operational performance to design, configuration, or implementation, and make recommendations for how to fix the problems. Connecting the performance of the service with design decisions in a regular meeting is an immensely powerful feedback loop.


#### Agenda

- Upcoming production changes
- Metrics
- Outages
- Paging events
- Nonpaging events
- Prior action items

#### Attendance

Occasionally you’ll have too many teams or busy-yet-crucial attendees to invite. There are a number of techniques you can use to handle those situations:
- Less active services might be attended by a single representative from the product development team, or only have commitment from the product development team to read and comment on the agenda minutes.
- If the production development team is quite large, nominate a subset of repre‐ sentatives.
- Busy-yet-crucial attendees can provide feedback and/or steering in advance to individuals, or using the prefilled agenda technique (described next).

### Collaboration within SRE

This pattern of distribution also informs how SRE teams tend to be organized. Because our raison d’être is bringing value through technical mastery, and technical mastery tends to be hard, we therefore try to find a way to have mastery over some related subset of systems or infrastructures, in order to decrease cognitive load. Spe‐ cialization is one way of accomplishing this objective; i.e., team X works only on product Y. Specialization is good, because it leads to higher chances of improved technical mastery, but it’s also bad, because it leads to siloization and ignorance of the broader picture. We try to have a crisp team charter to define what a team will—and more importantly, won’t—support, but we don’t always succeed.

#### Team Composition

Formally, SRE teams have the roles of “tech lead” (TL), “manager” (SRM), and “project manager” (also known as PM, TPM, PgM). Some people operate best when those roles have well-defined responsibilities: the major benefit of this being they can make in-scope decisions quickly and safely. Others operate best in a more fluid envi‐ ronment, with shifting responsibilities depending on dynamic negotiation. In gen‐ eral, the more fluid the team is, the more developed it is in terms of the capabilities of the individuals, and the more able the team is to adapt to new situations—but at the cost of having to communicate more and more often, because less background can be assumed.

#### Techniques for Working Effectively

In general, good SRE work calls for excellent communication skills when you’re working outside the boundary of your purely local team. For collaborations outside the building, effectively working across time zones implies either great written com‐ munication, or lots of travel to supply the in-person experience that is deferrable but
ultimately necessary for a high-quality relationship. Even if you’re a great writer, over time you decay into just being an email address until you turn up in the flesh again.

#### Recommendations

You should only develop projects cross-site when you have to, but often there are good reasons to have to. The cost of working across sites is higher latency for actions and more communication being required; the benefit is—if you get the mechanics right—much higher throughput. The single site project can also fall foul of no one outside of that site knowing what you’re doing, so there are costs to both approaches.

The standard “divide and conquer” strategy applies to cross-site projects; you reduce communication costs primarily by splitting the project into as many reasonably sized components as possible, and trying to make sure that each component can be assigned to a small group, preferably within one site. Divide these components among the project subteams, and establish clear deliverables and deadlines. (Try not to let Conway’s law distort the natural shape of the software too deeply.)4

A goal for a project team works best when it’s oriented toward providing some func‐ tionality or solving some problem. This approach ensures that the individuals work‐ ing on a component know what is expected of them, and that their work is only complete once that component is fully integrated and used within the main project.


### Conclusion

Given the globally distributed nature of SRE teams, effective communication has always been a high priority in SRE. This chapter has discussed the tools and techni‐ ques that SRE teams use to maintain effective relationships among their team and with their various partner teams.

Collaboration between SRE teams has its challenges, but potentially great rewards, including common approaches to platforms for solving problems, letting us focus on solving more difficult problems.

























