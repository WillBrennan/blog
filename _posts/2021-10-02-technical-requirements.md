---
layout: post
title: System Design Process - [2/3] Crafting Technical Requirements Documents
tags: [System Design, Technical Excellence]
--- 
Navigating the intricate realm of software development is akin to constructing a skyscraper. Just as an architect relies on blueprints to shape towering structures, software projects lean on Technical Requirements Documents (TRDs) to achieve vision and purpose. They provide a concise and objective perspective that is far broader than functional User Stories alone.

## More Than Just Checklists
Although TRDs may often find themselves in the shadows of dynamic User Stories or agile sprints, their significance is undeniable. An insightful TRD doesn't merely tick off boxes, it's setting up your project for success. The returns of this initial time investment — crystal clear clarity, aligned visions, reduced disputes, and streamlined development — can't be overstated. The path from conception to deployment is fraught with potential pitfalls, but with a well-prepared TRD in hand, you're better equipped to navigate them.

At its heart, a TRD should be viewed as a contract defining the success criteria for the system, entrusting the 'how' to your development team and their Technical Design. You should use a TRD to guardrail development, placing boundaries on scope, resources, and time to deliver. This is your opportunity to clearly state the outcome that you will be happy and proud of. This descriptive, rather than prescriptive approach promotes a generative culture. Leading individuals to become intrinsically motivated to deliver with excellence. They are encouraged and trusted to think creatively, take the initiative, and have the ownership of their work. 

> *"Be stubborn on vision, but flexible on details."* - **Jeff Bezos**

When armed with an insightful TRD, teams can:

* **Achieve Clarity & Alignment:** Ensuring all stakeholders share a coherent vision, thereby averting costly misunderstandings.
* **Manage Scope Effectively:** Keeping feature bloat in check and fostering timely deliveries.
* **Enhance Efficiency:** Offering developers a unified, holistic reference point to streamline development.
* **Mitigate Risks:** Addressing potential roadblocks early, thus minimizing unforeseen complications as the project unfolds.

## The Debate: Per System or Per Project?
A prevalent question is whether to maintain requirements for each system, or create new requirements for each large project or feature. From my experience, teams often struggle managing the maintenance debt caused by having multiple Technical Requirement Documents per system. Over time older documents often become invalid and contradicting newer ones. It becomes difficult to obtain a complete view of the system's requirements and understand what is no longer needed. 

To avoid these pitfalls; it is recommended to have a single set of requirements for each system and update it over time. This aligns closely with Agile development, iterating as new information becomes available. To provide a clear history, it is advisable to maintain a Revision Log stating when, why, and how the requirements have changed. 

## Anatomy of a TRD
Diving deep, let's unpack the fundamental components that a TRD should encapsulate. The structure of a TRD is not arbitrary. Each section has been meticulously shaped over years of industry evolution, integrating learnings from numerous projects, reflecting both challenges faced and victories celebrated.
While specifics might vary, ensuring consistency in presentation, often via bullet points, aids readability.

#### 1. Introduction
* **Purpose:** Establish the document's purpose, audience, system's scope, and overarching goals.
* **Impact:** It sets the stage, ensuring alignment right from the onset.

#### 2. Background & Context
* **Purpose:** Why is the project essential? What's the history and prior work that led us here?
* **Impact:** Motivates the reader and stores your organizational wisdom, reducing repeated pitfalls, and anticipating future challenges.

#### 3. Functional Requirements 
* **Purpose:** Define the system's inputs and outputs. Consider using User Stories.
* **Impact:** The devil is in the details. Clearly defined functionalities prevent feature drift, aligning resources with the highest impact tasks.

#### 4. Non-Functional Requirements
* **Purpose:** Highlight qualitative attributes such as speed, scalability, and uptime.
* **Impact:** Emphasizes quality and excellence, going beyond mere functionality and user stories to define an outstanding product.

#### 5. Constraints & Assumptions
* **Purpose:** State system and development constraints and assumptions such as development time or operational cost.
* **Impact:**  If left unstated, these can be the silent killers of projects. It's akin to knowing the rules of the game. Allow the team to win.

#### 6. Risks & Mitigations
* **Purpose:** Preemptively address risks and how you will mitigate them.
* **Impact:** Risks are inevitable, but surprises aren’t. Enable smoother sailing and swifter responses by demonstrating proactive leadership.

#### 7. Revision Log 
* **Purpose:** Maintains a record of how the document has changed over time
* **Impact:** A transparent history lets you resolve ambiguities by speaking to relevant individuals.

#### 8. References & Glossary
* **Purpose:** Guarantee a consistent understanding among stakeholders. Explain every acronym you used and link to all references.
* **Impact:** Creates seamless onboarding and sidestep misinterpretations.

