---
title: "Week 11 Worklog"
date: 2026-06-15
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objective: Building the Complete Gmail OAuth Flow — and Facing the Most Confusing Security Issue in the Project

This week was the core implementation period for the section I was responsible for: transforming the Google Cloud configuration from the previous week into a fully functional OAuth flow, from the moment the application requests a Gmail connection to the point where Gmail tokens are securely stored in DynamoDB and automatically refreshed when they expire.
The original plan appeared straightforward and linear. However, this week was ultimately defined by an incident in which every step appeared to report “success” while the system itself remained completely non-functional. This became the most valuable OAuth lesson I learned throughout the entire project.
- Develop the `oauth-init` Lambda function to generate the consent URL with critical parameters such as `access_type`, `prompt`, and `state`.
- Develop the `oauth-callback` Lambda function to exchange the authorization code for tokens, store them in DynamoDB, and return an HTML page to the browser.
- Solve the problem of preserving user identity across the redirect flow through the `state` parameter while honestly identifying the security limitations of the MVP implementation.
- Build a shared Lambda Layer named `gmail-shared` containing reusable refresh-token logic for multiple Lambda functions.
- Resolve a real-world issue in which OAuth appeared to complete successfully, but the Worker received a `403 Forbidden` response when calling the Gmail API.

### Tasks to Be Completed During the Week:

| Day | Task Description | Start Date | Completion Date | Reference Materials |
| --- | --- | --- | --- | --- |
| Monday | - Received the actual callback URL from the backend member after the REST API had been deployed and added it to the **Authorized redirect URIs** section of the Google OAuth Client configuration. <br> - Resolved the first configuration error: `Invalid Origin: URIs must not contain a path`, which occurred because the redirect URI had mistakenly been entered in the JavaScript origins field. | 15/06/2026 | 15/06/2026 | https://developers.google.com/identity/protocols/oauth2/web-server |
| Tuesday | - Developed the `oauth-init` Lambda function, protected by Cognito authentication, to generate a consent URL using the minimum required scopes, `access_type=offline` to request a refresh token, `prompt=consent`, and a `state` value encoding the user ID. <br> - Studied why Google may stop returning a refresh token after the first approval when `prompt=consent` is omitted. | 16/06/2026 | 16/06/2026 | https://developers.google.com/identity/protocols/oauth2 |
| Wednesday | - Developed the public `oauth-callback` Lambda function with no Authorizer because the redirect request from Google does not include a Cognito JWT. The function validates `state`, exchanges the authorization code for tokens, retrieves the Gmail address through the userinfo endpoint, and stores the connection in the `inboxiq-gmail-connections` table. <br> - Implemented logic to preserve the previously stored refresh token when a new token response does not contain one, since Google normally guarantees a refresh token only during the initial consent process. | 17/06/2026 | 17/06/2026 | https://developers.google.com/identity/protocols/oauth2/web-server |
| Thursday | - Built the `gmail-shared` Lambda Layer containing `getGoogleCredentials()` for cached secret retrieval and `refreshAccessToken(userId)` for refreshing expired access tokens. <br> - Resolved a SAM build issue in which `BuildMethod: nodejs20.x` automatically added another `nodejs/` directory, creating a duplicated folder structure. Although the build completed successfully, Lambda produced a `Cannot find module` error at runtime. The issue was fixed by pointing `ContentUri` directly to the existing `nodejs/` directory. | 18/06/2026 | 18/06/2026 | https://docs.aws.amazon.com/serverless-application-model/ |
| Friday | - Tested the complete OAuth flow end to end using a real Gmail account. Consent completed successfully and the token was stored in DynamoDB, but the backend Worker received `403 Forbidden` when calling the Gmail API. <br> - Traced the issue and discovered that the `scope` field stored in DynamoDB contained only `userinfo.email openid` and did not include `gmail.readonly`. | 19/06/2026 | 19/06/2026 | Internal project documentation |
| Saturday | - Identified the root cause as Google’s **granular permissions** mechanism, which allows users to deselect individual permissions on the consent screen. A token may still be issued even when a required scope is missing. <br> - Ran the OAuth flow again. Google displayed an incremental consent screen requesting only the missing permission. After approval, the token contained the full scope set and the Worker successfully called Gmail. Also fixed incorrect Vietnamese text rendering on the callback page by adding `charset=utf-8`. | 20/06/2026 | 20/06/2026 | https://developers.google.com/identity/protocols/oauth2/resources/granular-permissions |
| Sunday | - Wrote internal documentation about the granular-permissions incident and the lesson that “completed consent does not necessarily mean sufficient permissions.” <br> - Handed over the completed OAuth flow to the Flutter member for client-side deep-link integration in the following week and completed the weekly report. | 21/06/2026 | 21/06/2026 | Internal InboxIQ project documentation |

### Week 11 Results:

#### 1. Three Small Parameters That Determine the Entire OAuth Flow

