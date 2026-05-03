## I. HTTP Methods: GET vs. POST (Slide 17)
When a web browser communicates with a server, it uses specific "methods" (commands). The two most common are GET and POST.
*   **GET:** Designed for *requesting* or retrieving data (e.g., loading a webpage or a picture). 
  *   *Mechanism:* Any parameters (data) sent via GET are appended directly to the end of the URL in the address bar, visible to everyone.
  *   *Format:* `http://website.com/page?parameter=value`
*   **POST:** Designed for *sending* data to create or update a resource (e.g., submitting a login form or uploading a file).
  *   *Mechanism:* Parameters are hidden inside the "Body" of the HTTP request. The URL remains clean.
- ## II. Security Vulnerabilities of GET (Slides 18–21)
  Using GET to transmit sensitive state information (like a Session ID or a password) is highly insecure because URLs are heavily exposed.
  
  **The "Copy-Paste" Leak (User Error):**
  *   **The Scenario:** Alice orders a pizza. The form uses the `GET` method. When she submits, the browser puts her Session ID directly into the address bar: 
    `https://www.deliver-me-pizza.com/confirm_order?session-id=3927a8...`
  *   **The Mistake:** Alice wants to ask her friend Meg if the order looks good. She copies the URL from her address bar and emails it to Meg. 
  *   **The Exploit:** When Meg clicks the link, her browser sends that exact Session ID to the server. The server assumes Meg is Alice. Meg has just unintentionally hijacked Alice's session and can modify or place the order under Alice's account/credit card.
- ## III. The HTTP Referer Header Leak (Slides 22–23)
  Even if Alice never copy-pastes the URL, the browser itself might leak the Session ID automatically without any user interaction due to the **HTTP Referer Header** *(Note: "Referer" is famously misspelled in the official HTTP specification, but it is the standard term).*
  
  *   **What is the Referer Header?** Whenever you click a link to leave a website, your browser automatically tells the *new* website exactly where you came from. It's used for web analytics (e.g., a blog knowing it gets traffic from Google).
  *   **The Attack Scenario:**
    1.  Alice is on her checkout page. Her URL is polluted with her Session ID: `.../confirm_order?session-id=3927a8...`
    2.  The pizza site has an advertisement link to a partner site: `<a href="http://grocery-store.com">`.
    3.  Alice clicks the ad.
    4.  Alice's browser sends an HTTP GET request to `grocery-store.com`. Included in the hidden background headers of that request is:
        `Referer: https://www.deliver-me-pizza.com/submit_order?session-id=3927a8...`
  *   **The Consequence:** The administrator of `grocery-store.com` can look at their server traffic logs, see Alice's Session ID sitting in plain text, and use it to hijack her pizza account.
- ## IV. The POST Solution and its Caveats (Slide 24)
  To fix this, developers must use the `POST` method for all state-changing or sensitive forms.
  
  *   **How POST Fixes the UI Leak:** When Alice submits the form via POST, the browser puts the `session-id` into the HTTP request body. The URL in her address bar just says: `https://www.deliver-me-pizza.com/submit_order`. If she emails this link to Meg, Meg just gets a generic page or an error, not Alice's session.
  *   **How POST Fixes the Referer Leak:** Because the URL is clean, if Alice clicks a link to the grocery store, the `Referer` header will only send the clean URL (`.../submit_order`), keeping the Session ID safely hidden in the body of the previous request.
  
  **The Caveat (Zero-Click Leaks):**
  If the developer made a mistake earlier in the workflow and the *current* URL already has the Session ID in it, an attacker doesn't even need Alice to click a link.
  *   If the pizza page simply loads an image from an external server: `<img src="http://grocery-store.com/banner.gif">`
  *   The browser automatically fetches the image using a GET request.
  *   Because the browser is currently sitting on a polluted URL, it will send the polluted URL in the `Referer` header to the image server, leaking the Session ID instantly and invisibly.
  
  ---
- ### Study Questions for Part 3
  1. **GET vs. POST:** Why is the `GET` method considered acceptable for a Google Search query, but unacceptable for submitting a banking login form?
  2. **Session Hijacking:** If an attacker gains access to a corporate network's proxy server logs, how could the HTTP `Referer` header allow them to take over user accounts on external websites?
  3. **Architectural Fixes:** If a developer switches their HTML form from `<form method="GET">` to `<form method="POST">`, does this encrypt the user's data? (Hint: No. What exactly does it do to the data instead?)
  
  ---
-