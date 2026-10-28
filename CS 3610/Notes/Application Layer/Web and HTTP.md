### Web and HTTP (Hypertext Transfer Protocol)
  
  The World Wide Web is a distributed system of interlinked documents and other web resources. The protocol that underpins this system is HTTP.
#### Core Concepts
  
  *   **Web Page Structure:** A web page consists of various **objects**. An object is simply a file, such as an HTML file, a JPEG image, a CSS stylesheet, or a JavaScript file. A web page has a base HTML file that references the other objects on the page.
  *   **URL (Uniform Resource Locator):** Each object is addressable by a URL, which contains the hostname of the server where the object resides and the object's path name on that server (e.g., `http://www.someschool.edu/someDept/pic.gif`).
  *   **Client/Server Model:** HTTP operates on a strict client-server model.
    *   **Client:** The web browser (e.g., Chrome, Firefox, Safari) requests, receives, and displays web objects.
    *   **Server:** A web server (e.g., Apache, Nginx) stores the web objects and sends them to clients in response to requests.
  *   **Underlying Transport Protocol:** HTTP uses **TCP** as its transport protocol to ensure reliable data transfer. The client initiates a TCP connection to the server, typically on **port 80**.
  *   **Stateless Protocol:** HTTP is a **stateless protocol**. This means the server does not maintain any information or "state" about past requests from a client. Every request is treated independently. This simplifies server design but requires other mechanisms, like cookies, to create user sessions.
#### HTTP Connections: Persistent vs. Non-Persistent
  
  There are two main ways HTTP manages TCP connections:
  
  1.  **Non-Persistent HTTP (The old way):**
    *   A separate TCP connection is established for *every single object* to be downloaded.
    *   **Process:**
        1.  The client opens a TCP connection to the server.
        2.  The client sends an HTTP request for one object.
        3.  The server sends the object in an HTTP response.
        4.  The server closes the TCP connection.
    *   **Performance Cost:** This approach is inefficient. Downloading a page with 10 images requires 11 separate TCP connections. Each connection requires a **Round Trip Time (RTT)** just to be established. The total response time for one object is approximately **2 RTTs + file transmission time**. This overhead adds up quickly.
  
  2.  **Persistent HTTP (HTTP/1.1 and later):**
    *   A single TCP connection is established between the client and server.
    *   **Process:**
        1.  The client opens a TCP connection to the server.
        2.  The client sends requests for multiple objects over that *same connection*.
        3.  The server sends the objects back.
        4.  The connection remains open for a period, allowing for future requests without the TCP setup overhead.
    *   **Performance Benefit:** This significantly reduces the latency, as the 1 RTT overhead for TCP setup is only paid once for many objects.
#### HTTP Messages
  
  HTTP communication consists of two types of messages:
  
  *   **Request Messages:** Sent by the client to the server. They include methods like:
    *   `GET`: Requests a resource from the server.
    *   `POST`: Submits data to be processed to the server (e.g., filling out a form).
    *   `HEAD`: Asks for the headers of a resource without the actual resource body.
    *   `PUT`: Uploads a resource to the server.
  
  *   **Response Messages:** Sent by the server back to the client. A response includes:
    *   **Status Line:** Indicates the protocol version, a status code, and a status phrase (e.g., `HTTP/1.1 200 OK`).
    *   **Header Lines:** Provide metadata about the response (e.g., `Content-Type`, `Content-Length`, `Date`).
    *   **Entity Body:** The requested object itself (e.g., the HTML or image file).
    *   **Common Status Codes:**
        *   `200 OK`: The request succeeded.
        *   `301 Moved Permanently`: The requested resource has moved to a new URL.
        *   `400 Bad Request`: The server could not understand the request.
        *   `404 Not Found`: The server could not find the requested resource.
        *   `505 HTTP Version Not Supported`: The server doesn't support the HTTP version used in the request.
#### Maintaining State: Cookies
  
  Since HTTP is stateless, **cookies** were invented to allow servers to keep track of users and create stateful sessions (e.g., for shopping carts or user logins).
  
  *   **How Cookies Work:**
    1.  When a user first visits a site, the server's HTTP response includes a `Set-Cookie:` header with a unique ID.
    2.  The browser stores this ID in a local cookie file, managed by the browser.
    3.  On every subsequent request to that *same site*, the browser includes the ID in a `Cookie:` header.
    4.  The server can then use this ID to retrieve the user's session information from its backend database.
  *   **First-Party vs. Third-Party Cookies:**
    *   **First-party cookies** are from the website you are directly visiting (e.g., `nytimes.com`).
    *   **Third-party cookies** are from a different domain embedded in the page you are visiting (e.g., an ad server like `AdX.com`). These are a primary mechanism for tracking users' browsing behavior across multiple websites. Due to privacy concerns, most browsers are moving to block third-party cookies by default. Regulations like the EU's **GDPR** give users explicit control over cookie usage.
#### Web Caching (Proxy Servers)
  
  A web cache, or proxy server, is a network entity that satisfies HTTP requests on behalf of an origin web server.
  
  *   **Goal:** To reduce response time and save network bandwidth.
  *   **How it Works:** The browser is configured to send all requests to the local web cache.
    *   If the requested object is in the cache (**cache hit**), the cache immediately returns it to the client.
    *   If the object is not in the cache (**cache miss**), the cache requests the object from the origin server, stores a copy, and then forwards it to the client.
  *   **Conditional GET:** To ensure the objects in the cache are up-to-date, caches use a `Conditional GET`. The cache sends a request with an `If-Modified-Since` header. If the origin server's object has not changed, it responds with a `304 Not Modified` status and an empty body, saving a significant amount of bandwidth.
#### The Evolution: HTTP/2 and HTTP/3
  
  *   **HTTP/2 (2015):** The main goal was to decrease latency by addressing a problem in HTTP/1.1 called **Head-of-Line (HOL) blocking**. In HTTP/1.1, if a client requests a large object followed by several small objects over a persistent connection, the small objects have to wait for the entire large object to be sent first. HTTP/2 solves this by breaking objects into smaller frames and **interleaving** them on a single TCP connection, allowing multiple objects to be transmitted in parallel.
  
  *   **HTTP/3:** HTTP/2 fixed HOL blocking at the application layer, but it still existed at the transport layer. Since all interleaved frames travel over one TCP connection, the loss of a single packet would cause TCP to halt the delivery of *all* streams until it was retransmitted. HTTP/3 solves this by running over **UDP** instead of TCP, implementing reliability and congestion control independently for each stream. This eliminates HOL blocking entirely.