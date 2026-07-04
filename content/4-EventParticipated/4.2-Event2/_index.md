---
title: "Event 2"
date: 2026-05-23
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Event Summary: “FCAJ Community Day - May 2026 Edition”

## 1. Event Objectives

The **FCAJ Community Day - May 2026 Edition** was organized to update students and technology professionals on new trends in the IT industry, especially the strong impact of AI on the labor market, software development practices, and the way enterprises apply AI in real-world scenarios.
The main objectives of the event included:
- Providing an overview of the Vietnamese IT market during the rapid development of AI.
- Sharing methods for building effective prompts, optimizing context, and controlling randomness when working with LLMs.
- Introducing Amazon Q Business, QuickSight, and MCP Server in building data analytics assistants for enterprises.
- Sharing practical experiences from Hackathon projects and how to design Enterprise-grade Multi-Agent systems.
- Providing real-world perspectives on applying AI Agents in fields that require high security and reliability, such as finance and banking.

## 2. List of Speakers

The event featured several speakers from technology companies, banks, and the AWS community:
- **Mr. Nguyen Gia Hung**: Solutions Architect at AWS Vietnam and Founder of FCAJ, sharing insights on the IT market direction in the AI era.
- **Mr. Tinh Truong**: Platform Engineer at Gothamic X, presenting about context optimization and controlling the randomness of LLMs.
- **Mr. Hai Anh**: Solutions Architect at Pacific Vietnam, sharing about Amazon Q Business and MCP.
- **Mr. Nguyen Han Thinh**: DevOps Engineer, sharing about Flat-rate Pricing and advanced security layers of Amazon CloudFront.
- **Uyen, Mach, and Ms. Thao**: The Hackathon winning team, sharing their 36-hour journey of developing the UTMorpho project.
- **FinTech Speaker**: FinTech Solutions Advisor at VPBank, sharing about Multi-Agent architecture in banking operations.

## 3. Key Highlights of the Event

### 3.1. The IT Market and Career Opportunities in the AI Era

One of the notable opening topics was the changing landscape of the IT market as AI becomes increasingly popular. The speaker introduced the “LED light paradox” as an example. When the cost of lighting decreased significantly thanks to LED lights, the demand for lighting did not decline but increased. Similarly, when AI makes coding faster and cheaper, the demand for software development may grow even stronger than before.
AI does not make software disappear. Instead, it enables more people to create products. Even people who do not major in IT, such as doctors, lawyers, or business professionals, can use AI to participate in Hackathons or build MVPs. This creates many new opportunities, but it also brings major challenges related to software quality.
A key issue emphasized in the session was that the market will likely see many products created quickly with AI but lacking stability, maintainability, or proper quality control. As a result, the demand for engineers who can fix bugs, optimize systems, refactor code, and build sustainable platforms will continue to increase.
In particular, the role of **Platform Engineering** will become increasingly important. Large organizations need engineers who can build Internal Developer Platforms, automate infrastructure, and help software development teams work more efficiently.
For the Vietnamese market, many major global technology companies are still opening Technical Hubs to take advantage of young talent. Therefore, Vietnamese engineers need to invest more in English skills, practical domain knowledge, the ability to design complete products, and the mindset to build production-ready systems instead of stopping at simple demo projects.

### 3.2. Context Optimization and LLM Randomness Control

The session on LLMs helped participants better understand how AI processes information and why the same question may sometimes produce different answers.
A common mistake when using AI is putting too many unrelated topics into the same conversation. For example, one chat session may include questions about travel, CV improvement, and software coding at the same time. This constantly changes the Context Window, making it difficult for AI to stay focused and increasing the chance of incorrect or inconsistent responses.
The speaker also explained that an LLM works as a **probabilistic engine**, meaning that the model predicts the next word based on probability. Each possible word has a score called a **logit**, and the model selects suitable words to generate the final answer.
The **Temperature** parameter controls how creative or stable the output will be:
- When Temperature is high, the answer may become more creative but also more variable.
- When Temperature is low, especially when `Temperature = 0`, the model tends to choose the word with the highest probability to produce a more stable result.
- For tasks that require accurate structures such as JSON, HTML, or code, a low Temperature should be used to reduce formatting errors.
However, an important point is that even when `Temperature = 0`, results from Cloud APIs such as OpenAI or Amazon Bedrock can still be different across multiple runs. This can happen because of parallel GPU computation, floating-point rounding differences, and inference optimization techniques used by Cloud providers.
Therefore, if an enterprise needs absolute control over model consistency, self-hosting the model on local infrastructure may be a more suitable option.

