## 1. The Requirements Elicitation and Analysis Spiral
  
  Requirements elicitation (also known as requirements discovery, collection, or gathering) is the process where technical staff collaborate with customers and end users to discover the problem domain, the system services required, and the operational constraints (Slide 32).
  
  Slides 35 and 36 model this process not as a one-time linear task, but as a **continuous, iterative spiral**:
  
  ```
                  +-----------------------------------------+
                  |        1. Requirements Discovery        |
                  |  • Interacting with stakeholders        |
                  |  • Uncovering domain rules & workflows |
                  +--------------------+--------------------+
                                       |
                                       v
                  +-----------------------------------------+
                  | 2. Classification and Organization     |
                  |  • Clustering related needs             |
                  |  • Partitioning into subsystems         |
                  +--------------------+--------------------+
                                       |
                                       v
                  +-----------------------------------------+
                  |     3. Prioritization & Negotiation     |
                  |  • Resolving stakeholder conflicts      |
                  |  • Establishing delivery priorities     |
                  +--------------------+--------------------+
                                       |
                                       v
                  +-----------------------------------------+
                  |       4. Requirements Specification     |
                  |  • Documenting formal requirements      |
                  |  • Input into next spiral iteration     |
                  +--------------------+--------------------+
                                       |
                                       +=======> Loops back to Step 1
  ```
