# Q&A: Secure Uploaded Data at Rest and Partner Data Isolation

## Question
How are we ensuring that uploaded data is secure at rest, and in case of any data leak, other partners' data is not impacted?

## Tech Lead's Answer
All uploaded data is encrypted at rest using AWS KMS-managed keys. Each partner's data is stored in a logically separated S3 path or bucket, with IAM policies enforcing strict access controls. Cross-partner access is not permitted at any layer. Regular audits and monitoring are performed to detect and prevent misconfigurations.

## Recommendation
Continue to enforce strict tenant isolation and encryption at rest. Periodically review IAM policies and bucket configurations.

## Risk
A misconfiguration could allow unauthorized access to another partner's data in the event of a data leak.

## Mitigation
- Use separate S3 buckets or prefixes per partner.
- Enforce least-privilege IAM policies.
- Enable S3 access logging and AWS Config rules for compliance.

## Incident Response Plan
- Immediately restrict access to affected data.
- Notify impacted partners.
- Audit and remediate misconfigurations.
- Review and update isolation controls.
