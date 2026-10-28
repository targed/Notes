### **Practice Questions: Multiple Choice**
  
  Here are some multiple-choice questions designed to test your conceptual understanding of the Application Layer protocols and architectures.
  
  **Question 1**
  The Domain Name System (DNS) is a critical internet function that is implemented as an application-layer protocol. Why does it primarily use UDP instead of TCP for its transport service?
  *   A) Because UDP provides guaranteed, in-order delivery of DNS records.
  *   B) To establish a persistent connection to the root servers for faster subsequent lookups.
  *   **C) For its low overhead and speed, as DNS queries are typically small and require a quick request-response cycle.**
  *   D) To use flow control, preventing the client from overwhelming the DNS server with requests.
  
  **Question 2**
  An email client on your phone sends an email to a friend. Which of the following describes the role of the SMTP protocol in this process?
  *   A) It is used by your friend's email client to *pull* the message from their mail server.
  *   **B) It is used by your email client to *push* the message to your mail server.**
  *   C) It is used to look up the IP address of your friend's mail server.
  *   D) It is used to manage and create folders on your mail server.
  
  **Question 3**
  In the client-server file distribution model, what is the primary reason this architecture does not scale well to a large number of clients?
  *   A) The download speed of the fastest client creates a bottleneck for all other clients.
  *   B) The server must wait for all clients to connect before it can begin sending the file.
  *   C) The physical distance between the server and the clients introduces too much propagation delay.
  *   **D) The server's upload bandwidth is a central bottleneck, and the time to distribute the file increases linearly with the number of clients.**
  
  **Question 4**
  In BitTorrent, a peer (Alice) must decide which of her neighbors to send file chunks to. The "tit-for-tat" mechanism provides an incentive for cooperation. On what basis does Alice primarily choose who to send chunks to?
  *   A) She sends chunks to the peers who have the rarest file chunks.
  *   B) She sends chunks to a randomly selected group of peers every 10 seconds.
  *   **C) She sends chunks to the peers that are currently providing her with the highest data upload rate.**
  *   D) She sends chunks to the peers who have been in the torrent the longest.
  
  **Question 5**
  What is the fundamental difference between a persistent and a non-persistent HTTP connection?
  *   A) Persistent HTTP uses UDP, while non-persistent HTTP uses TCP.
  *   B) Persistent HTTP is stateless, while non-persistent HTTP uses cookies to maintain state.
  *   **C) Persistent HTTP allows multiple objects to be sent over a single TCP connection, while non-persistent HTTP requires a new TCP connection for every object.**
  *   D) Persistent HTTP encrypts the data with TLS, while non-persistent HTTP sends it in cleartext.
  
  **Question 6**
  Dynamic Adaptive Streaming over HTTP (DASH) is used by services like Netflix and YouTube. What is the primary function of the client in a DASH system?
  *   A) To tell the server which CDN is closest so the server can push video from there.
  *   B) To send a manifest file to the server indicating which video quality it can support.
  *   C) To decompress all versions of a video chunk and select the best one for playout.
  *   **D) To estimate the available bandwidth and then request the video chunk with the highest quality that this bandwidth can support.**
  
  **Question 7**
  A host needs to resolve the hostname `www.example.com`. Its local DNS server begins an *iterated query*. What will the root DNS server provide in its response?
  *   A) The final IP address for `www.example.com`.
  *   **B) The IP address of the Top-Level Domain (TLD) server for the `.com` domain.**
  *   C) The IP address of the authoritative server for `example.com`.
  *   D) A list of all authoritative servers it knows about.
  
  **Question 8**
  When programming a TCP server, the `accept()` call is critically important. What is the direct result of a server process successfully executing `serverSocket.accept()`?
  *   A) It sends a TCP SYN packet to initiate a connection with a client.
  *   B) It permanently binds the server's welcoming socket to a specific client's IP address.
  *   **C) It creates a new, dedicated connection socket for an incoming client, allowing the original welcoming socket to continue listening for other clients.**
  *   D) It reads all available data from an already-established client connection.
  
  **Question 9**
  You visit a news website, which has ads served by an ad network. Later, you visit a completely different shopping website, which also has ads from the same network, and you see ads related to the news articles you just read. What technology allows the ad network to track your activity across these different sites?
  *   A) A Conditional GET request.
  *   B) The DNS protocol.
  *   **C) A third-party cookie set by the ad network.**
  *   D) A first-party cookie set by the news website.
  
  **Question 10**
  HTTP/2 introduced a mechanism to mitigate Head-of-Line (HOL) blocking. How does it accomplish this?
  *   A) By opening a separate TCP connection for every single object on a webpage.
  *   B) By replacing TCP with QUIC, a new transport protocol that runs over UDP.
  *   **C) By breaking objects into smaller frames and interleaving them on a single TCP connection, so a large object does not block smaller ones.**
  *   D) By requiring the client to first request all small objects before requesting any large objects.
  
  ***
