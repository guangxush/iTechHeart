## Addressing Cascading Failures

If at first you don’t succeed, back off exponentially.

Why do people always forget that you need to add a little jitter?

A cascading failure is a failure that grows over time as a result of positive feedback.1 It can occur when a portion of an overall system fails, increasing the probability that other portions of the system fail. For example, a single replica for a service can fail due to overload, increasing load on remaining replicas and increasing their probabil‐ ity of failing, causing a domino effect that takes down all the replicas for a service.

###  Causes of Cascading Failures and Designing to Avoid Them

Local overload in one cluster may lead to its servers crashing; in response, the load balancing controller sends requests to other clusters, overloading their servers, leading to a service-wide over‐ load failure. It may not take long for these events to transpire (e.g., on the order of a couple minutes), because the load balancer and task scheduling systems involved may act very quickly.

#### Resource Exhaustion

Running out of a resource can result in higher latency, elevated error rates, or the substitution of lower-quality results. These are in fact desired effects of running out of resources: something eventually needs to give as the load increases beyond what a server can handle.

**CPU**

If there is insufficient CPU to handle the request load, typically all requests become slower. This scenario can result in various secondary effects, including the following:

- Increased number of in-flight requests
- Excessively long queue lengths
- Thread starvation
- CPU or request starvation
- Missed RPC deadlines
- Reduced CPU caching benefits

**Memory**

If nothing else, more in-flight requests consume more RAM from allocating the request, response, and RPC objects. Memory exhaustion can cause the following effects:
- Dying tasks
- Increased rate of garbage collection (GC) in Java, resulting in increased CPU usage
- Reduction in cache hit rates

**Threads**

Thread starvation can directly cause errors or lead to health check failures. If the server adds threads as needed, thread overhead can use too much RAM. In extreme cases, thread starvation can also cause you to run out of process IDs.

**File descriptors**

Running out of file descriptors can lead to the inability to initialize network connec‐ tions, which in turn can cause health checks to fail.

**Dependencies among resources**

1. A Java frontend has poorly tuned garbage collection (GC) parameters.
2. Under high (but expected) load, the frontend runs out of CPU due to GC.
3. CPU exhaustion slows down completion of requests.
4. The increased number of in-progress requests causes more RAM to be used to process the requests.
5. Memory pressure due to requests, in combination with a fixed memory allocation for the frontend process as a whole, leaves less RAM available for caching.
6. The reduced cache size means fewer entries in the cache, in addition to a lower hit rate.
7. The increase in cache misses means that more requests fall through to the back‐ end for servicing.
8. The backend, in turn, runs out of CPU or threads.
9. Finally, the lack of CPU causes basic health checks to fail, starting a cascading failure.

### Service Unavailability

Resource exhaustion can lead to servers crashing; for example, servers might crash when too much RAM is allocated to a container. Once a couple of servers crash on overload, the load on the remaining servers can increase, causing them to crash as well. The problem tends to snowball and soon all servers begin to crash-loop. It’s often difficult to escape this scenario because as soon as servers come back online they’re bombarded with an extremely high rate of requests and fail almost immediately.

### Preventing Server Overload

The following list presents strategies for avoiding server overload in rough priority order:
- Load test the server’s capacity limits, and test the failure mode for overload
- Serve degraded results
- Instrument the server to reject requests when overloaded
- Instrument higher-level systems to reject requests, rather than overloading servers

### Queue Management

Most thread-per-request servers use a queue in front of a thread pool to handle requests. Requests come in, they sit on a queue, and then threads pick requests off the queue and perform the actual work (whatever actions are required by the server). Usually, if the queue is full, the server will reject new requests.

For a system with fairly steady traffic over time, it is usually better to have small queue lengths relative to the thread pool size (e.g., 50% or less), which results in the server rejecting requests early when it can’t sustain the rate of incoming requests.

### Load Shedding and Graceful Degradation

Load shedding drops some proportion of load by dropping traffic as the server approaches overload conditions. The goal is to keep the server from running out of RAM, failing health checks, serving with extremely high latency, or any of the other symptoms associated with overload, while still doing as much useful work as it can.

One effective approach is to return an HTTP 503 (service unavailable) to any incoming request when there are more than a given number of client requests in flight.

Changing the queuing method from the standard first-in, first-out (FIFO) to last-in, first-out (LIFO) or using the controlled delay (CoDel) algorithm or similar approaches can reduce load by removing requests that are unlikely to be worth processing. If a user’s web search is slow because an RPC has been queued for 10 seconds, there’s a good chance the user has given up and refreshed their browser, issuing another request: there’s no point in responding to the first one, since it will be ignored! 

Graceful degradation takes the concept of load shedding one step further by reducing the amount of work that needs to be performed. In some applications, it’s possible to significantly decrease the amount of work or time needed by decreasing the quality of responses. For instance, a search application might only search a subset of data stored in an in-memory cache rather than the full on-disk database or use a less-accurate (but faster) ranking algorithm when overloaded.

When evaluating load shedding or graceful degradation options for your service, consider the following:

