---
title: "Week 12 Worklog"
date: 2026-06-22
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Week 12 Objective: Completing the OAuth Journey Back to the User and Conducting a Full-System Security Review

The backend OAuth flow had already been completed in the previous week, but “completed” only applied to the server boundary. Real users still needed one final step: after granting permission in the browser, they had to be returned **back to the mobile application** smoothly.
This week, I coordinated with the Flutter member to complete that final stage through deep linking, resolved an unexpected redirect-blocking behavior in Chrome, and spent the second half of the week performing an essential security task before submission: reviewing the entire system from an attacker’s perspective and honestly documenting a security mistake made by our own team.
- Modify the callback page so the browser can return users to the application through the `inboxiq://` deep link and handle Chrome’s automatic redirect restrictions.
- Support end-to-end OAuth testing on a real device together with the Flutter member.
- Manage the Google Cloud Test users list when the team needs to test with multiple Gmail accounts.
- Review the security of the entire system, including the two-layer JWT authentication model for REST and WebSocket, credential storage locations, and IAM permissions.
- Handle a process-related security incident after discovering that the Google Client Secret had previously appeared in a screenshot shared within the team, assess the exposure risk, and prepare a rotation plan.

### Tasks to Be Completed During the Week:

| Day | Task Description | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| Monday | - Tested the OAuth flow on a real device together with the Flutter member. Consent completed successfully, but the browser remained on the callback page and did not return to the application. <br> - Identified the cause: Chrome blocks automatic navigation to an unfamiliar scheme such as `inboxiq://` when there is no direct user gesture, as part of its malware-prevention mechanism. | 22/06/2026 | 22/06/2026 | Internal project documentation |
| Tuesday | - Modified `callbackHandler` so the returned HTML page included a `meta refresh` that attempted to redirect after one second, together with a fallback link reading “Click here if you are not redirected automatically.” <br> - Deployed and retested the flow with the Flutter member. The deep link worked correctly, the application received the callback, and the connection status was updated successfully. | 23/06/2026 | 23/06/2026 | Internal project documentation |
| Wednesday | - Documented the process for adding new Gmail accounts to the Test users list on the OAuth consent screen so the team could test with multiple mailboxes. <br> - Verified Google’s permission-memory behavior: accounts that had previously granted consent were not asked again, and the flow proceeded directly to the callback. | 24/06/2026 | 24/06/2026 | https://support.google.com/cloud/ |
| Thursday | - Reviewed the two-layer authentication model: REST requests validate JWTs individually through a Cognito Authorizer, while WebSocket connections authenticate once during `$connect` through a Lambda Authorizer using a token passed in the query string. <br> - Verified that the Lambda Authorizer correctly validated both the JWT signature and audience against the intended Cognito User Pool. | 25/06/2026 | 25/06/2026 | https://docs.aws.amazon.com/apigateway/ |
| Friday | - Audited the storage locations of every credential in the system. The OpenAI API key and Google Client Secret were stored in Secrets Manager, with no keys present in the source code, Git repository, or plaintext environment variables. <br> - Discovered during the audit that the Google Client Secret had previously appeared in a screenshot shared in an internal team discussion. Assessed the level of exposure and considered the appropriate response. | 26/06/2026 | 26/06/2026 | Internal project documentation |
| Saturday | - Prepared a Google OAuth Client Secret rotation plan: generate a new secret in Google Cloud Console, update Secrets Manager, and disable the old secret. The rotation was scheduled for after the demonstration period to avoid interrupting the grading process and was documented in the maintenance checklist. <br> - Conducted the final review, confirming that the unsigned `state` limitation had been recorded in the Limitations section and that the Test users list remained appropriately restricted. | 27/06/2026 | 27/06/2026 | https://developers.google.com/identity/protocols/oauth2 |
| Sunday | - Compiled the system’s “security dossier,” including the authentication model, credential-storage map, identified limitations, and corresponding remediation plans, then handed it over to the documentation member for inclusion in the report. <br> - Completed the weekly summary report. | 28/06/2026 | 28/06/2026 | Internal InboxIQ project documentation |

### Week 12 Results:

#### 1. The Final Stage of OAuth — When the Browser Becomes a Demanding “Fourth Party”

OAuth is usually described as involving three parties: the application, the backend, and Google. However, this week revealed a fourth party that is often overlooked: **the browser itself**.

