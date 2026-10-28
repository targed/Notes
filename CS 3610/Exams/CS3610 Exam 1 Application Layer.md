# `Exam 1: Application Layer`

## `Question 1`

`HTTP runs on top of which transport protocol?`

- [ ]   `IP`   
- [ ]   `ICMP`   
- [ ]   `UDP`   
- [x]   `TCP`

## `Question 2`

`Which of the following applications best exemplifies a client-server model?`

- [ ]   `Blockchain`   
- [ ]   `Skype`   
- [x]   `HTTP Web Browsing`   
- [ ]   `BitTorrent`

## `Question 3`

`The purpose of DASH (Dynamic Adaptive Streaming over HTTP) is to:`

- [x]   `Reduce buffering by adapting video rate to available bandwidth`   
- [ ]   `Avoid CDN replication`   
- [ ]   `Store videos at a single global server`   
- [ ]   `Replace TCP with UDP`

## `Question 4`

`DNS is a centralized database located at ICANN’s headquarters.`

- [ ]   `True`   
- [x]   `False`

## `Question 5`

`In a P2P file distribution, total upload capacity of the system equals:`

- [ ]   `Average download rate of all peers`   
- [ ]   `Server upload capacity only`   
- [ ]   `Minimum of all peer rates`   
- [x]   `Sum of all peer upload rates plus the server’s upload rate`

## `Question 6`

`A cookie allows a website to:`

- [ ]   `Encrypt communication between client and server`   
- [x]   `Track and maintain state between HTTP transactions`   
- [ ]   `Provide end-to-end congestion control`   
- [ ]   `Optimize TCP handshake performance`

## `Question 7`

`Which type of DNS record maps a hostname to an IP address?`

- [ ]   `MX`   
- [ ]   `NS`   
- [x]   `A`   
- [ ]   `CNAME`

## `Question 8`

`A major advantage of using CDNs is:`

- [ ]   `Eliminating the need for DNS lookups`   
- [ ]   `Allowing clients to bypass the application layer`   
- [ ]   `Increasing packet size for faster delivery`   
- [x]   `Reducing latency and congestion by distributing content geographically`

## `Question 9`

`Which protocol allows users to organize mail into folders and keep them on the server?`

- [x]   `IMAP`   
- [ ]   `DNS`   
- [ ]   `SMTP`   
- [ ]   `POP3`

## `Question 10`

`SMTP uses which transport protocol and port?`

- [ ]   `TCP port 80`   
- [ ]   `UDP port 110`   
- [x]   `TCP port 25`   
- [ ]   `UDP port 25`

## `Question 11`

`The client-server model primarily relies on:`

- [ ]   `Stateless communication only`   
- [x]   `Always-on servers and on-demand clients`   
- [ ]   `Dynamic IP servers`   
- [ ]   `Randomized peer connectivity`

## `Question 12`

`What is the main function of DNS?`

- [x]   `Translate domain names to IP addresses`   
- [ ]   `Cache HTTP objects`   
- [ ]   `Encrypt communication between clients`   
- [ ]   `Measure network delay`

## `Question 13`

`A 404 HTTP status code means “Bad Request.”`

- [ ]   `True`   
- [x]   `False`

## `Question 14`

`The Application Layer defines how network-core devices process data to ensure efficient routing.`

- [ ]   `True`   
- [x]   `False`

## `Question 15`

`In peer-to-peer (P2P) architecture, management complexity arises because peers can frequently disconnect and change IP addresses.`

- [x]   `True`   
- [ ]   `False`

## `Question 16`

`In the peer-to-peer model, scalability improves because:`

- [ ]   `All peers connect through a central router.`   
- [ ]   `IP addresses are static and predictable.`   
- [ ]   `Peers rely on the server’s upload bandwidth.`   
- [x]   `Each new peer contributes additional service capacity.`

## `Question 17`

`Conditional GET in browser caching helps by:`

- [x]   `Avoiding redundant data transfer if the cached object is up to date`   
- [ ]   `Replacing cookies with cache identifiers`   
- [ ]   `Sending entire objects over UDP`   
- [ ]   `Forcing clients to re-download data`

## `Question 18`

`In non-persistent HTTP, how many TCP connections are required to fetch one base HTML page with 10 images?`

