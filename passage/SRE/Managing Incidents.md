## Managing Incidents


### The Anatomy of an Unmanaged Incident

#### Sharp Focus on the Technical Problem

We tend to hire people like Mary for their technical prowess. So it’s not surprising that she was busy making operational changes to the system, trying valiantly to solve the problem. She wasn’t in a position to think about the bigger picture of how to miti‐ gate the problem because the technical task at hand was overwhelming.

#### Poor Communication

For the same reason, Mary was far too busy to communicate clearly. Nobody knew what actions their coworkers were taking. Business leaders were angry, customers were frustrated, and other engineers who could have lent a hand in debugging or fix‐ ing the issue weren’t used effectively.

#### Freelancing

Malcolm was making changes to the system with the best of intentions. However, he didn’t coordinate with his coworkers—not even Mary, who was technically in charge of troubleshooting. His changes made a bad situation far worse.


### Elements of Incident Management Process

#### Recursive Separation of Responsibilities

Several distinct roles should be delegated to particular individuals:

**Incident Command**

The incident commander holds the high-level state about the incident. They structure the incident response task force, assigning responsibilities according to need and priority. De facto, the commander holds all positions that they have not delegated. If appropriate, they can remove roadblocks that prevent Ops from working most effectively.

**Operational Work**

The Ops lead works with the incident commander to respond to the incident by applying operational tools to the task at hand. The operations team should be the only group modifying the system during an incident.

**Communication**

This person is the public face of the incident response task force. Their duties most definitely include issuing periodic updates to the incident response team and stakeholders (usually via email), and may extend to tasks such as keeping the incident document accurate and up to date.

**Planning**

The planning role supports Ops by dealing with longer-term issues, such as filing bugs, ordering dinner, arranging handoffs, and tracking how the system has diverged from the norm so it can be reverted once the incident is resolved.

#### A Recognized Command Post

Interested parties need to understand where they can interact with the incident commander. In many situations, locating the incident task force members into a cen‐ tral designated “War Room” is appropriate. Others teams may prefer to work at their desks, keeping alert to incident updates via email and IRC.

#### Live Incident State Document

The incident commander’s most important responsibility is to keep a living incident document. This can live in a wiki, but should ideally be editable by several people concurrently. Most of our teams use Google Docs, though Google Docs SRE use Goo‐ gle Sites: after all, depending on the software you are trying to fix as part of your inci‐ dent management system is unlikely to end well.


#### Clear, Live Handoff

It’s essential that the post of incident commander be clearly handed off at the end of the working day. If you’re handing off command to someone at another location, you can simply and safely update the new incident commander over the phone or a video call. Once the new incident commander is fully apprised, the outgoing commander should be explicit in their handoff, specifically stating, “You’re now the incident commander, okay?”, and should not leave the call until receiving firm acknowledg‐ ment of handoff. The handoff should be communicated to others working on the incident so that it’s clear who is leading the incident management efforts at all times.


### When to Declare an Incident

It is better to declare an incident early and then find a simple fix and close out the incident than to have to spin up the incident management framework hours into a burgeoning problem. Set clear conditions for declaring an incident. My team follows these broad guidelines—if any of the following is true, the event is an incident:
- Do you need to involve a second team in fixing the problem?
- Is the outage visible to customers?
- Is the issue unsolved even after an hour’s concentrated analysis?


Incident management proficiency atrophies quickly when it’s not in constant use. So how can engineers keep their incident management skills up to date—handle more incidents? Fortunately, the incident management framework can apply to other operational changes that need to span time zones and/or teams. If you use the framework frequently as a regular part of your change management procedures, you can easily follow this framework when an actual incident occurs. If your organization performs disaster-recovery testing (you should), incident man‐ agement should be part of that testing process. We often role-play the response to an on-call issue that has already been solved, perhaps by colleagues in another location, to further familiarize ourselves with incident management.

### Best Practices for Incident Management

- Prioritize. Stop the bleeding, restore service, and preserve the evidence for root- causing.
- Prepare. Develop and document your incident management procedures in advance, in consultation with incident participants.
- Trust. Give full autonomy within the assigned role to all incident participants. Introspect. Pay attention to your emotional state while responding to an incident. If
you start to feel panicky or overwhelmed, solicit more support.
- Consider alternatives. Periodically consider your options and re-evaluate whether it still makes sense to continue what you’re doing or whether you should be taking another tack in incident response.
- Practice. Use the process routinely so it becomes second nature.
- Change it around. Were you incident commander last time? Take on a different role this time. Encourage every team member to acquire familiarity with each role.













