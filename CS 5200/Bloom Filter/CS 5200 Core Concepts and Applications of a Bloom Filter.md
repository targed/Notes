### 1. What is a Bloom Filter? (Slides 1-2)
A Bloom Filter is a space-efficient, probabilistic data structure used to answer a very specific question: **"Is this item a member of a set?"** (A membership query).

Unlike a standard Hash Table or Binary Search Tree that stores the actual data (e.g., the string "apple"), a Bloom Filter does not store the data itself. Because of this, it can represent massive datasets in a fraction of the memory.

However, because it uses probability and hashing, it comes with a strict rule regarding its answers:
*   **"Definitely Not in Set":** If the Bloom filter says an item is not there, it is **100% guaranteed** to not be there. (There are **NO False Negatives**).
*   **"Possibly in Set":** If the Bloom filter says an item *is* there, it *might* be lying. (There **ARE False Positives**).
- ### 2. The "Why": Saving Space for Big Data (Slide 3)
  Why would we use a data structure that might lie to us? **Because memory is expensive.**
  
  Imagine you have a list of 1 billion known malicious URLs. 
  *   If each URL is about 50 bytes, storing them in a standard Hash Set would require over **50 Gigabytes of RAM**.
  *   Using a Bloom Filter, you can represent that exact same set with a 1% false-positive rate using only about **1.2 Gigabytes of RAM**.
- ### 3. Deep Dive into Applications (Slides 3 & 11)
  Your professor provided several excellent, real-world examples. Let's look at exactly *why* the Bloom Filter's "No False Negatives" rule makes it perfect for these.
  
  **A. Databases (HBase, Cassandra, Postgres)**
  *   **The Problem:** Reading from a disk is slow (remember the "Data Movement" chapter!). If a user queries `Select * from Table where id = X`, and `X` doesn't exist, the database wastes a very expensive disk read just to find nothing.
  *   **The Bloom Filter Solution:** The database keeps a tiny Bloom Filter in fast RAM. Before checking the disk, it asks the filter: "Does `id = X` exist?"
    *   If filter says **NO**: The database instantly returns "Not found." (0 disk reads! Massive time savings).
    *   If filter says **YES**: The database goes to the disk. If it's a false positive, the database just does one wasted read, realizes it's not there, and returns "Not found."
  
  **B. Google Chrome Malicious Website Warning**
  *   **The Problem:** Google knows millions of bad URLs. They cannot send a 50MB file to every user's browser every day, nor do they want browsers doing a network request for every single website a user visits.
  *   **The Solution:** Google sends a 1MB Bloom filter to your Chrome browser. When you click a link, Chrome checks the local filter. If it says "NO" (safe), you proceed instantly. If it says "YES" (danger), Chrome pauses and does a network check to Google's servers to see if it was a false positive or a true malicious site.
  
  **C. Medium (Article Recommendations)**
  *   Medium wants to recommend new articles to you, but never an article you've already read. They keep a Bloom Filter of your read history. If the filter says "YES" (you might have read it), they skip recommending it. If it's a false positive, you just miss out on one recommendation—no big deal.
  
  **D. Bitcoin Wallet Synchronization**
  *   "Simplified Payment Verification" (SPV) nodes (like a phone app) don't have the storage to download the entire 500+ GB Bitcoin blockchain. They use Bloom Filters to request only the transactions relevant to their specific wallet addresses from full nodes, preserving their privacy while saving massive bandwidth.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1. The Guarantee**
  A Bloom Filter is queried for the username "charlie_55". The filter returns **False**. 
  What is the probability that "charlie_55" is actually in the database?
  A) 1%
  B) 50%
  C) 0%
  D) 100%
  
  **Q2. False Positives vs. False Negatives**
  Why is a Bloom Filter acceptable for a spell-checker application? 
  *(Hint: Think about what happens if you type a correct word and get a false positive, versus typing a misspelled word and getting a false positive).*
  
  **Q3. Database Disk I/O**
  If a database uses a Bloom Filter to prevent unnecessary disk reads for non-existent records, what happens to the performance if the False Positive rate becomes too high (e.g., 90%)? Does the database return incorrect data to the user?