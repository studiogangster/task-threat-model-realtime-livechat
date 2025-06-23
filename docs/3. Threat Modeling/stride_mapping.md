# STRIDE Threat Mapping

This section maps each major component and data flow in the Sprinklr Live Chat system to the STRIDE threat categories, summarizing key threats and mitigations.

---

## STRIDE Summary Table

| Component / Flow                | S | T | R | I | D | E | Key Mitigations |
|---------------------------------|---|---|---|---|---|---|-----------------|
| End User Devices                | ✔ | ✔ |   | ✔ | ✔ |   | Secure tokens, HTTPS, user education, session validation |
| Partner Web App                  | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | CSP, input validation, SDK integrity, RBAC, audit logs, SRI, CORS, domain whitelisting |
| Partner Admin Users              | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | MFA, RBAC, audit logs, secure login, session management |
| Sprinklr Live Chat Service      | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | TLS, session auth, tenant isolation, logging, quotas |
| LLM Context Service             | ✔ | ✔ |   | ✔ | ✔ | ✔ | Upload validation, output monitoring, least privilege, S2S auth |
| Sprinklr Dashboard & Integration| ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | Secure coding, strong auth, audit logs, input validation |
| S2S LLM Callback to Partner      | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | Mutual TLS, strict validation, rate limiting, signed callbacks |
| SDK Delivery & Supply Chain     | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | Signed SDK, SRI, CORS, CSP, dependency audits, domain whitelisting |

Legend:  
S = Spoofing, T = Tampering, R = Repudiation, I = Information Disclosure, D = Denial of Service, E = Elevation of Privilege

---

## Detailed STRIDE Threat Mapping

### Partner Web App

| STRIDE Category         | Threats                                                                 | Mitigations                                                      |
|------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| Spoofing               | Session hijacking, fake login pages, token theft                        | Secure login, HTTPS, session validation, anti-phishing measures  |
| Tampering              | Malicious script injection, DOM manipulation, SDK tampering             | CSP, input validation, SRI, signed SDK, dependency audits        |
| Repudiation            | Users denying actions (e.g., chat sent, config changed)                 | Audit logs, non-repudiation tokens, user action tracking         |
| Information Disclosure | XSS, leaking chat data, API key exposure, improper CORS                 | Output encoding, CORS/CSP hardening, access controls, encryption |
| Denial of Service      | Resource exhaustion, browser DoS, SDK abuse                             | Rate limiting, input validation, quotas, SDK throttling          |
| Elevation of Privilege | Privilege escalation via vulnerable SDK or misconfig                    | RBAC, least privilege, secure SDK delivery, config validation    |

---

### Sprinklr Live Chat Service

| STRIDE Category         | Threats                                                                 | Mitigations                                                      |
|------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| Spoofing               | Forged session tokens, impersonation of users/partners                   | Strong token validation, mTLS, session management                |
| Tampering              | Message alteration, unauthorized config changes                         | Input validation, config integrity checks, audit logs            |
| Repudiation            | Users/partners denying chat actions or config changes                    | Audit logs, signed actions, trace IDs                            |
| Information Disclosure | Cross-tenant data leakage, log exposure, improper access                | Tenant isolation, access controls, encrypted logs                |
| Denial of Service      | Flooding chat service, resource exhaustion                              | Rate limiting, quotas, auto-scaling, DoS protection              |
| Elevation of Privilege | Service misconfig, privilege escalation via API                         | RBAC, least privilege, config validation                        |

---

### LLM Context Service

| STRIDE Category         | Threats                                                                 | Mitigations                                                      |
|------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| Spoofing               | Forged S2S calls, fake partner backends                                  | mTLS, signed requests, allowlists                                |
| Tampering              | Malicious uploads, prompt injection, data poisoning                     | Upload validation, prompt sanitization, input checks             |
| Repudiation            | Denial of LLM actions or S2S calls                                      | Audit logs, signed requests, traceability                        |
| Information Disclosure | LLM leaking sensitive data, improper callback data                      | Output filtering, data minimization, access controls             |
| Denial of Service      | Malicious uploads, LLM abuse, callback flooding                         | Rate limiting, input validation, quotas                          |
| Elevation of Privilege | LLM agentic flows invoking unauthorized actions                         | Function whitelisting, least privilege, config review            |

---

### Sprinklr Dashboard & Integration Backend

| STRIDE Category         | Threats                                                                 | Mitigations                                                      |
|------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| Spoofing               | Admin impersonation, session hijacking                                  | MFA, secure login, session validation                            |
| Tampering              | Unauthorized config changes, API key manipulation                       | Audit logs, config integrity, access controls                    |
| Repudiation            | Denial of admin actions, config changes                                 | Audit logs, signed actions, traceability                         |
| Information Disclosure | API key leakage, config data exposure                                   | Access controls, encryption, audit logs                          |
| Denial of Service      | Flooding dashboard APIs, resource exhaustion                            | Rate limiting, quotas, monitoring                                |
| Elevation of Privilege | Privilege escalation via dashboard bugs                                 | RBAC, least privilege, secure coding                             |

---

### S2S LLM Callback to Partner Backend

| STRIDE Category         | Threats                                                                 | Mitigations                                                      |
|------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| Spoofing               | Fake Sprinklr calls to partner backend                                   | mTLS, signed requests, allowlists                                |
| Tampering              | Callback data alteration, replay attacks                                | Nonces, input validation, signed requests                        |
| Repudiation            | Denial of callback actions                                              | Audit logs, signed requests, traceability                        |
| Information Disclosure | Sensitive data leakage in callback                                      | Data minimization, access controls, encrypted transport          |
| Denial of Service      | Callback flooding, resource exhaustion                                  | Rate limiting, quotas, monitoring                                |
| Elevation of Privilege | Callback invoking unauthorized partner actions                           | Function whitelisting, least privilege, config review            |

---

### SDK Delivery & Supply Chain

| STRIDE Category         | Threats                                                                 | Mitigations                                                      |
|------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| Spoofing               | Fake SDK delivery, MITM attacks                                         | HTTPS-only, signed SDK, SRI, domain whitelisting                 |
| Tampering              | Malicious SDK modification, dependency compromise                       | Signed SDK, SRI, dependency audits, version pinning              |
| Repudiation            | Denial of SDK delivery or update actions                                | Audit logs, signed delivery, traceability                        |
| Information Disclosure | SDK leaking data, dependency exfiltration                               | Output filtering, access controls, dependency review             |
| Denial of Service      | SDK delivery disruption, dependency DoS                                 | CDN redundancy, monitoring, fallback versions                    |
| Elevation of Privilege | SDK or dependency privilege escalation                                  | Least privilege, secure coding, dependency review                |
