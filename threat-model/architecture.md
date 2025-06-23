# Architecture: Sprinklr Live Chat

## 1. High-Level System Architecture

This section provides a top-level overview of the main trust boundaries and data flows in the Sprinklr Live Chat system. It illustrates how the End User interacts with the Client Web App, how authentication and session tokens are managed, and how the Client Web App and End User communicate with the Sprinklr SaaS platform for live chat functionality. The diagram highlights the separation between client and Sprinklr trust boundaries and the key steps in the authentication and chat flow.

```mermaid
flowchart TD
  subgraph EndUser["End User (Outside Trust Boundary)"]
    EU["End User"]
  end

  subgraph ClientOrg["Client Organization (Outside Trust Boundary)"]
    CWA["Client Web App"]
    CB["Client Backend"]
  end

  subgraph SprinklrSaaS["Sprinklr SaaS (Trust Boundary)"]
    SLC["Sprinklr SaaS"]
  end

  subgraph LLMExt["LLM AI Service (External)"]
    LLMAPI["OpenAI / Anthropic / etc."]
  end

  EU -- "1 - Login" --> CWA
  CWA -- "2 - Authenticate" --> CB
  CWA -- "3 - Request Sprinklr Token" --> SLC
  SLC -- "4 - Issue Session Token" --> CWA
  EU -- "5 - Live Chat to Sprinklr with Token" --> SLC
  SLC -- "6 - S2S or API Call" --> CB
  SLC -- "7 - LLM Query (API Call)" --> LLMAPI
```

---

## 2. Sprinklr SaaS Cloud Architecture & Data Flows

This section details the internal cloud architecture of the Sprinklr SaaS platform as deployed on AWS, and the major data flows between its components. These diagrams help identify cloud-specific attack surfaces, trust boundaries, and the secure delivery path for the SDK.

### 2.1 High-Level AWS System Architecture

The following diagram shows the main AWS infrastructure components used by Sprinklr SaaS, including load balancers, API gateways, compute clusters, storage, caching, logging, and the use of S3 and CloudFront for secure SDK delivery. WebSocket connections for live chat are supported via the Application Load Balancer (ALB) and routed to the ECS/EKS app services. This view is essential for understanding network segmentation, service isolation, and the entry points for both API and static asset requests.

```mermaid
flowchart TD
  subgraph Public["Public Internet"]
    PUB["End User / Client Web App"]
    CAU["Client Admin User"]
  end

  subgraph VPC["AWS VPC (Sprinklr SaaS Trust Boundary)"]
    ALB["Application Load Balancer (WebSocket-enabled, Public)"]
    AGW["API Gateway"]
    ECS["ECS/EKS (App Services)"]
    LLM["LLM Context Service"]
    RDS["RDS/Database"]
    S3PUB["S3 (Public: Static SPA Assets)"]
    S3PRIV["S3 (Private: FAQ/Knowledge Data)"]
    REDIS["Redis/Cache"]
    LOGS["CloudWatch Logs"]
    CF["CloudFront CDN"]
    DASH["Sprinklr Dashboard (SPA)"]
  end

  subgraph LLMExt["LLM AI Service (External)"]
    LLMAPI["OpenAI / Anthropic / etc."]
  end

  %% All users access via ALB
  PUB -- "HTTPS / WebSocket" --> ALB
  CAU -- "HTTPS" --> ALB

  %% ALB routes to API Gateway and ECS
  ALB -- "API / WebSocket" --> AGW
  ALB -- "API / WebSocket" --> ECS

  %% Dashboard (SPA) is served as static assets via CloudFront
  CF -- "Serves SPA Static Assets" --> PUB
  S3PUB -- "Static Assets" --> CF

  %% Dashboard (SPA) API calls go via ALB -> AGW -> ECS
  AGW -- "Routes API Calls" --> ECS
  DASH -- "API Calls" --> AGW

  %% ECS/LLM access private S3 for FAQ/Knowledge
  ECS -- "FAQ/Knowledge Access" --> S3PRIV
  LLM -- "FAQ/Knowledge Access" --> S3PRIV

  %% ECS/LLM internal flows
  ECS --> LLM
  ECS --> RDS
  ECS --> REDIS
  ECS --> LOGS
  LLM --> RDS

  %% LLM Context Service calls external LLM API
  LLM -- "API Call" --> LLMAPI

  %% Static SDK delivery (if needed)
  S3PUB -- "SDK Static Files" --> CF

  %% Dashboard login/integration management via ALB
  ALB -- "Dashboard HTTPS" --> DASH
```

---

### 2.2 Data Flow Diagram (DFD)

This diagram illustrates the flow of data between the main actors and services in the Sprinklr SaaS platform, including user authentication, chat session management, LLM context queries, S2S/API calls, and the secure delivery of the SDK via CloudFront and S3. It highlights the key integration points and the movement of sensitive data across trust boundaries.

```mermaid
flowchart TD
  EU["End User"]
  CWA["Client Web App"]
  AGW["API Gateway"]
  ECS["App Services"]
  LLM["LLM Context Service"]
  RDS["Database"]
  S3["S3 Storage & SDK"]
  CF["CloudFront CDN"]
  CB["Client Backend"]

  EU -- "Login/Chat" --> CWA
  CWA -- "API Request" --> AGW
  AGW -- "Routes" --> ECS
  ECS -- "Session/Chat Data" --> RDS
  ECS -- "Knowledge/Files" --> S3
  ECS -- "LLM Query" --> LLM
  LLM -- "Context/Data" --> RDS
  LLM -- "Knowledge" --> S3
  ECS -- "S2S/API Call" --> CB
  CWA -- "SDK Request" --> CF
  CF -- "Serves SDK" --> CWA
  CF -- "Fetches from" --> S3
```

---

## 3. Data Flow Diagrams

(See attached diagrams for each group and component)

---

## Summary

This architecture document provides a detailed view of the Sprinklr Live Chat platform's cloud infrastructure, data flows, and integration points. It is intended to support further analysis in threat modeling and secure architecture.
