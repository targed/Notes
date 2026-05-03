## I. Cookies: The Modern State Mechanism (Slides 25–26)
While hidden form fields and URL parameters require the developer to manually pass the Session ID back and forth, **Cookies** automate the entire process at the browser level.

*   **How Cookies Work:**
  1.  When Alice successfully logs in, the server sends an HTTP response with a special header: `Set-Cookie: session-id=3927a8...; secure`
  2.  Alice's web browser intercepts this header, saves the `session-id` to her hard drive (or memory), and associates it with `deliver-me-pizza.com`.
  3.  From that moment on, *every single time* Alice's browser sends a request to the pizza site (whether it's clicking a link, submitting a form, or loading an image), the browser automatically attaches the header: `Cookie: session-id=3927a8...`.
*   **The "Secure" Flag:** Notice the word `secure` at the end of the `Set-Cookie` command. This is a critical security directive. It tells the web browser: *"Never send this cookie over an unencrypted HTTP connection. Only send it if the connection is HTTPS (SSL/TLS)."* This prevents Eve (the passive eavesdropper) from sniffing the Session ID over public Wi-Fi.
- ### Problems with Cookies (Slide 26)
  Cookies are highly convenient, but they introduce new risks because they are tied to the *browser*, not the *human*.
  *   **Shared Computers:** If Alice forgets to click "Log Out" on a public library computer, her Session ID cookie remains active in the browser. The next person to use that browser will automatically be logged into Alice's account.
  *   **Mitigation:** This is why servers must enforce **Strict Session Timeouts**. A session ID should automatically expire on the server side after a period of inactivity.
  *   **Cross-Site Attacks:** (Alluded to on Slide 29). Because browsers attach cookies *automatically* to every request going to a specific domain, attackers can trick a user's browser into sending forged requests (Cross-Site Request Forgery, CSRF) or use malicious scripts to steal the cookies (Cross-Site Scripting, XSS).
- ## II. JavaScript: The Illusion of Security (Slides 27–28)
  JavaScript (JS) is a programming language that executes entirely on the client's machine (inside their web browser). Developers often use it to make web pages dynamic—such as calculating the total price of an order in real-time as the user changes the quantity.
  
  *   **The Vulnerability:** A developer might write a JS function `computePrice()` that multiplies the quantity by `$5.50` and puts the result into a hidden form field. The developer assumes this is safe because the code automatically calculates the correct price before submission.
  *   **The Attack (Bypassing JS):**
    *   **Method 1:** The attacker simply turns off JavaScript in their browser settings. The calculation never runs, and they can manually edit the hidden HTML field.
    *   **Method 2:** The attacker uses the browser's Developer Tools to rewrite the `computePrice()` function in memory to multiply by `$0.01` instead.
    *   **Method 3:** The attacker ignores the browser entirely and uses `curl` or `wget` (from Slide 11) to send a raw HTTP POST request directly to the server: `qty=1000&price=0`. Because `curl` is not a web browser, it doesn't even know what JavaScript is.
  *   **The Golden Rule Applied:** **Data validation or computations done by JavaScript CANNOT be trusted by the server.** 
    *   *Why use JS at all?* JavaScript validation (like checking if an email has an "@" symbol) is for **User Experience (UX)**—to give the user immediate feedback without waiting for a server reload. It is *never* for security.
    *   *The Fix:* Any calculation or validation done on the client side **must be redone on the server** before the transaction is processed.
- ## III. Module Summary (Slide 29)
  The final slide acts as a checklist for secure web application design regarding state management:
  
  1.  **Understand the Environment:** HTTP is stateless. You *must* maintain state using Hidden Fields, Cookies, or URL parameters.
  2.  **Don't Trust User Input:** Treat everything coming from the client as potentially malicious.
  3.  **Choose Your Trade-off:**
    *   Keep state on the server (Session IDs). *Cost: Space/Memory/DB lookups.*
    *   Keep state on the client with signatures (MACs). *Cost: Bandwidth and CPU processing.*
  4.  **Secure Your Cookies:** Always use the `secure` and `HttpOnly` flags, and be wary of cross-site attacks.
  5.  **Server-Side Authority:** Never rely on JavaScript or hidden HTML fields for critical computations or security validations.
  
  ---
- ### Study Questions for Part 4
  1. **Cookie Mechanics:** If a server sets a cookie without the `secure` flag, and a user connects to the website over public Wi-Fi using plain `http://`, what specific vulnerability does this expose them to?
  2. **JavaScript Misuse:** A website uses JavaScript to check if a user's password meets the complexity requirements (8 characters, 1 number) before allowing them to submit the registration form. If the backend server does not re-verify this, how could an attacker create an account with the password "123"?
  3. **State Management Trade-offs:** If a massive e-commerce company is experiencing Denial of Service outages because their database cannot handle the volume of Session ID lookups, which state management technique from this module should they switch to?
  
  ---