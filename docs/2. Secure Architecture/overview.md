# Overview:

This section provides a top-level overview of the main trust boundaries and data flows in the Sprinklr Live Chat system. It illustrates how the End User interacts with the Client Web App, how authentication and session tokens are managed, and how the Client Web App and End User communicate with the Sprinklr SaaS platform for live chat functionality. The diagram highlights the separation between client and Sprinklr trust boundaries and the key steps in the authentication and chat flow.

```mermaid
flowchart TD
  subgraph EndUser["End User (Outside Trust Boundary)"]
    EU["End User"]
  end

  subgraph PartnerOrg["Partner Organization (Outside Trust Boundary)"]
    PWA["Partner Web App"]
    PB["Partner Backend"]
  end

  subgraph SprinklrSaaS["Sprinklr SaaS (Trust Boundary)"]
    SLC["Sprinklr SaaS"]
  end

  subgraph LLMExt["LLM AI Service (External)"]
    LLMAPI["OpenAI / Anthropic / etc."]
  end

  EU -- "1 - Login" --> PWA
  PWA -- "2 - Authenticate" --> PB
  PWA -- "3 - Request Sprinklr Token" --> SLC
  SLC -- "4 - Issue Session Token" --> PWA
  EU -- "5 - Live Chat to Sprinklr with Token" --> SLC
  SLC -- "6 - S2S or API Call" --> PB
  SLC -- "7 - LLM Query (API Call)" --> LLMAPI
```