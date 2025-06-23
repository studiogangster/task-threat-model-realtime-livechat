# Data Storage & Compliance Considerations

This section details the data storage locations, compliance requirements, retention policies, and key risks and mitigations for the Sprinklr Live Chat system.

---

## Data Inventory

| Data Type         | Stored In                | Trust Boundary      | Compliance Req.         | Retention Policy | Encryption | Deletion/Access |
|-------------------|-------------------------|---------------------|-------------------------|------------------|------------|----------------|
| Chat Logs         | Sprinklr DB             | Internal (Trusted)  | GDPR, CCPA, Audit       | 90 days (default) | Yes        | On request     |
| User Profiles     | Partner Backend          | External (Partner)   | Partner's responsibility | N/A              | N/A        | N/A            |
| Uploaded FAQ/Knowledge | LLM Context Store  | Internal (Trusted)  | GDPR, CCPA              | Until deleted    | Yes        | On request     |
| Session Tokens    | Browser/Sprinklr Auth   | Mixed               | Secure storage, expiry  | Short-lived      | Yes        | Auto-expiry    |
| API Keys/Secrets  | Sprinklr Vault/Partner   | Mixed               | Secure storage, rotation| Until rotated    | Yes        | On rotation    |
| Audit Logs        | Sprinklr Logging        | Internal (Trusted)  | Audit, GDPR             | 1 year           | Yes        | On request     |
| User Data Fetched via S2S/LLM Agent | Sprinklr Memory/Cache (Ephemeral) | Internal (Trusted)  | GDPR, CCPA, Purpose Limitation | Ephemeral (request scope only) | Yes (in transit & at rest) | Deleted after use (no persistent storage) |

---

## Compliance Matrix

| Component                    | GDPR | CCPA | Data Residency | Retention | Deletion | Auditability |
|------------------------------|------|------|---------------|-----------|----------|--------------|
| Sprinklr Live Chat Service   |  ✔   |  ✔   |   ✔ (config)  |   ✔       |   ✔      |     ✔        |
| LLM Context Service          |  ✔   |  ✔   |   ✔ (config)  |   ✔       |   ✔      |     ✔        |
| Sprinklr Dashboard           |  ✔   |  ✔   |   ✔           |   ✔       |   ✔      |     ✔        |
| Integration Backend          |  ✔   |  ✔   |   ✔           |   ✔       |   ✔      |     ✔        |
| Partner Backend               |  ✖   |  ✖   |   Partner      |   Partner  |  Partner  |   Partner     |

---

## Key Risks & Mitigations

- **Supply Chain & SDK Delivery Attacks:**
  - Mitigation: Signed SDK delivery, SRI enforcement, strict domain whitelisting, secure HTTPS-only distribution, regular dependency audits, and rapid response to discovered vulnerabilities. Harden CSP and CORS policies to prevent unauthorized resource loading and cross-origin attacks.
- **Unauthorized Data Access:**  
  - Mitigation: Encryption at rest, strict access controls, audit logging.
- **Improper Data Retention/Deletion:**  
  - Mitigation: Automated retention policies, user-initiated deletion, compliance reviews.
- **Cross-Border Data Transfer:**  
  - Mitigation: Data residency controls, region-aware storage, contractual safeguards.
- **Lack of User Consent:**  
  - Mitigation: Consent management in partner apps, clear privacy policies.
- **Data Leakage via LLM/Agentic Flows or S2S Fetched Data:**  
  - Mitigation: Output filtering, data minimization, ephemeral storage, strict purpose limitation, and immediate deletion after use. Ensure no persistent logging of sensitive user data fetched from partner backends.
- **Cryptographic Isolation Failure (Key/Cert Leakage):**
  - Mitigation: Per-tenant key management, strict logical and physical separation of keys/certs, automated rotation, and monitoring. Compromise of one partner's keys or mTLS certificates must not impact other tenants. Use hardware security modules (HSMs) or secure enclaves for key storage where possible.

---

## Open Compliance Questions

- Are all data flows and storage locations documented and reviewed for compliance?
- How is user consent for data processing and storage obtained and tracked?
- Are data residency requirements configurable per partner/tenant?
- What is the process for fulfilling data subject access/deletion requests (DSARs)?
- How are compliance controls tested and audited?
- How is user data fetched via S2S/LLM Agentic flows handled, logged, and deleted? Is there any risk of inadvertent persistence or logging of sensitive data? Are ephemeral storage and deletion controls auditable?
- How is cryptographic isolation enforced between tenants? What is the process for detecting, containing, and remediating a key or certificate compromise for a single partner, and how is blast radius minimized?
