## 1. Project Selection Philosophy: Shifting from Technology to Problems
  
  The introductory five minutes of the Week 02 project matching session focuses on a foundational software engineering principle: **Shift from technology to problems.**
  
  ```
  +-------------------------------------------------------------------------+
  |                  THE PROJECT SELECTION PARADIGM SHIFT                   |
  +-------------------------------------------------------------------------+
  | ANTI-PATTERN: Solution Looking for a Problem                            |
  | "We want to build something using Rust, WebAssembly, and AI."           |
  | • Focuses on tools rather than value.                                   |
  | • Fails when developers realize the technology does not fit the domain. |
  | • Produces arbitrary features that no real user requested.              |
  |                                                                         |
  | CORRECT APPROACH: Problem-Driven Engineering                            |
  | "Users struggle to take turns playing single-player games online."      |
  | • Focuses on operational friction, user pain points, and workflows.     |
  | • Dictates technology selection based on technical constraints.        |
  | • Provides clear metrics for success: Did we solve the friction?        |
  +-------------------------------------------------------------------------+
  ```
  
  When evaluating a project concept, teams must subject the idea to three standard engineering filters before writing any code:
  1. **Desirability:** Is there a well-defined group of stakeholders or users who actually experience this problem?
  2. **Feasibility:** Can a team of student engineers design, implement, test, and deploy this system within eight months given available hardware, network environments, and technical skills?
  3. **Viability:** Does the project fit within the operational constraints of the course (e.g., testable requirements, measurable non-functional attributes, clear verification methods)?
  
  ---
## 2. Industry Benchmarking and Technology Inspiration
  
  Slide 5 highlights current industry innovations from events like CES (Consumer Electronics Show), such as assistive computer vision glasses for visually impaired individuals.
  
  The purpose of examining commercial technology at this stage is not to copy consumer electronics, but to observe how high-level software engineering paradigms are applied in production:
  * **Edge Processing vs. Cloud Latency:** Assistive devices cannot tolerate high cloud round-trip times because users navigating physical environments need sub-second feedback. This mirrors real-time networking problems in gaming or streaming where processing must occur locally or over optimized peer-to-peer protocols.
  * **Accessibility as a First-Class Requirement:** Designing systems for specialized user groups reinforces that user requirements must drive interface design, input mappings, and error handling from Day 1 rather than being tacked on at the end.
  * **Hardware-Software Boundaries:** Modern software frequently interfaces with external physical inputs (sensors, cameras, specialized input controllers). Software engineers must understand operating system drivers, hardware polling rates, and data serialization.
  
  ---
## 3. Project Tracks: Faculty-Sponsored vs. Student-Proposed
  
  The curriculum presents three institutional project tracks alongside independent student proposals. Each track carries distinct architectural characteristics, operational constraints, and risk profiles.
  
  ```
  +-------------------------------------------------------------------------+
  |                       CAPSTONE PROJECT PATHWAYS                         |
  +-------------------+--------------------------------+--------------------+
  | Pathway           | Technical Focus                | Primary Tradeoffs  |
  +-------------------+--------------------------------+--------------------+
  | Track 1: Dr. Park | Humanoid Robotics & Control    | High hardware risk |
  | (Robotics Track)  | Kinematics, real-time control, | Rigid external     |
  |                   | low-level sensor integration   | physical dependency|
  +-------------------+--------------------------------+--------------------+
  | Track 2: College  | Interdisciplinary Systems      | Complex cross-team |
  | of Engineering    | Embedded firmware, mechanical  | communication and  |
  | (Cross-Discipline)| interfaces, telemetry dashboards| external scheduling|
  +-------------------+--------------------------------+--------------------+
  | Track 3: Dr. Sid  | Applied AI / Machine Learning  | Data dependency,   |
  | (AI / ML Track)   | Model training, inference APIs,| non-deterministic  |
  |                   | data pipelines across Cap I/II | outputs to test    |
  +-------------------+--------------------------------+--------------------+
  | Track 4: Student- | Full-Stack Applications,       | Complete autonomy, |
  | Proposed (e.g.,   | Desktop utilities, real-time   | but requires team  |
  | Pass Controller)  | networking, custom tools       | to define own scope|
  +-------------------+--------------------------------+--------------------+
  ```
### Deep Dive into the Tracks
#### Track 1: Robotics and Control Systems (Dr. Park)
  * **Domain:** Autonomous systems and humanoid robotics.
  * **Engineering Focus:** Real-time operating systems (RTOS), embedded C/C++, control loops, and hardware actuator communication (e.g., CAN bus, ROS2).
  * **Key Challenge:** Hardware availability. If the physical robot breaks or parts are delayed, software development and testing can stall unless robust software simulation environments (e.g., Gazebo) are established early.
