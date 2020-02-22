## Postmortem Culture: Learning from Failure

As SREs, we work with large-scale, complex, distributed systems. We constantly enhance our services with new features and add new systems. Incidents and outages are inevitable given our scale and velocity of change. When an incident occurs, we fix the underlying issue, and services return to their normal operating conditions. Unless we have some formalized process of learning from these incidents in place, they may recur ad infinitum. Left unchecked, incidents can multiply in complexity or even cas‐ cade, overwhelming a system and its operators and ultimately impacting our users. Therefore, postmortems are an essential tool for SRE.

### Google’s Postmortem Philosophy

Teams have some internal flexibility, but common postmor‐ tem triggers include:
- User-visible downtime or degradation beyond a certain threshold
- Data loss of any kind
- On-call engineer intervention (release rollback, rerouting of traffic, etc.)
- A resolution time above some threshold
- A monitoring failure (which usually implies manual incident discovery)


Blameless postmortems are a tenet of SRE culture. For a postmortem to be truly blameless, it must focus on identifying the contributing causes of the incident without indicting any individual or team for bad or inappropriate behavior. A blamelessly written postmortem assumes that everyone involved in an incident had good inten‐ tions and did the right thing with the information they had. If a culture of finger pointing and shaming individuals or teams for doing the “wrong” thing prevails, peo‐ ple will not bring issues to light for fear of punishment.


### Best Practice: Avoid Blame and Keep It Constructive

Blameless postmortems can be challenging to write, because the postmortem format clearly identifies the actions that led to the incident. Removing blame from a post‐ mortem gives people the confidence to escalate issues without fear. It is also impor‐ tant not to stigmatize frequent production of postmortems by a person or team. An atmosphere of blame risks creating a culture in which incidents and issues are swept under the rug, leading to greater risk for the organization 


### Collaborate and Share Knowledge

Regardless of the specific tool you use, look for the following key features:
- **Real-time collaboration**
Enables the rapid collection of data and ideas. Essential during the early creation of a postmortem.
- **An open commenting/annotation system**
Makes crowdsourcing solutions easy and improves coverage.
- **Email notifications**
Can be directed at collaborators within the document or used to loop in others to provide input.

Writing a postmortem also involves formal review and publication. In practice, teams share the first postmortem draft internally and solicit a group of senior engineers to assess the draft for completeness. Review criteria might include:

- Was key incident data collected for posterity?
- Are the impact assessments complete?
- Was the root cause sufficiently deep?
- Is the action plan appropriate and are resulting bug fixes at appropriate priority?
- Did we share the outcome with relevant stakeholders?

### Best Practice: No Postmortem Left Unreviewed
An unreviewed postmortem might as well never have existed. To ensure that each completed draft is reviewed, we encourage regular review sessions for postmortems. In these meetings, it is important to close out any ongoing discussions and com‐ ments, to capture ideas, and to finalize the state.

### Introducing a Postmortem Culture

**Postmortem of the month**
In a monthly newsletter, an interesting and well-written postmortem is shared with the entire organization.

**Google+ postmortem group**
This group shares and discusses internal and external postmortems, best practi‐ ces, and commentary about postmortems.

**Postmortem reading clubs**
Teams host regular postmortem reading clubs, in which an interesting or impact‐ ful postmortem is brought to the table (along with some tasty refreshments) for an open dialogue with participants, nonparticipants, and new Googlers about what happened, what lessons the incident imparted, and the aftermath of the incident. Often, the postmortem being reviewed is months or years old!

**Wheel of Misfortune**
New SREs are often treated to the Wheel of Misfortune exercise, in which a previous postmortem is reenacted with a cast of engineers playing roles as laid out in the postmortem. The original inci‐ dent commander attends to help make the experience as “real” as possible.

### Conclusion and Ongoing Improvements
We can say with confidence that thanks to our continuous investment in cultivating a postmortem culture, Google weathers fewer outages and fosters a better user experi‐ ence. Our “Postmortems at Google” working group is one example of our commit‐ ment to the culture of blameless postmortems. This group coordinates postmortem efforts across the company: pulling together postmortem templates, automating post‐ mortem creation with data from tools used during an incident, and helping automate data extraction from postmortems so we can perform trend analysis. We’ve been able to collaborate on best practices from products as disparate.

