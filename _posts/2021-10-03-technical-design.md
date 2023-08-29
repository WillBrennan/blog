---
layout: post
title: System Design Process - [3/3] Crafting Technical Design Documents
tags: [System Design, Technical Excellence]
--- 

# Introduction
In today's fast-paced era, where development frequently pivots and agile processes reign supreme, the notion of a Technical Design Document (TDD) might seem antiquated to some. Why invest time in meticulously documenting something that might change tomorrow? But ask any technology leader and they will remind you of the adage "failing to plan is planning to fail", which remains resoundingly true. 

# The Timeless Value of Technical Design Documents
The Technical Design Document describes a solution to a technical problem. It is a detailed description of the architecture, data model, and interfaces of the system. It forces both the writer and reader to think deeply about the problem and solutions before diving into code. This reduces the chances of overlooking critical aspects.  

Imagine a system where changes become herculean tasks, budgets incessantly overshoot, release schedules become elusive, and each update risks introducing catastrophic bugs. Sounds familiar? You might've been grappling with a [Stovepipe System](https://sourcemaking.com/antipatterns/stovepipe-system) - a system characterized by rigid, isolated subsystems that lack standard interfaces. Features get slapped on ad-hoc, subsystems become unnecessarily coupled, and developers resort to quick fixes and workarounds rather than robust solutions.

While Agile champions swift iterations, a nebulous system architecture spells doom. As an experienced Architect, I've seen systems crumble under their own weight due to inadequate design documentation. The Agile Manifesto values "working software over comprehensive documentation," but it doesn't say to avoid documentation entirely. The goal is to have just enough documentation to be effective, and for many projects, a TDD falls into that "just enough" category. The TDD brings value not just during the initial design phase but throughout the entire life-cycle of the project. 

When armed with a detailed TDD, teams can: 

* **Preserve Knowledge:** Captures crucial system specifics, ensuring continuity even as team members come and go.
* **Foster Reviews:** As a reference point, it enables peers and stakeholders to evaluate design choices and offer feedback.
* **Foster Collaboration:** It facilitates open discussions, enhances team communication and resolves design ambiguities.
* **Scaling and Extending:** Teams can more easily adapt and integrate new features or component if they have a clear picture of the existing system.
# The Anatomy of an Effective TDD
#### 1. Description of the Problem
  * **Purpose:** In one paragraph, articulate the problem from the perspective of the customer and any relevant context. Limit the use of technical jargon.
  * **Impact:** This ensures that the team creating the design have understood the problem and encourages them to learn more about it.

#### 2. Solution Requirements 
  * **Purpose:** In one paragraph, summarize the critical aspects of the Technical Requirements Document.
  * **Impact:** Upon review, this focuses the readers attention on checking toughest or most subtle requirements.
 
#### 4. Out of Scope (Non-Goals)
  * **Purpose:** Explicitly call out what is not in scope. 
  * **Impact:** It is often clearer for the reader to specify what you are not doing.

#### 5. Assumptions
  * **Purpose:** State what assumptions are made in the design.
  * **Impact:** In Technical Designs, invalid assumptions lead to non-viable systems. Call them out now before they become silent killers.
  
#### 6. Proposed Solution
  * **Purpose:** Start at the high-level diagram and dive deep into how the architecture, data model, APIs, and other relevant pieces work together.
  * **Impact:** This is the most critical part; establishing the technical solution.
  * **Sections:**
    * System Architecture - Diagram and bullet-point list explaining components
    * Data Model - Describe how data is stored; including schema.
    * Interface / API Definitions - Describe how components talk to each other with schema and endpoints.
    * Business Logic - Describe any non-trivial logic or algorithms.
    * Migration Strategy - Describe how you will migrate to this system, and in the future migrate away from it.
  
#### 7. Security and Data Handling Considerations 
  * **Purpose:** Outline the strategies to ensure that the solution is secure. Discuss how data will be handled, stored, and protected.
  * **Impact:** Maintains customer trust by ensuring that the solution abides by industry best practices and regulatory requirements.

#### 8. Cost Analysis 
  * **Purpose:** Provide an estimated breakdown of costs associated with building, deploying, and maintaining the solution.
  * **Impact:** Helps stakeholders understand the financial implications of the project and assists in budgetary planning.
  
#### 9. Operational Readiness 
  * **Purpose:** How will you create operational excellence, ensuring excited customer with frugal support?
  * **Impact:** Ensures smooth deployment and post-launch operations, resulting in a better customer experience and easier system maintenance.
  * **FAQ:**
    * Deployment - How will the system be deployed? How long will it take? 
    * Incident Response - How will you revert the system in an incident?
    * Monitorability - How will you monitor the system health? What alarms and metrics will you have?
    * Traceability - How will you debug where problems occur?
    * Will there be any data recovery mechanisms? 
    * Multi-Tenant: How will you deal with noisy neighbors?
  
#### 10. High-Level Testing Plan
  * **Purpose:** Explain how you will test new features and deployments
  * **Impact:** Without considering QA at design; many systems become impossible to test. You will ship buggy code.
#### 11. Risks & Open Issues
  * **Purpose:** Are there any technical risks or unknowns? Address potential technology challenges, implementation barriers, and other uncertainties.
  * **Impact:** By identifying risks upfront, the team can be better prepared to handle surprises and develop mitigation strategies.
  
#### 12. Alternative Solutions Considered and Discarded
  * **Purpose:** Explain why the solution you chose is superior to other approaches.
  * **Impact:** Minimizes time spent revisiting already-discarded options during discussions.


# Conclusion
In today's fast-paced era, where agile methodologies dictate the pace, there's a temptation to perceive Technical Design Documents (TDD) as relics of the past. To perceive TDDs merely as legacy documentation would be to gravely underestimate their enduring strategic value.

TDDs serve as the architectural blueprint of our systems, guiding teams throughout the life-cycle, from initial design to subsequent scalability. The well-defined structure of an effective TDD ensures that all critical facets of a project are considered, from the clarity of the problem statement to on-going operational costs and maintenance. Equipping your team with a comprehensive TDD not only promotes knowledge preservation, peer review, and collaboration but also significantly reduces the likelihood of creating siloed systems that are brittle and difficult to manage. 

Ultimately, while agile methodologies emphasize adaptive planning and evolutionary development, a robust TDD complements these principles, ensuring that projects remain on track, within budget, and adaptable to future requirements. In the balancing act of agility and precision, the TDD emerges as an indispensable tool.

It's not just about planning for today but crafting resiliency for the future.

This is the third of three posts covering System Design. Continue by exploring, 

* [System Design Process - [1/3] A Guide for Technical Leaders](/blog/2021/10/02/system-design-process.html)
* [System Design Process - [2/3] Crafting Technical Requirements Documents](/blog/2021/10/02/technical-requirements.html)