#### Track 2: Interdisciplinary Systems (College of Engineering Collaboration)
  * **Domain:** Joint software-hardware development with Mechanical and Electrical Engineering teams.
  * **Engineering Focus:** Firmware development, microcontrollers (STM32, ESP32), sensor calibration, and desktop/web telemetry interfaces.
  * **Key Challenge:** Organizational friction. Computer science engineers must coordinate with non-software teams whose design cycles, testing methodologies, and academic deadlines may not align with the CS SDLC schedule.
#### Track 3: Applied Machine Learning Pipelines (Dr. Sid)
  * **Domain:** Applied machine learning spanning Capstone I and Capstone II.
  * **Engineering Focus:** Data preprocessing pipelines, model selection/fine-tuning, model quantization for edge deployment, and REST/gRPC inference servers.
  * **Key Challenge:** Verifiability and testing. Traditional software produces deterministic outputs for given inputs. Machine learning systems produce probabilistic outputs, making formal requirements validation, boundary-value analysis, and automated test cases more complex to structure in an SRS.
#### Track 4: Student-Proposed Systems (Independent Products)
  * **Domain:** Standalone applications, developer tools, distributed platforms, or gaming infrastructure (such as *Pass the Controller*).
  * **Engineering Focus:** End-to-end software architecture, network transport (WebSockets, WebRTC), database modeling, desktop wrappers, and driver emulation.
  * **Key Challenge:** Scope management. Without an external client setting rigid boundaries, student teams often fall victim to scope creep, proposing massive systems that exceed their delivery capacity.
  
  ---
## 4. The 70-Minute Project Matching Protocol
  
  Slide 13 outlines the structured process used during the Thursday session to move from loose project concepts to confirmed team-project assignments.
  
  ```
  +-------------------------------------------------------------------------+
  |             PROJECT INTEREST & MATCHING SCHEDULE (70 MIN)               |
  +------------+-----------------------------------+------------------------+
  | Time       | Activity                          | Deliverable / Purpose  |
  +------------+-----------------------------------+------------------------+
  | 0–5 min    | Project Selection Philosophy      | Shift from tech to user|
  | 5–20 min   | Lightning Project Pitches         | 60-second concept teasers|
  | 20–40 min  | Project Marketplace               | Informal open inquiry  |
  | 40–50 min  | Interest Clustering               | Theme grouping         |
  | 50–62 min  | Project-Team Fit Discussion       | Skill vs. scope audit  |
  | 62–67 min  | Project Preference Ranking        | Formal ranking forms   |
  | 67–70 min  | Next Steps                        | Finalize commitments   |
  +------------+-----------------------------------+------------------------+
  ```
### Protocol Execution Details
  
  1. **Lightning Project Pitches (5–20 min):**
   * Students or faculty leads pitch ideas in 60 to 90 seconds.
   * Pitch structure:
     * *The Problem:* Who is hurting and what is broken?
     * *The Proposed Solution:* What system will be built?
     * *Key Technical Needs:* What specific skills are required (e.g., low-level Windows drivers, WebRTC, database design)?
  2. **Project Marketplace (20–40 min):**
   * Pitchers stand at designated stations around the room.
   * Unassigned students rotate through stations to inspect project feasibility, ask technical questions, and assess whether the project matches their personal learning goals.
  3. **Interest Clustering (40–50 min):**
   * The class groups similar project proposals together (e.g., grouping all low-latency networking projects, web platforms, or embedded projects).
   * Prevents three separate teams from building nearly identical competing projects.
  4. **Project-Team Fit Discussion (50–62 min):**
   * Formed teams evaluate the selected project against their actual skill inventory:
     * Does our team have the systems capability to handle driver-level inputs?
     * Does someone know how to build the required user interface?
     * Do we have access to the physical hardware or test environments required?
  5. **Project Preference Ranking (62–67 min):**
   * Teams submit their top three project choices to the instructor to ensure balanced distribution and resolve duplicate claims on faculty-sponsored slots.
  
  ---
## 5. Technical Feasibility Pre-Screening Checklist
  
  Before a team formally registers a tentative project title on Canvas, the Project Manager and System Architect should run the concept through the following feasibility checklist:
  
  ```
  +--------------------------------------------------------------------------+
  |                     FEASIBILITY PRE-SCREENING AUDIT                      |
  +--------------------------------------------------------------------------+
  | [ ] 1. Core Workflow Simplicity: Can the fundamental user loop be        |
  |        demonstrated with 3 to 5 core screens or components?              |
  |                                                                          |
  | [ ] 2. Hardware Availability: Does every developer have access to the    |
  |        necessary testing hardware (e.g., GPUs, controllers, OS licenses)? |
  |                                                                          |
  | [ ] 3. Dependency Risk: Does the project rely on proprietary, expensive,  |
  |        or poorly documented external APIs that might get deprecated?     |
  |                                                                          |
  | [ ] 4. Testability: Can the QA lead write unambiguous, automated, or     |
  |        deterministic test cases for the functional requirements?         |
  |                                                                          |
  | [ ] 5. Division of Labor: Can the architecture be cleanly divided into   |
  |        subsystems so all members can write code concurrently?            |
  +--------------------------------------------------------------------------+
  ```
  
  ---