- [ ]   `10`   
- [ ]   `1`   
- [x]   `11`   
- [ ]   `5`

## `Question 19`

`True or False:`  
`DNS caching reduces average lookup latency but risks outdated entries.`

- [x]   `True`   
- [ ]   `False`

## `Question 20`

`SMTP is a "pull" protocol, similar to HTTP.`

- [ ]   `True`   
- [x]   `False`

## `Question 21`

`In email communication, which statement is correct?`

- [ ]   `SMTP transfers mail between user agents.`   
- [ ]   `DNS resolves email addresses directly.`   
- [ ]   `HTTP replaces IMAP in all cases.`   
- [x]   `SMTP transfers mail between mail servers, while IMAP retrieves it.`

## `Question 22`

`Third-party cookies are often used to:`

- [ ]   `Store cached DNS responses`   
- [ ]   `Authenticate HTTPS sessions`   
- [ ]   `Improve TCP throughput`   
- [x]   `Track users across multiple websites`

## `Question 23`

`Which of the following is not an example of a network application?`

- [x]   `Congestion Control of the Network`   
- [ ]   `Web Browsing`   
- [ ]   `Internet search`   
- [ ]   `Video Conferencing`

## `Question 24`

`The primary role of SMTP is:`

- [ ]   `Formatting mail headers`   
- [x]   `Transferring mail between servers`   
- [ ]   `Retrieving mail from the server`   
- [ ]   `Encrypting mail messages`

## `Question 25`

`In CDN architecture, “enter deep” means deploying many small servers closer to users.`

- [x]   `True`   
- [ ]   `False`

## `Question 26`

`Scenario:​`

`access link rate: 50 Mbps`  
`LAN rate: 500Mbps​`  
`RTT from institutional router to server: 1.5 sec​`  
`web object size: 80K bits​`  
`average request rate from browsers to origin servers: 600/sec`  
`What would be the bottleneck in this scenario, and how would you address it? Explain your reasoning with an example.`

`Your Answer:`  
`The main bottleneck in this scenario is that the access link utilization is at 96% which leads to long queueing delays as it is almost totally full.` 

`Here is the math for that:`

`Web object size = 80k Bits`

`AVG req rate = 600/sec`

`Avg data per sec = 600 * 80 = 48 Mbps`

`Access link utilization = 48/50 = 0.96`

`In order to fix it, there are really two main solutions. The first is to buy a faster access link. However, this can be expensive and will still result in slower times than a web cache, which is the second option. A web cache would be better because not only would it probably be cheaper, but it would also reduce strain/utilization of the access link while reducing request times. Some percentage of the requests will not have to go out to the internet to be fulfilled but can instead be served directly from the cache.`

## `Question 27`

`The problem is to distribute a file of size F = 5 Gbits to each of these 8 peers.`

`Suppose the server has an upload rate of u = 79 Mbps.`  
`The 8 peers have upload rates of: u1 = 24 Mbps, u2 = 24 Mbps, u3 = 24 Mbps, u4 = 26 Mbps, u5 = 11 Mbps, u6 = 12 Mbps, u7 = 17 Mbps, and u8 = 29 Mbps`  
`The 8 peers have download rates of: d1 = 38 Mbps, d2 = 13 Mbps, d3 = 13 Mbps, d4 = 40 Mbps, d5 = 32 Mbps, d6 = 32 Mbps, d7 = 24 Mbps, and d8 = 26 Mbps`  
 

`1. What is the minimum time needed to distribute this file from the central server to the 8 peers using the client-server model? (2.5 Points)`

`2. What is the minimum time needed to distribute this file using peer-to-peer download? (2.5 points)`

`Your Answer:`  
`Client server:`

`Server upload time = N*F/ Us = 5GB * 8 = 40GB/0.079 = 506.329 s`

`Client download time -> Slowest client download = F/ Dc = 5/0.013 = 384.165 s`

`Client server time = max(Server Upload Time, Client Download Time) = max(506.329, 384.165)`

`Client server min time = 506.329`

`P2P`

`Server Upload time = F/Us = 5/0.079 = 63.291 s`

`Client download time -> Slowest client download = F/ Dc = 5/0.013 = 384.165 s`

`P2P time = N*F/(Us + Uc) = (40)/(0.079+ 0.167) = 162.601 s`