### 3.3. Amazon Q Business, QuickSight, and MCP in Data Analytics

Another notable topic was how Amazon Q Business and QuickSight can help business users analyze data more quickly. Instead of requiring users to have deep BI knowledge, the system can receive raw data such as Excel files, then automatically analyze and visualize the information into dashboards.
This helps shorten the process from raw data to analytical reports. Users only need to ask questions or provide requests in natural language, and the system can support chart generation, trend summarization, and analytical insights.
In addition, the speaker introduced **MCP - Model Context Protocol**. This protocol allows AI Agents to go beyond text-based responses and perform real actions. Through an MCP Server, AI can connect with various systems such as Jira, Confluence, Microsoft Cloud, Google Suite, or internal enterprise tools.
As a result, an AI Agent can become a true “extended arm,” supporting tasks such as scheduling meetings, sending emails, searching documents, analyzing data, and interacting with enterprise systems.

### 3.4. UTMorpho Case Study - A Hackathon Winning Project Built in 36 Hours

The Hackathon winning team shared their journey of developing **UTMorpho** within 36 continuous hours. What made the project interesting was that the team did not choose an overly broad idea. Instead, they focused on solving a very practical pain point for developers when working with AI-generated interfaces.
Normally, when AI creates an HTML/CSS interface, making a small change such as adjusting a button position, block size, or color may require the user to ask AI to regenerate almost the entire interface. This wastes time, consumes tokens, and may break the original structure.
UTMorpho solves this problem by building a Multi-Agent system on AWS Serverless. The system consists of three main Agents:
- **Vision Agent**: Reads images or sketches from the camera, then converts the information into a structured JSON format.
- **Layout Agent**: Receives the JSON data and calculates the layout, size, CSS, and structural arrangement.
- **Design Agent**: Converts the output from the Layout Agent into complete HTML/CSS source code in real time.
A standout feature of UTMorpho is its visual editor, which allows developers to drag and drop components directly on the interface. After completion, the system can export a public HTML link so team members or colleagues can view and review the result together.
The project showed that if a problem is divided properly, Multi-Agent systems can strongly support the process of turning ideas into products quickly while still maintaining control over the output.

### 3.5. Multi-Agent Systems in Banking Operations

The final part of the event presented a real-world problem in the finance and banking sector: credit evaluation for startups.
In traditional loan approval models, businesses usually need to provide several years of financial reports and clear collateral. However, many startups have only operated for a few months, do not have long-term financial reports, and mainly own intellectual property, technology, or data. Therefore, they often find it difficult to access capital through traditional evaluation methods.
The proposed solution was to design a **Multi-Agent** system that analyzes multiple non-traditional data sources. Each Agent takes on a specialized role:
- **Credit Committee Agent**: Acts as the coordinator, assigns tasks, and summarizes the final report.
- **Financial Analyst Agent**: Analyzes short-term financial reports, cash flow, and business operations.
- **Market Research Agent**: Evaluates the market, competitors, market size, and industry trends.
- **Team Evaluator Agent**: Assesses the founding team through technology profiles, social media, and public activities.
- **Risk Assessor Agent**: Controls risk, filters input and output data, prevents sensitive information leakage, and protects against Prompt Injection.
In the banking environment, security and compliance are extremely important. Therefore, the Risk Assessor Agent plays a key role in ensuring that the AI system does not exceed its allowed scope and does not expose sensitive information.

## 4. Knowledge Gained

### 4.1. Design Thinking

After the event, I understood that when building a technology solution for an enterprise, the most important thing is not only whether the technology is new. More importantly, the solution must solve the right problem.
A technology product needs to answer the following questions:
- Who will use it?
- What will they use it for?
- Why do they need this solution?
- When is the right time to deploy it?
I also learned the concept of **Separation of Concerns** in Multi-Agent design. Each Agent should be responsible for one specific task, with a clear role and a limited scope. This helps avoid Context Window overload and makes the system easier to control.

### 4.2. Technical Architecture

From a technical perspective, I gained a better understanding of how to control the stability of LLMs through parameters such as `Temperature`, `Top P`, and `Repeat Penalty`. Depending on the desired output, users need to choose suitable configurations. For example, creative content may use a higher Temperature, while JSON or code should use a lower Temperature to maintain structural accuracy.
I was also introduced to the network infrastructure behind CloudFront, AWS Backbone, and Points of Presence. CloudFront not only helps accelerate content delivery but also supports Origin Server protection against large-scale attacks such as DDoS or Syn Flood through request handling and Syn Proxy mechanisms.
In addition, the event emphasized the role of **Terraform** and Infrastructure as Code. Instead of configuring resources manually through the AWS Console, engineers should manage infrastructure as code to make systems easier to reproduce, version, and deploy across different AWS Regions.

