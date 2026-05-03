## I. The Problem with Reusable Passwords (Slide 25)
If an attacker is eavesdropping on the network (sniffing) or has installed a keylogger (like a Trojan) on your machine, it does not matter how strong your password is, how many times it is hashed, or if it uses a 64-bit salt. The attacker will steal the password as you type it or transmit it, and then reuse it later.

*   **The Conceptual Fix:** "Disposable" or One-Time Passwords (OTPs). You use a password exactly once. If Eve sniffs it, it is useless to her because it will never be valid again.
*   **The Practical Problem:** Human memory. You cannot ask a user to memorize 1,000 different, unique, random passwords and keep track of which ones they have already used.
*   **The Mathematical Solution: The S/Key Protocol.** This uses a mathematical trick (Hash Chains) to generate a sequence of OTPs from a single, memorable password.
- ## II. The S/Key Protocol Mechanics (Slides 26–28)
  S/Key (created by Bellcore) relies heavily on the property of a **One-Way Hash Function** (like MD5 or SHA-1): It is easy to compute $H(x)$, but computationally infeasible to take the result and reverse it back to $x$.
- ### 1. Registration / Generation (Slides 26-27)
  1.  Alice chooses a single, secret seed password ($x$).
  2.  Alice decides she wants $n$ disposable passwords (e.g., $n = 100$).
  3.  Alice's local computer hashes the password $100$ times in a chain:
    *   $x_1 = H(x)$
    *   $x_2 = H(x_1)$
    *   ...
    *   $x_{100} = H(x_{99})$
  4.  Alice securely transmits **only the final hash** ($x_{100}$) to the server. The server saves this as her verifier.
- ### 2. Logging In (Working Backwards) (Slide 28)
  When Alice logs in for the very first time, the server challenges her for password `#99`.
  1.  Alice's computer calculates the chain up to 99 and sends $x_{99}$ to the server.
  2.  The server takes $x_{99}$ and hashes it *one single time*.
  3.  `If H(x_99) == x_100`, the server knows Alice is authentic, because only the person with the original secret $x$ could have generated $x_{99}$.
  4.  The server deletes $x_{100}$, saves $x_{99}$ as the new verifier, and logs Alice in.
  5.  *The next time she logs in*, she will send $x_{98}$, and the server will verify it against $x_{99}$.
- ### 3. Defeating the Eavesdropper
  If Eve intercepts $x_{99}$ as it flies across the network, it does her no good. To log in as Alice tomorrow, Eve would need $x_{98}$. Because hashing is a *one-way function*, Eve cannot take $x_{99}$ and reverse-engineer it to figure out $x_{98}$.
- ## III. Limitations of S/Key (Slide 29)
  While brilliant, S/Key has severe limitations in modern security architectures:
  1.  **Exhaustion:** Because $n$ is finite, Alice will eventually run out of passwords (she reaches $x_1$). She must then securely set up a brand new chain with the server.
  2.  **Lack of Mutual Authentication:** S/Key proves Alice's identity to the server, but it **does not authenticate the server to Alice!**
    *   *The Man-in-the-Middle (MITM) Attack:* An attacker sets up a fake Wi-Fi portal that looks like Alice's bank. The real bank asks for password $i$. The fake server asks Alice for password $i$. Alice hands $x_{i-1}$ to the fake server. The fake server instantly turns around and hands $x_{i-1}$ to the real bank, successfully logging into Alice's account.
- ## IV. Biometrics: "Something You Are" (Slides 30–33)
  Instead of relying on human memory (passwords) or physical tokens that can be stolen, Biometrics rely on the physical characteristics of the user.
- ### 1. Desired Properties of a Biometric (Slide 30)
  No biometric is perfect. A perfect biometric would hit all 6 of these marks:
  1.  **Uniquely Identifying (Uniqueness):** No two people on Earth have the same trait.
  2.  **Difficult to Forge / Circumvent:** A hacker cannot fake it.
  3.  **Highly Accurate:** Low False Positives (accepting an imposter) and low False Negatives (rejecting the real user).
  4.  **Easy to Scan (Collectability):** Can be read quickly without making the user uncomfortable.
  5.  **Fast to Measure/Compare (Performance):** The math to verify it doesn't take 10 minutes.
  6.  **Inexpensive:** The hardware (like a camera or scanner) is cheap to produce.
- ### 2. The Trade-offs (Slides 32–33)
  Every biometric compromises on at least one of these properties:
  *   **DNA:** Perfect uniqueness and nearly impossible to forge, but *terrible* collectability and performance (you can't draw blood and run a 2-hour lab test every time someone unlocks their phone).
  *   **Face Recognition:** Highly acceptable and easy to collect, but traditionally poor uniqueness (twins) and high circumvention (holding up a photograph or a 3D mask).
  *   **Fingerprints:** High uniqueness and permanence, but medium circumvention (hackers can lift your fingerprint from a glass and make a gummy replica).
  *   **The Ultimate Flaw:** *Privacy and Revocation.* You leave your fingerprints everywhere you go. If a database containing your biometric hash is compromised, you cannot change your face or your retinas like you can change a password.
  
  ---
- ### Study Questions for Part 4
  1. **S/Key Mechanics:** In the S/Key protocol, if the server currently holds $x_{50}$ as the verifier, which specific hash value must Alice transmit over the network to log in successfully?
  2. **One-Way Functions:** Explain why an attacker who intercepts Alice's S/Key password $x_{50}$ cannot use it to log into her account the next day.
  3. **Biometric Trade-offs:** Compare a **Retina Scan** to a **Signature (Penmanship)** using the "Collectability" and "Circumvention" properties from Slide 33. Which is harder to forge, and which is easier to collect?
  
  ---