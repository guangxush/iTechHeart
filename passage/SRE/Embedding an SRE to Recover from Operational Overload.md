## Embedding an SRE to Recover from Operational Overload

It’s standard policy for Google’s SRE teams to evenly split their time between projects and reactive ops work. In practice, this balance can be upset for months at a time by an increase in the daily ticket volume. A burdensome amount of ops work is espe‐ cially dangerous because the SRE team might burn out or be unable to make progress on project work. When a team must allocate a disproportionate amount of time to resolving tickets at the cost of spending time improving the service, scalability and reliability suffer.

### Phase 1: Learn the Service and Get Context

Your job while embedded with the team is to articulate why processes and habits con‐ tribute to, or detract from, the service’s scalability. Remind the team that more tickets should not require more SREs: the goal of the SRE model is to only introduce more humans as more complexity is added to the system. Instead, try to draw attention to how healthy work habits reduce the time spent on tickets. Doing so is as important as pointing out missed opportunities for automation or simplification of the service.


#### Identify the Largest Sources of Stress

SRE teams sometimes fall into ops mode because they focus on how to quickly address emergencies instead of how to reduce the number of emergencies. A default to ops mode usually happens in response to an overwhelming pressure, real or imag‐ ined. After you’ve learned enough about the service to ask hard questions about its design and deployment, spend some time prioritizing various service outages accord‐ ing to their impact on the team’s stress levels.

#### Identify Kindling

Sometimes impending emergencies come in the form of a new subsystem that isn’t designed to be self-managing. Other sources include:

- Knowledge gaps
- Services developed by SRE that are quietly increasing in importance
- Strong dependence on “the next big thing”
- Common alerts that aren’t diagnosed by either the dev team or SREs
- Any service that is both the subject of complaints from your clients and lacks a formal SLI/SLO/SLA
- Any service with a capacity plan that is effectively “Add more servers: our servers were running out of memory last night”
- Postmortems that only have action items for rolling back the specific changes that caused an outage
- Any serving-critical component for which the existing SREs respond to questions by say‐ ing, “We don’t know anything about that; the devs own it”

### Phase 2: Sharing Context

#### Write a Good Postmortem for the Team

Instead of trying to correct previous mistakes, take ownership of the next postmor‐ tem. There will be an outage while you’re embedded. If you aren’t the person on-call, team up with the on-call SRE to write a great, blameless postmortem. This document is an opportunity to demonstrate how a shift toward the SRE model benefits the team by making bug fixes more permanent. More permanent bug fixes reduce the impact of outages on team members’ time.

#### Sort Fires According to Type

There are two types of fires in this simplified-for-convenience model:
- Some fires shouldn’t exist. They cause what is commonly called ops work or toil.
- Other fires that cause stress and/or furious typing are actually part of the job. In
either case, the team needs to build tools to control the burn.

### Phase 3: Driving Change

#### Start with the Basics

Your first goal for the team should be writing a service level objective (SLO), if one doesn’t already exist. The SLO is important because it provides a quantitative measure of the impact of outages, in addition to how important a process change could be.

#### Get Help Clearing Kindling

1. Find useful work that can be accomplished by one team member.
2. Clearly explain how this work addresses an issue from the postmortem in a per‐ manent way. Even otherwise healthy teams can produce shortsighted action items.
3. Serve as the reviewer for the code changes and document revisions.
4. Repeat for two or three issues.

#### Explain Your Reasoning

As the team recovers its momentum and grasps the basics of your suggested changes, move on to tackle the quotidian decisions that originally led to ops overload. Prepare for this undertaking to be challenged. If you’re lucky, the challenge will be along the lines of, “Explain why. Right now. In the middle of the weekly production meeting.”

#### Ask Leading Questions

Leading questions are not loaded questions. When talking with the SRE team, try to ask questions in a way that encourages people to think about the basic principles. It’s particularly valuable for you to model this behavior because, by definition, a team in ops mode rejects this sort of reasoning from its own constituents. 


### Conclusion

Following the tenets outlined in this chapter provides an SRE team with the following:
- A technical, possibly quantitative, perspective on why they should change.
- A strong example of what change looks like.
- A logical explanation for much of the “folk wisdom” used by SRE.
- The core principles needed to address novel situations in a scalable manner.


























