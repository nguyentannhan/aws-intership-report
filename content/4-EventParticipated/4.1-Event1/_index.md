---
title: "Event 1"
date: 2026-05-09
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# "FCAJ Community Day" Workshop Report

## Event Purpose

The **FCAJ Community Day** workshop was designed to help IT students develop a stronger learning mindset, improve their ability to work with AI tools, and better understand the expectations of the technology industry.
The main objectives of the event included:
- Introducing scientific approaches to building motivation and maintaining discipline in self-learning through Dopamine management.
- Sharing advanced Prompt Engineering techniques to improve productivity when working with Large Language Models.
- Providing practical career guidance for third-year and fourth-year IT students based on real recruitment expectations.
- Presenting the BMX/BMM software development method as a way to apply AI Agents more effectively while reducing hallucination risks.

## Speakers

The workshop featured several speakers with practical experience in technology, software development, and career mentoring:
- **Mr. Long**: Shared methods for building learning habits through brain-hacking techniques.
- **Mr. Thinh**: Presented the topic of Ultimate Prompt Engineering.
- **Mr. Khang**: Solutions Architect at Cloud Kinetics, with experience in recruitment and career orientation for technology students.
- **Ms. Thao**: Software Developer at Vietnam International Bank (VIB).

## Main Highlights

### Brain-Hacking Method for Building Learning Motivation

One of the most interesting topics in the workshop was how to make learning more engaging by understanding how the brain works. The speaker explained that Dopamine is not only released when a reward is received. Instead, it becomes especially active when the brain expects a reward to happen soon.
This explains why social media platforms such as TikTok can keep users engaged for a long time. Users continue scrolling because they are curious about what will appear next. The same mechanism can be applied to learning if the learning process is designed to create curiosity, progress, and satisfaction.
The learning cycle can be summarized as:
**Curiosity → Experimentation → Discovery → Satisfaction**
Some practical techniques were introduced:
- **Loss Aversion**: Learners can use a physical calendar or an application to track daily study streaks. Once a long streak is built, the fear of losing it encourages learners to continue studying, even if only for a short time each day.
- **Breaking down large goals**: Instead of setting a difficult goal such as studying AWS for many hours, learners should divide the goal into smaller tasks, such as learning one AWS service in 10–20 minutes.
- **The 2-Minute Rule**: If a task can be started or completed within two minutes, such as opening a document or reading one page, it should be done immediately to reduce procrastination.
- **Gamification and feedback**: Learners can create a personal reward system using XP points, ranks, progress levels, or random rewards after each study session to make learning feel more motivating.

### Ultimate Prompt Engineering

The Prompt Engineering session showed that working effectively with AI requires more than simply asking a question. A good prompt should be clear, structured, and specific so that the AI can understand the task correctly.
A strong prompt usually contains seven important elements:
1. **Role**: Defines the role that the AI should take.
2. **Instruction**: Explains what the AI needs to do.
3. **Context**: Provides background information.
4. **Input Data**: Gives the information that the AI should use.
5. **Output Format**: Specifies how the answer should be presented.
6. **Examples**: Provides sample answers or patterns.
7. **Constraints**: Sets limits, rules, or special requirements.
The speaker also discussed **tokens**, which are the units that Large Language Models use to process text. A key point is that Vietnamese text often consumes more tokens than English text, which can increase API costs when AI systems are used in business environments.
Several advanced reasoning techniques were also introduced:
- **Chain of Thought (CoT)**: Guides the AI to reason step by step before producing an answer.
- **Tree of Thought (ToT)**: Allows the AI to explore multiple possible reasoning paths and compare them before choosing the best solution.
- **Self-Consistency**: Generates several independent reasoning results and selects the answer that appears most consistently.
The session also included a case study about the **Prompt Optimizer Project**. This project uses a serverless AWS architecture to automatically optimize prompts. The system combines services such as CloudFront, S3, Amazon Cognito, API Gateway, AWS Lambda, DynamoDB, and Amazon Bedrock to build a scalable and secure AI-based solution.

### Career Orientation for IT Students

Another important part of the workshop focused on career preparation for IT students. The speakers emphasized that AI can greatly improve productivity, but only when users understand what they are doing.
If students only copy AI-generated results without understanding the logic behind them, AI may actually make their skills weaker. However, if students know how to think critically, verify the output, and ask the right questions, AI can become a powerful tool that increases their productivity.
Some key career lessons included:
- **Focus on “Why” instead of only “What”**: Students should not only ask how to complete a task, but also why a specific architecture, database, or technology is chosen.
- **Do not depend blindly on AI**: A perfect-looking answer generated by AI is not valuable if the student cannot explain it.
- **Integrity in work**: A responsible engineer does not only complete the basic requirements. They also think about edge cases, risks, and possible errors even when no one is watching.
- **Long-term career vision**: Fresh graduates should not focus only on salary. They should also consider experience, knowledge, professional network, and future growth opportunities.
The speakers also introduced the three-circle career model. A suitable career path should balance:
- What the person enjoys doing.
- What the company or market needs.
- What benefits the person receives from the work.
In addition, the workshop explained five common factors used by companies to evaluate candidates:
- **Attitude**
- **Education**
- **Experience**
- **Exposure**
- **Talent**
Among these factors, attitude is especially important for fresher-level candidates because companies value willingness to learn, responsibility, and honesty.

### BMX/BMM Software Development Method with AI Agents

