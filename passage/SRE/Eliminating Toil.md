## Eliminating Toil

In SRE, we want to spend time on long-term engineering project work instead of operational work. Because the term operational work may be misinterpreted, we use a specific word: toil.

Cleaning up the entire alerting configuration for your service and removing clutter may be grungy, but it’s not toil.

Toil is the kind of work tied to running a production service that tends to be manual, repetitive, automatable, tactical, devoid of enduring value, and that scales linearly as a service grows.

Manual, Repetitive, Automatable, Tactical, No enduring value, O(n) with service growth

### Why Less Toil Is Better

Our SRE organization has an advertised goal of keeping operational work (i.e., toil) below 50% of each SRE’s time. 

At least 50% of each SRE’s time should be spent on engineering project work that will either reduce future toil or add service features. Feature development typically focuses on improving reliability, performance, or uti‐ lization, which often reduces toil as a second-order effect.

when we hire new SREs, we promise them that SRE is not a typical Ops organization, quoting the 50% rule just mentioned. We need to keep that promise by not allowing the SRE organization or any subteam within it to devolve into an Ops team.

Consistent with this data, SREs report that their top source of toil is interrupts (that is, non-urgent service-related messages and emails). The next leading source is on- call (urgent) response, followed by releases and pushes. 


### What Qualifies as Engineering?

Engineering work is novel and intrinsically requires human judgment. It produces a permanent improvement in your service, and is guided by a strategy. It is frequently creative and innovative, taking a design-driven approach to solving a problem—the more generalized, the better. Engineering work helps your team or the SRE organiza‐ tion handle a larger service, or more services, with the same level of staffing.

Typical SRE activities fall into the following approximate categories:

- Software engineering: Involves writing or modifying code, in addition to any associated design and documentation work. Examples include writing automation scripts, creating tools or frameworks, adding service features for scalability and reliability, or modifying infrastructure code to make it more robust.
- Systems engineering: Involves configuring production systems, modifying configurations, or docu‐ menting systems in a way that produces lasting improvements from a one-time effort. Examples include monitoring setup and updates, load balancing configu‐ ration, server configuration, tuning of OS parameters, and load balancer setup. Systems engineering also includes consulting on architecture, design, and pro‐ ductionization for developer teams.
- Toil: Work directly tied to running a service that is repetitive, manual, etc.
- Overhead: Administrative work not tied directly to running a service. Examples include hir‐ ing, HR paperwork, team/company meetings, bug queue hygiene, snippets, peer reviews and self-assessments, and training courses.

Toil isn’t always and invariably bad, and everyone needs to be absolutely clear that some amount of toil is unavoidable in the SRE role, and indeed in almost any engi‐ neering role. 

Among the many reasons why too much toil is bad, consider the following:

- Career stagnation: Your career progress will slow down or grind to a halt if you spend too little time on projects. Google rewards grungy work when it’s inevitable and has a big posi‐ tive impact, but you can’t make a career out of grunge.
- Low morale: People have different limits for how much toil they can tolerate, but everyone has a limit. Too much toil leads to burnout, boredom, and discontent.

Additionally, spending too much time on toil at the expense of time spent engineer‐ ing hurts an SRE organization in the following ways:

- Creates confusion: We work hard to ensure that everyone who works in or with the SRE organiza‐ tion understands that we are an engineering organization. Individuals or teams within SRE that engage in too much toil undermine the clarity of that communi‐ cation and confuse people about our role.
- Slows progress: Excessive toil makes a team less productive. A product’s feature velocity will slow if the SRE team is too busy with manual work and firefighting to roll out new features promptly.
- Sets precedent:If you’re too willing to take on toil, your Dev counterparts will have incentives to load you down with even more toil, sometimes shifting operational tasks that should rightfully be performed by Devs to SRE. Other teams may also start expecting SREs to take on such work, which is bad for obvious reasons.
- Promotes attrition:Even if you’re not personally unhappy with toil, your current or future team‐ mates might like it much less. If you build too much toil into your team’s proce‐ dures, you motivate the team’s best engineers to start looking elsewhere for a more rewarding job.
- Causes breach of faith:New hires or transfers who joined SRE with the promise of project work will feel cheated, which is bad for morale.




