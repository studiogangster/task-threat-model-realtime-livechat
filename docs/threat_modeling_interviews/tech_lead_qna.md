# Threat Modeling Q&A with Tech Leads

This document captures key questions and answers discussed with technical leads to clarify architectural and security decisions. Each entry includes the question, the tech lead's answer, our recommendation, identified risks, proposed mitigations, and an incident response plan.

---


### Question
How is authentication handled for API and WebSocket connections?

### Tech Lead's Answer
All API and WebSocket connections require JWT-based authentication. Tokens are issued by our identity provider and validated at the API Gateway and WebSocket entry points. Token expiry and refresh are managed by the partner.

API keys and secret keys are generated exclusively for each partner. The partner must use their API and secret key to sign the JWT token for the end user. This token is then validated by the Sprinklr service to ensure authenticity and integrity.

### Recommendation
Implement short-lived tokens with automatic refresh, enforce token validation at all entry points, and monitor for token misuse.

### Risk
If token validation is bypassed or tokens are leaked, unauthorized access to sensitive APIs or chat sessions is possible.

### Mitigation
- Enforce strict validation at API Gateway and WebSocket services.
- Use HTTPS everywhere.
- Rotate signing keys regularly.
- Monitor for suspicious authentication attempts.

### Incident Response Plan
- Immediately revoke compromised tokens.
- Rotate signing keys.
- Investigate access logs for unauthorized activity.
- Notify affected users and partners as per incident response policy.

---

### Question
How are we ensuring secure communication between partner and Sprinklr service?

### Tech Lead's Answer
All communication between partner systems and Sprinklr services is secured using mutual TLS (mTLS) for both API and WebSocket connections. Only trusted certificates are accepted, and connections are restricted to whitelisted IPs where possible.

### Recommendation
Continue enforcing mTLS for all partner-to-Sprinklr communication. Regularly audit certificate trust stores and IP whitelists.

### Risk
If mTLS is misconfigured or certificates are compromised, attackers could intercept or impersonate traffic.

### Mitigation
- Automate certificate rotation and revocation.
- Monitor for failed or suspicious connection attempts.
- Enforce strict IP whitelisting.

### Incident Response Plan
- Revoke compromised certificates.
- Update trust stores and IP whitelists.
- Notify affected partners and rotate credentials.

---

### Question
Are we saving any historical chat data?

### Tech Lead's Answer
Yes, chat transcripts and metadata are stored in encrypted databases for compliance, analytics, and support purposes. Retention policies are configurable per partner.

### Recommendation
Document and enforce data retention and deletion policies. Allow partners to configure retention as per their compliance needs.

### Risk
Excessive retention could expose sensitive data in case of a breach.

### Mitigation
- Enforce least-privilege access to chat data.
- Regularly purge data as per policy.
- Encrypt data at rest and in transit.

### Incident Response Plan
- Identify and contain any data exposure.
- Notify affected partners.
- Review and update retention policies.

---

### Question
Do we have audit logs to protect ourselves in case the partner app or user session is compromised?

### Tech Lead's Answer
Yes, all critical actions (authentication, data access, configuration changes) are logged with timestamps, user/partner IDs, and source IPs. Logs are stored in a tamper-evident system (e.g., AWS CloudWatch with retention and access controls).

### Recommendation
Regularly review audit logs and enable automated alerting for suspicious activity.

### Risk
If logs are incomplete or tampered with, incident investigation may be hindered.

### Mitigation
- Store logs in immutable storage.
- Restrict log access to security personnel.
- Enable log integrity verification.

### Incident Response Plan
- Preserve and analyze logs during incidents.
- Use logs to determine scope and impact.
- Report findings to stakeholders.

---

### Question
How are we ensuring that the SDK can only be loaded via trusted domains?

### Tech Lead's Answer
We use CORS headers and referer validation at the CDN and S3 level to restrict SDK asset delivery to a list of trusted partner domains.

### Recommendation
Maintain an up-to-date allowlist of trusted domains and monitor for unauthorized access attempts.

### Risk
If CORS or referer checks are bypassed, SDK could be loaded by malicious sites.

