-
- ## I. Administrative Essentials
  *   **Instructor:** Dr. Junjie Xiong (CS Building 316).
  *   **Textbook:** *Foundations of Security: What Every Programmer Needs to Know* (Daswani et al.).
    *   *Note:* The title implies a focus on software security (building secure code) rather than just network administration.
  *   **Prerequisites:** Introduction to Computer Networks (CS 3610).
    *   *Why this matters:* You cannot secure a network if you don't understand the TCP/IP stack, packets, ports, and protocols. Security concepts often exploit how these protocols were designed (or misdesigned).
- ## II. Grading & Strategy
  The grading structure suggests a heavy emphasis on practical application and individual performance.
  *   **The Breakdown:**
    *   **Exams (45%):** Midterm (20%) + Final (25%). Both are closed-book, but allow a **1-page cheat sheet**.
    *   **Assignments (30%):** 4 total.
    *   **Projects (20%):** 2 total. Focused on **Buffer Overflow** and **SQL Injection**.
    *   **Quizzes (5%):** Frequent (10+), likely used to check attendance and basic reading comprehension.
  *   **Crucial Policies:**
    *   **Hard Deadlines:** 10% grade reduction per day late.
    *   **Academic Integrity:** This is a security class; do not "hack" the grading system. Plagiarism or modifying graded tests results in an automatic "FF" grade.
- ## III. The Course Motivation: Why are we here?
  The slides argue that Computer Security is no longer optional; it is a critical infrastructure necessity.
  
  **1. Dependence on Software**
  *   **Slide Concept:** We rely on software as much as electricity or water.
  *   **Expanded Context:** This is often referred to as **Critical Infrastructure**. In the past, a computer virus might just delete your homework. Today, a compromised system can shut down a power grid, alter water treatment chemical levels, or crash autonomous vehicles. The "Attack Surface" (the sum of all points where an unauthorized user can try to enter data or extract data) has grown exponentially.
  
  **2. The Threat Landscape**
  *   **Slide Concept:** Hackers exploit vulnerabilities to cause damage, form botnets, and steal data.
  *   **Expanded Context:**
    *   **Botnets:** A network of private computers infected with malicious software and controlled as a group without the owners' knowledge. These are often used for **DDoS (Distributed Denial of Service)** attacks to overwhelm a target server.
    *   **The Economy of Hacking:** It is rarely just "for fun" anymore. It is an industry. Stolen passwords, credit cards, and proprietary source code have monetary value on the black market.
  
  **3. The Root Cause: "Functionality over Security"**
  *   **Slide Concept:** Historically, programmers prioritized "getting the job done" over security.
  *   **Expanded Context:** This is the concept of **Technical Debt**.
    *   In the early days of the internet (and software dev), the goal was connectivity and features. Security was an afterthought (or "bolted on" later).
    *   *The Problem:* It is infinitely harder to secure a system designed without security in mind than to build it securely from scratch.
    *   *Legacy Code:* We are still running code written 20 years ago that has these inherent flaws.
- ## IV. Course Objectives
  The goal of this course is to move you from a "Functional Mindset" (Does it work?) to a "Security Mindset" (How can I break it?).
  
  1.  **Understand Vulnerabilities:** specifically **Network-based threats** and **Code vulnerabilities**.
  2.  **Develop Intuition:** You should be able to look at a piece of code or a network diagram and instinctively spot the "weak links."
  3.  **Practical Guidelines:** Learning how to not write vulnerabilities into your future code.
  
  ---
- ### Key Terminology Added (Not explicitly in slides but implied)
  *   **CIA Triad:** The slides allude to this later, but the "Motivation" section is built around maintaining **Confidentiality** (stopping leaks), **Integrity** (stopping viruses/changes), and **Availability** (stopping DoS attacks).
  *   **Zero-Day Exploit:** An attack that targets a previously unknown vulnerability (one the developers have had "zero days" to fix). The slides mention "unknown vulnerabilities"—this is what they are.
  
  ---
- ### Study Questions for Part 1
  *To ensure you grasped the intro:*
  1.  Why is the prerequisite of "Computer Networks" essential for understanding a botnet attack?
  2.  The slides mention "Buffer Overflow" and "SQL Injection" as upcoming projects. Based on the course objectives, are these examples of *Network* flaws or *Application/Code* flaws?
  3.  How does the "Security Mindset" differ from the standard "Developer Mindset"?
  
  ---