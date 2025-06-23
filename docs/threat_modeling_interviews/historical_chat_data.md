# Q&A: Historical Chat Data Storage

## Question
Are we saving any historical chat data?

## Tech Lead's Answer
Yes, chat transcripts and metadata are stored in encrypted databases for compliance, analytics, and support purposes. Retention policies are configurable per partner.

## Recommendation
Document and enforce data retention and deletion policies. Allow partners to configure retention as per their compliance needs.

## Risk
Excessive retention could expose sensitive data in case of a breach.

## Mitigation
- Enforce least-privilege access to chat data.
- Regularly purge data as per policy.
- Encrypt data at rest and in transit.

## Incident Response Plan
- Identify and contain any data exposure.
- Notify affected partners.
- Review and update retention policies.