### Mitigation
- Enforce strict CORS and referer policies.
- Monitor CDN logs for anomalies.
- Periodically review and update domain allowlists.

### Incident Response Plan
- Revoke access for compromised domains.
- Notify affected partners.
- Investigate and remediate root cause.

---

### Question
How are we ensuring integrity and versioning of our SDK artifacts?

### Tech Lead's Answer
All SDK artifacts are versioned and published with cryptographic hashes (e.g., SHA-256). Partners are encouraged to verify hashes before use. CDN and S3 enforce immutability for released versions.

### Recommendation
Automate hash generation and verification in CI/CD. Communicate hash values to partners.

### Risk
If artifacts are tampered with, partners may load malicious code.

### Mitigation
- Enforce artifact immutability.
- Monitor for unauthorized changes.
- Provide hash verification tools.

### Incident Response Plan
- Invalidate compromised artifacts.
- Notify partners to update SDK.
- Investigate distribution pipeline.

---

### Question
Do we have a rate control check?

### Tech Lead's Answer
Yes, API Gateway and WebSocket endpoints enforce rate limits per partner and per end user. Limits are configurable and monitored.

### Recommendation
Regularly review and adjust rate limits based on usage patterns and threat intelligence.

### Risk
Insufficient rate limiting could allow abuse or DoS attacks.

### Mitigation
- Enforce sensible default limits.
- Alert on rate limit violations.
- Block abusive clients automatically.

### Incident Response Plan
- Throttle or block offending IPs/partners.
- Notify affected parties.
- Review and update rate limiting rules.

---

### Question
How are we dealing with the possibility of DoS and DDoS attacks, especially when allowing WebSocket communication?

### Tech Lead's Answer
We use AWS Shield and WAF for DDoS protection at the edge (CloudFront, ELB). WebSocket connections are rate-limited and monitored for abuse. Auto-scaling and connection quotas are enforced.

### Recommendation
Test DDoS response plans and regularly review protection configurations.

### Risk
Large-scale attacks could impact service availability.

### Mitigation
- Use AWS Shield Advanced.
- Enforce connection quotas.
- Monitor and auto-scale resources.

### Incident Response Plan
- Engage AWS DDoS Response Team.
- Scale up resources as needed.
- Communicate with partners/end users.

---

### Question
How are we securing communication over WebSocket from MITM attacks and session impersonation?

### Tech Lead's Answer
All WebSocket traffic is encrypted with TLS. JWT tokens are validated on every connection and periodically during session. Session IDs are random and unguessable.

### Recommendation
Enforce TLS 1.2+ for all WebSocket endpoints. Rotate session tokens regularly.

### Risk
If encryption or token validation fails, sessions could be hijacked.

### Mitigation
- Monitor for invalid or expired tokens.
- Enforce strict session timeouts.
- Use secure random session IDs.

### Incident Response Plan
- Invalidate affected sessions.
- Notify impacted users/partners.
- Investigate and patch vulnerabilities.

---

### Question
How do we gracefully handle token expiry and refresh mechanisms?

### Tech Lead's Answer
Partners are required to implement token refresh logic. Expired tokens result in a 401 response, prompting the partner to obtain a new token and retry the request.

### Recommendation
Document token refresh flows and provide SDK support for automatic refresh.

### Risk
Poorly handled expiry could disrupt user sessions.

### Mitigation
- Provide clear error codes and messages.
- Support seamless token refresh in SDK.

### Incident Response Plan
- Monitor for excessive token expiry errors.
- Assist partners with integration issues.

---

### Question
How are we securely allowing partners to upload FAQ and knowledge base data to our S3 bucket?

### Tech Lead's Answer
Partners are given pre-signed, time-limited S3 URLs for uploads. Access is scoped to specific buckets and paths per partner.

### Recommendation
Expire pre-signed URLs quickly and audit all uploads.

### Risk
If URLs are leaked, unauthorized uploads could occur.

### Mitigation
- Limit URL validity.
- Monitor and alert on unusual upload activity.
- Restrict bucket policies.

