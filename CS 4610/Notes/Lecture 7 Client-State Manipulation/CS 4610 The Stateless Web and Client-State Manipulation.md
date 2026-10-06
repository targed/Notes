## I. The Core Problem: HTTP is Stateless (Slide 2)
*   **What "Stateless" Means:** The Hypertext Transfer Protocol (HTTP) was designed to be simple. A client asks for a web page, the server sends the web page, and then the server *completely forgets who you are*. Every single request is treated as an independent, isolated event.
*   **The Need for State:** Modern web applications cannot function this way. If Amazon forgot who you were every time you clicked a new page, you could never build a shopping cart or stay logged in.
*   **The Workaround:** Because the server inherently forgets, web applications must force the *client* (your web browser) to hold onto "state" information and echo it back to the server with every new request (e.g., "Hi, I'm still Alice, and I still have a pizza in my cart!").
- ## II. The Illusion of "Hidden" State (Slides 3–5)
  In the early days of the web, developers used a cheap trick to pass state back and forth: **Hidden HTML form fields**.
  
  *   **The Pizza Delivery Example:** 
    *   Alice clicks "Order 1 Pizza."
    *   The server calculates the price ($5.50) and sends back a confirmation page.
    *   To ensure it remembers the price when Alice clicks "Confirm", the server embeds the price in the HTML code invisibly: `<input type="hidden" name="price" value="5.50">`.
    *   When Alice clicks "Confirm," her browser automatically sends that hidden `$5.50` value back to the Payment Gateway.
  *   **The Fatal Flaw:** The golden rule of web security is **"Never Trust the Client."** Just because a field is labeled `type="hidden"` does not mean it is secure. It just means the browser chooses not to draw a box around it on the screen. The data is sitting in plain text in the raw HTML.
- ## III. The Attack Mechanics (Slides 6–11)
  An attacker can easily view, modify, and submit this "hidden" data to exploit the business logic of the application.
- ### Method 1: The Browser UI Attack (Slides 6–10)
  1.  The attacker goes to the Pizza checkout page.
  2.  They right-click and select "View Page Source" (or use modern browser Developer Tools like "Inspect Element").
  3.  They find the line: `<input type="hidden" name="price" value="5.50">`.
  4.  They edit the HTML directly in their browser, changing `5.50` to `0.01`.
  5.  They click the "Confirm" button on the web page. The browser dutifully sends the tampered price to the server, and the attacker buys a pizza for a penny.
- ### Method 2: The Command-Line Attack (Slide 11)
  Attackers rarely use web browsers for serious exploits. They use command-line HTTP clients like **`curl`** or **`wget`**.
  *   **Why use `curl`?** A web browser forces you to interact with the UI. Command-line tools allow you to craft raw HTTP requests from scratch, completely bypassing any buttons, forms, or restrictions the web page tries to enforce.
  *   **Automating the Attack:** An attacker can write a script to send thousands of customized requests instantly. 
    *   *Example `wget` attack:* `wget --post-data 'price=0.01&pay=yes' https://www.deliver-me-pizza.com/submit_order`
    *   This bypasses the order page entirely and directly hits the backend submission script with forged data.
  
  ---
- ### Expanded Context: Client-Side vs. Server-Side
  This entire attack relies on the developer making a severe architectural mistake: calculating the price on the server, sending it to the client, and then *trusting the client to send it back accurately*. In a secure application, the price should never touch the client's machine until it is time to display the final receipt. 
  
  ---
- ### Study Questions for Part 1
  1. **HTTP Fundamentals:** Why did early web developers rely on tricks like "hidden form fields" to keep track of a user's shopping cart?
  2. **The Golden Rule:** Explain why `<input type="hidden">` provides zero actual security against a malicious user.
  3. **Attacker Tooling:** Why might a hacker prefer to use a tool like `curl` over simply using Google Chrome's "Inspect Element" feature when exploiting a web application?
  
  ---
-