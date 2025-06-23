# Q&A: Secure Storage of Sprinklr Session Tokens

## Question
How are we storing the session tokens for Sprinklr? Ideally, we should not store them in localStorage or cookies, as they can be exploited in case of XSS on the partner app (over which we have no or limited control). Tokens should be stored in memory to reduce the risk.

## Tech Lead's Answer
Session tokens for Sprinklr are never stored in localStorage or cookies. The SDK is designed to keep tokens only in memory (e.g., JavaScript variables or closures) for the duration of the session. This approach minimizes the risk of token theft via XSS attacks on the partner app.

## Recommendation
Continue to avoid persistent storage of session tokens in the browser. Educate partners on the risks of storing tokens in localStorage or cookies, and provide SDK patterns that keep tokens in memory only.

## Risk
If tokens are stored in localStorage or cookies, they can be stolen via XSS, leading to session hijacking and unauthorized access.

## Mitigation
- Enforce in-memory storage of tokens in the SDK.
- Provide clear documentation and code samples for partners.
- Monitor for suspicious session activity.

## Incident Response Plan
- Invalidate affected tokens and sessions.
- Notify partners of best practices and remediation steps.
- Review SDK and partner implementations for compliance.
