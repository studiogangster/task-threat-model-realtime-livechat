# Thread Modeling Report

## Sprinklr Live Chat

---

# Preface

**Project Details:**  
**"Sprinklr Live Chat"** is a centralized SaaS platform designed to enable organizations to integrate real-time chat functionality into their web and mobile applications. The platform provides a secure, scalable, and context-aware chat experience, leveraging large language models (LLMs) for intelligent responses and knowledge integration.

**Activity:**  
This document presents a comprehensive threat modeling exercise for the **Sprinklr Live Chat** system. The goal is to systematically identify, analyze, and mitigate security threats across all major components and data flows. The report includes high-level and component-level diagrams, threat analysis, mitigations, and open questions to guide ongoing security improvements.

**Assumptions:**  
- **Sprinklr SaaS** (including all Sprinklr-operated services and infrastructure) is considered **within our trust boundary**.
- All other entities—including **end users**, **client organizations**, **client web apps**, and **client backends** —are considered **outside the trust boundary**.
- The threat model assumes a zero-trust approach for all interactions crossing into Sprinklr from external users, systems, or services.
- The analysis is based on the current system architecture and may require updates as the platform evolves. 

**Note:** For the scope of simplicity, we will restrict our focus on web service and SDK, and mobile SDK . services would be considered out of scope for this activity.

---

# Overview

**Sprinklr Live Chat** is a SaaS platform enabling organizations to integrate real-time chat into their web and mobile applications. 

The system is designed with strong isolation between *end users**, *client organizations***, and *Sprinklr SaaS****, and leverages LLMs (local models + external API based services) for contextual responses. 

Key trust boundaries and data flows are identified to ensure robust security.

---

# Definitions


**Client (Client Organization):**  (Outside of our trust-zone)
An organization (e.g., a bank, retailer) that integrates Sprinklr Live Chat into its own web or mobile applications for its customers. Ex: HDFC Bank, ICICI Bank, etc.



**Client Web App:**   
The web or mobile application owned by the client organization, into which the Sprinklr SDK is embedded.


**Client Admin User:**  
A user from the client organization responsible for configuring, managing, and monitoring the Sprinklr integration via the Sprinklr Dashboard.

**End User:**   
A person who interacts with the client’s web or mobile application to access live chat functionality. Typically a customer of the client organization. Ex: A customer of HDFC Bank

*Note*: The same end-user, but from different organization client, shall be considered as an unique user in our system.


**Sprinklr SaaS:**  
Sprinklr is a centralized cloud platform offering a comprehensive suite of services, including live chat, LLM-powered intelligence, dashboards, and integrations. 
This platform unifies all unique services within our trusted system, delivering a cohesive and secure SaaS experience.

**Sprinklr Live Chat Service:**   
The backend service within Sprinklr SaaS that manages real-time chat sessions between end users and the system.

**Sprinklr LLM Context Service:**   
A service that ingests FAQs and knowledge uploads from clients, provides context to the LLM, and may call back to client backends for personalized data.

**Sprinklr Dashboard:**  
A web interface for client admins to onboard, configure, and monitor their Sprinklr integration.

**Sprinklr Integration Backend:**  
Sprinklr’s backend service for authentication, verification, API key/secret issuance, and integration management.

**S2S (Server-to-Server) Callback:** 
A direct API call from Sprinklr’s LLM Context Service to the client’s backend to retrieve personalized data for an end user query.

**Session Token:**  
A secure, time-limited token used to authenticate and authorize end users details and services in the system. 

**Trust Boundary:**  
A logical separation between different security domains (e.g., end user, client, Sprinklr SaaS) where data and trust must be explicitly managed.


---

# Detailed Report

## 1. High-Level System Architecture

(See attached diagram: High-Level Trust Boundaries & Data Flows)

## 2. Component-Level Analysis

### 2.1 End User Devices
- **Threats:** Device compromise, session hijacking, phishing
- **Mitigations:** Secure tokens, HTTPS, user education

### 2.2 Client Web App
- **Threats:** XSS/CSRF, supply chain (SDK), API key leakage
- **Mitigations:** CSP, input validation, SDK integrity checks

### 2.3 Client Admin Users
- **Threats:** Credential theft, privilege escalation
- **Mitigations:** MFA, RBAC, audit logs

### 2.4 Sprinklr Live Chat Service
- **Threats:** WebSocket hijacking, DoS, cross-tenant leakage
- **Mitigations:** TLS, session authentication, tenant isolation

### 2.5 LLM Context Service
- **Threats:** Malicious uploads, data poisoning
- **Mitigations:** Upload validation, LLM output monitoring

### 2.6 Sprinklr Dashboard & Integration Backend
- **Threats:** XSS/CSRF, API key leakage, insider threats
- **Mitigations:** Secure coding, strong auth, audit logs

### 2.7 S2S LLM Callback to Client Backend
- **Threats:** Unauthorized S2S access, data leakage, replay/injection
- **Mitigations:** Mutual TLS, strict validation, rate limiting

---

## 3. Data Flow Diagrams

(See attached diagrams for each group and component)

---

## 4. Threat Model Summary

- **Authentication & Authorization:** Strong tokens, MFA, RBAC
- **Data Isolation:** Per-tenant isolation at all layers
- **Real-Time Communication:** TLS, session validation, quotas
- **LLM Context & S2S:** Input validation, least privilege, monitoring
- **Incident Response:** Logging, anomaly detection, key rotation

---

## 5. Open Questions & Gaps

- Third-party integrations, mobile-specific threats, incident response process
- Key management, privacy compliance, LLM abuse controls
- S2S endpoint scoping and authentication

---

_This report should be reviewed and updated as the system evolves and new features or integrations are added._
