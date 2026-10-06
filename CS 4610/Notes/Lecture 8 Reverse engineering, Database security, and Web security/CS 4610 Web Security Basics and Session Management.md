## I. The Web Browser as an Interpreter (Slides 13–16)
When you type a URL, the browser sends an HTTP `GET` request. The server replies with an HTTP `200 OK` status and a payload of HTML, CSS, and JavaScript. 
*   **The Browser's Job:** A web browser is essentially a massive language interpreter. It takes raw text (HTML/JS) and renders it into the visual interface you interact with. 
*   **The Security Challenge:** A modern browser must be able to safely visit `evil.com` without getting the host machine infected, and it must be able to visit `bank.com` and `evil.com` *at the same time* in different tabs without `evil.com` stealing your banking data.
- ## II. The Bedrock of Web Security: Same Origin Policy (SOP) (Slides 17–20)
  To achieve this safe isolation, browsers enforce the **Same Origin Policy (SOP)**. This is arguably the most important security concept in web development.
- ### 1. The SOP Rule
  SOP dictates that a script loaded from one "Origin" cannot read or modify the data (like cookies or DOM elements) of another "Origin." 
  An **Origin** is defined by exactly three matching components:
  1.  **Protocol** (e.g., `http` vs. `https`)
  2.  **Host/Domain** (e.g., `www.example.com`)
  3.  **Port** (e.g., `:80` for HTTP, `:443` for HTTPS)
- ### 2. SOP Match Examples (Slides 18–19)
  If you are currently on `http://www.example.com/dir/page2.html`:
  *   *Matches:* `http://www.example.com/dir2/other.html` (Only the path changed; Protocol, Host, and Port are identical. Access Granted.)
  *   *Fails:* `https://www.example.com/dir/page2.html` (Protocol differs: HTTPS vs HTTP).
  *   *Fails:* `http://example.com/dir/page2.html` (Host differs: `example.com` is not `www.example.com`).
  *   *Fails:* `http://www.example.com:81/dir/page2.html` (Port differs: 81 vs default 80).
- ### 3. Why SOP is Critical (Slide 20)
  Imagine you are logged into `www.mail.com` in Tab 1. In Tab 2, you browse to `www.evil.com`. `evil.com` runs a JavaScript command: `get_inbox("http://www.mail.com")`. 
  *   Because your browser automatically attaches your mail cookies to requests heading to `mail.com`, the request would succeed!
  *   **However**, when the data comes back, the browser's SOP kicks in. It sees that a script from `evil.com` is trying to read data from `mail.com`. The origins don't match, so the browser blocks the script from seeing the response, protecting your emails.
- ### 4. Extreme Isolation (Slide 21)
  Because zero-day browser bugs sometimes allow attackers to bypass SOP, highly paranoid users use **Multiple Browsers for Security**. For example, strictly using Chrome *only* for banking and Firefox for general browsing. This guarantees physical separation of the cookie jars and session states in the OS memory.
- ## III. Session Management (Slides 24–26)
  Because HTTP is "Stateless" (it forgets who you are after every request), we use **Sessions** to string multiple requests together into a single authenticated login.
  
  *   **The Mechanism:**
    1. You visit a site. The server gives you an "Anonymous Session Token."
    2. You submit a `POST` request with your username and password.
    3. The server verifies your credentials and **elevates** your token to a "Logged-in Session Token."
    4. Your browser sends this token with every subsequent request, proving you are authenticated.
- ## IV. Attacking Sessions (Slides 27–33)
  If an attacker can get your token, they *become* you. They don't need your password. There are three main ways this happens:
- ### 1. Predictable Tokens (Slide 28)
  *   *The Flaw:* The developer writes their own custom session ID generator, perhaps using a simple counter (User 1 gets Token `1001`, User 2 gets `1002`) or a weak hash of the username.
  *   *The Exploit:* The attacker simply guesses the next token in the sequence to hijack another user's session.
  *   *The Fix:* Always use the cryptographically secure, built-in session generators provided by modern frameworks (Tomcat, ASP, PHP).
- ### 2. Cookie Theft (Slide 29)
  *   *The Flaw:* A website uses HTTPS (SSL) for the login page, but then drops you back to regular HTTP for browsing the rest of the site to save server CPU power.
  *   *The Exploit:* Because the subsequent pages are on HTTP, your browser sends the Session Cookie in plaintext. An attacker on the same public Wi-Fi (e.g., using a packet sniffer) reads your cookie out of the air and hijacks your session.
  *   *The Fix:* Force HTTPS everywhere. Ensure cookies are invalidated on the server when the user clicks "Logout."
- ### 3. Session Fixation (Slides 30–32)
  This is a highly testable, tricky concept. Instead of *stealing* your token, the attacker *forces you to use a token they already know*.
  *   *The Attack:*
    1. The attacker visits `bank.com` and receives an anonymous session token: `12345`.
    2. The attacker sends Alice a phishing email: "Check your balance! `http://bank.com?token=12345`".
    3. Alice clicks the link. Her browser goes to `bank.com` using the attacker's token (`12345`).
    4. Alice types in her username and password. 
    5. The buggy server says "Great, password is correct. I will now elevate token `12345` to be logged-in as Alice."
    6. The attacker, who already has token `12345` saved on their computer, refreshes their page and is now logged into Alice's bank account.
  *   *The Fix (Slide 32):* **Always issue a brand new session token at the exact moment of login.** Never elevate a pre-existing anonymous token. If the server throws away `12345` and gives Alice a new token `99999` upon login, the attacker's saved token remains useless.
  
  ---
- ### Study Questions for Part 2
  1. **SOP Evaluation:** An attacker hosts a script at `http://admin.example.com`. Can this script use JavaScript to read cookies belonging to `http://www.example.com`? Why or why not based on the Same Origin Policy?
  2. **Session Hijacking:** Why does implementing a "Logout" button that merely deletes the cookie from the user's browser fail to protect against a session hijacking attack if an attacker already sniffed the cookie 5 minutes ago? (Hint: See Slide 29 regarding server-side invalidation).
  3. **Session Fixation:** In a Session Fixation attack, whose token is actually elevated to "Logged-In" status: a token originally generated for the victim, or a token originally generated for the attacker? How does a secure server prevent this?
  
  ---
-