- **Question 11**
  An email server needs to send a message to a user at `example.com`. Which type of DNS Resource Record (RR) must it query to find the hostname of the mail server responsible for that domain?
  *   A) A Record
  *   B) CNAME Record
  *   C) NS Record
  *   **D) MX Record**
  
  **Question 12**
  A user is streaming a video and notices that the playback occasionally freezes for a moment before resuming. This is often caused by network **jitter**. What client-side mechanism is specifically designed to absorb these variable packet delays and provide a smooth viewing experience?
  *   A) The manifest file
  *   B) A third-party cookie
  *   **C) The playout buffer**
  *   D) A persistent HTTP connection
  
  **Question 13**
  A user composes and sends an email, and their recipient later opens it to read it. Which sequence of protocols is most likely used for this entire process?
  *   A) The sender uses IMAP to send the message, and the receiver uses SMTP to retrieve it.
  *   B) The entire transaction, from sending to receiving, is handled by HTTP.
  *   **C) The sender's client uses SMTP to send the message, and the receiver's client uses IMAP to retrieve it.**
  *   D) DNS is used to send the message, and SMTP is used to retrieve it.
  
  **Question 14**
  A server application is built using UDP sockets. When the server receives a datagram from a client, what essential information must it extract directly from that datagram in order to send a reply back to the correct client process?
  *   A) The client's MAC address only.
  *   B) A TCP connection identifier.
  *   **C) The client's source IP address and source port number.**
  *   D) The destination port number only.
  
  **Question 15**
  In BitTorrent, peers are programmed to request missing file chunks in "rarest first" order. What is the primary reason for this strategy?
  *   A) To reduce the load on the central tracker server.
  *   **B) To ensure that rare chunks get replicated quickly, improving the overall health and availability of all file pieces in the torrent.**
  *   C) Because rare chunks are smaller and can be downloaded faster.
  *   D) To reward the peers who have been part of the torrent the longest.
  
  **Question 16**
  HTTP is known as a stateless protocol. Why, then, are cookies necessary for modern web applications like e-commerce sites?
  *   A) To cache frequently accessed objects in the browser, reducing latency.
  *   B) To provide a secure, encrypted channel for transmitting passwords.
  *   **C) To allow the server to maintain user-specific state (like a shopping cart) across multiple, independent HTTP requests.**
  *   D) To guarantee that web pages are delivered in the correct order.
  
  **Question 17**
  Which of the following is a primary architectural advantage of using a Peer-to-Peer (P2P) model instead of a client-server model for distributing a single, popular file to millions of users?
  *   A) It is simpler to manage and secure than a client-server system.
  *   B) It guarantees that the file will be delivered more reliably.
  *   **C) It exhibits self-scalability, where new users add to the system's total distribution capacity.**
  *   D) It requires significantly less total download bandwidth from the peers.
  
  **Question 18**
  In a DASH (Dynamic Adaptive Streaming over HTTP) system, what is the primary responsibility of the origin server before any streaming begins?
  *   A) To estimate the client's available bandwidth in real-time.
  *   B) To select the best CDN server for the client to connect to.
  *   C) To combine all video objects into a single, large file for efficient transfer.
  *   **D) To encode the video into chunks at multiple different quality/bitrate levels and to create a manifest file that lists them.**
  
  **Question 19**
  A university installs a web cache (proxy server) on its network. What are the TWO primary benefits the university expects to achieve with this cache?
  *   A) To guarantee a minimum throughput for all users and encrypt all web traffic.
  *   **B) To reduce traffic on its expensive internet access link and to lower the average response time for its users.**
  *   C) To block all third-party cookies by default and to assign permanent IP addresses to all users.
  *   D) To increase the speed of its LAN and to eliminate the need for DNS lookups.
  
  **Question 20**
  What is the role of a user's **Local DNS server** in the DNS resolution process?
  *   A) It holds the authoritative records for the user's domain (e.g., `mst.edu`).
  *   B) It belongs to the top level of the DNS hierarchy and directly communicates with the root servers only.
  *   **C) It acts as a proxy, forwarding the user's query into the DNS hierarchy, and it caches recent results to speed up future lookups.**
  *   D) It is only used to assign a dynamic IP address to the user's host.
