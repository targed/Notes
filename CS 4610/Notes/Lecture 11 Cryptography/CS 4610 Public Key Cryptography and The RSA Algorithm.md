## I. Public Key (Asymmetric) Cryptography (Slides 10–13)
Invented in the 1970s, this system revolutionized security by splitting the "Key" into two mathematically linked parts. 
*   **The Key Pair:** Every user generates a pair of keys:
  1.  **Public Key:** Published to the world (like a public phone number).
  2.  **Private Key:** Kept absolutely secret by the owner.
*   **The Golden Rule:** What one key encrypts, *only* the other key can decrypt. You cannot derive the Private Key from the Public Key.
*   **The Trade-off:** Asymmetric cryptography involves heavy mathematics (exponentiation with massive numbers). It is *much slower* than secret key (symmetric) cryptography. Therefore, we usually don't use it to encrypt large files; we use it to securely establish a smaller symmetric key.
- ### The Four Major Applications (Slides 11–13)
  The way you use the keys depends on your security goal:
  
  1.  **Communicating Securely (Confidentiality):**
    *   *Goal:* Alice wants to send a secret message to Bob.
    *   *Process:* Alice encrypts the plaintext using **Bob's Public Key**. 
    *   *Result:* Since the message was locked with Bob's public key, only **Bob's Private Key** can unlock it. Even Alice cannot decrypt the message once she encrypts it!
  2.  **Secure Storage:** 
    *   *Goal:* Encrypting your own files in the cloud. You encrypt data with your Public Key; you retrieve and decrypt it later with your Private Key.
  3.  **User Authentication:**
    *   *Goal:* Bob needs to prove to Alice that he is actually Bob.
    *   *Process:* Alice sends a challenge. Bob encrypts the challenge using **his Private Key**. Alice decrypts it using **Bob's Public Key**. Because it successfully decrypted, Alice knows absolutely that the person holding Bob's private key sent it.
  4.  **Digital Signatures (Non-Repudiation):**
    *   *Goal:* Proving a document wasn't forged.
    *   *Process:* Alice "Signs" a document using **her Private Key**. Anyone in the world can "Verify" it using **her Public Key**. Because only Alice has her private key, she cannot deny signing it.
- ## II. Mathematical Foundations for RSA (Slides 15–16)
  To understand how the keys are mathematically linked, we must review Number Theory.
  
  *   **Prime Numbers:** Numbers only divisible by 1 and themselves (e.g., 2, 3, 5, 7, 11).
  *   **Greatest Common Divisor (GCD):** The largest number that divides both $a$ and $b$ evenly. Example: $gcd(60, 24) = 12$.
  *   **Relatively Prime (Coprime):** Two numbers are relatively prime if they share NO common factors other than 1. (Their GCD = 1). Example: 8 and 15. Neither is a prime number, but they are relatively prime to *each other*.
  *   **Modular Arithmetic ($mod$):** The "remainder" of division. 
    *   Example: $13 \pmod 5 = 3$ (Because $13 / 5 = 2$ with a remainder of $3$).
  *   **Multiplicative Inverse:** 
    *   In normal math, the inverse of $3$ is $1/3$ (because $3 \times 1/3 = 1$). 
    *   In *modular* math, $z$ is the multiplicative inverse of $m \pmod n$ if: $(m \times z) \pmod n = 1$.
    *   *Example (Slide 16):* The inverse of $12 \pmod{35}$ is $3$. Why? Because $12 \times 3 = 36$. And $36 \pmod{35} = 1$.
    *   *Crucial Rule:* A multiplicative inverse only exists if $m$ and $n$ are relatively prime.
- ## III. The RSA Algorithm (Slides 14, 17–20)
  Created by **R**ivest, **S**hamir, and **A**dleman, RSA is the most popular public-key cryptosystem in the world. 
  *   **The Foundation (The "Trapdoor"):** RSA is based on the mathematical fact that it is very easy to multiply two massive prime numbers together, but it is computationally infeasible for modern computers to take the result and factor it back into the original primes.
- ### 1. Generating the Keys (Slide 17 & 19)
  Here is the exact step-by-step math to generate an RSA key pair, using the example from Slide 19:
  
  1.  **Choose two primes ($p, q$):** $p = 23$, $q = 11$.
  2.  **Compute the Modulus ($n$):** $n = p \times q \rightarrow 23 \times 11 = \mathbf{253}$. *(This is the public trapdoor. We publish $253$, but keep $23$ and $11$ secret).*
  3.  **Compute Euler's Totient ($\phi(n)$):** $\phi(n) = (p-1) \times (q-1) \rightarrow 22 \times 10 = \mathbf{220}$. 
  4.  **Choose the Public Exponent ($e$):** Choose an $e$ that is relatively prime to $\phi(n)$. Let's pick $\mathbf{39}$.
    *   **Public Key:** $(e, n) = \mathbf{(39, 253)}$
  5.  **Calculate the Private Exponent ($d$):** Find the multiplicative inverse of $e \pmod{\phi(n)}$. 
    *   We need $d$ such that $(39 \times d) \pmod{220} = 1$. 
    *   $d = \mathbf{79}$ (because $39 \times 79 = 3081$, and $3081 \pmod{220} = 1$).
    *   **Private Key:** $(d, n) = \mathbf{(79, 253)}$
- ### 2. RSA Operations: Encrypting and Signing (Slides 18 & 20)
  Now that we have the keys, we use modular exponentiation to scramble the data.
  
  **Encryption (Alice sends a secret to Bob):**
  *   *Rule:* Plaintext block size must be strictly smaller than $n$.
  *   *Message ($m$):* 80
  *   *Formula:* $Ciphertext (c) = m^e \pmod n$
  *   *Math:* $80^{39} \pmod{253} = \mathbf{37}$. (The encrypted message is 37).
  
  **Decryption (Bob reads the secret):**
  *   *Formula:* $m = c^d \pmod n$
  *   *Math:* $37^{79} \pmod{253} = \mathbf{80}$. (Bob perfectly recovers the message!).
  
  **Digital Signatures (Bob proves he sent a message):**
  *   *To Sign:* Bob uses his *Private Key* ($d$). $Signature (s) = m^d \pmod n$.
    *   $80^{79} \pmod{253} = \mathbf{224}$.
  *   *To Verify:* Alice uses Bob's *Public Key* ($e$). $m = s^e \pmod n$.
    *   $224^{39} \pmod{253} = \mathbf{80}$. The math checks out; Bob definitely signed it.
  
  ---
- ### Study Questions for Part 2
  1. **Public Key Concepts:** If Alice wants to send a highly confidential email to Bob, but she also wants Bob to know with 100% certainty that the email actually came from her, what must Alice do to the message before sending it? (Hint: Which keys are used in what order?)
  2. **Number Theory:** Why is the number $\mathbf{7}$ considered relatively prime to $\mathbf{20}$, even though $20$ is not a prime number?
  3. **RSA Math:** Given $p = 5$ and $q = 11$, what is the value of the modulus $n$ and the totient $\phi(n)$?
  
  ---