`Min P2P time = max(Server Upload Time, Client Download Time, P2P Time) = max(63.291, 384.165, 162.601)`

`Min P2P time = 384.165s`

## `Question 28`

`Scenario:​`

`access link rate: 50 Mbps`  
`LAN rate: 500Mbps​`  
`RTT from institutional router to server: 1.5 sec​`  
`web object size: 80K bits​`  
`average request rate from browsers to origin servers: 600/sec​`  
 

`Find out:`

`1. Average data rate from browsers (1 point)`

`2. Access Link Utilization  (1 point)`

`3. LAN Utilization  (1 point)`

`4. End to End Delays  (2 points)`

`Your Answer:`  
`1).` 

`(Avg req rate * (Avg object size)`

`80K * 600/s = 48 Mbps`

`Avg data rate = 48 Mbps`

`2).` 

`Traffic Intensity = (Avg Data Rate)/(Link Capacity)`

`48/50 = 0.96`

`Access link utilization = 0.96 or 96%`

`3).` 

`Traffic Intensity = (Avg Data Rate)/(Link Capacity)`

`48/500 = 0.096`

`LAN Utilization = 0.096 or 9.6%`

`4).` 

`A-Link Transmission Delay = 0.08/50 = 0.0016 s`  
`A-Link Queue delay: (0.96/1-0.96) * (0.0016) = 0.0384 s`

`LAN Transmission Delay = 0.000016 S`

`LAN Queue Delay: (0.096/1-0.096) * 0.000016 = 0.000000169 s`

`RTT = 1.5 sec`

`Total = 0.0016 + 0.0384 + 0.000016 + 0.000000169 + 1.5 = 1.54 s`

# `Exam 2: Transport & Network Layer`

## `Question 1`

`UDP is preferred for video streaming because it guarantees no congestion.`

- [ ]   `True`   
- [x]   `False`

## `Question 2`

`A router in distance vector routing updates its table when:`

- [ ]   `A packet arrives`   
- [ ]   `Output queue fills`   
- [x]   `A neighbor sends new DV`   
- [ ]   `TTL expires`

## `Question 3`

`BGP differs from intra-domain routing because it:`

- [ ]   `Computes shortest paths`   
- [x]   `Allows policy-based path selection`   
- [ ]   `Ignores link failures`   
- [ ]   `Runs only on routers with TCAMs`

## `Question 4`

`A TCP connection has a very small congestion window but a large receiver window. What is the limiting factor?`

- [ ]   `Application slowness`   
- [x]   `Network congestion`   
- [ ]   `Receiver buffering`   
- [ ]   `Poor checksum usage`

## `Question 5`

`The data plane determines routes; the control plane forwards packets.`

- [ ]   `True`   
- [x]   `False`

## `Question 6`

`In SDN, the controller installs forwarding rules. What is a major risk?`

- [ ]   `IPv4 incompatibility`   
- [ ]   `Distributed inconsistencies`   
- [ ]   `Excessive queueing`   
- [x]   `Controller failure becomes a single point of failure`

## `Question 7`

`NAT creates challenges for:`

- [x]   `End-to-end connectivity`   
- [ ]   `DHCP`   
- [ ]   `DNS caching`   
- [ ]   `Ethernet forwarding`

## `Question 8`

`A TCP sender increases congestion window slowly at first. Why?`

- [ ]   `Preserve TCP handshake`   
- [ ]   `Reduce MSS`   
- [x]   `Avoid overwhelming the network prematurely`   
- [ ]   `Reduce ACK overhead`

## `Question 9`

`HOL blocking may occur even when output ports are idle.`

- [x]   `True`   
- [ ]   `False`

## `Question 10`

`Why does UDP include a checksum even though it offers no reliability guarantees?`

- [ ]   `It enables ordered delivery`   
- [ ]   `It speeds up transmission`   
- [ ]   `It reduces congestion`   
- [x]   `It provides minimal error detection`

## `Question 11`

`TCP uses NAKs in addition to ACKs.`

- [ ]   `True`   
- [x]   `False`

## `Question 12`

`A DV protocol converges slowly when:`

- [ ]   `Link state information arrive too quickly`   
- [x]   `Link cost increases, causing count-to-infinity`   
- [ ]   `Network uses static routing`   
- [ ]   `Costs decrease`