- **Question 21**
  An application developer is creating a fast-paced, real-time multiplayer game where low latency is the most critical requirement. A small amount of data loss (e.g., an occasional update of another player's position) is acceptable. Which transport protocol should the developer choose and why?
  *   A) TCP, because its reliability guarantees that every game-state update arrives.
  *   **B) UDP, because it has no connection setup delay or congestion control, minimizing latency.**
  *   C) Both, using TCP for player movements and UDP for chat messages.
  *   D) HTTP, because it is the standard protocol for all modern game development.
  
  **Question 22**
  A web browser sends an HTTP request for a webpage. The first line of the server's response is: `HTTP/1.1 200 OK`. What does this status code signify?
  *   A) The server has permanently moved the requested content to a new URL.
  *   B) The server could not find the requested document.
  *   **C) The request was successful, and the requested object is included in the body of the response message.**
  *   D) The browser's cached version of the object is still valid, and no data needs to be transferred.
  
  **Question 23**
  A startup company, "Network Utopia," wants to create a website at `www.networkutopia.com`. What is the first essential step they must take to get their hostname registered in the global DNS system?
  *   A) Email ICANN directly to request that their hostname be added to the root servers.
  *   B) Configure their own authoritative DNS server and wait for it to be discovered automatically.
  *   C) Send an HTTP PUT request with their DNS records to the appropriate TLD server.
  *   **D) Register the domain name `networkutopia.com` with a DNS registrar, who will then insert records into the `.com` TLD server pointing to their authoritative server.**
  
  **Question 24**
  When programming a network application, what is a fundamental difference between creating a TCP client and a UDP client?
  *   A) Only a UDP client needs to know the server's port number.
  *   **B) A TCP client must first establish a connection using `connect()` before sending data, while a UDP client can send data immediately using `sendto()`.**
  *   C) A TCP client's socket provides automatic encryption, while a UDP client's does not.
  *   D) A TCP client must close the socket after every message, while a UDP client can keep it open.
  
  **Question 25**
  In a Content Distribution Network (CDN) that supports DASH, a client's manifest file may list multiple CDN servers in different geographic locations. How does the client typically decide which server to request a video chunk from?
  *   A) The manifest file explicitly tells the client which server it must use.
  *   B) It always chooses the server that is physically closest, regardless of network conditions.
  *   C) It requests the chunk from all available servers and uses the one that arrives first.
  *   **D) The client logic makes an intelligent decision, often selecting the server that offers the best performance (e.g., highest throughput or lowest RTT) at that moment.**
  
  **Question 26**
  The internet's architecture is often described as having a "smart edge" and a "dumb core." What does this principle imply for application developers?
  *   A) Developers must write complex software for routers in the network core to handle their application's logic.
  *   **B) Complex application logic is implemented in the end systems (clients and servers), while the network core simply focuses on forwarding packets.**
  *   C) The transport layer (TCP/UDP) is part of the application and must be implemented by the developer.
  *   D) All security functions are handled exclusively by the network core, simplifying application design.
  
  **Question 27**
  Which of the following statements accurately describes the roles of SMTP and IMAP in the email system?
  *   A) SMTP is a pull protocol used to retrieve message headers, while IMAP is a push protocol for the message body.
  *   **B) SMTP is a push protocol used to deliver a message from the sender's mail server to the recipient's mail server.**
  *   C) IMAP is used to send a message from a user agent to their mail server.
  *   D) Both SMTP and IMAP are used to look up the IP address of the recipient's mail server.
  
  **Question 28**
  A malicious actor intercepts a user's DNS query and sends back a fake reply that maps `www.mybank.com` to the IP address of a fraudulent server. This attack, which can cause subsequent users to receive the wrong IP from their local DNS server's cache, is known as:
  *   A) A Denial of Service (DoS) attack.
  *   B) TCP Session Hijacking.
  *   C) An HTTP `GET` flood.
  *   **D) DNS Cache Poisoning.**
  
  **Question 29**
  HTTP/3 uses QUIC over UDP instead of TCP. What is the primary motivation for this change from HTTP/2's use of TCP?
  *   A) UDP is more reliable than TCP and guarantees packet delivery.
  *   **B) A single lost TCP packet will stall all object streams due to TCP's in-order delivery (transport-layer HOL blocking), a problem that QUIC's independent streams solve.**
  *   C) UDP provides built-in, mandatory encryption for all web traffic.
  *   D) TCP connections cannot be made persistent, increasing latency.
  
  **Question 30**
  While P2P is highly scalable for large file distribution, in which scenario might the traditional client-server model actually be *faster*?
  *   A) When the file size is extremely large.
  *   **B) When the number of peers (N) is very small, as the time saved by P2P may not outweigh the overhead of peer discovery and coordination.**
  *   C) When the server's upload speed is very low.
  *   D) When all peers have very high upload speeds.
