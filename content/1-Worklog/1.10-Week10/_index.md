---
title: "Week 10 Worklog"
date: 2026-06-08
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objective: Building the Identity and Security Layer — the Invisible Foundation of the Entire System

In the InboxIQ team assignment, I was responsible for the authentication and security components, including Cognito, IAM, Secrets Manager, and the entire Google OAuth flow.
This is the “invisible” part of the system. Users cannot directly see it, and it is not something that can easily be showcased during a demonstration. However, every other component depends on it: the application needs Cognito to identify logged-in users, Lambda functions need IAM Roles to access AWS resources, and the Worker needs secrets to call the OpenAI API.
The objective for this week was to complete this foundational layer before the backend and Flutter members needed to integrate with it.
- Create and configure a Cognito User Pool, including password policies, email verification, and an App Client for the mobile application.
- Understand and select the appropriate authentication flow for the App Client, including why a mobile application is considered a public client and must not use a client secret.
- Design IAM Roles for Lambda functions according to the Principle of Least Privilege.
- Configure Secrets Manager to store the OpenAI API key and provide the backend member with a secure method for retrieving secrets.
- Create a Google Cloud project, enable the Gmail API, and configure the OAuth consent screen in Testing mode with the minimum required scopes.

### Tasks to Be Completed During the Week:

| Day | Task Description | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| Monday | - Conducted a team meeting to finalize the system architecture and received responsibility for the authentication and security components. <br> - Studied the fundamentals of Amazon Cognito, including the differences between User Pools and Identity Pools, and determined that the project only required a User Pool for user authentication and JWT issuance. | 08/06/2026 | 08/06/2026 | https://docs.aws.amazon.com/cognito/ |
| Tuesday | - Created a User Pool with email-based sign-in, a password policy, and email verification during registration. <br> - Created the `inboxiq-flutter-client` App Client without enabling a client secret because a mobile application is a public client and cannot securely store confidential credentials. Selected the appropriate authentication flow. | 09/06/2026 | 09/06/2026 | https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-client-apps.html |
| Wednesday | - Created a test user and verified the login flow through the AWS CLI using `initiate-auth`, ensuring that the response returned the complete token set, including the ID token, access token, and refresh token. <br> - Handed over the User Pool ID, App Client ID, and enabled authentication flow to the Flutter member, together with a note to ensure that the client-side `authenticationFlowType` matched the actual Cognito configuration. | 10/06/2026 | 10/06/2026 | https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/ |
| Thursday | - Designed the `inboxiq-lambda-role` IAM Role for Lambda functions, granting only the necessary permissions for DynamoDB, SQS, Secrets Manager, CloudWatch Logs, X-Ray, and `execute-api:ManageConnections` for future WebSocket push notifications. <br> - Studied the trade-off between using one shared role for simplicity during the MVP phase and assigning a separate role to each function for strict Least Privilege compliance. | 11/06/2026 | 11/06/2026 | https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html |
| Friday | - Created the `inboxiq/openai-api-key` secret in Secrets Manager using JSON format. <br> - Researched and wrote an internal guideline for retrieving secrets within Lambda functions using global-scope caching, while strictly avoiding plaintext API keys in environment variables or source code repositories. | 12/06/2026 | 12/06/2026 | https://docs.aws.amazon.com/secretsmanager/ |
| Saturday | - Created a Google Cloud project and enabled the Gmail API. <br> - Configured the OAuth consent screen in Testing mode, selected the minimum required scopes (`gmail.readonly` and `userinfo.email`), and added test email accounts to the Test users list. | 13/06/2026 | 13/06/2026 | https://developers.google.com/identity/protocols/oauth2 |
| Sunday | - Created an OAuth Client ID of the Web application type in Google Cloud and securely stored the Client ID and Client Secret while waiting for the backend callback URL to become available in the following week. <br> - Wrote the weekly summary report and prepared documentation for handing over the authentication layer to the entire team. | 14/06/2026 | 14/06/2026 | Internal InboxIQ project documentation |

### Week 10 Results:

#### 1. Cognito — Understanding Why We “Rent” Instead of Building an Authentication System from Scratch

