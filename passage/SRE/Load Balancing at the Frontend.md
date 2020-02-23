## Load Balancing at the Frontend

How we balance user traffic between datacenters. The following chapter zooms in to explore how we implement load balancing inside a datacenter.


### Power Isn’t the Answer

Traffic load balancing is how we decide which of the many, many machines in our datacenters will serve a particular request. Ideally, traffic is distributed across multiple network links, datacenters, and machines in an “optimal” fashion. But what does “optimal” mean in this context? There’s actually no single answer, because the optimal solution depends heavily on a variety of factors:

- The hierarchical level at which we evaluate the problem (global versus local)
- The technical level at which we evaluate the problem (hardware versus software)
- The nature of the traffic we’re dealing with



### Load Balancing Using DNS


Before a client can even send an HTTP request, it often has to look up an IP address using DNS. This provides the perfect opportunity to introduce our first layer of load balancing: DNS load balancing. The simplest solution is to return multiple A or AAAA records in the DNS reply and let the client pick an IP address arbitrarily. While con‐ ceptually simple and trivial to implement, this solution poses multiple challenges.

We can mitigate this scenario by using an anycast address for authoritative nameservers and leverage the fact that DNS queries will flow to the closest address. In its reply, the server can return addresses routed to the closest data‐ center. A further improvement builds a map of all networks and their approximate physical locations, and serves DNS replies based on that mapping. However, this sol‐ ution comes at the cost of having a much more complex DNS server implementation and maintaining a pipeline that will keep the location mapping up to date.

End users rarely talk to authoritative nameservers directly. Instead, a recursive DNS server usually lies somewhere between end users and nameservers. This server proxies queries between a user and a server and often provides a caching layer. The DNS middleman has three very important implications on traffic management:
- Recursive resolution of IP addresses
- Nondeterministic reply paths
- Additional caching complications

Recursive resolution of IP addresses is problematic, as the IP address seen by the authoritative nameserver does not belong to a user; instead, it’s the recursive resolv‐ er’s. This is a serious limitation, because it only allows reply optimization for the shortest distance between resolver and the nameserver. A possible solution is to use the EDNS0 extension proposed in [Con15], which includes information about the client’s subnet in the DNS query sent by a recursive resolver. This way, an authorita‐ tive nameserver returns a response that is optimal from the user’s perspective, rather than the resolver’s perspective. While this is not yet the official standard, its obvious advantages have led the biggest DNS resolvers (such as OpenDNS and Google1) to support it already.

Finally, recursive resolvers typically cache responses and forward those responses within limits indicated by the time-to-live (TTL) field in the DNS record. The end result is that estimating the impact of a given reply is difficult: a single authoritative reply may reach a single user or multiple thousands of users. We solve this problem in two ways:
- We analyze traffic changes and continuously update our list of known DNS resolvers with the approximate size of the user base behind a given resolver, which allows us to track the potential impact of any given resolver.
- We estimate the geographical distribution of the users behind each tracked resolver to increase the chance that we direct those users to the best location.

Given that authoritative nameservers cannot flush resolvers’ caches, DNS records need a relatively low TTL. This effectively sets a lower bound on how quickly DNS changes can be propa‐ gated to users.2 Unfortunately, there is little we can do other than to keep this in mind as we make load balancing decisions.


### Load Balancing at the Virtual IP Address

Virtual IP addresses (VIPs) are not assigned to any particular network interface. Instead, they are usually shared across many devices. However, from the user’s per‐ spective, the VIP remains a single, regular IP address. In theory, this practice allows us to hide implementation details (such as the number of machines behind a particu‐ lar VIP) and facilitates maintenance, because we can schedule upgrades or add more machines to the pool without the user knowing.

There are several possible approaches the balancer can take in deciding which back‐ end should receive the request. The first (and perhaps most intuitive) approach is to always prefer the least loaded backend. In theory, this approach should result in the best end-user experience because requests are always routed to the least busy machine. Unfortunately, this logic breaks down quickly in the case of stateful proto‐ cols, which must use the same backend for the duration of a request. This require‐ ment means that the balancer must keep track of all connections sent through it in order to make sure that all subsequent packets are sent to the correct backend. The alternative is to use some parts of a packet to create a connection ID (possibly using a hash function and some information from the packet), and to use the connection ID to select a backend. For example, the connection ID could be expressed as:

    id(packet) mod N

where id is a function that takes packet as an input and produces a connection ID,
and N is the number of configured backends.

One solution is to perform a Network Address Translation. However, this requires keeping an entry of every single connection in the tracking table, which precludes having a completely stateless fallback mechanism.

Our current VIP load balancing solution uses packet encapsulation. A net‐ work load balancer puts the forwarded packet into another IP packet with Generic Routing Encapsulation (GRE), and uses a backend’s address as the destina‐ tion. A backend receiving the packet strips off the outer IP+GRE layer and processes the inner IP packet as if it were delivered directly to its network interface. The net‐ work load balancer and the backend no longer need to exist in the same broadcast domain; they can even be on separate continents as long as a route between the two exists.

Packet encapsulation is a powerful mechanism that provides great flexibility in the way our networks are designed and evolve. Unfortunately, encapsulation also comes with a price: inflated packet size. Encapsulation introduces overhead (24 bytes in the case of IPv4+GRE, to be precise), which can cause the packet to exceed the available Maximum Transmission Unit (MTU) size and require fragmentation.

Once the packet reaches the datacenter, fragmentation can be avoided by using a larger MTU within the datacenter; however, this approach requires a network that supports large Protocol Data Units. As with many things at scale, load balancing sounds simple on the surface—load balance early and load balance often—but the difficulty is in the details, both for frontend load balancing and for handling packets once they reach the datacenter.

