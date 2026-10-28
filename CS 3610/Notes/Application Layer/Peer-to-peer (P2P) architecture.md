### Peer-to-Peer (P2P) Architecture
  
  P2P architecture represents a fundamental shift from the centralized client-server model. Instead of relying on a central, always-on server, P2P applications distribute tasks and workloads among peers, which are end-user computers that participate in the network.
#### Core Principles
  
  *   **No Always-On Server:** There is no single, central server that manages the system or stores the data. The infrastructure is the collection of peers themselves.
  *   **Direct Communication:** Arbitrary end systems (peers) communicate directly with each other.
  *   **Dual Role of Peers:** Each peer acts as both a client (requesting services and data from other peers) and a server (providing services and data to other peers).
  *   **Self-Scalability:** This is the most significant advantage of P2P systems, especially for content distribution. In a client-server model, as more clients join, the server's fixed resources become a bottleneck. In a P2P system, every new peer that joins to download content also brings new service capacity by contributing its own upload bandwidth to the system. This means the total system capacity grows as the number of users grows, allowing it to scale massively.
  *   **Churn and Complexity:** P2P systems must be designed to handle **churn**—the natural behavior of peers joining and leaving the network frequently and unpredictably. This makes managing the system more complex than a centralized server model.
#### File Distribution Time: Client-Server vs. P2P
  
  A quantitative analysis clearly shows the power of P2P's self-scalability for distributing a large file to many users.
  
  *   **Client-Server Time:** The total time to distribute a file is limited by two factors:
    1.  The server must transmit a full copy of the file to *every single peer*. This time increases linearly with the number of peers.
    2.  The peer with the slowest download connection must download a full copy.
    As the number of peers (N) grows, the server's upload link quickly becomes the bottleneck, and the distribution time grows without bound.
  
  *   **P2P Time:** The distribution time is also limited, but by different factors:
    1.  The original server only needs to upload *one single copy* of the file into the peer network.
    2.  The peer with the slowest download connection must still download a full copy.
    3.  The *total* upload capacity of the entire system (the server's upload speed *plus the upload speeds of all participating peers*) must be sufficient to deliver a copy of the file to everyone.
    The key insight is that as N grows, the total upload capacity of the system also grows. This allows P2P systems to distribute files to a massive number of peers with incredible efficiency and speed, as shown by the graph in the slides.
### Case Study: BitTorrent
  
  BitTorrent is a popular and effective P2P protocol designed for distributing large files.
#### Terminology
  
  *   **Torrent:** The group of all peers actively exchanging chunks of a specific file or set of files.
  *   **Tracker:** A central server that keeps track of the peers participating in the torrent. **Crucially, the tracker does not store the file itself.** Its only job is to introduce peers to one another.
  *   **Chunks:** The file is broken down into smaller, fixed-size pieces (e.g., 256KB). Peers download and upload these individual chunks.
#### How BitTorrent Works
  
  1.  A new peer (let's call her Alice) wants to download a file. She first obtains a `.torrent` file.
  2.  Alice's BitTorrent client contacts the tracker, which provides her with a list of other peers in the torrent.
  3.  Alice, initially having no chunks, connects to a subset of these peers (her "neighbors") and begins to request chunks.
  4.  As soon as Alice downloads her first chunk, she can immediately start uploading that chunk to other peers who need it.
  5.  Peers continuously exchange information about the chunks they have and request the chunks they need.
#### Key Policies for Chunk Exchange
  
  BitTorrent's success lies in its clever incentive mechanisms that encourage cooperation.
  
  *   **Requesting Chunks (Rarest First):** When deciding which chunk to request next, a peer will ask for the chunk that is the **least common** among its neighbors. This strategy does two things:
    *   It helps to quickly spread all chunks throughout the torrent, preventing a situation where the last few chunks are only available from the original seeder.
    *   It ensures that a new peer can quickly acquire chunks that are in high demand and thus have something valuable to trade.
  
  *   **Sending Chunks (Tit-for-Tat):** When deciding who to send chunks to, a peer (Alice) prioritizes peers who are providing her with the best download rates.
    *   **Top Four Providers:** Alice continuously measures her download rate from all her neighbors. She will upload chunks to the four peers who are sending her data at the highest rate. These peers are said to be **"unchoked."**
    *   **Reciprocity:** This creates a powerful incentive: to receive a high download rate from Alice, you must provide a high upload rate to Alice.
    *   **Optimistic Unchoking:** To discover potentially better partners, every 30 seconds Alice will pick one random, "choked" neighbor and start sending them chunks. If this new peer reciprocates and provides a good upload rate, it may become one of Alice's new top four partners. This keeps the system dynamic and efficient.