## Lessons Learned from Other Industries

Google’s engineers about their previous work experiences in a variety of other high-reliability fields in order to address the following comparative questions:

- Are the principles used in Site Reliability Engineering also important outside of Google, or do other industries tackle the requirements of high reliability in mark‐ edly different ways?
- If other industries also adhere to SRE principles, how are the principles manifes‐ ted?
- What are the similarities and differences in the implementation of these princi‐ ples across industries?
- What factors drive similarities and differences in implementation?
- What can Google and the tech industry learn from these comparisons?

We distilled these concepts into four key themes:
- Preparedness and Disaster Testing
- Postmortem Culture
- Automation and Reduced Operational Overhead
- Structured and Rational Decision Making

### Preparedness and Disaster Testing

SREs push production systems to the limit and inflict actual outages in order to:
- Ensure that systems react the way we think they will
- Determine unexpected weaknesses
- Figure out ways to make the systems more robust in order to prevent uncontrol‐ led outages

Several strategies for testing disaster readiness and ensuring preparedness in other industries emerged from our conversations. Strategies included the following:
- Relentless organizational focus on safety
- Attention to detail
- Swing capacity
- Simulations and live drills
- Training and certification
- Obsessive focus on detailed requirements gathering and design
- Defense in depth

#### Relentless Organizational Focus on Safety

The manufacturing industry prepares itself for the unexpected by establishing highly defined processes that are strictly followed at every level of the organization. It is crit‐ ical that all employees take safety seriously, and that workers feel empowered to speak up if and when anything seems amiss. 

#### Attention to Detail

The nuclear Navy focuses on routine maintenance to ensure that small issues don’t snowball.

#### Swing Capacity

System utilization in the telecom industry can be highly unpredictable. Absolute capacity can be strained by unforeseeable events such as natural disasters, as well as large, predictable events like the Olympics.

#### Simulations and Live Drills

The live drills involve “actually breaking real stuff but with control parameters. Live drills are carried out religiously, every week, two to three days per week.” For the nuclear Navy, thought exercises are useful, but not suffi‐ cient to prepare for actual incidents. Responses must be practiced so they are not forgotten.

#### Training and Certification

Our interviews suggest that training and certification are particularly important when lives are at stake. 

#### Focus on Detailed Requirements Gathering and Design

Some of the engineers we interviewed discussed the importance of detailed require‐ ments gathering and design docs. 

#### Defense in Depth and Breadth

Defense in depth is particularly important in the nuclear industry due to the zero tolerance for failures and incidents.

### Postmortem Culture

It’s important to evaluate all of the following:
- What happened
- The effectiveness of the response
- What we would do differently next time
- What actions will be taken to make sure a particular incident doesn’t happen again

This exercise is undertaken without pointing fingers at any individual. Instead of assigning blame, it is far more important to figure out what went wrong, and how, as an organization, we will rally to ensure it doesn’t happen again. Dwelling on who might have caused the outage is counterproductive. Postmortems are conducted after incidents and published across SRE teams so that all can benefit from the lessons learned.

### Automating Away Repetitive Work and Operational Overhead
    
At their core, Google’s Site Reliability Engineers are software engineers with a low tol‐ erance for repetitive reactive work. It is strongly ingrained in our culture to avoid repeating an operation that doesn’t add value to a service. If a task can be automated away, why would you run a system on repetitive work that is of low value? Automa‐ tion lowers operational overhead and frees up time for our engineers to proactively assess and improve the services they support.
  
### Structured and Rational Decision Making

At Google in general, and in Site Reliability Engineering in particular, data is critical. The team aspires to make decisions in a structured and rational way by ensuring that:
- The basis for the decision is agreed upon advance, rather than justified ex post facto
- The inputs to the decision are clear
- Any assumptions are explicitly stated
- Data-driven decisions win over decisions based on feelings, hunches, or the opin‐ ion of the most senior employee in the room

Google SRE operates under the baseline assumption that everyone on the team:
- Has the best interests of a service’s users at heart
- Can figure out how to proceed based on the data available

### Conclusions

Many of the principles that are core to Site Reliability Engineering at Google are evi‐ dent across a wide range of industries. The lessons already learned by well-established industries likely inspired some of the practices in use at Google today.

A main takeaway of our cross-industry survey was that in many parts of its software business, Google has a higher appetite for velocity than players in most other indus‐ tries. The ability to move or change quickly must be weighed against the differing implications of a failure. In the nuclear, aviation, or medical industries, for example, people could be injured or even die in the event of an outage or failure. When the stakes are high, a conservative approach to achieving high reliability is warranted.





