### Incident Response Plan
- Revoke affected URLs.
- Audit and remove unauthorized data.
- Notify partners.

---

### Question
How are we securing our S3 bucket data?

### Tech Lead's Answer
All S3 buckets are private by default. Access is controlled via IAM policies and bucket policies. Data is encrypted at rest using AWS KMS.

### Recommendation
Regularly audit bucket policies and access logs.

### Risk
Misconfigured buckets could expose sensitive data.

### Mitigation
- Enforce least-privilege access.
- Enable S3 access logging.
- Use AWS Config for compliance monitoring.

### Incident Response Plan
- Restrict access immediately.
- Investigate access logs.
- Notify affected partners.

---

### Question
Are the SPA frontend assets and sensitive data from partners (FAQ, etc.) stored in separate buckets?

### Tech Lead's Answer
Yes, SPA frontend assets and partner-uploaded data are stored in separate S3 buckets with distinct access controls and encryption keys.

### Recommendation
Maintain strict separation and audit access regularly.

### Risk
Mixing data could lead to accidental exposure.

### Mitigation
- Use separate IAM roles and policies.
- Tag and monitor buckets for compliance.

### Incident Response Plan
- Isolate affected buckets.
- Notify partners if exposure occurs.

---

### Question
How are we storing data at rest in our database?

### Tech Lead's Answer
All data at rest is encrypted using AWS KMS-managed keys. Access to the database is restricted to application services within the VPC.

### Recommendation
Rotate encryption keys regularly and audit access.

### Risk
If keys or access are compromised, data could be exposed.

### Mitigation
- Enforce IAM-based access.
- Enable database audit logging.
- Use KMS key rotation.

### Incident Response Plan
- Rotate keys.
- Investigate access logs.
- Notify affected parties.

---

### Question
How are we ensuring that in case of API key compromise of any partner, other partners and their end users are not compromised?

### Tech Lead's Answer
Each partner has unique API and secret keys. Access is scoped per partner, and keys cannot be used to access other partners' data or services.

### Recommendation
Enforce strict tenant isolation and monitor for cross-tenant access attempts.

### Risk
Improper isolation could lead to data leakage.

### Mitigation
- Use unique credentials per partner.
- Enforce RBAC and tenant checks in all APIs.

### Incident Response Plan
- Revoke compromised keys.
- Audit for cross-tenant access.
- Notify affected partners.

---

### Question
Apart from mTLS, do we also allow trusted IP whitelisting from partner to our service communication?

### Tech Lead's Answer
Yes, partners can request IP whitelisting for additional security. Only traffic from registered IPs is allowed at the firewall and load balancer.

### Recommendation
Encourage partners to use static IPs and maintain up-to-date whitelists.

### Risk
If IPs are spoofed or whitelists are outdated, unauthorized access could occur.

### Mitigation
- Monitor for unexpected source IPs.
- Require regular review of whitelists.

### Incident Response Plan
- Remove compromised IPs from whitelist.
- Notify partners of changes.

---

### Question
How are we ensuring mTLS certificate rotation?

### Tech Lead's Answer
Certificate rotation is automated using AWS ACM or a managed PKI. Expiring certificates trigger alerts and are replaced before expiry.

### Recommendation
Test rotation procedures regularly and document the process.

### Risk
Expired certificates could disrupt service.

### Mitigation
- Monitor certificate expiry dates.
- Automate renewal and deployment.

### Incident Response Plan
- Replace expired certificates immediately.
- Communicate with partners about rotation schedules.

---

### Question
Are we using KMS or Vault to ensure our configs and secrets are securely stored?

### Tech Lead's Answer
Yes, all secrets and configuration values are stored in AWS Secrets Manager or HashiCorp Vault, encrypted with KMS keys. Access is tightly controlled and audited.

### Recommendation
Regularly rotate secrets and audit access logs.

### Risk
If secrets are leaked, attackers could gain privileged access.

### Mitigation
- Enforce least-privilege access.
- Enable automatic secret rotation.
- Monitor for unauthorized access.

### Incident Response Plan
- Rotate affected secrets.
- Investigate and remediate root cause.
- Notify stakeholders.

---
