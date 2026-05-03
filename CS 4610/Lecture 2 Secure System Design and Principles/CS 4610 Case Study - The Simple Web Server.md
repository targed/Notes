## I. The Setup: How HTTP Works
To understand the hack, you must understand the protocol.
*   **The Protocol:** HTTP (Hypertext Transfer Protocol). It is text-based.
*   **The Request:** When you type a URL into a browser, it sends a text command to the server.
  *   Format: `GET /filename.html HTTP/1.0`
  *   It expects a response code (e.g., `200 OK`) and the file content.
- ## II. The Code Walkthrough
  The slides present a basic Java web server. Here is the logic flow:
  
  1.  **`main()`**: Starts the server.
  2.  **`run()`**:
    *   Opens a `ServerSocket` on Port 8080.
    *    enters a `while(true)` loop (infinite loop) to wait for connections.
    *   When a client connects, it calls `processRequest(socket)`.
  3.  **`processRequest()` (The Vulnerable Function):**
    *   Reads the input line from the client: `String request = br.readLine();`
    *   **Parsing:** It uses `StringTokenizer` to chop the request into parts (Command and Path).
        *   `command = st.nextToken();` (Expects "GET")
        *   `pathname = st.nextToken();` (Expects "/index.html")
  4.  **`serveFile()`**:
    *   Finds the file on the hard drive.
    *   Reads it character by character.
    *   Sends it back to the client.
- ## III. The Vulnerability: Denial of Service
  **The Flaw:** The code assumes the user is "nice." It assumes the input will *always* follow the format `GET /file HTTP/1.0`.
  
  **The Attack:**
  *   An attacker connects to the server (via Telnet or a script).
  *   Instead of sending `GET /index.html`, the attacker sends **an empty line** (just a carriage return `\r\n`) or garbage text.
  
  **The Crash:**
  1.  `br.readLine()` reads the empty line.
  2.  `StringTokenizer` is created with an empty string.
  3.   The code calls `st.nextToken()`.
  4.  **EXCEPTION!** Because there are no tokens to read, Java throws a `NoSuchElementException` (or similar runtime exception).
  5.  **Result:** Since there is no `try/catch` block handling this specific error, the Exception bubbles up and **crashes the entire server program.**
  6.  **Impact:** The server shuts down. No one else can access the website. This is a **Denial of Service (DoS)**.
- ## IV. The Solution: Robust Programming
  **1. The Fix: Exception Handling (Try/Catch)**
  *   Wrap the dangerous parsing code in a `try` block.
  *   If the input is bad (malformed), the `catch` block activates.
  *   **Graceful Failure:** Instead of crashing, the server:
    1.  Catches the error.
    2.  Sends a `400 Bad Request` error message to the user.
    3.  Closes that specific connection.
    4.  **Loops back to wait for the next user.** The server stays alive.
  
  **2. Specifying Requirements**
  *   Your design documents should explicitly state: "The system must handle malformed inputs without crashing."
  *   *Example:* If a user sends a carriage return as the first byte, the system should discard it, not die.
- ## V. Testing for Security
  How do we find these bugs before the hackers do?
  
  **1. "Ping-of-Death" & Edge Cases**
  *   Standard testing checks if the software works when used *correctly*.
  *   Security testing checks if the software survives when used *incorrectly*.
  *   *Ping-of-Death Example:* Historically, sending a packet larger than the maximum IP limit (65,535 bytes) would crash operating systems.
  
  **2. Fuzz Testing (Fuzzing)**
  *   **Definition:** Automated software testing that provides invalid, unexpected, or random data (fuzz) as input to a computer program.
  *   **Goal:** To see if the program crashes (DoS) or leaks memory.
  *   *Application to SWS:* A fuzzer would send thousands of random strings (`AAAAA...`, `%^&*(`, `GET /../../passwords`) to the server to ensure the `try/catch` block handles all of them.
  
  **3. Internal Error Handling**
  *   **Information Leakage:** When an error occurs, do not show the "Stack Trace" to the user.
  *   *Why?* A stack trace tells the attacker exactly what code you are running, what database you use, and where the error happened. This helps them craft a better attack.
  
  ---
- ### Study Questions for Part 3
  1.  **The Crash:** In the `SimpleWebServer` code, exactly which line of code causes the program to terminate when an attacker sends an empty string?
  2.  **HTTP Codes:** In the fixed version of the code, the server sends back `HTTP /1.0 400 Bad Request`. Why is `400` the correct code here, rather than `404 Not Found` or `500 Internal Server Error`?
  3.  **Fuzzing:** If you were writing a Fuzzing script to test this server, list three specific "bad inputs" you would send to try and break it.
  
  ---