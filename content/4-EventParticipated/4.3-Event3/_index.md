---
title: "Event 3"
date: 2026-06-13
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# "FCAJ Community Day - June 2026 Edition" Workshop Report

## 1. Event Objectives

The **FCAJ Community Day - June 2026 Edition** was organized to provide participants with practical perspectives on system architecture, data, DevOps, and career development in the modern technology industry. The event did not only focus on theoretical knowledge but also explored real-world implementation problems in enterprises and technology communities.
The main objectives of the event included:
- Introducing how to design a high-traffic system through the case study of building a URL Shortener service on AWS.
- Sharing the role, skills, and mindset required for a **Data Analytics Engineer** working in multinational corporations.
- Helping participants better understand the real responsibilities of a **DevOps Engineer**, including recruitment demand, salary ranges, and long-term foundational skills.
- Inspiring participants about self-development, community contribution, and the mindset of doing the right work in the technology field.

## 2. List of Speakers

The event featured several speakers from different areas of the technology industry:
- **Mr. Dinh Trung Kien**: Lead Developer at a Startup.
- **Nguyen Minh Tho**: Student & Cloud Contributor.
- **Mr. Dat Pham**: Data Analytics Engineer at a multinational corporation.
- **Mr. Cuong Nguyen**: Process Engineer.
- **Mr. Trong H. Truong**: DevOps Engineer at Endava Vietnam.
- **Mr. Danh Hoang Hieu Nghi**: AI Engineer, AWS Community Builder & SBG Leader.

## 3. Key Highlights of the Event

### 3.1. Large-Scale URL Shortener Architecture on AWS

One of the most notable sessions of the event focused on designing a scalable URL Shortener system. Although this problem may seem simple at first, it becomes much more complex when deployed at a large scale due to performance, security, latency, and traffic-handling challenges.
In a traditional approach, the system usually generates a short code at the moment a user submits a request. This approach can lead to several limitations:
- High latency because the system must process and check data in real time.
- Risk of duplicate short codes if the code generation mechanism is not well controlled.
- Difficulty scaling when traffic increases suddenly.
- Possible bottlenecks or single points of failure in the system.
To solve these problems, the speaker introduced the **Key Generation Service (KGS)** model. Instead of generating codes on demand, the system prepares a large number of short codes in advance.
Some important points in the architecture included:
- **Separating the read path and write path**: The read path and write path are designed independently so that each can be optimized based on its own traffic characteristics.
- **Pre-generating codes instead of processing on demand**: Containers running on **Amazon ECS** generate short codes in advance and push them into a queue in **Amazon ElastiCache for Redis** using the `LPUSH key_queue` command.
- **Fast code retrieval when a request arrives**: When a user wants to shorten a URL, the Spring Boot backend running on ECS only needs to use the `RPOP` command to retrieve an available code from Redis, then store the data in **Amazon DynamoDB** using that code as the primary key.
- **Reducing duplicate-code risks**: Because the codes are generated and controlled beforehand, the system can reduce the risk of collisions during URL creation.
- **Applying the Cache-aside Pattern**: When a user accesses a shortened link, the system first checks Redis. If the data is not found in the cache, it then queries DynamoDB.
- **Protecting the system at the edge layer**: AWS WAF and Amazon CloudFront are placed closer to users to block harmful requests before they reach the core system.
Through this session, I learned that even a functionally simple system can become highly complex when it needs to serve a large number of users. Therefore, system architecture must consider performance, security, scalability, and stability from the beginning.

### 3.2. The Role and Growth Mindset of a Data Analytics Engineer

