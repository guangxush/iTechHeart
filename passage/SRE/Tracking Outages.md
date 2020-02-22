## Tracking Outages

Improving reliability over time is only possible if you start from a known baseline and can track progress. “Outalator,” our outage tracker, is one of the tools we use to do just that. Outalator is a system that passively receives all alerts sent by our moni‐ toring systems and allows us to annotate, group, and analyze this data.


### Escalator

At Google, all alert notifications for SRE share a central replicated system that tracks whether a human has acknowledged receipt of the notification. If no acknowledg‐ ment is received after a configured interval, the system escalates to the next config‐ ured destination(s)—e.g., from primary on-call to secondary. This system, called “The Escalator,” was initially designed as a largely transparent tool that received copies of emails sent to on-call aliases.


### Outalator

Following Escalator’s example, where we added useful features to existing infrastruc‐ ture, we created a system that would deal not just with the individual escalating noti‐ fications, but with the next layer of abstraction: outages.

Outalator lets users view a time-interleaved list of notifications for multiple queues at once, instead of requiring a user to switch between queues manually.


### Aggregation

The ability to group multiple alerts together into a single incident is critical in dealing with this duplication. Sending an email saying “this is the same thing as that other thing; they are symptoms of the same incident” works for a given alert: it can prevent duplication of debugging or panic. But sending an email for each alert is not a practi‐ cal or scalable solution for handling duplicate alerts within a team, let alone between teams or over longer periods of time.


### Tagging

Of course, not every alerting event is an incident. False-positive alerts occur, as well as test events or mistargeted emails from humans. The Outalator itself does not distin‐ guish between these events, but it allows general-purpose tagging to add metadata to notifications, at any level. Tags are mostly free-form, single “words.” Colons, however, are interpreted as semantic separators, which subtly promotes the use of hierarchical namespaces and allows some automatic treatment. 

Overall, tags have been a remarkably powerful tool for teams to obtain and provide an overview of a given service’s pain points, even without much, or even any, formal analysis. As trivial as tagging appears, it is probably one of the Outalator’s most useful unique features.


### Analysis

Enabling such analysis is one of the most important functions of an outage tracking tool.

The bottom layer of analysis encompasses counting and basic aggregate statistics for reporting. The details depend on the team, but include information such as incidents per week/month/quarter and alerts per incident. The next layer is more important, and easy to provide: comparison between teams/services and over time to identify first patterns and trends.

### Unexpected Benefits

Being able to identify that an alert, or a flood of alerts, coincides with a given other outage has obvious benefits: it increases the speed of diagnosis and reduces load on other teams by acknowledging that there is indeed an incident. There are additional nonobvious benefits. To use Bigtable as an example, if a service has a disruption due to an apparent Bigtable incident, but you can see that the Bigtable SRE team has not been alerted, manually alerting the team is probably a good idea. Improved cross- team visibility can and does make a big difference in incident resolution, or at least in incident mitigation.