- Which metrics should you use to determine when load shedding or graceful deg‐ radation should kick in (e.g,. CPU usage, latency, queue length, number of threads used, whether your service enters degraded mode automatically or if manual intervention is necessary)?
- What actions should be taken when the server is in degraded mode?
- At what layer should load shedding and graceful degradation be implemented? Does it make sense to implement these strategies at every layer in the stack, or is it sufficient to have a high-level choke-point?

As you evaluate options and deploy, keep the following in mind:
- Graceful degradation shouldn’t trigger very often
- You can make sure that graceful degradation stays working by regularly running a small subset of servers near over‐ load in order to exercise this code path.
- Monitor and alert when too many servers enter these modes.
- Complex load shedding and graceful degradation can cause problems themselves —excessive complexity may cause the server to trip into a degraded mode when it is not desired, or enter feedback cycles at undesired times. 

### Retries

```Go
func exampleRpcCall(client pb.ExampleClient, request pb.Request) *pb.Response {
    // Set RPC timeout to 5 seconds.
    opts := grpc.WithTimeout(5 * time.Second)
    // Try up to 20 times to make the RPC call. 
    attempts := 20
    for attempts > 0 {
        conn, err := grpc.Dial(*serverAddr, opts...) 
        if err != nil {
        // Something went wrong in setting up the connection. Try again. 
        attempts--
        continue
    }
    defer conn.Close()
    // Create a client stub and make the RPC call.
    client := pb.NewBackendClient(conn)
    response, err := client.MakeRequest(context.Background, request) 
    if err != nil {
        // Something went wrong in making the call. Try again. 
        attempts--
        continue
    }
    return response
    }
    grpclog.Fatalf("ran out of attempts") 
}
```

Two factors are at play here:
- If the backend spends a significant amount of resources processing requests that will ultimately fail due to overload, then the retries themselves may be keeping the backend in an overloaded mode.
- The backend servers themselves may not be stable.

When issuing automatic retries, keep in mind the following considerations:
- In particular, testing the system can highlight problems, and graceful degradation can reduce the effect of the retries on the backend.
- Always use randomized exponential backoff when scheduling retries.
- Limit retries per request. Don’t retry a given request indefinitely.
- Consider having a server-wide retry budget. 
- Think about the service holistically and decide if you really need to perform retries at a given level.
- Use clear response codes and consider how different failure modes should be handled. 

### Latency and Deadlines

When a frontend sends an RPC to a backend server, the frontend consumes resources waiting for a reply. RPC deadlines define how long a request can wait before the frontend gives up, limiting the time that the backend may consume the frontend’s resources.

#### Picking a deadline

High deadlines can result in resource consumption in higher levels of the stack when lower levels of the stack are having problems. Short deadlines can cause some more expensive requests to fail consistently. Balancing these constraints to pick a good deadline can be something of an art.

#### Missing deadlines

If handling a request is performed over multiple stages (e.g., there are a few callbacks and RPC calls), the server should check the deadline left at each stage before attempt‐ ing to perform any more work on the request.

#### Deadline propagation

Rather than inventing a deadline when sending RPCs to backends, servers should employ deadline propagation and cancellation propagation.

#### Bimodal latency

- Detecting this problem can be very hard. In particular, it may not be clear that bimodal latency is the cause of an outage when you are looking at mean latency. When you see a latency increase, try to look at the distribution of latencies in addition to the averages.

- This problem can be avoided if the requests that don’t complete return with an error early, rather than waiting the full deadline. For example, if a backend is unavailable, it’s usually best to immediately return an error for that backend, rather than consuming resources until it the backend available. If your RPC layer supports a fail-fast option, use it.

- Having deadlines several orders of magnitude longer than the mean request latency is usually bad. In the preceding example, a small number of requests ini‐ tially hit the deadline, but the deadline was three orders of magnitude larger than the normal mean latency, leading to thread exhaustion.

- When using shared resources that can be exhausted by some keyspace, consider either limiting in-flight requests by that keyspace or using other kinds of abuse tracking. Suppose your backend processes requests for different clients that have wildly different performance and request characteristics. You might consider only allowing 25% of your threads to be occupied by any one client in order to provide fairness in the face of heavy load by any single client misbehaving.

### Slow Startup and Cold Caching

Processes are often slower at responding to requests immediately after starting than they will be in steady state. This slowness can be caused by either or both of the fol‐ lowing:

- Required initialization: Setting up connections upon receiving the first request that needs a given backend

- Runtime performance improvements in some languages, particularly Java: Just-In-Time compilation, hotspot optimization, and deferred class loading

If the service is not provisioned to handle requests under a cold cache, it’s at greater risk of outages and should take steps to avoid them.
- Turning up a new cluster
- Returning a cluster to service after maintenance
- Restarts