The session about **Data Analytics Engineering** helped me understand more clearly how data-related work is performed in real enterprises. Depending on the business domain, the focus of data work can be very different.
In technology or e-commerce companies, Data Analytics Engineers often build recurring reports, operational dashboards, and business metric tracking systems. The goal is to detect anomalies, identify root causes, and support different departments in improving performance.
For example, in companies such as Kamereo, data can be used to monitor operational efficiency, analyze GMV, optimize orders, and support decision-making in the supply chain.
Meanwhile, in manufacturing corporations such as Colgate-Palmolive, data may come from machines, production lines, IoT devices, or factory operation systems. In this environment, Data Analytics Engineers do not only create reports but also help discover opportunities to reduce production costs, optimize equipment performance, and improve manufacturing capacity.
The speaker also emphasized four important skills for data professionals:
- **Critical thinking**: Not only looking at surface-level numbers but also asking questions and checking the reasons behind them.
- **Communication skills**: Being able to work with multiple departments to understand problems and explain analytical results.
- **Data storytelling**: Turning data into clear and meaningful stories that support decision-making.
- **Real-world problem-solving ability**: Analyzing data not just to create charts, but to generate actions and business value.
One point that impressed me was the career growth path:
`Follower` → `Learner` → `Problem Solver` → `System Thinker` → `Super Star`
This path shows that technology professionals should not focus only on job titles. What matters more is the ability to solve problems, understand the overall system, and create real contributions to the organization.
In addition, the **No-Blame Post-Mortem** culture was introduced as a valuable practice in large corporations. When a system incident occurs, the goal is not to find someone to blame. Instead, the team works together to analyze the cause, identify weaknesses in the process or architecture, and improve the system to prevent similar issues in the future.

### 3.3. A Practical View of the DevOps Engineer Role

The DevOps session helped me understand that DevOps is not only about writing CI/CD pipelines, configuring Docker and Kubernetes, or monitoring systems during incidents. In reality, the scope of DevOps work depends heavily on company size, infrastructure maturity, and the way engineering teams are organized.
The speaker also shared an overview of the DevOps market in Vietnam during 2025–2026. DevOps Engineer and Cloud Engineer remain highly demanded roles, especially as more companies move their systems to the Cloud and need to automate their operation processes.
DevOps salary levels are also considered competitive in the market. At the Junior level, the salary may range from **16 to 28 million VND per month**. At the Lead or Expert level, income can reach **65 to 100 million VND per month**, depending on skills, experience, and company scale.
However, the most important lesson is not to chase every new tool. The speaker emphasized the idea that **“Fundamentals Stay.”** Tools may change over time, but foundational knowledge remains valuable.
A strong DevOps Engineer needs to build solid foundations in areas such as:
- Linux and operating systems.
- Basic networking.
- Programming languages such as Python or Golang.
- System thinking.
- The ability to ask “Why” before looking for “How.”
I realized that if learners only memorize tool syntax, they may quickly fall behind when technologies change. In contrast, if they understand the fundamentals well, they can adapt to many different tools and working environments.

## 4. Knowledge Gained

### 4.1. Design Thinking

After the event, I learned that system design is not simply about combining different services together. Engineers need to understand traffic patterns, bottlenecks, user behavior, and operational risks in order to choose a suitable architecture.
One important lesson was the concept of **Pre-computation**. Instead of making users wait while the system performs heavy tasks in real time, we can prepare the required resources or data in advance. This approach helps reduce latency and improve the user experience.
I also gained a better understanding of the value of a **No-Blame** culture. When a system fails, the most important thing is not to criticize individuals, but to treat the incident as an opportunity to improve processes, architecture, and the operational capability of the whole team.

### 4.2. Technical Architecture

From a technical perspective, I understood more clearly how to implement **Key Generation Service** in a URL Shortener system. Combining Redis to store short codes in memory and DynamoDB to store the main data helps the system achieve both high speed and strong scalability.
I also learned more about the role of dashboards in enterprise operations. Metrics such as Fulfillment, Last Mile Cost, and Fill Rate are not just numbers. They reflect operational performance and help businesses make more accurate decisions.
In addition, the event helped me distinguish different security layers in Cloud architecture. The combination of AWS WAF, CloudFront, and AWS KMS helps reduce system load, strengthen security, and protect data.

### 4.3. Modernization Strategy