- **Question 31**
  When a multi-threaded TCP server successfully executes the `accept()` call, it creates a new "connection socket" to handle a specific client. What happens to the original "welcoming socket"?
  *   A) It is closed, and the server can no longer accept new clients.
  *   B) It is passed to a new thread along with the connection socket.
  *   C) It is transformed into the connection socket for that client.
  *   **D) It immediately returns to a listening state, ready to accept connections from other new clients.**
  
  **Question 32**
  A user types `www.google.com` into their browser and presses Enter. Which of the following describes the correct sequence of initial protocol actions?
  *   A) The browser sends an HTTP request, which prompts a DNS query, followed by a TCP handshake.
  *   B) The browser initiates a TCP handshake, which then triggers a DNS query to find the server.
  *   **C) The browser triggers a DNS query to get an IP address, then initiates a TCP handshake to that IP, and then sends an HTTP request.**
  *   D) The browser sends a UDP packet to port 80, which contains both the DNS and HTTP requests.
  
  **Question 33**
  A developer is writing a tool to check when a large number of web objects were last updated without downloading the objects themselves. Which HTTP method is the most efficient for this specific purpose?
  *   A) `GET`
  *   B) `POST`
  *   C) `PUT`
  *   **D) `HEAD`**
  
  **Question 34**
  Both CDNs and P2P systems like BitTorrent are used for large-scale content distribution. What is a key difference in their fundamental infrastructure?
  *   A) P2P systems require a central server for all file transfers, while CDNs do not.
  *   **B) CDNs are managed and owned by a single entity that strategically places servers, while P2P systems are composed of a decentralized, user-operated network.**
  *   C) CDNs can only serve video content, while P2P can serve any file type.
  *   D) CDNs rely on UDP for speed, while P2P systems exclusively use TCP for reliability.
  
  **Question 35**
  When you see `https://` in your browser's address bar, it signifies a secure connection. Which protocol is primarily responsible for providing the encryption, integrity, and authentication for this connection?
  *   A) DNSSEC
  *   B) SFTP
  *   C) HTTP/2
  *   **D) TLS (Transport Layer Security)**
  
  **Question 36**
  SMTP was designed in an earlier era of the internet and has a specific requirement regarding the character set of the message body. What is this requirement?
  *   A) The message must be encoded in UTF-8.
  *   B) The message must be sent as raw binary data.
  *   **C) The message must be in 7-bit ASCII.**
  *   D) The message must be pre-encrypted using PGP.
  
  **Question 37**
  A startup is designing a new, large-group video conferencing application where dozens of users can interact in real-time. To minimize latency and server costs, what would be the most efficient architectural choice for distributing the multiple video streams among the participants?
  *   A) A single, powerful central server that receives all video streams and re-broadcasts them to everyone (Client-Server).
  *   **B) A peer-to-peer (P2P) mesh where participants exchange video streams directly with a subset of other participants.**
  *   C) Storing the video streams on a CDN and having users pull them with DASH.
  *   D) Using email with video attachments sent via SMTP.
  
  **Question 38**
  DNS caching is essential for the internet's performance, but it also has a potential downside. What is the primary drawback of this mechanism?
  *   A) It increases the traffic load on the root DNS servers.
  *   B) It violates network security policies by storing sensitive data.
  *   C) It can only cache `A` records, not `MX` or `NS` records.
  *   **D) If a host's IP address changes, cached DNS entries across the internet may be temporarily out-of-date until their TTL expires.**
  
  **Question 39**
  In a DASH system, what is the specific role of the **manifest file**?
  *   A) It is the video file itself, just with a different name.
  *   **B) It provides the client with a list of URLs for all available chunks of a video at their various quality and bitrate levels.**
  *   C) It is a client-side file that tracks the user's current estimated bandwidth.
  *   D) It is a security certificate used to encrypt the video stream.
  
  **Question 40**
  In the context of creating a server socket (either TCP or UDP), what is the purpose of the `bind()` function call?
  *   A) To initiate a connection with a specific client.
  *   B) To specify the IP address of the destination client for an outgoing message.
  *   **C) To associate the server's socket with a specific port number on the server's local machine.**
  *   D) To encrypt the data that will be sent through the socket.
