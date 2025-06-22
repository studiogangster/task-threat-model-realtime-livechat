# 

# High-Level Architecture: Sprinklr Live Chat (Trust Boundaries & Isolation)

This diagram provides a high-level view of the main isolated components and trust boundaries in the Sprinklr Live Chat system, focusing on the relationships between the End User, Client, and Sprinklr SaaS.

```mermaid
flowchart TD
  INTERNET_LABEL["Internet (Untrusted Network)"]
  INTERNAL_LABEL["Internal Network"]

  subgraph End_User_Devices ["End User Devices"]
    MOBILE_DEVICE["Mobile Device"]
    WEB_DEVICE["Web Device"]
    OTHER_DEVICE["Other Device"]
  end

  subgraph Client_Org_TB ["Client Organization (Trust Boundary)"]
    INTERNAL_LABEL
    subgraph Client_Admin_TB ["Client Admin Users"]
      CLIENT_ADMIN["Client Admin User"]
    end
    subgraph Client_App_TB ["Client Web App"]
      CLIENT_APP["Client Web App (with Sprinklr SDK)"]
    end
  end

  subgraph Sprinklr_SaaS_TB ["Sprinklr SaaS (Trust Boundary)"]
    subgraph Sprinklr_Live_Chat_Backend ["Live Chat Backend (End User Facing)"]
      LIVE_CHAT["Sprinklr Live Chat Service"]
      LLM_SERVICE["LLM Context Service"]
      CHAT_BACKEND["Backend for Live Chat"]
    end
    subgraph Sprinklr_Integration_Backend ["Integration Backend (Client Admin Facing)"]
      DASHBOARD["Sprinklr Dashboard (Integration/Config)"]
      INTEGRATION_BACKEND["Backend for Integration/Config"]
    end
  end

  %% No arrows from INTERNET_LABEL

  MOBILE_DEVICE -- "Login/Session" --> CLIENT_APP
  WEB_DEVICE -- "Login/Session" --> CLIENT_APP
  OTHER_DEVICE -- "Login/Session" --> CLIENT_APP

  CLIENT_APP -- "Authenticate (API Key/Secret)" --> CHAT_BACKEND
  CLIENT_APP -- "WebSocket (WSS, CORS)" --> LIVE_CHAT
  CLIENT_APP -- "Upload FAQ/Knowledge Graph" --> LLM_SERVICE
  LIVE_CHAT -- "Query" --> LLM_SERVICE
  LLM_SERVICE -- "LLM Response" --> LIVE_CHAT
  LIVE_CHAT -- "Chat Response" --> CLIENT_APP

  CLIENT_ADMIN -- "Integration/Config (Internet)" --> DASHBOARD
  DASHBOARD -- "Onboarding/Authentication" --> INTEGRATION_BACKEND
  INTEGRATION_BACKEND -- "API Key/Secret, SDK" --> CLIENT_APP
```

**Key Points:**
- Each subgraph represents a trust boundary: End User, Client Organization, and Sprinklr SaaS.
- All flows crossing boundaries are labeled and show the direction of trust and data.
- This diagram is more abstract and focused on trust/isolation than previous detailed DFDs.

---

## Component-Level Breakdown, Diagrams, and Threat Model Templates

---

### 1. End User Devices

#### 1.1 Mobile Device

**Description:**  
A smartphone or tablet used by the end user to access the client web app.

```mermaid
flowchart TD
  MOBILE["Mobile Device"]
  BROWSER["Mobile Browser/App"]
  SESSION["Session Storage"]
  MOBILE --> BROWSER
  BROWSER --> SESSION
```

**Threat Model Template:**
- **Threats:**
  - Device compromise (malware, rooting/jailbreak)
  - Insecure app/browser
  - Session hijacking
  - Phishing attacks
- **Mitigations:**
  - Encourage OS/app updates
  - Use secure cookies/tokens
  - Enforce HTTPS
  - Educate users on phishing
- **Open Questions:**
  - Are there any device-specific security controls?