Before starting this task, I assumed that implementing authentication using JWT and bcrypt would not be particularly difficult.
However, after spending a week studying Cognito documentation, I realized that the real complexity does not lie in simply making login work. It lies in everything surrounding the login process, including secure password storage, brute-force protection, password recovery, email verification, token refresh, and token revocation.
Cognito manages the entire user authentication lifecycle. As a result, the team does not need to store any user passwords in its own database, which completely removes an important category of data leakage risk.
The most important decision when creating the App Client was to **disable the client secret**.
This decision was based on the nature of mobile applications. Since an APK file is distributed directly to users, any “secret” embedded inside the application can potentially be extracted through reverse engineering.
Therefore, OAuth standards classify mobile applications as *public clients*, which are not capable of securely storing client secrets. I documented this point clearly for the team because it is also a likely topic during the project defense.

#### 2. IAM Role — Least Privilege Is a Series of Decisions, Not a Checkbox

Designing the Lambda IAM Role required me to answer specific questions for each function:
- Which DynamoDB tables does the Worker need to read or write?
- Does the Producer need permission to access Secrets Manager?
- How narrowly should the `execute-api:ManageConnections` permission be scoped for WebSocket push notifications?
The Producer does not need permission to access Secrets Manager because only the Worker requires the OpenAI API key.
For the MVP, the team selected one shared role named `inboxiq-lambda-role`, containing the minimum combined set of permissions required by all Lambda functions.
However, I also documented the limitation honestly: strict adherence to the Principle of Least Privilege would require a separate IAM Role for each function. This was recorded as a future improvement if the project were deployed to a production environment.

#### 3. Secrets Manager — The Correct Place for Information That Must Never Be Exposed

The team agreed on one core principle: API keys and all other credentials must exist in only one secure location, which is AWS Secrets Manager.
They must not be stored in:
- Plaintext environment variables.
- Source code.
- Git repositories.
I also prepared a guideline for retrieving secrets from Lambda functions and caching them at the global scope.
This approach reduces both latency and operational cost because AWS Secrets Manager charges based on API requests. The backend member immediately applied this method to the Worker function.

#### 4. Google OAuth Consent Screen — Working Outside the AWS Ecosystem

This was my first time working with Google Cloud Console.
The most important observation was the behavior of the OAuth consent screen in **Testing** mode. An application that has not completed Google's verification process only allows accounts included in the Test users list to authenticate, with a maximum of 100 test users.
The Gmail read scope belongs to the sensitive or restricted scope category.
Making the application publicly available would require a demanding verification process, including:
- A publicly accessible privacy policy.
- A demonstration video.
- Potential business verification.
This process was not suitable for the scope of an academic project.
Therefore, the team decided to keep the consent screen in Testing mode and clearly document this limitation in the Limitations section of the report instead of allowing evaluators to discover it independently.

### Week 10 Evaluation:

- The Cognito authentication layer was fully configured, verified through the AWS CLI, and handed over to the Flutter member with all required integration information.
- The IAM Role was designed based on careful permission analysis, and the related security trade-offs were documented for use during the project defense.
- Secrets Manager was successfully configured, together with a unified credential-management policy for the entire team.
- The Google Cloud project was prepared and only required the actual backend callback URL to complete the OAuth Client configuration.

### Challenges Encountered:

- The new Cognito Console interface had changed significantly compared with older tutorials and online documentation. Many settings had been renamed or moved, requiring me to rely on the latest official documentation.
- Defining the permission boundaries for `execute-api:ManageConnections` was challenging because the WebSocket API had not yet been created. The permission design had to anticipate the architecture planned for the following week.
- OAuth concepts such as client types, scopes, consent, and incremental authorization were numerous and easy to confuse, especially because Google and AWS documentation sometimes used slightly different terminology for similar concepts.

### Solutions and Best Practices Learned:

- Always follow the latest official documentation instead of relying on outdated blog posts or tutorials, especially for cloud consoles whose interfaces change frequently.
- Record every security trade-off at the time the decision is made, such as shared roles versus individual roles or Testing mode versus Production consent. Each decision should include a clear justification to ensure transparency in the report and prepare the team for defense questions.
- Draw the OAuth flow on paper before configuring any settings in the cloud console. Abstract concepts become much easier to understand when the path of each token is visually represented.

### Direction for the Following Week:

- Develop the two OAuth Lambda functions, including the initialization function and callback function, as well as a shared Lambda Layer for refresh-token handling. This will be the most technically demanding part of my assigned responsibilities.
- Solve the problem of preserving the user’s identity throughout Google’s redirect flow by using the `state` parameter.
- Coordinate with the backend member to add the actual callback URL to the Google OAuth Client configuration.
