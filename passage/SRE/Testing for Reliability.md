## Testing for Reliability

One key responsibility of Site Reliability Engineers is to quantify confidence in the systems they maintain. SREs perform this task by adapting classical software testing techniques to systems at scale.1 Confidence can be measured both by past reliability and future reliability. The former is captured by analyzing data provided by monitor‐ ing historic system behavior, while the latter is quantified by making predictions from data about past system behavior. In order for these predictions to be strong enough to be useful, one of the following conditions must hold:
- The site remains completely unchanged over time with no software releases or changes in the server fleet, which means that future behavior will be similar to past behavior.
- You can confidently describe all changes to the site, in order for analysis to allow for the uncertainty incurred by each of these changes.

Testing is the mechanism you use to demonstrate specific areas of equivalence when changes occur.

The amount of testing you need to conduct depends on the reliability requirements for your system. As the percentage of your codebase covered by tests increases, you reduce uncertainty and the potential decrease in reliability from each change. 

### Types of Software Testing

Software tests broadly fall into two categories: traditional and production. Traditional tests are more common in software development to evaluate the correctness of soft‐ ware offline, during development. Production tests are performed on a live web ser‐ vice to evaluate whether a deployed software system is working correctly.

#### Traditional Tests

[](../../image/SRE/test.png)


**Unit tests**

A unit test is the smallest and simplest form of software testing. These tests are employed to assess a separable unit of software, such as a class or function, for cor‐ rectness independent of the larger software system that contains the unit. Unit tests are also employed as a form of specification to ensure that a function or module
exactly performs the behavior required by the system. Unit tests are commonly used to introduce test-driven development concepts.

**Integration tests**

Software components that pass individual unit tests are assembled into larger compo‐ nents. Engineers then run an integration test on an assembled component to verify that it functions correctly. Dependency injection, which is performed with tools such as Dagger,3 is an extremely powerful technique for creating mocks of complex depen‐ dencies so that an engineer can cleanly test a component. A common example of a dependency injection is to replace a stateful database with a lightweight mock that has precisely specified behavior.

**System tests**

A system test is the largest scale test that engineers run for an undeployed system. All modules belonging to a specific component, such as a server that passed integration tests, are assembled into the system. Then the engineer tests the end-to-end function‐ ality of the system. System tests come in many different flavors:


_Smoke tests_

Smoke tests, in which engineers test very simple but critical behavior, are among the simplest type of system tests. 

_Performance tests_

Because response times for dependencies or resource requirements may change dramatically during the course of development, a system needs to be tested to make sure that it doesn’t become incrementally slower without anyone noticing (before it gets released to users). 

_Regression tests_

Regression tests can be analogized to a gallery of rogue bugs that his‐ torically caused the system to fail or produce incorrect results. By documenting these bugs as tests at the system or integration level, engineers refactoring the
codebase can be sure that they don’t accidentally introduce bugs that they’ve already invested time and effort to eliminate.

#### Production Tests

Production tests interact with a live production system, as opposed to a system in a hermetic testing environment. These tests are in many ways similar to black-box monitoring (see Chapter 6), and are therefore sometimes called black-box testing.

_Configuration test_

Configuration tests are built and tested for a specific version of the checked-in con‐ figuration file. Comparing which version of the test is passing in relation to the goal version for automation implicitly indicates how far actual production currently lags behind ongoing engineering work.

_Stress test_

In order to safely operate a system, SREs need to understand the limits of both the system and its components. In many cases, individual components don’t gracefully degrade beyond a certain point—instead, they catastrophically fail. Engineers use stress tests to find the limits on a web service. Stress tests answer questions such as:
- How full can a database get before writes start to fail?
- How many queries a second can be sent to an application server before it becomes overloaded, causing requests to fail?

_Canary test_

To conduct a canary test, a subset of servers is upgraded to a new version or configu‐ ration and then left in an incubation period. Should no unexpected variances occur, the release continues and the rest of the servers are upgraded in a progressive fashion.4 Should anything go awry, the single modified server can be quickly reverted to a known good state. We commonly refer to the incubation period for the upgraded server as “baking the binary.”

A canary test isn’t really a test; rather, it’s structured user acceptance. Whereas config‐ uration and stress tests confirm the existence of a specific condition over determinis‐ tic software, a canary test is more ad hoc. It only exposes the code under test to less predictable live production traffic, and thus, it isn’t perfect and doesn’t always catch newly introduced faults.


