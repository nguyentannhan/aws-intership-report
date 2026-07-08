---
title: "Event 1"
date: 2026-06-27
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Reflection Report on “FCAJ Community Day - June 2026 Edition”

### 1. Objectives of the Event

- To help participants gain a clearer understanding of career development paths in the Cloud/DevOps field, especially in the context of AI creating many changes in the labor market.
- To share how to build an AI-powered voice assistant system that can process Vietnamese and be applied to real-world problems in the Banking industry.
- To introduce an approach to automating operational processes, monitoring, and infrastructure incident handling through a DevOps AI Agent.
- To present how Amazon Q Business, the MCP protocol, and private connection models can be applied to improve human resource management efficiency while ensuring the security of internal data.

### 2. List of Speakers

- **Mr. Steve Trần** - Founder of Cloud Thinker, former Solution Architect at Amazon/AWS Vietnam.
- **Mr. Hiếu Nghị** - Cloud Specialist from Renova Cloud.
- **Mr. Kiệt** - Solution Engineer from Student Video Group.
- **Mr. Trung** - Founder and CEO of R AI, who previously founded a technology startup in the United States that was later acquired by a Google subsidiary.
- **Mr. Nguyên Nguyễn & Ms. Bảo** - Cloud Engineers from Cloud Kinetics.
- **Mr. Trường (Owen) & Ms. Minh Anh** - Solution Architects from Noventic.
- **Mr. Toàn Nguyễn** - AWS Security Builder.

### 3. Key Highlights of the Event

#### 3.1. Career Path Orientation and the “Agentic Ops” Foundation for Cloud Infrastructure

- **Changes in the recruitment market**: The job market for developers is undergoing a clear shift. Many businesses, from large corporations to startups, are reducing recruitment for general positions and prioritizing candidates with experience who know how to leverage AI to accelerate product development and code deployment.
- **The role of humans in Production infrastructure**: For Cloud Infrastructure systems, especially in Production environments, every minute of downtime can cause major financial and reputational damage to a business. Therefore, AI cannot completely replace humans but mainly acts as a supporting tool. Businesses still need highly skilled engineers to make accurate decisions in critical incident situations.
- **Cloud Thinker’s Agentic Platform solution**: Cloud Thinker introduced its direction of building an Agent operating system that can understand system architecture from the source code layer to the business logic layer. This platform supports engineers in reading logs, analyzing incident root causes in a short time, automatically reviewing source code, evaluating errors, and optimizing Cloud costs based on the FinOps approach. A notable point is that the system is not fixed to a single infrastructure provider.

#### 3.2. Voice AI Agent Architecture for the Vietnamese Market

- **Challenges of the Vietnamese language**: Modern Speech-to-Speech models in the world currently support English better than Vietnamese. Since Vietnamese has many characteristics related to tones, regional accents, and communication context, directly applying international models to the Vietnamese market still has many limitations.
- **Three-layer Voice Agent model: STT → LLM → TTS**: To deploy a call center Voice Bot for major banks such as VPBank or VIB, a suitable solution is to divide the processing workflow into three main layers:
  1. *Speech-to-Text (STT)*: The system receives real-time audio and converts speech into streaming text.
  2. *Large Language Model (LLM)*: After being converted, the text is sent to a large language model to process business logic, understand customer requests, and generate responses in text form. This layer helps businesses control response content, reduce the risk of AI giving answers in the wrong context, and support system tool calls such as locking bank cards, verifying citizen ID information, or checking customer information.
  3. *Text-to-Speech (TTS)*: The text response is then converted into natural speech. The system can handle gender-based forms of address such as “Anh/Chị” and apply turn-taking mechanisms to handle situations where users speak intermittently or interrupt during the conversation.

#### 3.3. Automating Production Incident Handling with a DevOps AI Agent

- **Difficulties in operating large systems**: When a system encounters incidents such as application crashes, increased latency, or abnormal service responses, DevOps teams often need a lot of time to investigate. This is because logs, traces, and monitoring data are usually scattered across multiple places, making the process of identifying errors and restoring the system take longer.
- **The four-step workflow of the DevOps Agent**:
  - *Step 1 - Incident classification*: AI receives trigger signals from CloudWatch alerts, then automatically collects and groups logs related to the incident.
  - *Step 2 - Root cause investigation*: Based on the system topology diagram that has been learned beforehand, AI proposes possible error hypotheses and performs analysis to identify the root cause.
  - *Step 3 - Remediation proposal*: AI generates an incident-handling scenario in a safe direction. However, before execution in the Production environment, the proposed solution still requires human confirmation through a Human-in-the-loop mechanism to reduce risks.
  - *Step 4 - Long-term improvement*: After the incident is resolved, the system continues to suggest optimization methods to prevent similar errors from recurring in the future.
- **Operational cost**: The service is charged based on the actual runtime of the Agent. The shared cost is approximately **0.083 USD/second** for the entire process of computation, topology construction, and incident handling.

#### 3.4. Applying Amazon Q Business and the MCP Protocol in Human Resource Management

- **Problems in traditional recruitment processes**: HR departments often spend a lot of time reading and filtering CVs manually. This process can last from one to two months, may overlook suitable candidates, and sometimes the evaluation results can be influenced by personal judgment instead of clear data.
- **Building an HR Assistant with Quick Desktop**: The speaker presented how to upload structured Markdown files into the system so that AI can automatically configure processing skills. The tool can perform OCR and deeply analyze CV data, including scanned files or images. After that, AI compares candidate information with the requirements in the JD to score and classify candidates into levels such as Strong, Good, or Low, while also supporting interview schedule synchronization and email drafting for candidates.

#### 3.5. Private Security Solution and Secure MCP Server Connection