The workshop also introduced the BMX/BMM method, which is a structured approach for applying AI Agents in software development. Instead of putting all requirements into one long AI conversation, this method separates the development process into different roles, such as:
- Project Manager
- Architect
- Product Owner
- Scrum Master
- Developer
- Reviewer/QA
This approach helps reduce hallucination, which happens when AI generates incorrect information or faulty code. When the context window becomes too large or the instructions are unclear, AI can easily lose focus. BMX/BMM solves this by breaking the Product Requirement Document and system architecture into smaller Epics and Stories.
Each Story follows a controlled workflow:
**Draft → Approved → Review → Done**
The Developer Agent only works on Stories that have already been approved. After that, the Reviewer Agent and QA Agent check the output, identify issues, and continue the review process until the result meets the required quality.
The main idea of this method is that developers do not only manage source code. They must first create clear, detailed, and structured documentation so that AI Agents can generate better and more reliable results.

## Key Takeaways

### Design Thinking

After joining the workshop, I realized that learning technology should not be about trying to master everything at once. For example, AWS has hundreds of services, but students do not need to learn all of them immediately. It is more important to deeply understand the core services first and know how they can be combined in a real architecture.
The workshop also helped me understand the importance of critical thinking when using AI. Instead of accepting AI answers immediately, I should question the reason behind each suggestion and evaluate whether it truly fits the problem.

### Technical Knowledge

I learned how to build better prompts by using the seven essential prompt components. This structure can help improve the quality of AI responses in tasks such as requirement analysis, documentation writing, planning, and programming support.
I also gained a better understanding of how token usage affects the operating cost of AI systems. In real projects, prompt length, language choice, and conversation history can all influence the cost of using LLM APIs.
In addition, I learned how multiple AI Agents can work together in a software development workflow. By assigning each Agent a specific role and limiting its context, the system becomes easier to control and less likely to generate unrelated or incorrect outputs.

### Modern Development Strategy

A major lesson from the event is the importance of **Document-Driven Development**. In the AI era, the value of a developer is not only in writing code manually, but also in describing problems clearly, creating accurate technical documents, and checking the quality of AI-generated outputs.
The workshop also reminded me that career development is a long-term journey. Mistakes are not always negative. They can become valuable learning experiences if I reflect on them and improve continuously.

## Practical Applications

The knowledge from this workshop can be applied to my study and future work in several ways:
- **Improving daily prompt writing**: I will use the seven prompt components when working with ChatGPT, Gemini, or other AI tools to get more accurate and useful responses.
- **Applying “Why” thinking to projects**: For each technical decision, I will ask why that solution is chosen, what alternatives exist, and what trade-offs need to be considered.
- **Building a consistent self-learning habit**: I can use study streaks, the 2-minute rule, and small learning goals to maintain daily progress in learning new technologies.
- **Experimenting with AI Agent workflows**: I can study the BMX/BMM method and try applying it to personal or group projects to manage tasks, documentation, and testing more effectively.
- **Improving collaboration skills**: I should participate more actively in tech communities, workshops, and meetups to practice communication, presentation, and technical discussion.

## Personal Experience at the Event

For me, **FCAJ Community Day** was a valuable experience because it provided both technical knowledge and practical career advice. The event helped me understand not only how to use AI tools better, but also how to think and work more professionally in the technology field.

### Learning from Experienced Speakers

The sharing from Mr. Khang, Ms. Thao, and other speakers gave me a clearer view of how companies evaluate young candidates. I realized that GPA or a polished final result is not enough. What matters more is whether I truly understand my work and can explain the decisions behind it.

### Exposure to Real Technical Examples

The serverless architecture demo of the Prompt Optimizer Project helped me understand how AWS services can work together in a real system. Services such as Amazon Bedrock, DynamoDB, Lambda, API Gateway, Cognito, S3, and CloudFront showed how a modern system can be designed to be scalable, secure, and flexible.

The explanation of techniques such as Chain of Thought, Tree of Thought, and Self-Consistency also helped me see that using AI effectively requires method and structure. It is not just about entering a simple question and waiting for an answer.

### A New Perspective on AI

After the workshop, I understood that AI should not replace human thinking. If used incorrectly, AI can make learners dependent and reduce their ability to solve problems independently.

However, when used properly, AI becomes a powerful assistant. It can support brainstorming, documentation, programming, and analysis while humans still remain responsible for understanding, verifying, and making final decisions.

### Networking and Personal Growth

The event created an open environment where students could ask questions, exchange ideas, and learn from each other. This encouraged me to become more active in technology communities and to be less afraid of making mistakes.

I also learned that asking questions, sharing ideas, and receiving feedback are important parts of personal and professional growth.

## Final Lessons

After attending the workshop, I gained several important lessons:
- Sustainable growth does not only come from talent. It comes from discipline, consistency, and a good learning environment.
- AI will not completely replace software engineers, but engineers who understand core principles and know how to use AI effectively will have a strong advantage.
- Integrity is essential in both study and work. A good engineer should think about edge cases, ask “Why”, and take responsibility for the quality of their work.
- Career development should be viewed from a long-term perspective, focusing on knowledge, experience, network, and growth opportunities rather than only short-term income.

## Event Evidence Images

- ![Event Participation Evidence Image](<images/4-EventParticipated/Event1/event1(0).jpg>)
- ![Event Participation Evidence Image](<images/4-EventParticipated/Event1/event1(1).jpg>)

>Overall, **FCAJ Community Day** was more than a technical workshop. It helped me strengthen my learning mindset, understand how to use AI responsibly, and prepare myself better for a future career as a technology professional.