## Handling Overload

Avoiding overload is a goal of load balancing policies. But no matter how efficient your load balancing policy, *eventually* some part of your system will become overloa‐ ded. Gracefully handling overload conditions is fundamental to running a reliable serving system. 

One option for handling overload is to serve degraded responses: responses that are not as accurate as or that contain less data than normal responses, but that are easier to compute. For example: 

-  Instead of searching an entire corpus to provide the best available results to a search query, search only a small percentage of the candidate set. 

-  Rely on a local copy of results that may not be fully up to date but that will be cheaper to use than going against the canonical storage. 



At the end of the day, it’s best to build clients and backends to handle resource restrictions gracefully:
redirect when possible, serve degraded results when necessary, and handle resource errors transparently when all else fails.

### The Pitfalls of “Queries per Second”

modeling capacity as “queries per second” or using static features of the requests that are believed to be a proxy for the resources they consume (e.g., “how many keys are the requests reading”) often makes for a
poor metric. 

A better solution is to measure capacity directly in available resources. For example, you may have a total of 500 CPU cores and 1 TB of memory reserved for a given ser‐ vice in a given datacenter. Naturally, it works much better to use those numbers directly to model a datacenter’s capacity. We often speak about the cost of a request to refer to a normalized measure of how much CPU time it has consumed (over differ‐ ent CPU architectures, with consideration of performance differences).

- In platforms with garbage collection, memory pressure naturally translates into increased CPU consumption.
- In other platforms, it’s possible to provision the remaining resources in such a way that they’re very unlikely to run out before CPU runs out.

### Per-Customer Limits

One component of dealing with overload is deciding what to do in the case of global overload. In a perfect world, where teams coordinate their launches carefully with the owners of their backend dependencies, global overload never happens and backend services always have enough capacity to serve their customers. Unfortunately, we don’t live in a perfect world. Here in reality, global overload occurs quite frequently.

### Client-Side Throttling

Client-side throttling addresses this problem. When a client detects that a significant portion of its recent requests have been rejected due to “out of quota” errors, it starts self-regulating and caps the amount of outgoing traffic it generates. Requests above the cap fail locally without even reaching the network.

- _requests_: The number of requests attempted by the application layer (at the client, on top of the adaptive throttling system)
- _accepts_:The number of requests accepted by the backend

Client request rejection probability:
```aidl
max(0, (requests- K*accepts)/(requests+1))
```
### Criticality

Criticality is another notion that we’ve found very useful in the context of global quo‐ tas and throttling. 

### Utilization Signals

Our implementation of task-level overload protection is based on the notion of uti‐ lization. In many cases, the utilization is just a measurement of the CPU rate (i.e., the current CPU rate divided by the total CPUs reserved for the task), but in some cases we also factor in measurements such as the portion of the memory reserved that is currently being used. As utilization approaches configured thresholds, we start reject‐ ing requests based on their criticality (higher thresholds for higher criticalities).


### Handling Overload Errors

If a large subset of backend tasks in the datacenter are overloaded, requests should not be retried and errors should bubble up all the way to the caller (e.g., returning an error to the end user). It’s much more typical that only a small portion of tasks become overloaded, in which case the preferred response is to retry the request immediately. 

### Deciding to Retry

- First, we implement a per-request retry budget of up to three attempts. 
- Secondly, we implement a per-client retry budget.
- A third approach has clients include a counter of how many times the request has already been tried in the request metadata.

Our larger services tend to be deep stacks of systems, which may in turn have depen‐ dencies on each other. In this architecture, requests should only be retried at the layer immediately above the layer that is rejecting them. When we decide that a given request can’t be served and shouldn’t be retried, we use an “overloaded; don’t retry” error and thus avoid a combinatorial retry explosion.

### Load from Connections

As mentioned previously, our RPC protocol requires inactive clients to perform peri‐ odic health checks. After a connection has been idle for a configurable amount of time, the client drops its TCP connection and switches to UDP for health checking.

Handling bursts of new connection requests is a second (but related) problem. We’ve seen bursts of this type happen in the case of very large batch jobs that create a very large number of worker client tasks all at once.

- Expose the load to the cross-datacenter load balancing algorithm
- Mandate that batch client jobs use a separate set of batch proxy backend tasks that do nothing but forward requests to the underlying backends and hand their responses back to the clients in a controlled way. Therefore, instead of “batch cli‐ ent → backend,” you have “batch client → batch proxy → backend.” In this case, when the very large job starts, only the batch proxy job suffers, shielding the actual backends (and higher-priority clients). Effectively, the batch proxy acts like a fuse. Another advantage of using the proxy is that it typically reduces the num‐ ber of connections against the backend, which can improve the load balancing against the backend (e.g., the proxy tasks can use bigger subsets and probably have a better view of the state of the backend tasks).


It’s a common mistake to assume that an overloaded backend should turn down and stop accepting all traffic. However, this assumption actually goes counter to the goal of robust load balancing. We actually want the backend to continue accepting as much traffic as possible, but to only accept that load as capacity frees up. A well- behaved backend, supported by robust load balancing policies, should accept only the requests that it can process and reject the rest gracefully.









