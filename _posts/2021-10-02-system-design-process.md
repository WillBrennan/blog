---
layout: post
title: System Design Process - [1/3] A Guide for Technical Leaders
tags: [System Design, Technical Excellence]
---
In today's competitive landscape, the ability to create robust, scalable, and efficient systems is no longer a luxury - it's a necessity. From tech giants like Amazon & Google to emerging start-ups, the common thread among successful organizations is an effective system design process. 

Technical leaders play a pivotal role in steering these processes, ensuring coherent direction across functions and teams. You are responsible for working backwards from the customer's problem to the long-term product direction and technical delivery. This post aims to provide an insightful guide for technologists and thought leaders, helping them navigate the complexities of delivering groundbreaking systems. 

## Guiding Principles 
You will often be presented with tough or unfamiliar situations in System Design & Development. Guiding Principles are not abstract ideals but a guiding light in these situations and should be thought about in every step of the process. Your Guiding Principles should be refined over time as the organization learns from both successes and missteps; this is a vital mechanism to ensure organizational learning, consistent decisions, and a cohesive culture. Based on Amazon's Leadership Principles and lessons learned from retrospectives; here are my recommended Guiding Principles:

#### Stay Customer Obsessed
The only way to grow a business is through loyal customers; solve their problems and earn their trust. It's easy to be distracted by new technologies or your own wishlist.

#### Stay Concise
A customer's problem should be described completely in 1~2 sentences. If you struggle to be succinct, it usually indicates a lack of understanding the customer's perspective or that it is not an urgent problem (hair-on-fire).

#### Think Big
Thinking small can be a self-fulfilling prophecy. Aim to be inspirational, not mundane. While thinking big keeps teams motivated and resilient to setbacks, it's crucial to remain grounded. Understand the feasibility of your Technical Requirements and push just beyond it.

#### Invent & Simplify
Simplify across product, technology, and processes. The simpler you can make it, the quicker you can iterate, onboard new staff, and identify the root-cause to problems. When there is an urgent issue, it's easier to add complex temporary solutions to a simple system than it is a complex one.

#### Insist on High Standards
Push for technical, execution, and organizational excellence. Raise the bar on the monitoring, modularity, robustness, and documentation. It is the only way to build the organization's momentum while Thinking Big.

## Establish Big Picture 
Before you begin, step back and take a panoramic view. See beyond the immediate details and context, understand the needs of your customers and companies. Not just in the current moment, but how they will evolve over time. Think adaptive, ensuring systems stay relevant as landscapes shift.

By putting customers front and center, promoting cross-functional collaboration, and future-proofing your approach, you're setting yourself up to not just hit but surpass targets, promoting sustained long-term value over fleeting short-term results.
#### Engage with the Customer
Customers are your compass. Dive deep: 

* **Listen Actively:** Hear both what they say and do not say. The unsaid holds invaluable insights.
* **Validate Assumptions:** A small misinterpretation can misalign the product direction.
* **Iterate on Feedback:** Keep the dialogue dynamic, refining as you move.
* **Validate Direction:** Understand how their priorities will change in the coming years.

#### Engage with each Function
Each function brings its expertise and focus to the table. Engaging with each of them ensures you solve the customer problem in a way that earns their trust and benefits your venture. You should consider each aspect, even if they don't exist as a function in your company.

* **Product Management:** Focuses on long-term product direction, aligning your customer growth with your business strategy.
* **Sales & Marketing:** Focuses on solving short-term customer pain points, and earns the most customer trust.
* **Tech Teams:** Focuses on feasibility and delivery of the system.
* **User Experience (UX):** Focuses on removing frustrating interactions, ensuring the product delights.
* **QA:** Focuses on ensuring high standards; so your products earn the customers trust.

## Establish Coherent Direction
Have you ever wondered why some projects sail smoothly towards success while others sink in the abyss of miscommunication and inefficiencies? You need to establish a structured approach that is logical, consistent, and unifies the organization. A Coherent Direction. Following the guiding principle of Customer Obsession, you should work backwards from the customer problem to a concrete plan for solving it across Product, UX, Tech, and QA.

This is done by leveraging several documents. They are often discussed separately but together they form a cohesive tool for building a coherent direction across functions. In essence, these documents provide a structured approach to move from a high-level vision (PR/FAQ) to a detailed plan of execution (Roadmap) while keeping both the customer and technical perspectives in mind.

* **PR/FAQ (Press Release and Frequently Asked Questions):** This is a future-oriented document that details what a new product or feature will look like when it's launched. It's written as though it has already been released, and a press release is being made to announce it. The FAQ should define how you measure success, the risks, and market window. Helps to establish the vision for the product/feature. It's the "north star" for the project. Provides clarity on customer benefits and ensures the team has a clear, customer-centric goal.
* **User Journeys:** Describe step-by-step how users will interact with the system. They capture the user's perspective. Ensuring that the designed system meets the actual needs of the user and provides a seamless experience. Helping to identify gaps, pain points, and areas for optimization. It provides the depth required for Technical Requirements & Design.
* **Technical Requirements:** Providing a detailed description of what the system should and shouldn't do. Defining the specific functionalities, constraints, performance metrics, and other criteria the system needs to fulfill.It's the bridge between the user-oriented perspective and the technical design. It is the contract with the Development Team defining delivery of a successful product.
* **Technical Design:** A deep dive into the system's architecture, database design, algorithms, and more. It's the blueprint for how the system will be built. It's where trade-offs are made, technologies are chosen, and the system's intricacies are laid out. Over the span of years, this consistent reference point will mitigate drift in the design, minimizing the technical debt incurred and facilitate efficient extensions to the system.
* **Roadmap:** Provides a timeline for the project, breaking down phases, work-streams, and delivery milestones. It creates a clear picture of when different parts of the system will be developed, tested, and released. It determines the resource allocation and sets expectations. It will be used to coordinate delivery through from the Development Team, QA, Customer Success, and the Customers themselves.

