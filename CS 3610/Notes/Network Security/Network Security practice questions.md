# Practice Questions: Multiple Choice
- **Question 1**
- Alice wants to send a large, confidential document to Bob using the standard hybrid approach (session keys). Which of the following correctly describes the encryption process Alice will perform?
  * [ ] She encrypts the entire document with her own private key.
  * [ ] She encrypts the entire document with Bob's public key.
  * [x] She encrypts the document with a new symmetric session key, and then encrypts that session key with Bob's public key.
  * [ ] She encrypts a hash of the document with a symmetric session key.
- **Question 2**
- To create a digital signature for a message, what does a sender (Bob) do?
  * [ ] He encrypts the entire message with his private key.
  * [ ] He encrypts a hash of the message with the receiver's (Alice's) public key.
  * [ ] He computes a hash of the message and appends it to the message in plaintext.
  * [x] He computes a hash of the message and encrypts the resulting hash with his own private key.
- **Question 3**
- An attacker, Trudy, records a user's encrypted login credentials (username and encrypted password) and then re-sends them to the server at a later time to gain access. This is a playback attack. What is the most effective defense against this specific attack?
  * [ ] Using a stronger encryption algorithm like AES-256.
  * [x] Using a challenge-response protocol where the server sends a unique, one-time number (a nonce) that must be included in the encrypted response.
  * [ ] Requiring the user to have a longer password.
  * [ ] Encrypting the login credentials with the server's public key.