### **Practice Questions: Math-Based Problems**
#### **Scenario 1 (Based on Quiz 1)**
  
  An institution connects to the internet via a **400 Mbps** access link. The local area network (LAN) inside the institution is a **1 Gbps** Ethernet. Users on the LAN request web objects from various origin servers.
  
  *   Access link rate: **400 Mbps**
  *   LAN speed: **1000 Mbps**
  *   RTT from the institutional router to any origin server: **800 ms** (0.8 seconds)
  *   Average web object size: **2 Mbits**
  *   Average request rate from all browsers combined: **150 requests/sec**
  
  ---
  
  **Question 1**
  Calculate the **average data rate** to the browsers. What is the **traffic intensity** on the access link? What is the traffic intensity on the LAN?
  
  **Question 2**
  Assuming that the average queueing delay at a link is given by the formula $\frac{\text{Traffic Intensity}}{(1 - \text{Traffic Intensity})} \times (\text{Average Object Size} / \text{Link Rate})$, calculate the **total average end-to-end delay** for fetching a web object. The end-to-end delay is the sum of the RTT, the access link delay (transmission + queueing), and the LAN delay (transmission + queueing).
  
  **Question 3**
  Now, suppose the institution installs a web cache with a **30% hit rate**. What is the new traffic intensity on the access link? How does this change impact the total average end-to-end delay? (For the 30% of requests satisfied by the cache, assume the delay is negligible, i.e., 0 ms).
  
  ---
#### **Scenario 2 (Based on Quiz 2)**
  
  A server is distributing a file of size **F = 25 Gbits** to **N = 15 peers**. The server has an upload rate of **$u_s$ = 80 Mbps**. The peer download and upload speeds are given in the table below.
  
  For convenience, use **1 Gbps = 1000 Mbps** and **1 Gbit = 1000 Mbits**.
  
  | Client # | Upload (Mbps) | Download (Mbps) |
  | :--- | :--- | :--- |
  | 1-5 (5 peers) | 10 | 20 |
  | 6-10 (5 peers) | 20 | 50 |
  | 11-15 (5 peers)| 30 | 100 |
  
  ---
  
  **Question 4**
  In the **client-server paradigm**, how much total data (in Gbits) must the server upload? What is the minimum time required to distribute the file to all 15 peers?
  
  **Question 5**
  Now consider the **P2P paradigm**. What is the minimum time required to distribute the file to all 15 peers?
  
  **Question 6**
  Suppose in the P2P scenario, the server's upload speed ($u_s$) is drastically reduced to only **10 Mbps**. Does this change the minimum P2P distribution time? Why or why not? What if the server's upload speed were increased to 500 Mbps?
  
  ---
