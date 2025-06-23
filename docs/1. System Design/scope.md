# Scope

This section defines the scope and trust boundaries for the Sprinklr Live Chat platform.

## Trust Boundaries

- **Sprinklr SaaS**: All Sprinklr-operated services and infrastructure are considered within the trust boundary. We assume full control and responsibility for security, compliance, and operations within this environment.
- **External Entities**: All other entities—including partner organizations, partner web/mobile apps, partner backends, partner admin users, and end users—are considered outside the trust boundary. These are treated as untrusted or semi-trusted, and all interactions crossing into Sprinklr SaaS are subject to strict security controls and monitoring.

## Deployment Environment

- The Sprinklr Live Chat platform is deployed on AWS Cloud, leveraging managed services for scalability, availability, and security.
- Multi-region deployment is used to ensure high availability and performance.
- All public and partner access is routed through secure gateways (API Gateway/ELB) with mTLS and strict authentication/authorization.

## Out of Scope

- Services not operated by Sprinklr (e.g., third-party analytics, external LLM APIs) are considered out of scope for direct control, but are subject to risk assessment and monitoring.
- On-premises deployments and non-cloud environments.