Chrome on Android blocks automatic navigation to non-HTTP or non-HTTPS schemes when there is no direct user interaction. This is a reasonable anti-malware mechanism, but in our case it caused the successful callback page to trap the user inside the browser instead of returning them to the application.
The selected solution used a defensive two-layer approach. A `meta refresh` attempted to redirect automatically after one second, which worked on browsers that allowed it. At the same time, a manual fallback link gave users a guaranteed way to return to the application on browsers that blocked the automatic redirect.
The main principle learned from this issue was that any flow involving a browser should never depend entirely on an automatic behavior outside the application’s control. A manual fallback path should always be available.
Another interesting observation appeared during repeated testing: Google remembered previously granted permissions. Accounts that had already completed consent were not asked again, and the flow moved directly to the callback in less than two seconds. This was the other side of the incremental-consent behavior encountered in the previous week.

#### 2. The Two-Layer Authentication Model — Different Verification Methods for Different Protocols

The security review helped me clearly explain an architectural point that could easily be raised during the project defense: the same Cognito JWT is verified differently at the REST and WebSocket layers because the two protocols behave differently.
REST validates **every request** through a Cognito Authorizer, with each request carrying the token in the authorization header.
WebSocket authentication occurs **only once during `$connect`** through a Lambda Authorizer, with the token passed through the query string. This approach is used because a mobile WebSocket handshake may not support the same custom-header mechanism as a normal REST request. Once the connection is established, all following messages travel through the authenticated channel.
Although the verification approaches differ, both layers rely on the same source of truth: the JWT signature and audience are validated against the correct Cognito User Pool.

#### 3. Credential Review — and a Lesson from Our Own Mistake

The credential audit produced a generally positive result. All credentials were stored in the correct location inside Secrets Manager, and no sensitive keys were found in the source code, Git repository, or plaintext environment variables.
However, during the review, I discovered a process-related security mistake. The Google Client Secret had previously been **visible in a screenshot** shared while the team was requesting technical assistance. This was not an architectural vulnerability, but a workflow failure caused by sharing an image without masking sensitive information.
The practical risk assessment concluded that the exposure was limited because the screenshot had only been shared within a private internal channel. However, security principles do not allow a team to rely on the assumption that “it is probably fine.” Once a secret has been exposed, it should be replaced.
A specific rotation plan was therefore created:
1. Generate a new client secret in Google Cloud Console.
2. Update the corresponding value in AWS Secrets Manager.
3. Disable the previous secret.
The rotation was scheduled for after the demonstration period to avoid disrupting the grading process, and the action was documented transparently in the report’s maintenance checklist.
The process lesson for the entire team was simple: every screenshot must be reviewed and all credentials must be masked before sharing, even in an internal channel. This habit is far less costly than secret rotation and the risks associated with accidental exposure.

#### 4. The “Security Dossier” — the Final Deliverable of This Responsibility

The main deliverable at the end of the week was a consolidated security document containing the two-layer authentication model, a map of credential locations, and, equally importantly, a list of known limitations together with remediation plans.
These limitations included:
- The `state` value was not yet cryptographically signed.
- The exposed Google Client Secret required rotation.
- The OAuth consent screen remained in Testing mode.
The central philosophy was that a trustworthy security report is not one that claims to have no weaknesses. It is one that clearly understands where its weaknesses are and explains how they will be addressed.

### Week 12 Evaluation:

- The OAuth flow was completed all the way back to the mobile application on a real device, fulfilling the technical objective of my assigned responsibility.
- The two-layer authentication model was reviewed and clearly documented, ready for presentation during the project defense.
- The exposed-secret incident was handled transparently through risk assessment and a specific rotation plan.
- The complete security dossier was handed over to the documentation member.

### Challenges Encountered:

- Chrome’s redirect-blocking behavior produced no error message and no useful log. The only symptom was a callback page that remained stationary, so the cause had to be identified through reasoning and repeated testing.
- It was necessary to balance a security principle, which recommends rotating an exposed secret immediately, against an operational concern, which was avoiding system interruption during the grading period. The final decision required a written risk assessment rather than an arbitrary choice.
- Documenting the team’s own mistake required overcoming the instinct to hide the issue for the sake of presenting a cleaner report. However, transparency is the correct professional attitude in security work.

### Solutions and Best Practices Learned:

- Every browser-dependent flow should include a manual fallback in addition to the automatic mechanism.
- All credentials must be masked in screenshots before they are shared, including screenshots sent through internal channels.
- Any exposed secret must be rotated, regardless of how limited the exposure appears. Only the timing of the rotation may be assessed based on documented operational risk.
- Security reports should list known limitations together with remediation plans instead of presenting an unrealistic and unconvincing “perfect” system.

### Post-Project Direction:

- Rotate the Google Client Secret according to the documented plan immediately after the grading period.
- Upgrade the `state` mechanism from plain Base64 encoding to a signed format using JWT, or temporarily store it in DynamoDB with a TTL if the project continues to be developed.
- Replace the shared IAM Role with a separate role for each Lambda function to achieve strict Least Privilege compliance.
- Study Google’s verification process if the application is later opened to users outside the Test users list.