Developing `oauth-init` taught me that an OAuth flow that merely “works” is very different from one that continues to work correctly over time. The difference often depends on several parameters that are easy to overlook:
- **`access_type=offline`**: Without this parameter, Google only returns an access token that remains valid for approximately one hour. The integration would therefore stop working silently one hour after the user connected their account.
- **`prompt=consent`**: Without this parameter, Google may stop returning a refresh token from the second approval onward. This issue only appears when a user reconnects an account, making it difficult to reproduce in a one-time test.
- **`state`**: This is the only channel used to preserve the user’s identity across the redirect flow because the callback request comes from the browser through Google and does not contain the Cognito JWT.
Regarding `state`, I honestly documented an important limitation in the MVP implementation. The current value is only Base64-encoded and is not cryptographically signed. In theory, an attacker could create a forged `state` value containing another user ID.
The production-ready solution would be to sign the `state` value using a JWT or store it temporarily in DynamoDB with a TTL for later validation. This limitation was recorded in the Limitations section instead of being ignored.

#### 2. The Granular Permissions Incident — a False “Success” and the Most Important Security Lesson

This was the most memorable incident of the entire security implementation.
The OAuth flow completed successfully from beginning to end. The token was stored in DynamoDB, and no error message appeared anywhere in the authorization process. However, the Worker received `403 Forbidden` when calling the Gmail API, and the SQS job silently retried every five minutes.
The key clue was already present in the stored data. The record’s `scope` field contained only `userinfo.email openid` and was **missing `gmail.readonly`**.
The root cause was Google’s relatively recent **granular permissions** mechanism. The consent screen allowed the user to select or deselect each permission individually. The test user clicked “Continue” without selecting the Gmail access permission. Google still issued a token successfully, but the token did not contain the required Gmail scope.
Many older OAuth tutorials do not mention this behavior.
The solution was unexpectedly straightforward. When the OAuth flow was started again, Google recognized that the application already had some permissions and displayed an **incremental consent** screen requesting only the missing scope.
After the missing permission was approved, the token contained the complete scope set and the Worker successfully called the Gmail API.
The most important lesson was clear: **in OAuth integrations, always validate the actual `scope` field returned in the token response instead of assuming that a completed consent flow means all requested permissions were granted**.
A production application should validate the scope directly in the callback and proactively request re-consent when required permissions are missing.

#### 3. Lambda Layer — Sharing Code Correctly Across Multiple Lambda Functions

The refresh-token logic was required by both `oauth-callback` and the Worker. However, AWS SAM packages and builds each function separately according to its own `CodeUri`, meaning that one Lambda function cannot directly import files from another function.
Lambda Layer is the official mechanism for sharing reusable code across Lambda functions.
A practical build issue occurred with `BuildMethod: nodejs20.x`. SAM automatically wrapped the Layer content in an additional `nodejs/` directory. When `ContentUri` pointed to a parent directory that already contained a `nodejs/` folder, the final package contained a duplicated structure:
`nodejs/nodejs/`
As a result, the Lambda runtime reported:
`Cannot find module '/opt/nodejs/gmail-shared.mjs'`
This happened even though the SAM build completed successfully.
The issue was resolved by pointing `ContentUri` one level deeper directly to the existing `nodejs/` directory. I also deleted the `.aws-sam` directory and rebuilt the project because cached artifacts could preserve the incorrect structure.

#### 4. Small Errors That Consumed a Significant Amount of Time

Two secondary issues were also worth documenting.
The first was entering the redirect URI in the wrong field in Google Cloud Console. The error message was:
`Invalid Origin: URIs must not contain a path`
A complete URI containing a path must be entered under **Authorized redirect URIs**, not under the JavaScript origins field.
The second issue involved corrupted Vietnamese text such as `Káº¿t ná»'i...` on the callback page. The response header only specified `Content-Type: text/html` and did not include `charset=utf-8`, causing the browser to guess the wrong character encoding.

### Week 11 Evaluation:

- The complete Gmail OAuth flow was successfully implemented: initialization → consent → callback → token storage in DynamoDB → automatic token refresh after expiration.
- The granular-permissions incident was resolved and documented. This lesson extends beyond the project and is applicable to any Google OAuth integration.
- The shared Lambda Layer operated correctly and was used by both `oauth-callback` and the Worker.
- The security limitation of using an unsigned `state` value was identified and documented honestly, providing a clear explanation for the project defense.

### Challenges Encountered:

- The `403 Forbidden` issue was completely silent from the OAuth perspective. No authorization error appeared, and the problem was only discovered after tracing the Worker’s symptoms back to the scope data stored in DynamoDB.
- Google’s granular-permissions behavior is rarely discussed in common OAuth tutorials. Confirmation required locating the specific official documentation page.
- The duplicated `nodejs/nodejs/` folder issue in the SAM Layer appeared only at runtime. The build process produced no warning.

### Solutions and Best Practices Learned:

- Always validate the actual scopes returned in the token response. This should be treated as a mandatory OAuth step rather than an optional check.
- For third-party services whose behavior changes frequently, such as Google, always prioritize the latest official documentation and record the consultation date in the team documentation.
- After changing a Layer or build structure, always delete the `.aws-sam` directory before rebuilding to eliminate outdated cached artifacts.
- Every HTML response containing Vietnamese text must explicitly declare `charset=utf-8`.

### Direction for the Following Week:

- Support the Flutter member in completing the client-side OAuth flow, including the deep link used to return to the mobile application after consent.
- Conduct a complete security review before submission, including IAM permissions, credential-storage locations, and the Google OAuth Test users list.
- Prepare the presentation section explaining the project’s security decisions and the limitations identified during implementation.