## TRDs in Action: Demonstrating Effectiveness
As an illustration of constructing a proficient TRD, we will craft one tailored for the Google Home Mini. While the requirements presented in this example are hypothetical, in real-world situations, they would stem from processes detailed in [System Design Process - A Guide for Technical Leaders](/blog/2023/04/02/system-design-process.html). 

 The emphasis here is on specifying each aspect of the system distinctly and underlining the significance of objective metrics. These metrics act as benchmarks to confidently determine if Technical Requirements have been met, serving as a clear directive for developers. 

We will also leverage the [Amazon Writing Style](https://medium.com/fact-of-the-day-1/amazon-writing-style-tip-a349b4bd3839).

#### 1. Introduction
This Technical Requirements Document (TRD) provides a comprehensive overview of the requirements for the proposed Google Home Mini system. Its objective is to outline specifications, constraints, and risks, ensuring the device is efficient, reliable, and meets user expectations.

#### 2. Background & Context
The Google Home Mini will be a voice-activated smart speaker, designed to be integrated with the Google Assistant. It is a part of Google’s broader smart home ecosystem. With the growth in smart home systems, there's a need to ensure that such devices are effective, user-friendly, secure, and scalable.

#### 3. Functional Requirements 
* **Voice Recognition:**
   * Achieve a 95% or higher accuracy rate in voice command recognition under standard conditions.
   * Recognize and process commands in the 3 most popular languages and regional accents.
   * Provide auditory or visual feedback within 0.5 seconds if a voice command is not understood.
* **Integration with Google Assistant:**
  * Process and act upon 98% of standard Google Assistant commands by volume.
  * Integration with third-party services through APIs or SDKs. Including leading services like Spotify and Netflix,
* **Connectivity:**
  * WiFi: Support and maintain stable connections under 802.11b/g/n/ac (2.4GHz/5Ghz) standards, with a maximum drop rate of 0.5% per day.
  * OTA Updates: Successfully download and install updates with a success rate of 98% when updates are available.
* **User Management:**
  * Store and manage up to 10 individual user profiles per device.
  * Achieve a 98% accuracy rate in distinguishing between registered user voice

#### 4. Non-Functional Requirements
* **Performance:**
  * Achieve a p50 < 2s and p99 < 5s.
  * Maintain a system uptime of 99.9%, translating to no more than 8.76 hours of unplanned downtime annually.
* **Usability:**
  * Allow users to complete the setup process in under 10 minutes for standard configurations.
  * Incorporate LED indicators with a minimum of 5 distinguishable patterns to communicate different modes or issues.
* **Reliability:**
  * Ensure less than 0.5% device failure rate within the first year of standard usage.
  * Design the device to endure a minimum of 10,000 hours of operation.
* **Compatibility:**
  * Interoperate seamlessly with at least 90% of smart home devices from a list of top 20 market-leading devices.
  * Ensure 100% backward compatibility with prior versions of Google Home Mini for at least two generations.
* **Power Consumption:**
  * Operate under 2w under standard operating conditions.

#### 5. Constraints & Assumptions
* **Constraints:**
  * Dimensions: The device should not exceed 100mm in diameter, 50mm in height, and 400g in weight to retain its compact form.
  * Production Cost: The unit production cost, including parts and assembly, should not exceed $25 to ensure a competitive market price.
  * Operational Cost: The operational cost we incur should not exceed $5 a year per device.
  * Timeline: The product can be developed and shipped in 24 months.
* **Assumptions:**
  * Users have a stable internet connection with a minimum speed of 5 Mbps.
  * The device is assumed to be used in typical household ambient noise levels, averaging 40-60 dB.


#### 6. Risks & Mitigations
* **Risk:** Hardware failure rate exceeds 0.5% within the first year.
    * **Mitigation:** Partner with trusted component suppliers, enforce strict QA procedures, and implement robust firmware error handling.
    * **Likelihood:** Medium. **Impact:** High.
* **Risk:** Voice recognition inaccuracies surpass the 5% threshold.
    * **Mitigation:** Continuously refine voice models, integrate adaptive learning algorithms, and solicit user feedback for system improvements.
    * **Likelihood:** High. **Impact:** Medium.
  
#### 7. Revision Log 

| Editor        | When        | What        | Why       | Decision Makers   |
| ------------- | ----------- | ----------- | --------- | ----------------- |
|               |             |             |           |                   |
|               |             |             |           |                   |

## Conclusion: A collective Endeavour

> *"Innovation distinguishes between a leader and a follower."* – **Steve Jobs**

In an era where rapid innovation is essential, maintaining clarity and focus can be challenging. The Technical Requirements Document, while seemingly just a piece of documentation, serves as a mechanism to create alignment across functions and guardrail development. Where everyone, from the novice developer to the seasoned Systems Architect, understands the significance of their contribution. It's our responsibility as technical leaders to prioritize clarity, coherence, and collaboration. It's not just about what we're building – it's about how we're building it, and the generative culture we wish to create.

This is the second of three posts covering System Design. Continue by exploring, 

* [System Design Process - [1/3] A Guide for Technical Leaders](/blog/2021/10/02/system-design-process.html)
* [System Design Process - [3/3] Crafting Technical Design Documents](/blog/2021/10/03/technical-design.html)