- **Question 4**
- What is the primary purpose of a digital certificate issued by a Certification Authority (CA)?
  * [ ] To guarantee that a website's content is free of viruses.
  * [ ] To provide confidentiality by encrypting all data sent to and from a server.
  * [x] To bind a public key to a specific identity (like a website's domain name), which allows a user to verify that the public key is authentic and not a fake one from a man-in-the-middle.
  * [ ] To serve as a temporary symmetric session key for a TLS connection.
- **Question 5**
- Which of the following are primary goals of the TLS Handshake protocol? (Check all that apply).
  * [x] To allow the client to authenticate the server.
  * [ ] To determine the fastest network route between the client and server.
  * [x] To securely generate or exchange a shared symmetric key (the master secret) for the session.
  * [ ] To compress the application data before it is sent.
- **Question 6**
- Public key cryptography (like RSA) is significantly slower than symmetric key cryptography (like AES). What is the main reason public key cryptography is used at all in protocols like TLS?
  * [ ] It provides better message integrity than symmetric key crypto.
  * [ ] It can encrypt much larger amounts of data than symmetric key crypto.
  * [ ] Its security is based on a more complex mathematical problem.
  * [x] It provides a mechanism to securely establish a shared secret key over an insecure channel without prior arrangement (solving the key distribution problem).
- **Question 7**
- Which of the following is a required property of a cryptographic hash function (e.g., SHA-256) used for creating digital signatures?
  * [ ] The function must be reversible, allowing the original message to be recovered from the hash.
  * [ ] The hash output size must be the same as the input message size.
  * [x] It must be computationally infeasible to find two different messages that produce the same hash output (collision resistance).
  * [ ] The function must use a secret key shared between the sender and receiver.
- **Question 8**
- Alice wants to send an email to Bob that provides both confidentiality (only Bob can read it) and a digital signature (Bob can verify it's from Alice and unaltered). Which combination of keys must Alice use to achieve this?
  * [ ] She encrypts the message with her public key and signs it with Bob's private key.
  * [ ] She encrypts the message with a session key and signs it with Bob's public key.
  * [x] She signs the message with her private key, and then encrypts the message and signature using a session key that has been encrypted with Bob's public key.
  * [ ] She encrypts the message with Bob's private key and signs it with her public key.
- **Question 9**
- True or False: The security of the RSA public key algorithm is based on the mathematical difficulty of factoring very large numbers.
  * [x] True
  * [ ] False
- **Question 10**
- In the TLS 1.3 1-RTT handshake, how is the latency reduced compared to older versions?
  * [ ] By using UDP instead of TCP for the handshake.
  * [ ] By eliminating the need for the server to send a certificate.
  * [x] The client "guesses" which key exchange algorithm the server will use and sends its key exchange parameters in the very first "Client Hello" message.
  * [ ] The handshake is performed entirely by network routers instead of the end hosts.
- **Question 11**
- In a "man-in-the-middle" attack against a public key authentication protocol, how does the attacker (Trudy) deceive both Alice and Bob?
  * [ ] Trudy guesses Alice's private key through a brute-force attack.
  * [ ] Trudy floods Bob's server, forcing it to accept any connection as authentic.
  * [x] Trudy intercepts Alice's public key, substitutes her own, and relays messages, posing as Alice to Bob and as Bob to Alice.
  * [ ] Trudy modifies the nonce sent by Bob, causing the authentication to fail for Alice.
- **Question 12**
- What is the primary reason protocols like TLS use a hybrid approach, employing both asymmetric (public key) and symmetric key cryptography?
  * [ ] Symmetric key crypto is a newer standard and not all clients support it.
  * [ ] Asymmetric key crypto provides message integrity, which symmetric key crypto cannot.
  * [x] Asymmetric key crypto is computationally slow, so it is used to securely exchange a fast symmetric session key which is then used for bulk data encryption.
  * [ ] Symmetric keys are easier for network administrators to distribute manually.
- **Question 13**
- A Message Authentication Code (MAC) is created by hashing a message along with a secret key shared between the sender and receiver. What two security properties does a MAC provide?
  * [ ] Confidentiality and Non-repudiation.
  * [ ] Confidentiality and Integrity.
  * [x] Authentication and Integrity.
  * [ ] Non-repudiation and Availability.
- **Question 14**
- The property of non-repudiation ensures that a sender cannot falsely deny having sent a message. Which cryptographic technique is essential for providing this guarantee?
  * [ ] Encryption with a symmetric session key.
  * [ ] A cryptographic hash function by itself.
  * [ ] A Message Authentication Code (MAC).
  * [x] A digital signature created with the sender's unique private key.
- **Question 15**
- When you visit a secure website (https://...) and your browser establishes a secure connection, what is the fundamental purpose of the website's digital certificate?
  * [ ] It guarantees the website's content is accurate and trustworthy.
  * [ ] It acts as the symmetric session key used for encrypting the data.
  * [x] It allows your browser to verify that the public key it received actually belongs to the website it is trying to connect to.
  * [ ] It proves that the website has a fast connection to the internet.
- **Question 16**
- In the RSA algorithm, the public key is (n, e) and the private key is (n, d). Which of the following statements is true about these components?
  * [ ] n is a large prime number, and e and d are its factors.
  * [x] n is the product of two large prime numbers, and it is computationally difficult for an attacker to find those factors.
  * [ ] d is made public, while e is kept private.
  * [ ] The security of RSA relies on the secrecy of the value n.
- **Question 17**
- An attacker intercepts an encrypted message but is unable to decrypt it. However, they are able to change several bits in the ciphertext before forwarding it to the receiver. When the receiver decrypts the message, the content is now meaningless garbage. Which security property was violated?
  * [ ] Confidentiality
  * [ ] Authentication
  * [x] Message Integrity
  * [ ] Availability
- **Question 18**
- True or False: The 0-RTT (Zero Round-Trip Time) feature in TLS 1.3, while fast, is vulnerable to playback attacks and therefore should only be used for application requests that do not change the state of the server (e.g., an HTTP GET).
  * [x] True
  * [ ] False
- **Question 19**
- In the context of authentication protocols, what is a "nonce"?
  * [ ] A short, memorable password.
  * [ ] The public key of the sender.
  * [x] A random, one-time-use number used to prevent playback attacks.
  * [ ] The result of a cryptographic hash function.
- **Question 20**
- What is the primary reason that digital signature schemes (like those used for email) operate on a cryptographic hash of the message, rather than signing the entire message directly?
  * [ ] Hashing the message adds the necessary confidentiality.
  * [x] Public key operations (signing) are computationally slow, so signing a short, fixed-length hash is much more efficient than signing a large, variable-length message.
  * [ ] A hash is required to make the signature legally non-repudiable.
  * [ ] The entire message is often too large to be encrypted with a public key.
- **Question 21**
- Both a Message Authentication Code (MAC) and a digital signature provide message integrity and authentication. What is the key security property that a digital signature provides, which a MAC does not?
  * [ ] Confidentiality
  * [x] Non-repudiation
  * [ ] Availability
  * [ ] Protection against playback attacks
- **Question 22**
- The RSA algorithm has the property that K+(K-(m)) = m, meaning a message encrypted with a private key can be decrypted by the corresponding public key. This property is the fundamental basis for which security service?
  * [ ] Confidentiality using a session key.
  * [x] Digital signatures.
  * [ ] The Diffie-Hellman key exchange.
  * [ ] A challenge-response protocol using a nonce.
- **Question 23**
- Once a TLS handshake is complete and a set of symmetric session keys has been established, how is the application data (e.g., an HTTP message) protected during the data transfer phase?
  * [ ] The data is sent in plaintext, but over a TCP connection that is managed by the server's private key.
  * [ ] The entire HTTP message is encrypted once with the server's public key.
  * [x] The data is broken into records, a MAC is calculated for each record, and then the record and its MAC are encrypted using the symmetric session keys.
  * [ ] The data is sent in plaintext, but each packet header is signed by the server.
- **Question 24**
- What is considered the most significant operational challenge of using symmetric key cryptography (like AES) on its own in a large-scale network like the internet?
  * [ ] Symmetric key algorithms are too slow for modern network speeds.
  * [ ] The key lengths are too short to be considered secure.
  * [ ] It does not provide message integrity.
  * [x] The difficulty of securely distributing and managing the shared secret key between parties who have never communicated before.
- **Question 25**
- A user receives an email that appears to be from their bank, asking them to click a link and enter their password to "verify their account." The link, however, leads to a fraudulent website run by an attacker. This is an example of what kind of attack?
  * [ ] A Denial of Service (DoS) attack.
  * [x] Impersonation (specifically, a phishing attack).
  * [ ] A man-in-the-middle attack on a TLS session.
  * [ ] A playback attack.
- **Question 26**
- What would be the most severe consequence if the private key of a trusted root Certification Authority (CA) were compromised and stolen by an attacker?
  * [ ] The attacker could decrypt past TLS sessions that were recorded.
  * [x] The attacker could sign fraudulent certificates for any website (e.g., google.com), allowing them to perform undetectable man-in-the-middle attacks.
  * [ ] All websites would have to switch from RSA to AES encryption.
  * [ ] The attacker could shut down the DNS root servers.
- **Question 27**
- The simple Internet Checksum is a poor choice for a cryptographic hash function (like one needed for digital signatures) primarily because:
  * [ ] It produces a variable-length output.
  * [x] It is not collision-resistant; it is computationally easy to find two different messages that produce the same checksum.
  * [ ] It is too slow to compute on modern hardware.
  * [ ] It requires a secret key to compute.
- **Question 28**
- In an authentication protocol, what is the specific purpose of the "challenge" (e.g., sending a nonce)?
  * [ ] To verify that the other party has a valid digital certificate.
  * [ ] To establish a symmetric session key for the communication.
  * [x] To force the other party to perform a cryptographic operation on fresh, unpredictable data, thus proving they are "live" and not a pre-recorded playback.
  * [ ] To check the integrity of the communication channel.
- **Question 29**
- True or False: Using a 256-bit key with the AES algorithm provides greater security against a brute-force attack than using a 128-bit key.
  * [x] True
  * [ ] False
- **Question 30**
- In the TLS handshake, the client and server negotiate a "cipher suite." What does a cipher suite define?
  * [ ] Only the version of TLS that will be used (e.g., 1.2 or 1.3).
  * [x] The specific combination of algorithms to be used for key exchange, bulk encryption, and message authentication (MAC).
  * [ ] The public key of the server.
  * [ ] The maximum amount of data that can be sent in a single TLS record.
- **Question 31**
- Alice wants to post a public announcement online (e.g., on a blog). She is not concerned with confidentiality, but she wants readers to be certain that the announcement came from her and has not been modified. What cryptographic action should she take?
  * [ ] Encrypt the entire announcement with her public key.
  * [ ] Encrypt the entire announcement with a well-known symmetric key.
  * [x] Attach a digital signature to the announcement, created by encrypting a hash of the announcement with her private key.
  * [ ] Encrypt the announcement using the public key of the blog's web server.
- **Question 32**
- Your browser displays a security warning stating that a website's digital certificate has expired. What is the most significant security risk if you choose to proceed to the website?
  * [ ] The data you send will not be encrypted.
  * [ ] The website will load much more slowly than usual.
  * [ ] The Certification Authority (CA) that issued the certificate has definitely been compromised.
  * [x] You can no longer trust that the public key belongs to the website, making you vulnerable to a man-in-the-middle attack.
- **Question 33**
- In the context of symmetric key ciphers, what is the fundamental difference between a block cipher (like AES) and a stream cipher?
  * [ ] Block ciphers use public keys, while stream ciphers use symmetric keys.
  * [ ] Block ciphers are much faster than stream ciphers.
  * [x] Block ciphers operate on fixed-size chunks of data at a time, while stream ciphers encrypt data one bit or one byte at a time.
  * [ ] Block ciphers provide message integrity, while stream ciphers do not.
- **Question 34**
- An attacker, Trudy, captures a packet from Alice containing an encrypted request. Trudy cannot decrypt it. Instead, she floods the server with millions of copies of this single captured packet, forcing the server to perform a cryptographic check for each one. This action consumes all of the server's CPU. What type of attack is this?
  * [ ] A man-in-the-middle attack.
  * [x] A Denial of Service (DoS) attack.
  * [ ] An eavesdropping attack.
  * [ ] An IP spoofing attack.
- **Question 35**
- A user (Alice) wants to prove her identity to a server (Bob) by signing a nonce. Which key does Alice use to create the signature, and which key does Bob use to verify it?
  * [ ] Alice uses her public key; Bob uses his private key.
  * [ ] Alice uses Bob's public key; Bob uses his private key.
  * [x] Alice uses her private key; Bob uses her public key.
  * [ ] Alice and Bob both use a pre-shared symmetric key.
- **Question 36**
- A receiver, Bob, gets a digitally signed message from Alice. He calculates the hash of the received message and finds that it does not match the hash obtained by decrypting the signature. What can Bob conclude with certainty?
  * [ ] Alice's private key has been stolen.
  * [ ] The network connection is experiencing high latency.
  * [x] The message lacks integrity and/or is not authentically from Alice.
  * [ ] Bob must be using the wrong hash function.
- **Question 37**
- What is the historical and technical relationship between TLS (Transport Layer Security) and SSL (Secure Sockets Layer)?
  * [ ] They are two competing standards developed by different organizations.
  * [ ] SSL is an enhancement to TLS that adds better support for UDP.
  * [ ] SSL is used for email security, while TLS is used for web security.
  * [x] TLS is the standardized successor protocol to SSL, which is now considered deprecated and insecure.
- **Question 38**
- If the only security requirement for a communication is confidentiality, what is the minimum cryptographic operation the sender (Alice) must perform?
  * [ ] Send the message in plaintext.
  * [ ] Encrypt the message with her own private key.
  * [ ] Compute a hash of the message and send it along with the message.
  * [x] Encrypt the message with the receiver's (Bob's) public key.
- **Question 39**
- True or False: A digital certificate issued by a Certification Authority (CA) contains the entity's name, their private key, and the CA's signature.
  * [ ] True
  * [x] False (A certificate contains the public key, never the private key.)
- **Question 40**
- A standard DNS query and response are sent in plaintext over UDP. Which of the following security properties are inherently missing from this exchange?
  * [ ] Confidentiality
  * [ ] Message Integrity
  * [ ] Authentication of the DNS server
  * [x] All of the above
# Practice Questions: Math
#### **Question 1: RSA Public Key Cryptography**
  
  Suppose Alice wants to set up an RSA public/private key pair. She chooses the prime numbers p = 7 and q = 13.
  
  1. **(4 points)** Alice computes her modulus n and the value z. She then selects e = 5 as her public exponent. Show the calculation for n and z, and explain why e = 5 is a valid choice.  
  2. **(4 points)** Alice must now calculate her private exponent d. Show the equation that d must satisfy and find the value of d.  
  3. **(2 points)** Bob wants to send the plaintext message m = 10 to Alice. What is the ciphertext c that Bob sends? Show the calculation.
  
  ---
#### **Question 2: Internet Checksum Calculation**
  
  A sender needs to compute the 1's complement checksum for a payload that consists of two 16-bit words. The words are given in hexadecimal:
  
  * Word 1: 0xABCD  
  * Word 2: 0x4567  
  1. **(5 points)** Show the 1's complement binary addition of these two words. Be sure to show the sum and indicate if a wraparound carry occurs and how it is handled.  
  2. **(3 points)** What is the final 16-bit checksum value that will be placed in the header? (Remember, the checksum is the 1's complement of the sum).  
  3. **(2 points)** If a receiver gets a packet containing these two words and the checksum you just calculated, what will be the result when the receiver sums all three 16-bit values (the two words and the checksum)?
  
  ---
#### **Question 3: TLS Handshake Latency**
  
  A client wants to establish a secure connection to a server to fetch a small web object. The one-way propagation delay between the client and server is 50 ms. Assume that transmission time and processing delays are negligible.
  
  Consider two different TLS handshake protocols:
  
  * **Protocol A (Legacy):** Requires 2 full round-trips for the TLS handshake after the initial TCP handshake is complete. The client sends its HTTP GET request on the next trip.  
  * **Protocol B (TLS 1.3):** Requires only 1 full round-trip for the TLS handshake after the initial TCP handshake. The client sends its HTTP GET request along with its final handshake message.  
  1. **(4 points)** Calculate the total time elapsed from when the client sends the first TCP SYN packet until it receives the first byte of the requested web object for Protocol A.  
  2. **(4 points)** Calculate the total time elapsed from when the client sends the first TCP SYN packet until it receives the first byte of the requested web object for Protocol B.  
  3. **(2 points)** What is the total time saved by using the more modern TLS 1.3 handshake in this scenario?
  
  ---
#### **Question 4: Digital Signature Verification Process**
  
  Alice receives a message and a digital signature that are supposedly from Bob. She needs to verify its authenticity and integrity.
  
  * The plaintext message m she receives is: "network"  
  * She knows Bob's public key is (n=55, e=7).  
  * The digital signature S she receives is 10.  
  * She uses a hash function H. Assume that for her received message, she calculates H("network") = 15.
  
  List the specific steps Alice must take to verify the signature, showing the key calculation and the final comparison. State her final conclusion.
  
  ---
#### **Question 5: Performance of Encryption Methods**
  
  A user wants to securely send a 40 MB file. The network has a throughput of 100 Mbps. You are given the following performance characteristics for the user's computer:
  
  * Symmetric Encryption Speed (AES): 80 MB/s (Megabytes per second)  
  * Asymmetric Encryption Speed (RSA): 2 MB/s  
  * Time for the initial public key exchange to establish a session key: 200 ms  
  1. **(3 points)** Calculate the total time it would take to encrypt and transmit the file using only symmetric encryption (assuming a key was already magically pre-shared).  
  2. **(3 points)** Calculate the total time it would take to encrypt and transmit the file using only asymmetric encryption.  
  3. **(4 points)** Calculate the total time it would take using the standard hybrid approach (asymmetric to exchange a key, then symmetric for the data). Based on your results, explain in one sentence why the hybrid approach is used.