Another lesson was the transition from “making it work” to “making it meet proper standards.” In enterprise environments or large-scale systems, a product does not only need to run. It must also meet operational, security, and compliance standards.
For physical supply chains, businesses need to consider standards such as GMP, GSP, or GDP. For digital supply chains and Cloud infrastructure, standards such as ISO 27001, SOC 2, and GDPR become very important.
This helped me understand that technology engineers should not only know how to build features. They also need to understand standards, processes, and responsibilities when bringing systems into real-world environments.

## 5. Applications to Study and Work

From the knowledge gained, I can apply these lessons to my study and work in the following ways:
- **Applying Cache-aside to backend projects**: When building APIs, I can add a Redis Cache layer in front of the database to reduce query load and improve response speed.
- **Developing a System Thinker mindset**: When changing a feature or system configuration, I should evaluate its impact on other modules, Cloud costs, and overall performance.
- **Focusing on DevOps fundamentals**: Instead of only learning tools, I need to study Linux, Networking, Git branching strategies, and system operation principles more deeply.
- **Working with the spirit of “doing the right work”**: I need to build self-discipline, work responsibly, and aim to solve real problems in society.
- **Practicing and sharing knowledge back**: I should participate more in hands-on labs, experiment with new technologies, and contribute knowledge to the community.

## 6. Personal Experience at the Event

Attending **FCAJ Community Day - June 2026 Edition** on June 13, 2026, was a memorable experience for me. The event provided many practical lessons about Cloud, DevOps, data, and personal development in the technology industry.

### 6.1. Learning from Experienced Speakers

The speakers shared many real-life stories, helping me better understand the gap between learning technology in theory and applying it in a real enterprise environment.
One statement that impressed me was that we should not use AI to “turn off our brain,” but rather use AI to upgrade our own capabilities. This made me reflect on my learning method in the AI era and realize that tools only become truly valuable when users still maintain strong foundational thinking.

### 6.2. Exposure to Real Technical Architecture

The analysis of high-scale AWS architecture helped me visualize how a request passes through multiple layers of a system. From Route 53 and ALB to services running on Fargate, Redis, and DynamoDB, each component plays a specific role in ensuring performance and high availability.
Because of this, I understood that a high-availability system is not achieved simply by using Cloud services. It depends on how the request flow is designed, how responsibilities are separated, and how fallback plans are prepared for each layer.

### 6.3. Experience with Enterprise Dashboards and Tools

I also had the opportunity to explore examples of enterprise operational dashboards. Through this, I realized that raw data, when processed and presented properly, can become a powerful tool for decision-making.
A dashboard is not only used to display numbers. It also helps managers identify problems, monitor trends, and decide on suitable actions.

### 6.4. Networking and Discussion

The atmosphere of the event was open and friendly. In addition to the in-person presentations, the event also connected with many members online through Google Meet. This gave me the opportunity to listen to experiences from AWS Community Builders and expand my professional network.
I realized that participating in technology communities is a very effective way to learn, stay updated, and gain more motivation for personal growth.

## 7. Lessons Learned

After the event, I gained several important lessons:
- Tools and frameworks may change constantly, but foundational knowledge and system thinking will always remain essential.
- To grow sustainably in the technology industry, engineers need to learn through practice, build real products, and continuously improve their skills.
- AI should be used as a thinking-support tool, not as a replacement for human learning and reasoning.
- A good system should not only run successfully, but also be stable, secure, scalable, and aligned with real-world standards.
- Sharing knowledge back to the community helps me learn more deeply and create more value for the people around me.

## 8. Event Participation Images

![Event Participation Evidence Image](<images/4-EventParticipated/Event3/event3(0).jpg>)

> In summary, **FCAJ Community Day - June 2026 Edition** provided me with many practical and valuable perspectives on Cloud, DevOps, Data Analytics, and career mindset. The event not only supported my internship report but also helped me define more clearly my path toward becoming a technology engineer with strong foundations, professional responsibility, and an international-standard mindset.