> *"Perhaps the most common error of a smart engineer is to optimize a thing that should not exist."* - **Elon Musk**

![Flow Diagram for System Design Process](/blog/assets/img/system-design-process/process.png)

As you cascade from the high-level vision (PR/FAQ) to the detailed plan of execution (Roadmap) you may find that it isn't feasible. It is an iterative process to define the system and what is possible. You may have to adjust your PR/FAQ if the User Journeys aren't seamless, and you may have to adjust your Technical Requirements if the Development Team can't find a Technical Design that meets it in the required time and resources. 

Focus on the minimum writing at each stage required to create a coherent direction. 

This is a simplified and software focused version of the [System Design Process used by NASA](https://www.nasa.gov/seh/4-design-process).

## Establishing Momentum
Designing a system is just one side of the coin. The crucial side is establishing momentum during system development. Momentum – the force that keeps teams pushing forward with enthusiasm, even through challenges – is vital to ensure your system doesn't just remain a concept on paper but evolves into a tangible product that delights users. 


From my experience here's how you can establish and maintain momentum.

#### 1. Set Clear and Ambitious but Achievable Milestones
Set milestones in your Roadmap and think of them as mini product launches. Ask yourself how you can continuously deliver customer value? Each milestone provides an opportunity to assess the product's trajectory, make course corrections, and celebrate the progress. Such celebrations can be pivotal in re-energizing the team and reminding them of the larger vision. Push the boundaries of what they believe they can achieve to raise the bar of executional excellence.

Your role as a leader is to ensure every member knows their role in achieving the milestone and how that milestone impacts the product and company. This creates clarity of purpose.

#### 2. Foster Open and Written Communication
Embracing a culture of open, long-form written communication ensures clarity and depth. Consistently formatting documentation from Sprint Planning & Review allows teams to engage in meaningful discussions, reduce understandings, and provide a reliable historical reference. This literally keeps everyone on the same page. Written communication scales far better across the organization, time, and regional offices. 

This should be coupled with an open-door policy. Encourage teams to bring forth issues as they arise, rather than waiting for scheduled meetings. 

#### 3. Implement a Transparent Tracking System
A robust, transparent tracking system is vital for system development, merging both technical and non-technical teams such as Customer Success or UX. Beyond tools like JIRA, a holistic approach provides visibility into progress, aligns teams, and swiftly pinpoints bottlenecks. A shared "Definition of Done" ensures consistent task completion criteria, while RAG (Red-Amber-Green) statuses for each workstream offer a clear health check, streamlining decisions and prompting timely interventions.

This can be done as part of the Sprint Review process.

#### 4. Celebrate Wins and Learn from Losses
Every milestone achieved, regardless of its size, deserves recognition. Equally important is the ability to treat setbacks as learning opportunities.

Publicly acknowledging and celebrating team achievements boosts morale. On the flip side, when things don’t go as planned, lead with empathy. Encourage a culture of retrospection to understand what went wrong without blame and how it can be avoided in the future. 

Use retrospectives post milestones to get feedback from teams. This continuous feedback loop can be invaluable in refining processes, making them more efficient and effective over time.

#### 5. Stay Adaptive
The landscape is ever-evolving. What's considered revolutionary today might become obsolete tomorrow. Always have an ear to the ground. Be open to pivoting strategies based on market feedback, technological advancements, or shifts in customer behavior. 

Promote a culture of continuous learning within your organization. Encourage them to stay updated with the latest in tech and be open to implementing new methodologies or tools that can enhance efficiency.

Stay curious yourself. Dedicate time to research, explore new tech stacks, or attend conferences. Bringing fresh perspectives to the table can be the differentiator in delivering a product that's not just good, but exceptional.

## Conclusion
System design and development is a balance between meticulously designing the system and driving relentless momentum to bring it to fruition. As we've explored, successful system design hinges on a foundation of guiding principles, a clear vision that anticipates the future, and a coherent structured direction. 

Even the most elegant design remains just a blueprint without momentum. By setting ambitious but achievable milestones, championing transparency, consistently tracking progress, and fostering a generative culture that both celebrates successes and learns from setbacks, technical leaders can translate visions into reality. 

Your role is pivotal in this balance. Embrace these insights, and you'll not only design but also deliver systems that resonate, innovate, and captivate your organization and more importantly your customers. 

This is the first of three posts covering System Design. Continue by exploring, 

* [System Design Process - [2/3] Crafting Technical Requirements Documents](/blog/2021/10/02/technical-requirements.html)
* [System Design Process - [3/3] Crafting Technical Design Documents](/blog/2021/10/03/technical-design.html)
