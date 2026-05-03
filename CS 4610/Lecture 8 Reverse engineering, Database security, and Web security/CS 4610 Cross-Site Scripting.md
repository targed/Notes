## I. What is XSS? (Slides 35–36, 41)
*   **The Definition:** An XSS vulnerability occurs when a web application takes untrusted data from a user and sends it back to a web browser without properly validating or encoding it. 
*   **The Illusion of Trust:** JavaScript is essential for the modern web. Your browser blindly trusts any JavaScript that is delivered by a server you are visiting. If `bank.com` sends a script, the browser executes it with full access to `bank.com`'s cookies and session tokens.
*   **The Attack:** The attacker exploits this trust by injecting their own malicious JavaScript into the vulnerable web server's output. When the victim's browser receives the page, it sees the attacker's script, assumes it was written by `bank.com`, and executes it.
- ## II. The Two Primary Types of XSS (Slides 37–40)
- ### 1. Reflected XSS (Type 1)
  *   **The Mechanism:** The malicious payload is bounced (reflected) off the web server back to the victim's browser. It is usually embedded directly in a crafted URL.
  *   **The Scenario (Slides 38-39):**
    1.  A search page uses PHP to echo whatever you search for: `Results for <?php echo $_GET['term'] ?>`.
    2.  If you search for `apple`, the page prints "Results for apple".
    3.  The attacker crafts a malicious link: `http://victim.com/search.php?term=<script>send_cookie_to_hacker()</script>` and emails it to Alice.
    4.  Alice clicks the link. Her browser sends the script to the server.
    5.  The server blindly echoes the script back in the HTML response.
    6.  Alice's browser reads the response, sees the `<script>` tags, and executes the code, silently stealing her session cookie.
- ### 2. Stored XSS (Type 2)
  *   **The Mechanism:** The malicious payload is permanently saved on the target server's database (e.g., in a forum post, a comment section, or a user profile).
  *   **The Scenario (Slide 40):**
    1.  An attacker writes a malicious JavaScript payload and posts it as a comment on a message board.
    2.  The server stores this comment in its database.
    3.  *Any* user who visits that message board later will automatically download and execute the script when their browser tries to render the comments section.
  *   **The MySpace Samy Worm:** A famous real-world example of Stored XSS. Samy added a malicious script to his MySpace profile. Anyone who viewed his profile unknowingly executed the script, which automatically added Samy as a friend and copied the malicious script to *their* profile. It spread exponentially, infecting millions of users in 24 hours.
- ## III. Defending Against XSS (Slides 43–45)
  Because XSS relies on the browser misinterpreting user input as executable code, the defense relies on strict validation and encoding.
- ### 1. Positive vs. Negative Security Policies (Slide 43)
  *   **Negative Policy (Block-list):** Trying to block bad input (e.g., "Block anything that contains `<script>`"). 
    *   *Why it fails:* Attackers use **Filter Evasion** (Slide 45). If you block `<script>`, they use `<ScRiPt>` (case manipulation), or they use `<img src="x" onerror="malicious_code()">` which executes JavaScript without ever using the word "script".
  *   **Positive Policy (Allow-list):** The **best** way to protect against XSS. You strictly define exactly what is allowed (e.g., "This input field can ONLY contain alphanumeric characters A-Z and 0-9"). Anything else is instantly rejected.
- ### 2. Input Validation (Slide 44)
  *   Never trust client-side data. Check everything (headers, cookies, query strings) on the server.
  *   Beware of alternate encodings. Attackers might send long UTF-8 encodings or hex codes (like `%3C` instead of `<`) to sneak past basic input filters.
- ### 3. Output Filtering / Encoding (Slide 45)
  *   *The Concept:* Before you echo any user-supplied data back to the browser, you must "sanitize" or "encode" it.
  *   *How it works:* You convert HTML special characters into safe HTML entities. 
    *   `<` becomes `&lt;`
    *   `>` becomes `&gt;`
    *   `"` becomes `&quot;`
  *   *The Result:* If an attacker inputs `<script>`, the server transforms it into `&lt;script&gt;`. When the browser receives this, it does *not* execute it. It simply prints the literal text `<script>` safely on the screen.
- ## IV. Browser-Level Mitigation (Slide 46)
  As mentioned in the previous module, relying on **Multiple Browsers** is a drastic but effective defense. 
  *   If you only use Chrome for banking, and Firefox for random web surfing, a Stored XSS attack you accidentally trigger in Firefox cannot steal your banking session, because the banking cookie physically does not exist in Firefox's memory space.
  
  ---
- ### Study Questions for Part 3
  1. **Reflected vs. Stored:** Which type of XSS requires the attacker to actively trick the victim into clicking a specifically crafted, malicious link, and why?
  2. **Filter Evasion:** A web developer writes a security filter that deletes the exact string `<script>` from any user input. Provide an example of an HTML tag an attacker could use to bypass this filter and still execute JavaScript.
  3. **The Ultimate Defense:** How does converting the `<` symbol into the HTML entity `&lt;` completely neutralize an XSS attack before it reaches the victim's browser?
  
  ---
-