### **Solutions to Math-Based Problems**
#### **Scenario 1 (Based on Quiz 1)**
  
  **Given:**
  *   Access link rate = 400 Mbps
  *   LAN speed = 1000 Mbps
  *   RTT = 0.8 sec
  *   Object size = 2 Mbits
  *   Request rate = 150 requests/sec
  
  ---
  
  **Question 1: Traffic Intensity**
  
  1.  **Calculate the average data rate to browsers:** This is the rate at which bits are flowing from the internet to the users.
    *   Average data rate = (Request rate) × (Object size)
    *   Average data rate = (150 requests/sec) × (2 Mbits/request) = **300 Mbps**
  
  2.  **Calculate the traffic intensity on the access link:** This is the ratio of the average data rate to the capacity of the link.
    *   Traffic Intensity = (Average data rate) / (Access link rate)
    *   Traffic Intensity = 300 Mbps / 400 Mbps = **0.75**
  
  3.  **Calculate the traffic intensity on the LAN:** The same 300 Mbps of traffic must also traverse the local LAN to reach the users.
    *   Traffic Intensity = (Average data rate) / (LAN speed)
    *   Traffic Intensity = 300 Mbps / 1000 Mbps = **0.30**
  
  ---
  
  **Question 2: End-to-End Delay (No Cache)**
  
  The total end-to-end delay is the sum of delays on each segment of the path.
  Total Delay = RTT + (Access Link Delay) + (LAN Delay)
  
  1.  **Calculate Access Link Delay:**
    *   Transmission Delay = Object Size / Link Rate = 2 Mbits / 400 Mbps = 0.005 sec.
    *   Queueing Delay = (Intensity / (1 - Intensity)) × (Transmission Delay)
    *   Queueing Delay = (0.75 / (1 - 0.75)) × 0.005 sec = (0.75 / 0.25) × 0.005 = 3 × 0.005 = 0.015 sec.
    *   **Total Access Link Delay** = 0.005 + 0.015 = **0.020 sec**.
  
  2.  **Calculate LAN Delay:**
    *   Transmission Delay = Object Size / Link Rate = 2 Mbits / 1000 Mbps = 0.002 sec.
    *   Queueing Delay = (Intensity / (1 - Intensity)) × (Transmission Delay)
    *   Queueing Delay = (0.30 / (1 - 0.30)) × 0.002 sec = (0.30 / 0.70) × 0.002 ≈ 0.429 × 0.002 ≈ 0.00086 sec.
    *   **Total LAN Delay** = 0.002 + 0.00086 = **0.00286 sec**.
  
  3.  **Calculate Total End-to-End Delay:**
    *   Total Delay = RTT + Access Link Delay + LAN Delay
    *   Total Delay = 0.8 sec + 0.020 sec + 0.00286 sec = **0.82286 seconds**
  
  ---
  
  **Question 3: Performance with a 30% Cache Hit Rate**
  
  1.  **Calculate the new traffic rate over the access link:** With a 30% hit rate, only 70% of the requests go to the origin servers.
    *   New Access Link Rate = 300 Mbps × (1 - 0.30) = 300 Mbps × 0.7 = **210 Mbps**.
  
  2.  **Calculate the new traffic intensity on the access link:**
    *   New Intensity = 210 Mbps / 400 Mbps = **0.525**.
  
  3.  **Calculate the average end-to-end delay as a weighted average:**
    Average Delay = (Hit Rate × Delay_hit) + (Miss Rate × Delay_miss)
  
  4.  **Calculate the delay for a miss (Delay_miss):** This is the delay for the 70% of requests that use the access link. We must recalculate the access link's queueing delay with the new, lower intensity.
    *   New Access Queueing Delay = (0.525 / (1 - 0.525)) × 0.005 sec ≈ 1.105 × 0.005 ≈ 0.00553 sec.
    *   New Access Link Delay = (Transmission) + (New Queueing) = 0.005 + 0.00553 = 0.01053 sec.
    *   LAN delay is unchanged because all 300 Mbps of traffic (hits + misses) still flows over it.
    *   Delay_miss = RTT + New Access Link Delay + LAN Delay = 0.8 + 0.01053 + 0.00286 = 0.81339 sec.
  
  5.  **Calculate the final average delay:**
    *   Average Delay = (0.30 × 0 sec) + (0.70 × 0.81339 sec) = **0.5694 seconds**.
    The cache reduces the average delay from ~0.82 sec to ~0.57 sec, a significant improvement.
  
  ---