### 4.3. Modernization Strategy

An important lesson is that AI projects in enterprises cannot stop at the demo stage. To be truly deployed, a project needs to prove its value through specific numbers.
Enterprises often care about metrics such as:
- How much cost the project can save each year.
- How long the payback period is.
- How much productivity increases.
- Whether operational risks are reduced.
In other words, an AI solution needs a clear ROI to convince business leaders.
Continuous testing is also essential. An AI system may generate incorrectly formatted outputs or unexpected responses. Therefore, engineers need to test carefully across multiple layers, especially downstream services, to ensure that the system remains stable even when AI outputs are not as expected.

## 5. Applications to Study and Work

From the knowledge gained, I can apply the lessons to my study and work in the following ways:
- **Configuring Temperature more appropriately**: When asking AI to generate JSON, HTML, or code, I will prioritize using `Temperature = 0` or `0.1` to reduce structural errors.
- **Breaking down problems when working with AI**: Instead of asking one chat session to handle an entire process, I will divide the problem into smaller tasks and check the output step by step.
- **Applying Multi-Agent thinking**: For complex projects, I can separate roles such as requirement analysis, layout design, code generation, and review to reduce errors.
- **Improving data security**: When working with AI in an enterprise environment, input and output filtering layers are necessary to prevent sensitive data leakage.
- **Learning Terraform and Infrastructure as Code**: I will gradually move from manual configuration to infrastructure management through code for better control and easier redeployment.
- **Focusing on backend fundamentals**: AI is a supporting tool, but system thinking, API knowledge, data structures, and software engineering fundamentals remain the most important factors for developers.

## 6. Personal Experience at the Event

For me, **FCAJ Community Day - May 2026 Edition** was a memorable event because the content was not only theoretical but also connected to real enterprise problems.

### 6.1. Learning from Experienced Speakers

The speakers from AWS Vietnam, VPBank, VIB, Pacific Vietnam, and Gothamic X provided many practical perspectives. I understood more clearly the difference between a personal project or a small school assignment and a real product that serves a large number of users.

Especially in the banking sector, a system does not only need to run. It must also be secure, stable, compliant, and capable of protecting user data.

### 6.2. Exposure to Modern Technical Models

The event helped me better visualize how Multi-Agent systems can work together in a real system. Each Agent takes on a separate responsibility, from financial analysis and market research to team evaluation and risk control.

I also learned that even when Temperature is set to 0, AI can still produce different results due to infrastructure factors and inference optimization behind the scenes. This gave me a more realistic perspective, instead of assuming that AI always produces perfectly identical results.

### 6.3. Experience with Modern Tools

The Amazon Q Business and QuickSight demo showed that data analysis is becoming much more accessible. Users do not need to write complex commands. They can use natural language to request analysis, visualization, and dashboard generation.

In addition, MCP opens a new direction for AI Agents, allowing AI to connect with tools and perform actions instead of only replying with text.

### 6.4. Networking and Discussion

The event created an open space for students to meet, exchange ideas, and connect with people who share similar interests. Activities such as discussion, Q&A, and Lucky Draw made the atmosphere more friendly and encouraged participants to be more proactive.
I realized that participating in technology communities not only helps me learn new knowledge but also expands my network, creates collaboration opportunities, and builds confidence.

## 7. Lessons Learned

After the event, I gained several important lessons:
- AI makes software development faster and cheaper, but it also increases the demand for engineers with strong system thinking.
- AI should not be used by blindly copying results without understanding the underlying logic, because this can create serious risks in production environments.
- A good technology system is not only one that runs, but one that is secure, reliable, maintainable, and able to create real value for users.
- In enterprises, technology must be connected with business effectiveness, measurable ROI, and a serious testing process.
- To grow sustainably in the IT industry, engineers need to focus on technical fundamentals, English skills, domain knowledge, and the ability to build real products.

## 8. Event Participation Images

![Event Participation Evidence Image](<images/4-EventParticipated/Event2/event2(0).jpg>)

> In summary, **FCAJ Community Day - May 2026 Edition** provided me with valuable knowledge about AI, Prompt Engineering, Multi-Agent systems, Cloud, security, and career mindset. The event helped me understand that in the AI era, engineers do not only need to know how to use new tools. They also need system thinking, product responsibility, and the ability to create real value for enterprises.