## `Question 13`

`A "hot potato" routing choice in BGP prefers:`

- [ ]   `Highest bandwidth path`   
- [ ]   `Lowest AS-path length`   
- [ ]   `Lower policy cost`   
- [x]   `The closest exit point from the AS`

## `Question 14`

`OSPF supports hierarchical routing using areas.`

- [x]   `True`   
- [ ]   `False`

## `Question 15`

`Excessively large router buffers can degrade performance because:`

- [ ]   `They force shortest-path routing`   
- [ ]   `They increase packet corruption`   
- [ ]   `They disable ECN`   
- [x]   `They inflate/increase RTT`

## `Question 16`

`Head-of-Line blocking occurs when:`

- [x]   `First packet at input queue blocks packets behind it`   
- [ ]   `The control plane restarts`   
- [ ]   `Switch fabric is malfunctioning`   
- [ ]   `Output port is overloaded`

## `Question 17`

`Why is link-state routing more robust to count-to-infinity problems?`

- [ ]   `Uses reverse poisoning`   
- [x]   `Has global knowledge of topology`   
- [ ]   `Avoids routing tables`   
- [ ]   `Uses random forwarding`

## `Question 18`

`A receiver advertising a small window forces the sender to slow down even if network has capacity.`

- [x]   `True`   
- [ ]   `False`

## `Question 19`

`Longest-prefix matching is required because:`

- [ ]   `IP headers change at each hop`   
- [x]   `IP addressing aggregates networks into hierarchical blocks`   
- [ ]   `Routers use MAC addresses`   
- [ ]   `Routers cannot store full addresses`

## `Question 20`

`Link-state routing scales poorly for huge networks primarily because:`

- [ ]   `Works only with IPv6`   
- [x]   `Flooding link state information is expensive`   
- [ ]   `Packets are too large`   
- [ ]   `Requires no timers`

## `Question 21`

`Bus-based switching can operate at terabit-per-second scale.`

- [ ]   `True`   
- [x]   `False`

## `Question 22`

`In DV, no router ever sees the full topology.`

- [x]   `True`   
- [ ]   `False`

## `Question 23`

`Weighted Fair Queueing (WFQ) guarantees:`

- [ ]   `FIFO ordering`   
- [ ]   `Removal of congestion`   
- [ ]   `Zero delay`   
- [x]   `Proportional bandwidth allocation among classes`

## `Question 24`

`A corrupted ACK in rdt2.0 may cause the sender to send duplicates.`

- [x]   `True`   
- [ ]   `False`

## `Question 25`

`TCP can deliver data reliably even when routers reorder packets.`

- [x]   `True`   
- [ ]   `False`

## `Question 26`

`TCP flow control prevents:`

- [ ]   `Router congestion`   
- [ ]   `Sequence number wraparound`   
- [x]   `Receiver buffer overflow`   
- [ ]   `Packet reordering`

## `Question 27`

`IPv6 removes fragmentation to:`

- [x]   `Reduce router state and overhead`   
- [ ]   `Increase routing table size`   
- [ ]   `Enable NAT`   
- [ ]   `Improve broadcast performance`

## `Question 28`

`In an output-queued router, packet loss primarily happens when:`

- [ ]   `Control plane is slow`   
- [ ]   `Input queue fills up`   
- [x]   `Output buffer overflows`   
- [ ]   `Link speed exceeds switch speed`

## `Question 29`

`UDP is chosen for DNS primarily because:`

- [ ]   `It provides higher security`   
- [ ]   `It ensures no reordering`   
- [x]   `It avoids connection setup delay`   
- [ ]   `It eliminates loss`

## `Question 30`

`In Dijkstra’s algorithm, the set N′ contains:`

- [ ]   `Routers with zero degree`   
- [ ]   `All neighbors`   
- [x]   `Nodes whose least-cost path is finalized`   
- [ ]   `Only leaf nodes`

## `Question 31`

`![][image1]`  
`Solve routing problem for Router C,`

`1. Show step by step solutions of Dijkstra's Algorithm (Table format) - 6 points`  
`2. Draw the resulting shortest path tree from C - 2 points`  
`3. Show the forwarding table in C - 2 points`

[image1]: <data:image/png;base64,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>