### The Four Spiral Activities (Slide 36)
  1. **Requirements Discovery:** The initial fact-finding stage. Engineers interact with diverse stakeholders (from clinic nurses and frontline staff to company executives and external regulators) to extract operational goals and identify domain constraints.
  2. **Requirements Classification and Organization:** Raw notes, interview transcripts, and observations are analyzed and grouped into coherent, structured functional clusters (e.g., partitioning requirements into Authentication, Media Streaming, Input Handling, and Session Moderation).
  3. **Prioritization and Negotiation:** Because stakeholders always request more features than time and budget allow, engineers must prioritize capabilities (e.g., using MoSCoW: Must, Should, Could, Won't) and formally negotiate compromises when stakeholder requests contradict one another.
  4. **Requirements Specification:** The negotiated baseline is formalized into structured requirements and diagrams, becoming the input for subsequent validation and implementation rounds.
  
  ---
## 2. The Five Classic Challenges of Requirements Elicitation
  
  Slide 33 identifies five structural obstacles that cause requirements discovery to fail if not actively managed:
  
  ```
  +--------------------------------------------------------------------------+
  |                  THE FIVE ELICITATION CHALLENGES (SLIDE 33)              |
  +--------------------------------------------------------------------------+
  | 1. Stakeholders do not know what they really want.                       |
  | 2. Stakeholders express requirements in their own terms.                 |
  | 3. Different stakeholders have conflicting requirements.                 |
  | 4. Organizational and political factors influence requirements.          |
  | 5. Requirements change dynamically during the analysis process.          |
  +--------------------------------------------------------------------------+
  ```
### 1. Stakeholders Do Not Know What They Really Want
  * **The Problem:** Non-technical users understand their daily frustrations, but they rarely know what technical capabilities are possible, practical, or optimal. They often express solutions rather than problems (e.g., asking for an Excel spreadsheet export when what they actually need is an automated dashboard alert).
  * **The Tacit Knowledge Problem:** Experienced workers perform routine tasks instinctively. When asked to describe their job, they omit critical intermediate steps because those steps are "obvious" to them.
### 2. Stakeholders Express Requirements in Their Own Terms
  * **The Problem:** The vocabulary barrier. Domain experts speak using internal business acronyms, clinical terms, or shop-floor jargon. Software developers speak in data structures, API endpoints, and database keys.
  * **The Consequence:** Misunderstandings occur when identical words carry different meanings in technical versus business contexts.
### 3. Different Stakeholders Have Conflicting Requirements
  * **The Problem:** Systems serve multiple user classes whose operational goals are directly opposed:
  * *End Users:* Desire minimal clicks, persistent logins, and friction-free interfaces.
  * *Security Officers:* Demand multi-factor authentication, automatic 5-minute session timeouts, and granular permission gates.
  * *System Managers:* Demand exhaustive audit logging and mandatory data-entry fields that slow down end users.
  * **The Engineering Role:** The software engineer cannot simply say "yes" to everyone; they must act as a mediator to negotiate technical compromises.
### 4. Organizational and Political Factors Influence Requirements
  * **The Problem:** System boundaries are political judgments. Software changes how information flows and who holds authority in an organization:
  * Department managers may demand specific reporting features simply to assert jurisdiction over another group's data.
  * Employees may resist automation features if they perceive the software as a threat to their job security or status.
### 5. Requirements Are Volatile and Change Dynamically
  * **The Problem:** Requirements change *during* the elicitation process itself. As stakeholders learn what software can do, their expectations shift. New competitors release products, corporate leadership rotates, or government compliance laws update mid-project.
  
  ---
## 3. Case Study: The Pentagon Wars and the Bradley Fighting Vehicle
  
  Slide 34 links to the satirical clip from the film *The Pentagon Wars*, documenting the real-world evolution of the US Army's Bradley Fighting Vehicle. This case study is the standard engineering illustration of what happens when requirements elicitation lacks boundaries, prioritization, and conflict management.
  
  ```
  +--------------------------------------------------------------------------+
  |               CASE STUDY: THE BRADLEY FIGHTING VEHICLE DRIFT             |
  +--------------------------------------------------------------------------+
  | Original Mission (The Core Need):                                        |
  | A fast, lightweight, amphibious troop transport designed to carry an     |
  | entire squad of 11 infantrymen safely across battlefields.               |
  |                                                                          |
  | Creeping Featurism (Uncontrolled Stakeholder Demands):                   |
  | 1. General A: "It needs firepower to defend itself."                     |
  |    -> Adds a 25mm automatic cannon and rotating turret.                  |
  |    -> The turret mechanisms and ammo storage consume interior space.     |
  |    -> Troop capacity drops from 11 men down to 9 men.                    |
  |                                                                          |
  | 2. General B: "What if it encounters enemy tanks?"                       |
  |    -> Adds heavy TOW anti-tank missile launchers and tracking computers. |
  |    -> Missile racks consume more space; troop capacity drops to 6 men.   |
  |                                                                          |
  | 3. General C: "The armor is too thin against heavy artillery."           |
  |    -> Adds tons of thick steel armor plates.                             |
  |    -> The vehicle becomes so heavy it sinks in water, destroying its     |
  |       amphibious capability. A massive inflatable canvas skirt is added. |
  |                                                                          |
  | 4. General D: "The troops inside must be able to shoot back."            |
  |    -> Cuts firing ports into the armor, structurally weakening the hull. |
  |                                                                          |
  | The Result:                                                              |
  | "A troop transport that can't carry troops, a scout that is too big to   |
  | hide, and a tank that has less armor than a real tank."                  |
  +--------------------------------------------------------------------------+
  ```
### Engineering Lessons for Software Development:
  * **The Danger of Feature Creep:** Continuously tacking on features requested by powerful stakeholders without re-evaluating the system architecture causes the project to deviate from its core purpose.
  * **The Necessity of Saying "No":** Good requirements engineering requires trade-off analysis. Adding a capability to satisfy one stakeholder often breaks a non-functional constraint (such as performance, battery life, or latency) required by another.
  * **Architecture Cannot Satisfy Incompatible Goals:** You cannot build a system that is simultaneously ultra-lightweight and heavily armored, just as you cannot build a software system that has zero-latency streaming and heavy cryptographic overhead on weak edge hardware.
  
  ---
## 4. Fact-Finding and Elicitation Techniques Toolkit
  
  Slide 31 provides a list of nine proven fact-finding techniques used to gather requirements:
  
  ```
  +--------------------------------------------------------------------------+
  |                    RE FACT-FINDING TECHNIQUES (SLIDE 31)                 |
  +-------------------------+------------------------------------------------+
  | Technique               | Operational Purpose & Best Usage               |
  +-------------------------+------------------------------------------------+
  | 1. Background Reading   | Studying domain literature, company structure, |
  |                         | existing regulations, and industry standards.  |
  +-------------------------+------------------------------------------------+
  | 2. Document Sampling    | Inspecting old forms, paper logs, database     |
  |                         | dumps, and legacy system error logs.           |
  +-------------------------+------------------------------------------------+
  | 3. Stakeholder          | Formal 1-on-1 conversations to discover user   |
  |    Interviews           | goals, operational pain points, and desires.   |
  +-------------------------+------------------------------------------------+
  | 4. Questionnaires &     | Gathering quantitative data, frequencies, and  |
  |    Surveys              | preferences from a large, distributed userbase.|
  +-------------------------+------------------------------------------------+
  | 5. Observation /        | Watching real users perform tasks in their     |
  |    Ethnography          | actual working environment without disruption. |
  +-------------------------+------------------------------------------------+
  | 6. Workshops            | Structured multi-stakeholder sessions to align |
  |                         | priorities and hammer out conflicting needs.   |
  +-------------------------+------------------------------------------------+
  | 7. Brainstorming        | Open, non-critical idea-generation sessions    |
  |                         | aimed at exploring creative technical options. |
  +-------------------------+------------------------------------------------+
  | 8. Use Cases            | Visual and tabular modeling of actor-system    |
  |                         | interactions to achieve specific goals.        |
  +-------------------------+------------------------------------------------+
  | 9. Storyboarding /      | Rapid mockups and low-fidelity simulations     |
  |    Prototyping          | designed to elicit early user reactions.       |
  +-------------------------+------------------------------------------------+
  ```
  
  ---
### Detailed Analysis of Primary Techniques
#### 1. Observation vs. Interviewing (The Contrast)
  * **Interviews** reveal what users *think* they do, or what they believe management wants them to say. However, workers routinely forget edge cases, skip boring manual steps, or describe ideal procedures that they do not actually follow.
  * **Observation (Ethnography)** reveals what users *actually* do. Engineers discover workarounds (e.g., sticky notes with passwords, manual paper ledgers kept as backup because the current software is untrusted, or undocumented keyboard shortcuts). Observation is essential for discovering tacit requirements.
#### 2. Document Sampling
  Before asking users to explain their data, inspect the actual physical or digital documents they handle:
  * Look at daily intake forms, purchase orders, exception logs, and spreadsheets.
  * Identify which fields are always filled out, which fields are left blank (indicating obsolete requirements), and what handwritten notes workers scrawl in the margins (indicating missing functional fields).
#### 3. Structured Requirements Workshops
  When stakeholders disagree on priorities:
  * Bring representatives from all conflicting groups (e.g., End Users, Security, Managers) into a single facilitated workshop.
  * Use collaborative ranking exercises (such as feature dot-voting or forced budget allocation) so stakeholders directly confront the trade-offs of their demands rather than treating the engineering team as an infinite resource.
  
  ---