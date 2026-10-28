### DNS: Domain Name System
#### What is DNS and Why is it Needed?
  
  The internet has two parallel systems for identifying hosts:
  
  *   **IP Addresses:** Numeric addresses (e.g., `128.119.245.12`) that computers use to identify and locate each other in the network.
  *   **Hostnames:** Human-friendly names (e.g., `www.amazon.com`) that are easier for people to remember.
  
  The core function of the Domain Name System (DNS) is to act as the translator between these two systems. It's often called the "phonebook of the internet."
  
  DNS is two things at once:
  1.  A **distributed, hierarchical database** containing mappings between hostnames and IP addresses.
  2.  An **application-layer protocol** that allows hosts to query this database to resolve names.
#### Why Not a Centralized DNS?
  
  A single, centralized server for the entire internet is not feasible for several reasons:
  
  *   **Single Point of Failure:** If that one server crashed, the entire internet would be inaccessible.
  *   **Traffic Volume:** A single server could not handle the trillions of DNS queries generated every day.
  *   **Geographic Distance:** Users far from the server would experience significant delays.
  *   **Maintenance:** It would be impossible to update and maintain a single, massive database in real-time.
  
  The solution is a **distributed and hierarchical** system that is incredibly scalable and resilient.
#### The DNS Hierarchy
  
  DNS is organized in a tree-like hierarchy. At the top are the root servers, followed by the Top-Level Domain servers, and finally the Authoritative servers.
  
  1.  **Root DNS Servers:** There are 13 logical root name servers in the world (replicated many times for redundancy and performance). They are the first point of contact for any DNS query that cannot be resolved locally. They don't know the IP address for `www.amazon.com`, but they know the IP addresses of the servers responsible for the `.com` domain.
  
  2.  **Top-Level Domain (TLD) Servers:** These servers are responsible for all domains within a specific top-level domain, such as `.com`, `.org`, `.net`, `.edu`, and all country-specific domains like `.uk` or `.jp`. The `.com` TLD server doesn't know the IP for `www.amazon.com`, but it knows the IP of the authoritative DNS server for `amazon.com`.
  
  3.  **Authoritative DNS Servers:** This is the final level in the hierarchy. Every organization that hosts a website, mail server, etc., must provide its own publicly accessible authoritative DNS server. This server holds the definitive records that map the organization's hostnames (like `www.amazon.com`) to their specific IP addresses.
#### The DNS Resolution Process
  
  When you type a URL in your browser, a multi-step process occurs to find the IP address. This process involves your **Local DNS Server**, which is typically provided by your Internet Service Provider (ISP).
  
  Let's trace a query from a host for `gaia.cs.umass.edu`:
  
  *   **Iterated Query (The common practice):**
    1.  The host sends a query to its **Local DNS Server**.
    2.  The Local DNS Server asks a **Root Server**: "Where can I find `gaia.cs.umass.edu`?"
    3.  The Root Server replies: "I don't know, but here is the IP for the `.edu` **TLD Server**."
    4.  The Local DNS Server asks the `.edu` TLD Server: "Where is `gaia.cs.umass.edu`?"
    5.  The TLD Server replies: "I don't know, but here is the IP for the **Authoritative DNS Server** for `umass.edu`."
    6.  The Local DNS Server asks the `umass.edu` Authoritative Server: "What is the IP for `gaia.cs.umass.edu`?"
    7.  The Authoritative Server finds the record and replies with the IP address.
    8.  The Local DNS Server sends the IP address back to the original host.
  
  *   **Recursive Query:** In this alternate method, the host asks the Local DNS Server, which then does all the subsequent work on behalf of the host and returns only the final answer. This puts a heavier burden on the servers in the hierarchy.
#### DNS Caching
  
  To improve performance and reduce traffic, DNS relies heavily on caching.
  
  *   When a DNS server receives a mapping, it stores that mapping in its local cache.
  *   The next time it receives a query for the same hostname, it can return the answer immediately from its cache without having to query the root, TLD, or authoritative servers again.
  *   Each DNS record comes with a **Time-to-Live (TTL)** value, which tells the server how long it should cache the record before discarding it.
#### DNS Records
  
  The database entries in DNS are called **Resource Records (RRs)**. The most common types are:
  
  *   **Type A:** Maps a hostname to an IPv4 address.
  *   **Type NS:** Specifies the authoritative Name Server for a domain.
  *   **Type CNAME (Canonical Name):** Creates an alias from one hostname to another (e.g., `www.example.com` might be a CNAME for `server1.example.com`).
  *   **Type MX (Mail Exchanger):** Specifies the mail server responsible for handling email for a domain.
#### DNS Security
  
  DNS is a critical but vulnerable part of the internet infrastructure.
  
  *   **DDoS Attacks:** Attackers can try to overload Root or TLD servers with traffic to disrupt name resolution. This is difficult to succeed at due to the massive redundancy of the system.
  *   **Spoofing Attacks:** An attacker can intercept a DNS query and return a fraudulent reply, redirecting a user to a malicious site. This is also known as **DNS cache poisoning**.
  *   **DNSSEC (DNS Security Extensions):** This is a security protocol that helps prevent spoofing by using digital signatures to authenticate DNS replies, ensuring the data has not been tampered with.