#### **Scenario 2 (Based on Quiz 2)**
  
  **Given:**
  *   File Size (F) = 25 Gbits = 25,000 Mbits
  *   Number of Peers (N) = 15
  *   Server Upload Rate ($u_s$) = 80 Mbps
  *   Peer download/upload rates from the table.
  
  ---
  
  **Question 4: Client-Server Distribution**
  
  1.  **Total data the server must upload:**
    *   Total Upload = N × F = 15 × 25 Gbits = **375 Gbits**.
  
  2.  **Calculate minimum distribution time:** The formula is \(D_{c-s} \ge \max \left( \frac{N \cdot F}{u_s}, \frac{F}{d_{min}} \right)\).
    *   Find the slowest download peer ($d_{min}$): Looking at the table, the slowest download rate is **20 Mbps**.
    *   Server bottleneck term: (N × F) / $u_s$ = (15 × 25,000 Mbits) / 80 Mbps = 375,000 / 80 = **4,687.5 seconds**.
    *   Client bottleneck term: F / $d_{min}$ = 25,000 Mbits / 20 Mbps = **1,250 seconds**.
    *   The minimum time is the maximum of these two values: max(4687.5, 1250) = **4,687.5 seconds**.
  
  ---
  
  **Question 5: P2P Distribution**
  
  1.  **Calculate minimum distribution time:** The formula is \(D_{P2P} \ge \max \left( \frac{F}{u_s}, \frac{F}{d_{min}}, \frac{N \cdot F}{u_s + \sum u_i} \right)\).
    *   First, calculate the sum of all peer upload rates ($\sum u_i$):
        *   (5 peers × 10 Mbps) + (5 peers × 20 Mbps) + (5 peers × 30 Mbps)
        *   50 + 100 + 150 = **300 Mbps**.
    *   Calculate the three terms in the formula:
        *   Term 1 (Server uploads one copy): F / $u_s$ = 25,000 / 80 = **312.5 seconds**.
        *   Term 2 (Slowest client downloads): F / $d_{min}$ = 25,000 / 20 = **1,250 seconds**.
        *   Term 3 (Aggregate upload capacity): (N × F) / ($u_s$ + $\sum u_i$) = (15 × 25,000) / (80 + 300) = 375,000 / 380 ≈ **986.84 seconds**.
    *   The minimum time is the maximum of these three values: max(312.5, 1250, 986.84) = **1,250 seconds**.
  
  ---
  
  **Question 6: P2P with Changed Server Speed**
  
  1.  **Case 1: Server upload speed ($u_s$) is reduced to 10 Mbps.**
    *   Recalculate the three P2P terms with $u_s = 10$:
        *   Term 1: F / $u_s$ = 25,000 / 10 = **2,500 seconds**.
        *   Term 2: F / $d_{min}$ = **1,250 seconds** (unchanged).
        *   Term 3: (N × F) / ($u_s$ + $\sum u_i$) = 375,000 / (10 + 300) = 375,000 / 310 ≈ **1,209.7 seconds**.
    *   New minimum time = max(2500, 1250, 1209.7) = **2,500 seconds**.
    *   **Answer:** Yes, the time changes. The server's ability to inject the first copy of the file into the peer network has now become the primary bottleneck, significantly slowing down the entire distribution.
  
  2.  **Case 2: Server upload speed ($u_s$) is increased to 500 Mbps.**
    *   Recalculate the three P2P terms with $u_s = 500$:
        *   Term 1: F / $u_s$ = 25,000 / 500 = **50 seconds**.
        *   Term 2: F / $d_{min}$ = **1,250 seconds** (unchanged).
        *   Term 3: (N × F) / ($u_s$ + $\sum u_i$) = 375,000 / (500 + 300) = 375,000 / 800 = **468.75 seconds**.
    *   New minimum time = max(50, 1250, 468.75) = **1,250 seconds**.
    *   **Answer:** No, the time does not change. Even though the server is much faster, the bottleneck remains the slowest client's download speed. Increasing the server's speed beyond the point where it is no longer the bottleneck yields no further improvement to the total distribution time.