## Dealing with Interrupts

Any complex system is as imperfect as its creators. In managing the operational load created by these systems, remember that its creators are also imperfect machines.

- Operational load, when applied to managing complex systems, takes many forms, some more obvious than others. The terminology may change, but operational load falls into three general categories: pages, tickets, and ongoing operational activities.
- Pages concern production alerts and their fallout, and are triggered in response to production emergencies. They can sometimes be monotonous and recurring, requir‐ ing little thought. They can also be engaging and involve tactical in-depth thought. Pages always have an expected response time (SLO), which is sometimes measured in minutes.
- Tickets concern customer requests that require you to take an action. Like pages, tick‐ ets can be either simple and boring, or require real thought. A simple ticket might request a code review for a config the team owns. A more complex ticket might entail a special or unusual request for help with a design or capacity plan. Tickets may also have an SLO, but response time is more likely measured in hours, days, or weeks.

### Managing Operational Load

Ongoing operational responsibilities are also managed in varying ways. Sometimes, the on-call engineer does the work (pushes, flag flips, etc.). Alternately, each responsibil‐ ity can be assigned to team members ad hoc, or an on-call engineer might pick up a lasting responsibility (i.e., a multiweek rollout or ticket) that lasts beyond their shift week.

### Factors in Determining How Interrupts Are Handled

SRE teams at Google have found the following metrics to be useful in deciding how to manage interrupts:
- Interrupt SLO or expected response time
- The number of interrupts that are usually backlogged
- The severity of the interrupts
- The frequency of the interrupts
- The number of people available to handle a certain kind of interrupt (e.g., some teams require a certain amount of ticket work before going on-call)

### Imperfect Machines

#### Cognitive Flow State

The concept of flow state1 is widely accepted and can be empirically acknowledged by pretty much everyone who works in Software Engineering, Sysadmin, SRE, or most other disciplines that require focused periods of concentration. Being in “the zone” can increase productivity, but can also increase artistic and scientific creativity. Ach‐ ieving this state encourages people to actually master and improve the task or project they’re working on. Being interrupted can kick you right out of this state, if the inter‐ rupt is disruptive enough. You want to maximize the amount of time spent in this state.

Cognitive flow can also apply to less creative pursuits where the skill level required is lower, and the essential elements of flow are still fulfilled (clear goals, immediate feed‐ back, a sense of control, and the associated time distortion); examples include house‐ work or driving.

- Cognitive flow state: Creative and engaged

This is the zone: someone works on a problem for a while, is aware of and comforta‐ ble with the parameters of the problem, and feels like they can fix it or solve it. The person works intently on the problem, losing track of time and ignoring interrupts as much as possible. Maximizing the amount of time a person can spend in this state is very desirable—they’re going to produce creative results and do good work by vol‐ ume. They’ll be happier at the job they’re doing.

- Cognitive flow state: Angry Birds

People enjoy performing tasks they know how to do. In fact, executing such tasks is one of the clearest paths to cognitive flow. Some SREs are on-call when they reach a state of cognitive flow. It can be very fulfilling to chase down the causes of problems, work with others, and improve the overall health of the system in such a tangible way. Conversely, for most stressed-out on-call engineers, stress is caused either by pager volume, or because they’re treating on-call as an interrupt. They’re trying to code or work on projects while simultaneously being on-call or on full-time interrupts. These engineers exist in a state of constant interruption, or interruptability. This working environment is extremely stressful.

#### Do One Thing Well

- Distractibility

You can claim that some level of distraction is inevitable and by design. This assump‐ tion is correct: people do hang onto bugs for which they’re the primary contact, and people also build up other responsibilities and obligations. However, there are ways that a team can manage interrupt response so that more people (on average) can come into work in the morning and feel undistractible.

- Polarizing time

Polarizing time means that when a person comes into work each day, they should know if they’re doing just project work or just interrupts. Polarizing their time in this way means they get to concentrate for longer periods of time on the task at hand. They don’t get stressed out because they’re being roped into tasks that drag them away from the work they’re supposed to be doing.

#### Seriously, Tell Me What to Do

- General suggestions

For any given class of interrupt, if the volume of interrupts is too high for one person, add another person. This concept most obviously applies to tickets, but can poten‐ tially apply to pages, too—the on-call can start bumping things to their secondary, or downgrading pages to tickets.

- On-call

The primary on-call engineer should focus solely on on-call work. If the pager is quiet for your service, tickets or other interrupt-based work that can be abandoned fairly quickly should be part of on-call duties. 

- Tickets

If you currently assign tickets randomly to victims on your team, stop. Doing so is extremely disrespectful of your team’s time, and works completely counter to the principle of not being interruptible as much as possible.

- Ongoing responsibilities

As much as possible, define roles that let anyone on the team take up the mantle. If there’s a well-defined procedure for performing and verifying pushes or flag flips, then there’s no reason a person has to shepherd that change for its entire lifetime, even after they stop being on-call or on interrupts. Define a push manager role who can juggle pushes for the duration of their time on-call or on interrupts. Formalize the handover process—it’s a small price to pay for uninterrupted make time for the people not on-call.

- Be on interrupts, or don’t be

Sometimes when a person isn’t on interrupts, the team receives an interrupt that the person is uniquely qualified to handle. While ideally this scenario should never hap‐ pen, it sometimes does. You should work to make such occurrences rare.

#### Reducing Interrupts

Your team’s interrupt load may be unmanageable if it requires too many team mem‐ bers to simultaneously staff interrupts at any given time. There are a number of tech‐ niques you can use to reduce your ticket load overall.

- Actually analyze tickets
- Respect yourself, as well as your customers


Remember:
- Your team sets the level of service provided by your service.
- It’s OK to push back some of the effort onto your customers.

 




