If caching has a significant effect on the service, you may want to use one or some of the following strategies:
- Overprovision the service. It’s important to note the distinction between a latency cache versus a capacity cache: when a latency cache is employed, the service can sustain its expected load with an empty cache, but a service using a capacity cache cannot sustain its expected load under an empty cache. Service owners should be vigilant about adding caches to their service, and make sure that any new caches are either latency caches or are sufficiently well engineered to safely function as capacity caches. Sometimes caches are added to a service to improve performance, but actually wind up being hard dependencies.
- Employ general cascading failure prevention techniques. In particular, servers should reject requests when they’re overloaded or enter degraded modes, and testing should be performed to see how the service behaves after events such as a large restart.
- When adding load to a cluster, slowly increase the load. The initially small request rate warms up the cache; once the cache is warm, more traffic can be added. It’s a good idea to ensure that all clusters carry nominal load and that the caches are kept warm.

### Always Go Downward in the Stack

However, suppose the backends cross-communicate amongst each other. For exam‐ ple, the backends might proxy requests to one another to change who owns a user when the storage layer can’t service a request. This intra-layer communication can be problematic for several reasons:
- The communication is susceptible to a distributed deadlock.
- If intra-layer communication increases in response to some kind of failure or heavy load condition (e.g., load rebalancing that is more active under high load), intra-layer communication can quickly switch from a low to high intra-layer request mode when the load increases enough.
- Depending on the criticality of the cross-layer communication, bootstrapping the system may become more complex.


### Triggering Conditions for Cascading Failures

#### Process Death
Some server tasks may die, reducing the amount of available capacity. Tasks might die because of a Query of Death (an RPC whose contents trigger a failure in the process), cluster issues, assertion failures, or a number of other reasons. 

#### Process Updates
 
To prevent this scenario, either account for necessary capacity overhead when setting up the service’s update infrastructure, or push off-peak. 

#### New Rollouts

During a cascading failure, it’s usually wise to check for recent changes and consider reverting them, particularly if those changes affected capacity or altered the request profile.

#### Organic Growth

In many cases, a cascading failure isn’t triggered by a specific service change, but because a growth in usage wasn’t accompanied by an adjustment to capacity.

#### Planned Changes, Drains, or Turndowns

If your service is multihomed, some of your capacity may be unavailable because of maintenance or outages in a cluster. Similarly, one of the service’s critical dependen‐ cies may be drained, resulting in a reduction in capacity for the upstream service due to drain dependencies, or an increase in latency due to having to send the requests to a more distant cluster.
- Request profile changes
- Resource limits

###  Testing for Cascading Failures

#### Test Until Failure and Beyond
Load testing also reveals where the breaking point is, knowledge that’s fundamental to the capacity planning process. It enables you to test for regressions, provision for worst-case thresholds, and to trade off utilization versus safety margins.

You should also test and understand how the component behaves as it returns to nominal load after having been pushed well beyond that load. Such testing may answer questions such as:
- If a component enters a degraded mode on heavy load, is it capable of exiting the degraded mode without human intervention?
- If a couple of servers crash under heavy load, how much does the load need to drop in order for the system to stabilize?


You might consider some of the following production tests:
- Reducing task counts quickly or slowly over time, beyond expected traffic patterns
- Rapidly losing a cluster’s worth of capacity
- Blackholing various backends


#### Test Popular Clients

Understand how large clients use your service. For example, you want to know if clients:
- Can queue work while the service is down
- Use randomized exponential backoff on errors
- Are vulnerable to external triggers that can create large amounts of load (e.g., an externally triggered software update might clear an offline client’s cache)

#### Test Noncritical Backends

Test your noncritical backends, and make sure their unavailability does not interfere with the critical components of your service.

### Immediate Steps to Address Cascading Failures

**Increase Resources**


**Stop Health Check Failures/Deaths**

If servers are somehow wedged and not making progress, restarting them may help. Try restarting servers when:
- Java servers are in a GC death spiral
- Some in-flight requests have no deadlines but are consuming resources, leading them to block threads, for example
- The servers are deadlocked

**Drop Traffic**

Dropping load is a big hammer, usually reserved for situations in which you have a true cascading failure on your hands and you cannot fix it by other means. For exam‐ ple, if heavy load causes most servers to crash as soon as they become healthy, you can get the service up and running again by:
1. Addressing the initial triggering condition (by adding capacity, for example).
2. Reducing load enough so that the crashing stops. Consider being aggressive here —if the entire service is crash-looping, only allow, say, 1% of the traffic through.
3. Allowing the majority of the servers to become healthy.
4. Gradually ramping up the load.

**Enter Degraded Modes**

Serve degraded results by doing less work or dropping unimportant traffic. 

**Eliminate Batch Load**

For example, if index updates, data copies, or statistics gathering con‐ sume resources of the serving path, consider turning off those sources of load during an outage.

**Eliminate Bad Traffic**

If some queries are creating heavy load or crashes (e.g., queries of death), consider blocking them or eliminating them via other means.

### Closing Remarks

Without proper care, some system changes meant to reduce background errors or otherwise improve the steady state can expose the service to greater risk of a full out‐ age. Retrying on failures, shifting load around from unhealthy servers, killing unheal‐ thy servers, adding caches to improve performance or reduce latency: all of these might be implemented to improve the normal case, but can improve the chance of causing a large-scale failure. Be careful when evaluating changes to ensure that one outage is not being traded for another.