### Creating a Test and Build Environment

You can start your approach by asking the following questions:
- Can you prioritize the codebase in any way? 
- Are there particular functions or classes that are absolutely mission-critical or business-critical?
- Which APIs are other teams integrating against?

Another key task for creating well-tested software is to set up a testing infrastructure.

Once source control is in place, you can add a continuous build system that builds the software and runs tests every time code is submitted. 

The concepts of stability and agility are traditionally in tension in the world of SRE.

There are a variety of tools to help you quantify and visualize the level of test cover‐ age you need.

### Testing at Scale

#### Testing Scalable Tools

As pieces of software, SRE tools also need testing. SRE-developed tools might per‐ form tasks such as the following:
- Retrieving and propagating database performance metrics
- Predicting usage metrics to plan for capacity risks
- Refactoring data within a service replica that isn’t user accessible
- Changing files on a server

SRE tools share two characteristics:
- Their side effects remain within the tested mainstream API
- They’re isolated from user-facing production by an existing validation and release barrier

Automation tools are also software. Because their risk footprint appears out-of-band for a different layer of the service, their testing needs are more subtle. Automation tools perform tasks like the following:
- Database index selection
- Load balancing between datacenters
- Shuffling relay logs for fast remastering

Automation tools share two characteristics:
- The actual operation performed is against a robust, predictable, and well-tested API
- The purpose of the operation is the side effect that is an invisible discontinuity to another API client


#### Testing Disaster

Many disaster recovery tools can be carefully designed to operate offline. Such tools do the following:
- Compute a checkpoint state that is equivalent to cleanly stopping the service
- Push the checkpoint state to be loadable by existing nondisaster validation tools
- Support the usual release barrier tools, which trigger the clean start procedure

#### The Need for Speed

For every version (patch) in the code repository, every defined test provides a pass or fail indication. That indication may change for repeated and seemingly identical runs. You can estimate the actual likelihood of a test passing or failing by averaging over those many runs and computing the statistical uncertainty of that likelihood. How‐ ever, performing this calculation for every test at every version point is computationally infeasible.

#### Pushing to Production

While production configuration management is commonly kept in a source control repository, configuration is often separate from the developer source code. Similarly, the software testing infrastructure often can’t see production configuration. Even if the two are located in the same repository, changes for configuration management are made in branches and/or a segregated directory tree that test automation has his‐ torically ignored.

#### Expect Testing Fail

Configuration files generally exist because changing the configuration is faster than rebuilding a tool. 

- A configuration file that exists to keep MTTR low, and is only modified when there’s a failure, has a release cadence slower than the MTBF. 
- A configuration file that changes more than once per user-facing application release (for example, because it holds release state) can be a major risk if these changes are not treated the same as application releases.

Make sure of the following:
- Each configuration file has enough test coverage to support regular routine editing.
- Before releases, file edits are somewhat delayed while waiting for release testing.
- Provide a break-glass mechanism to push the file live before completing the test‐ ing. Since breaking the glass impairs reliability, it’s generally a good idea to make the break noisy by (for example) filing a bug requesting a more robust resolution for next time.

#### Integration

Interpreted languages such as Python are commonly used for configuration files because their interpreters can be embedded, and some simple sandboxing is available to protect against nonmalicious coding errors.

Writing your configuration files in an interpreted language is risky, as this approach is fraught with latent failures that are hard to definitively address.


The role of SRE generally includes writing systems engineering tools17 (if no one else is already writing them) and adding robust validation with test coverage. All tools can behave unexpectedly due to bugs not caught by testing, so defense in depth is advisa‐ ble. When one tool behaves unexpectedly, engineers need to be as confident as possi‐ ble that most of their other tools are working correctly and can therefore mitigate or resolve the side effects of that misbehavior. A key element of delivering site reliability is finding each anticipated form of misbehavior and making sure that some test (or another tool’s tested input validator) reports that misbehavior. The tool that finds the problem might not be able to fix or even stop it, but should at least report the prob‐ lem before a catastrophic outage occurs.


#### Production Probes

This production test by probe does indeed offer protection to the site, plus clear feed‐ back to the engineers. The earlier that feedback is given to engineers, the more useful it is. It’s also preferable that the test is automated so that the delivery of warnings to engineers is scalable.