---

#### 1.2 Web Device

**Description:**  
A desktop or laptop computer used by the end user to access the client web app.

```mermaid
flowchart TD
  WEB["Web Device"]
  BROWSER["Web Browser"]
  SESSION["Session Storage"]
  WEB --> BROWSER
  BROWSER --> SESSION
```

**Threat Model Template:**
- **Threats:**
  - Browser vulnerabilities (XSS, CSRF)
  - Keyloggers/malware
  - Session hijacking
- **Mitigations:**
  - Use CSP, input validation
  - Secure session management
  - Encourage endpoint security
- **Open Questions:**
  - What browsers are officially supported?

---

#### 1.3 Other Device

**Description:**  
Any other device (e.g., kiosk, IoT) used to access the client web app.

```mermaid
flowchart TD
  OTHER["Other Device"]
  BROWSER["Browser/App"]
  SESSION["Session Storage"]
  OTHER --> BROWSER
  BROWSER --> SESSION
```

**Threat Model Template:**
- **Threats:**
  - Physical access attacks
  - Untrusted device environment
- **Mitigations:**
  - Limit session duration
  - Require re-authentication
- **Open Questions:**
  - Are there device restrictions for access?

---

### 2. Client Organization

#### 2.1 Client Web App

**Description:**  
The web application delivered to end users, integrating the Sprinklr SDK.

```mermaid
flowchart TD
  UI["UI Layer"]
  SDK["Sprinklr SDK"]
  AUTH["Auth Module"]
  WS["WebSocket Handler"]
  FAQ["FAQ/Knowledge Upload"]
  UI --> SDK
  SDK --> AUTH
  SDK --> WS
  SDK --> FAQ
```

**Threat Model Template:**
- **Threats:**
  - XSS/CSRF
  - Supply chain attacks (malicious SDK)
  - API key leakage
- **Mitigations:**
  - CSP, input/output validation
  - SDK integrity checks
  - Never expose secrets in frontend
- **Open Questions:**
  - How is SDK versioning managed?

---

#### 2.2 Client Admin User

**Description:**  
An admin user from the client organization who configures integration via the Sprinklr Dashboard.

```mermaid
flowchart TD
  ADMIN["Client Admin User"]
  PORTAL["Admin Portal"]
  ADMIN --> PORTAL
```

**Threat Model Template:**
- **Threats:**
  - Credential theft
  - Privilege escalation
  - Phishing/social engineering
- **Mitigations:**
  - Enforce MFA
  - Role-based access control
  - Audit admin actions
- **Open Questions:**
  - How are admin accounts provisioned and deprovisioned?

---

### 3. Sprinklr SaaS

#### 3.1 Live Chat Service

**Description:**  
Backend service handling real-time chat sessions.

```mermaid
flowchart TD
  WS["WebSocket Endpoint"]
  ROUTER["Message Router"]
  SESSION["Session Manager"]
  WS --> ROUTER
  ROUTER --> SESSION
```

**Threat Model Template:**
- **Threats:**
  - WebSocket hijacking
  - DoS via open connections
  - Cross-tenant data leakage
- **Mitigations:**
  - Enforce TLS
  - Authenticate every session
  - Tenant isolation in routing
- **Open Questions:**
  - What is the max concurrent connection policy?

---

#### 3.2 LLM Context Service

**Description:**  
Service ingesting FAQ/knowledge uploads and providing context to LLM.

```mermaid
flowchart TD
  UPLOAD["FAQ/Knowledge Upload"]
  CONTEXT["Context Store"]
  LLM["LLM Engine"]
  UPLOAD --> CONTEXT
  CONTEXT --> LLM
```

**Threat Model Template:**
- **Threats:**
  - Malicious uploads (prompt injection)
  - Data poisoning
- **Mitigations:**
  - Validate/sanitize uploads
  - Monitor LLM outputs
- **Open Questions:**
  - How is per-tenant context isolation enforced?

---

