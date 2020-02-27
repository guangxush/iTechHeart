## Reliable Product Launches at Scale

Launching a new product or feature is the moment of truth for every company—the point at which months or years of effort are presented to the world. Traditional com‐ panies launch new products at a fairly low rate. The launch cycle at Internet compa‐ nies is markedly different. Launches and rapid iterations are far easier because new features can be rolled out on the server side, rather than requiring software rollout on individual customer workstations.


### Launch Coordination Engineering

Google approached the challenges inherent to launches by creating a dedicated con‐ sulting team within SRE tasked with the technical side of launching a new product or feature. Staffed by software engineers and systems engineers—some with experience in other SRE teams—this team specializes in guiding developers toward building reli‐ able and fast products that meet Google’s standards for robustness, scalability, and reliability. This consulting team, Launch Coordination Engineering (LCE), facilitates a smooth launch process in a few ways:

- Auditing products and services for compliance with Google’s reliability standards and best practices, and providing specific actions to improve reliability
- Acting as a liaison between the multiple teams involved in a launch
- Driving the technical aspects of a launch by making sure that tasks maintain momentum
- Acting as gatekeepers and signing off on launches determined to be “safe”
- Educating developers on best practices and on how to integrate with Google’s services, equipping them with internal documentation and training resources to speed up their learning

#### The Role of the Launch Coordination Engineer

A team dedicated to coordinating launches offers the following advantages:

- Breadth of experience: As a true cross-product team, the members are active across almost all of Goo‐ gle’s product areas. Extensive cross-product knowledge and relationships with many teams across the company make LCEs excellent vehicles for knowledge transfer.
- Cross-functional perspective: LCEs have a holistic view of the launch, which enables them to coordinate among disparate teams in SRE, development, and product management. This holistic approach is particularly important for complicated launches that can span more than half a dozen teams in multiple time zones.
- Objectivity: As a nonpartisan advisor, an LCE plays a balancing and mediating role between stakeholders including SRE, product developers, product managers, and market‐ ing.

### Setting Up a Launch Process

Google has honed its launch process over a period of more than 10 years. Over time we have identified a number of criteria that characterize a good launch process:
- _Lightweight_:Easy on developers
- _Robust_:Catches obvious errors
- _Thorough_:Addresses important details consistently and reproducibly
- _Scalable_:Accommodates both a large number of simple launches and fewer complex launches
- _Adaptable_:Works well for common types of launches (for example, adding a new UI lan‐ guage to a product) and new types of launches (for example, the initial launch of the Chrome browser or Google Fiber)

Google has successfully employed a few tactics to help us achieve these criteria::

- _Simplicity_:Get the basics right. Don’t plan for every eventuality.
- _A high touch approach_:Experienced engineers customize the process to suit each launch.
- _Fast common paths_:Identify classes of launches that always follow a common pattern (such as launch‐ ing a product in a new country), and provide a simplified launch process for this class.


#### The Launch Checklist

- Question: Do you need a new domain name?: Action item: Coordinate with marketing on your desired domain name, and
request registration of the domain. Here is a link to the marketing form.
- Question: Are you storing persistent data?: Action item: Make sure you implement backups. Here are instructions for implementing backups.
- Question: Could a user potentially abuse your service?: Action item: Implement rate limiting and quotas. Use the following shared
service.

LCE now uses the following guidelines:
- Every question’s importance must be substantiated, ideally by a previous launch disaster.
- Every instruction must be concrete, practical, and reasonable for developers to accomplish.


#### Driving Convergence and Simplification

In a large organization, engineers may not be aware of available infrastructure for common tasks (such as rate limiting). Lacking proper guidance, they’re likely to re- implement existing solutions. Converging on a set of common infrastructure libraries avoids this scenario, and provides obvious benefits to the company: it cuts down on duplicate effort, makes knowledge more easily transferable between services, and results in a higher level of engineering and service quality due to the concentrated attention given to infrastructure.

#### Launching the Unexpected

When a project enters into a new product space or vertical, an LCE may need to cre‐ ate an appropriate checklist from scratch. Doing so often involves synthesizing expe‐ rience from relevant domain experts. When drafting a new checklist, it can be helpful to structure the checklist around broad themes such as reliability, failure modes, and processes.

### Developing a Launch Checklist

#### Architecture and Dependencies

Example checklist questions:

- What is your request flow from user to frontend to backend?
- Are there different types of requests with different latency requirements?

Example action items:

- Isolate user-facing requests from non user–facing requests.
- Validate request volume assumptions. One page view can turn into many requests.

#### Integration

Example action items:

- Set up a new DNS name for your service.
- Set up load balancers to talk to your service.
- Set up monitoring for your new service.

#### Capacity Planning

Example checklist questions

- Is this launch tied to a press release, advertisement, blog post, or other form of promotion?
- How much traffic and rate of growth do you expect during and after the launch?
- Have you obtained all the compute resources needed to support your traffic?

#### Failure Modes

Example checklist questions
- Do you have any single points of failure in your design?
- How do you mitigate unavailability of your dependencies?

Example action items
- Implement request deadlines to avoid running out of resources for long-running requests.
- Implement load shedding to reject new requests early in overload situations.

#### Client Behavior

This axiom no longer holds when we consider clients that initiate actions without user input—for example, a cell phone app that periodically syncs its data into the
cloud, or a website that periodically refreshes. In either of these scenarios, abusive cli‐ ent behavior can very easily threaten the stability of a service. (There is also the topic of protecting a service from abusive traffic such as scrapers and denial-of-service attacks—which is different from designing safe behavior for first-party clients.)

