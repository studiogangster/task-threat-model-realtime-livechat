# Q&A: SDK Loading via Trusted Domains

## Question
How are we ensuring that the SDK can only be loaded via trusted domains?

## Tech Lead's Answer
We use CORS headers and referer validation at the CDN and S3 level to restrict SDK asset delivery to a list of trusted partner domains.

## Recommendation
Maintain an up-to-date allowlist of trusted domains and monitor for unauthorized access attempts.

## Risk
If CORS or referer checks are bypassed, SDK could be loaded by malicious sites.

## Mitigation
- Enforce strict CORS and referer policies.
- Monitor CDN logs for anomalies.
- Periodically review and update domain allowlists.

## Incident Response Plan
- Revoke access for compromised domains.
- Notify affected partners.
- Investigate and remediate root cause.
