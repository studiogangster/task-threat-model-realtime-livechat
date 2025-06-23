# Q&A: Audit Logs for Session Compromise

## Question
Do we have audit logs to protect ourselves in case the partner app or user session is compromised?

## Tech Lead's Answer
Yes, all critical actions (authentication, data access, configuration changes) are logged with timestamps, user/partner IDs, and source IPs. Logs are stored in a tamper-evident system (e.g., AWS CloudWatch with retention and access controls).

## Recommendation
Regularly review audit logs and enable automated alerting for suspicious activity.

## Risk
If logs are incomplete or tampered with, incident investigation may be hindered.

## Mitigation
- Store logs in immutable storage.
- Restrict log access to security personnel.
- Enable log integrity verification.

## Incident Response Plan
- Preserve and analyze logs during incidents.
- Use logs to determine scope and impact.
- Report findings to stakeholders.