#### 3.3 Dashboard

**Description:**  
Web interface for onboarding, configuration, and analytics.

```mermaid
flowchart TD
  UI["Dashboard UI"]
  AUTH["Auth Service"]
  API["API Layer"]
  UI --> AUTH
  UI --> API
```

**Threat Model Template:**
- **Threats:**
  - XSS/CSRF in dashboard
  - Unauthorized access
- **Mitigations:**
  - Secure coding practices
  - Strong authentication
- **Open Questions:**
  - Is there SSO support for client admins?

---

#### 3.4 Backend (Integration/Config)

**Description:**  
Backend for authentication, verification, API key/secret issuance, and integration.

```mermaid
flowchart TD
  API["API Gateway"]
  AUTH["Auth Service"]
  DB["Database"]
  LOG["Audit/Logging"]
  API --> AUTH
  API --> DB
  API --> LOG
```

**Threat Model Template:**
- **Threats:**
  - API key leakage
  - Insider threats
- **Mitigations:**
  - Audit logs
  - Key rotation
- **Open Questions:**
  - How are API keys delivered to clients?

---

#### 3.5 Backend (Live Chat)

**Description:**  
Backend for live chat session management and message routing.

```mermaid
flowchart TD
  SESSION["Session Store"]
  ROUTER["Message Router"]
  DB["Chat DB"]
  SESSION --> ROUTER
  ROUTER --> DB
```

**Threat Model Template:**
- **Threats:**
  - Session hijacking
  - Data leakage between tenants
- **Mitigations:**
  - Per-tenant session isolation
  - Secure session tokens
- **Open Questions:**
  - How is session expiration handled?

---

## 4. S2S LLM Callback to Client Backend

### 4.1 High-Level Diagram

**Description:**  
Shows the flow when the LLM service, upon receiving a user query, makes a server-to-server (S2S) API call to the client backend to fetch personalized data.

```mermaid
flowchart TD
  ENDUSER["End User"]
  LIVECHAT["Sprinklr Live Chat Service"]
  LLM["LLM Context Service"]
  CLIENTBACKEND["Client Backend Service"]

  ENDUSER -- "Query" --> LIVECHAT
  LIVECHAT -- "Query" --> LLM
  LLM -- "S2S API Call" --> CLIENTBACKEND
  CLIENTBACKEND -- "Personalized Data" --> LLM
  LLM -- "LLM Response" --> LIVECHAT
  LIVECHAT -- "Chat Response" --> ENDUSER
```

---

### 4.2 LLM Context Service (S2S Callback Component)

**Description:**  
Handles outbound S2S API calls to client backend for personalized data retrieval.

```mermaid
flowchart TD
  LLM["LLM Context Service"]
  S2S["S2S API Handler"]
  CLIENTBACKEND["Client Backend Service"]
  LLM --> S2S
  S2S -- "API Call" --> CLIENTBACKEND
  CLIENTBACKEND -- "Data" --> S2S
  S2S --> LLM
```

**Threat Model Template:**
- **Threats:**
  - Unauthorized S2S access
  - Data leakage
  - Replay or injection attacks
- **Mitigations:**
  - Mutual TLS or signed tokens
  - Strict data validation
  - Rate limiting and logging
- **Open Questions:**
  - How is S2S authentication managed?

---

### 4.3 Client Backend Service (S2S Endpoint Component)

**Description:**  
Receives S2S API calls from Sprinklr LLM service and returns personalized data.

```mermaid
flowchart TD
  S2S["S2S API Endpoint"]
  DATASTORE["Personalized Data Store"]
  S2S --> DATASTORE
  DATASTORE --> S2S
```

**Threat Model Template:**
- **Threats:**
  - Unauthorized data access
  - Excessive data exposure
  - Abuse of S2S endpoint
- **Mitigations:**
  - Strong authentication/authorization
  - Data minimization
  - Monitoring and alerting
- **Open Questions:**
  - What data is exposed via S2S, and how is access scoped?