- **Risk of data leakage in Enterprise environments**: When AI Agents are connected to third-party platforms such as Gmail, Jira, GitHub, or Zalo through Public Endpoints, businesses may face many security risks such as Prompt Injection, Man-in-the-middle attacks, or internal data leakage.
- **Secure internal connection architecture through VPC Connection**:
  - The entire MCP Server is deployed inside the enterprise’s **Private Subnet**, limiting direct access from the public Internet environment.
  - **VPC Connection** combined with an **Interface Endpoint** is used so that Amazon Q Business can privately query internal data without going through the public Internet.
  - Traffic is distributed through an **Application Load Balancer (ALB)** combined with TLS certificates from AWS Certificate Manager (ACM) to encrypt data in transit.
  - User authentication is handled through **Amazon Cognito**, helping control access permissions more securely.
  - *Estimated cost*: This private infrastructure model costs around **250 - 350 USD/month**, including components such as Route 53 Resolver, ALB, EC2, and related services. This is a necessary cost to ensure that internal data flows are more strictly protected.

### 4. Knowledge Gained

#### 4.1. Design Thinking

- **Thinking about collaboration between AI and humans**: AI should be viewed as a tool that supports and amplifies human work capabilities rather than a solution that completely replaces humans. In particular, for tasks related to source code fixes or direct intervention in Production infrastructure, humans must still be the final reviewers.
- **Zero Trust security mindset**: When deploying AI in sensitive fields such as Banking, Finance, or Healthcare, the most important factor is not only making the system work, but also ensuring security, privacy, and compliance with legal requirements.

#### 4.2. Technical Architecture

- I gained a clear understanding of the **three-layer Voice Bot model**, including STT, LLM, and TTS, as well as the role of each layer in processing Vietnamese conversations.
- I recognized the importance of building diverse training data, especially regional voice data, to improve the end-user experience in the Vietnamese market.
- I understood how AI is granted permissions and isolated in its operating environment through the concept of **Agent Space** in the DevOps Agent ecosystem.
- I learned how to design a **VPC Connection** model combined with an MCP Server to create a closed, secure data transmission path suitable for enterprise environments on AWS.

#### 4.3. Modernization Strategy

- **Modernizing processes through a platform-based approach**: Bringing AI Agents into large enterprises is not simply about adding a new tool. Businesses need to restructure a large part of their traditional operating processes and build a suitable transformation roadmap through Managed Services so that users can gradually become familiar with and apply the technology step by step.

### 5. Application to Study and Work

- **Optimizing CVs for AI Filter systems**: When writing a CV, information should be presented clearly and structurally, while using technical keywords that closely match the requirements in the JD. This helps CVs become easier for automated AI screening systems to identify and evaluate accurately.
- **Practicing with DevOps AI Agent**: The two-month free trial program of DevOps Agent on AWS Console can be used to experience configuration, topology mapping, and simulated incident analysis on small clusters.
- **Learning more about the MCP protocol**: I can study how to build a basic MCP Server in a local environment to understand how AI tools can connect to personal or enterprise data in a controlled way.
- **Focusing on foundational knowledge**: It is not advisable to rely entirely on automatic AI code generation tools. Instead, I should continue strengthening core knowledge such as Backend development, computer networking, JWT, security, Terraform, and Infrastructure as Code so that I can take control of the system when real incidents occur.

### 6. Personal Experience at the Event

Participating in the **“FCAJ Community Day - June 2026 Edition”** event at the office spaces on the 26th and 36th floors was a meaningful learning and networking experience. The event gave me a more practical perspective on how GenAI is being applied in large Enterprise environments, especially in fields that require high security and stability.

#### 6.1. Learning from Experienced Speakers

I had the opportunity to listen to real-world sharing from founders, Cloud experts, and experienced engineers such as Mr. Steve Trần from Cloud Thinker and Mr. Trung from R AI. Their stories about career development journeys, especially the process of growing into a Solution Architect position at Amazon, gave me a lot of motivation for self-learning and future career orientation.

#### 6.2. Observing Real Technical Demonstrations

One of the highlights of the event was the live demo sessions. I observed a Voice Agent system responding to Apple product information on the Bedrock Agent Core platform, as well as a DevOps Agent demo that automatically scanned and mapped more than 300 resource relationships in a system within about 15 minutes.

The flow diagram presentations also helped me better understand how language processing works in a banking environment, where accuracy, safety, and information control are extremely important.

#### 6.3. Accessing Modern Tools

Through the presentation about Amazon Quick Desktop, I understood more clearly how users without deep technical backgrounds can still make use of data and perform basic analysis tasks. This tool helps automate many repetitive tasks in the human resources department, from reading CVs and evaluating candidates to supporting interview scheduling.

#### 6.4. Networking and Exchanging Ideas in the Community

The event atmosphere was very open and lively. Direct Q&A activities, discussions with speakers, and minigames helped participants connect with one another more easily. This was also a good opportunity for students to get closer to the community of engineers, experts, and people who are working in the industry.

#### 7. Key Takeaways

- After the event, I realized that technology should not be pursued blindly. What matters more is understanding the business problem, understanding the real needs of the enterprise, and choosing the right technology to solve that problem.
- In addition, every architectural solution involves trade-offs. No matter how modern a system is, it still needs to balance performance, latency, security, scalability, and operational cost.

#### 8. Some Photos from the Event

![Proof photo: event participation](<images/4-EventParticipated/Event1/event1(0).jpg>)

> In summary, the June Community Day session provided me with a lot of practical knowledge about Agentic Ops, Voice AI Agent, DevOps AI Agent, and AI security in enterprise environments. More importantly, the event helped me develop a clearer systems-thinking mindset, understand the role of AI in modern work, and gain more confidence in my future career direction.
