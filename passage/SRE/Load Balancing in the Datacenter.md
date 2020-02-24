## Load Balancing in the Datacenter

It discusses algorithms for distributing work within a given datacenter for a stream of queries

### Identifying Bad Tasks: Flow Control and Lame Ducks

#### A Simple Approach to Unhealthy Tasks: Flow Control


Assume our client tasks track the number of active requests they have sent on each connection to a backend task. When this active-request count reaches a configured limit, the client treats the backend as unhealthy and no longer sends it requests. For most backends, 100 is a reasonable limit; in the average case, requests tend to finish fast enough that it is very rare for the number of active requests from a given client to reach this limit under normal operating conditions. This (very basic!) form of flow control also serves as a simplistic form of load balancing: if a given backend task becomes overloaded and requests start piling up, clients will avoid that backend, and the workload spreads organically among the other backend tasks. 

Unfortunately, this very simplistic approach only protects backend tasks against very extreme forms of overload and it’s very easy for backends to become overloaded well before this limit is ever reached. The converse is also true: in some cases, clients may reach this limit when their backends still have plenty of spare resources. For example, some backends may have very long-lived requests that prohibit quick responses. We’ve seen cases in which this default limit has backfired, causing all backend tasks to become unreachable, with requests blocked in the clients until they time out and fail. Raising the active-request limit can avoid this situation, but doesn’t solve the underly‐ ing problem of knowing if a task is truly unhealthy or simply slow to respond. 

#### A Robust Approach to Unhealthy Tasks: Lame Duck State

From a client perspective, a given backend task can be in any of the following states:
- **Healthy**
The backend task has initialized correctly and is processing requests.
- **Refusing connections**
The backend task is unresponsive. This can happen because the task is starting up or shutting down, or because the backend is in an abnormal state (though it would be rare for a backend to stop listening on its port if it is not shutting down).
- **Lame duck**
The backend task is listening on its port and can serve, but is explicitly asking clients to stop sending requests.

1. The job scheduler sends a SIGTERM signal to the backend task.
2. The backend task enters lame duck state and asks its clients to send new requests to other backend tasks. This is done through an API call in the RPC implementa‐ tion that is explicitly called in the SIGTERM handler.
3. Any ongoing request started before the backend task entered lame duck state (or after it entered lame duck state but before a client detected it) executes normally.
4. As responses flow back to the clients, the number of active requests against the backend gradually decreases to zero.
5. After a configured interval, the backend task either exits cleanly or the job sched‐ uler kills it. The interval should be set to a large enough value that all typical requests have sufficient time to finish. This value is service dependent, but a good rule of thumb is between 10s and 150s depending on client complexity.

This strategy also allows a client to establish connections to backend tasks while per‐ forming potentially long-lived initialization procedures (and thus are not yet ready to start serving). The backend tasks could otherwise start listening for connections only when they’re ready to serve, but doing so would delay the negotiation of the connec‐ tions unnecessarily. 

### Limiting the Connections Pool with Subsetting

Each client in our RPC system maintains a pool of long-lived connections to its back‐ ends that it uses to send new requests. These connections are typically established early on as the client is starting and usually remain open, with requests flowing through them, until the client’s death. An alternative model would be to establish and tear down a connection for each request, but this model has significant resource and latency costs. In the corner case of a connection that remains idle for a long time, our RPC implementation has an optimization that switches the connection to a cheap “inactive” mode where, for example, the frequency of health checks is reduced and the underlying TCP connection is dropped in favor of UDP.


#### Picking the Right Subset


For example, you may want to use a larger subset size if:
- The number of clients is significantly smaller than the number of backends. In this case, you want the number of backends per client to be large enough that you don’t end up with backend tasks that will never receive any traffic.
- There are frequent load imbalances within the client jobs (i.e., one client task sends more requests than others). 

#### A Subset Selection Algorithm: Random Subsetting

A naive implementation of a subset selection algorithm might have each client ran‐ domly shuffle the list of backends once and fill its subset by selecting resolvable/ healthy backends from the list. Shuffling once and then picking backends from the start of the list handles restarts and failures robustly (e.g., with relatively little churn) because it explicitly limits them from consideration. However, we’ve found that this strategy actually works very poorly in most practical scenarios because it spreads load very unevenly.

#### A Subset Selection Algorithm: Deterministic Subsetting

```python
def Subset(backends, client_id, subset_size):
    subset_count = len(backends) / subset_size
    
    # Group clients into rounds; each round uses the same shuffled list: round = client_id / subset_count
    random.seed(round)
    random.shuffle(backends)
    
    # The subset id corresponding to the current client: subset_id = client_id % subset_count
    start = subset_id * subset_size
    return backends[start:start + subset_size]
```

### Load Balancing Policies

#### Simple Round Robin

One very simple approach to load balancing has each client send requests in round- robin fashion to each backend task in its subset to which it can successfully connect and which isn’t in lame duck state. For many years, this was our most common approach, and it’s still used by many services.

Such a spread is extremely wasteful and occurs for a number of reasons, including:
- Small subsetting
- Varying query costs
- Machine diversity
- Unpredictable performance factors: Antagonistic neighbors, Task restarts

#### Least-Loaded Round Robin

An alternative approach to Simple Round Robin is to have each client task keep track of the number of active requests it has to each backend task in its subset and use Round Robin among the set of tasks with a minimal number of active requests.

Least-Loaded Round Robin has two important limitations:

- The count of active requests may not be a very good proxy for the capability of a given backend
- The count of active requests in each client doesn’t include requests from other clients to the same backends

#### Weighted Round Robin

Weighted Round Robin is an important load balancing policy that improves on Sim‐ ple and Least-Loaded Round Robin by incorporating backend-provided information into the decision process.

Weighted Round Robin is fairly simple in principle: each client task keeps a “capabil‐ ity” score for each backend in its subset. Requests are distributed in Round-Robin fashion, but clients weigh the distributions of requests to backends proportionally. In each response (including responses to health checks), backends include the current observed rates of queries and errors per second, in addition to the utilization (typi‐ cally, CPU usage). Clients adjust the capability scores periodically to pick backend tasks based upon their current number of successful requests handled and at what utilization cost; failed requests result in a penalty that affects future decisions.

