Example checklist question
- Do you have auto-save/auto-complete/heartbeat functionality?

Example action items
- Make sure that your client backs off exponentially on failure.
- Make sure that you jitter automatic requests.

#### Processes and Automation

These remaining processes should be documented before launch to ensure that the information is translated from an engineer’s mind onto paper while it is still fresh, and that it is available in an emergency. Processes should be documented in such a way that any team member can execute a given process in an emergency.

Example checklist question
- Are there any manual processes required to keep the service running?

Example action items
- Document all manual processes.
- Document the process for moving your service to a new datacenter.
- Automate the process for building and releasing a new version.

#### Development Process

Example action items
- Check all code and configuration files into the version control system.
- Cut each release on a new release branch.

#### External Dependencies

Example checklist questions
- What third-party code, data, services, or events does the service or the launch depend upon?
- Do any partners depend on your service? If so, do they need to be notified of your launch?
- What happens if you or the vendor can’t meet a hard launch deadline?

#### Rollout Planning

Example action items
- Set up a launch plan that identifies actions to take to launch the service. Identify who is responsible for each item.
- Identify risk in the individual launch steps and implement contingency measures.

### Selected Techniques for Reliable Launches

#### Gradual and Staged Rollouts

One adage of system administration is “never change a running system.” Any change represents risk, and risk should be minimized in order to assure reliability of a sys‐ tem.

Almost all updates to Google’s services proceed gradually, according to a defined pro‐ cess, with appropriate verification steps interspersed. A new server might be installed on a few machines in one datacenter and observed for a defined period of time. If all looks well, the server is installed on all machines in one datacenter, observed again, and then installed on all machines globally. The first stages of a rollout are usually
called “canaries”—an allusion to canaries carried by miners into a coal mine to detect dangerous gases. Our canary servers detect dangerous effects from the behavior of the new software under real user traffic.

Canary testing is a concept embedded into many of Google’s internal tools used to make automated changes, as well as for systems that change configuration files. Tools that manage the installation of new software typically observe the newly started server for a while, making sure that the server doesn’t crash or otherwise misbehave. If the change doesn’t pass the validation period, it’s automatically rolled back.

#### Feature Flag Frameworks

Whenever a product introduced any such frame‐ work, the framework itself was hardened as much as possible so that most of its appli‐
cations would not need any LCE involvement. Such frameworks usually meet the following requirements:
- Roll out many changes in parallel, each to a few servers, users, entities, or datacenters
- Gradually increase to a larger but limited group of users, usually between 1 and 10 percent
- Direct traffic through different servers depending on users, sessions, objects, and/or locations
- Automatically handle failure of the new code paths by design, without affecting users
- Independently revert each such change immediately in the event of serious bugs or side effects
- Measure the extent to which each change improves the user experience

Google’s feature flag frameworks fall into two general classes:
- Those that primarily facilitate user interface improvements
- Those that support arbitrary server-side and business logic changes

#### Dealing with Abusive Client Behavior

The ability to control the behavior of a client from the server side has proven an important tool in the past. For an app on a device, such control might mean instruct‐ ing the client to check in periodically with the server and download a configuration file. The file might enable or disable certain features or set parameters, such as how often the client syncs or how often it retries.

#### Overload Behavior and Load Tests

Overload situations are a particularly complex failure mode, and therefore deserve additional attention. Runaway success is usually the most welcome cause of overload
when a new service launches, but there are myriad other causes, including load balancing failures, machine outages, synchronized client behavior, and external attacks.

At some point, many services reach a point of nonlinearity as they approach overload. In the most benign cases, response times simply begin to increase, resulting in a degraded user experience but not necessarily causing an outage (although a slow dependency might cause user-visible errors up the stack, due to exceeded RPC dead‐ lines). In the most drastic cases, a service locks up completely in response to overload.

Unfortunately, it is very hard to predict from first principles how a service will react to overload. Therefore, load tests are an invaluable tool, both for reliability reasons and capacity planning, and load testing is required for most launches.

### Development of LCE

To mitigate the repetition of such mistakes by capturing the lessons learned from past launches, a small band of experienced engineers, called the “Launch Engineers,” vol‐
unteered to act as a consulting team. The Launch Engineers developed checklists for new product launches, covering topics such as:

- When to consult with the legal department
- How to select domain names
- How to register new domains without misconfiguring DNS
- Common engineering design and production deployment pitfalls

#### Evolution of the LCE Checklist

As the volume of launches grew, keeping pace with the annual doubling of Google’s engineering team, LCEs sought ways to streamline their reviews. LCEs identified cat‐
egories of low-risk launches that were highly unlikely to face or cause mishaps. For example, a feature launch involving no new server executables and a traffic increase under 10% would be deemed low risk. Such launches were faced with an almost trivial checklist, while higher-risk launches underwent the full gamut of checks and balances. By 2008, 30% of reviews were considered low-risk.


#### Problems LCE Didn’t Solve

- Scalability changes
- Growing operational load
- Infrastructure churn

### Conclusion

Companies undergoing rapid growth with a high rate of change to products and serv‐ ices may benefit from the equivalent of a Launch Coordination Engineering role. Such a team is especially valuable if a company plans to double its product developers every one or two years, if it must scale its services to hundreds of millions of users, and if reliability despite a high rate of change is important to its users.




















