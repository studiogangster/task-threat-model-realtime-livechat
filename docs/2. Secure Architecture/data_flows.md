# Data Flow Diagram (DFD)

This section illustrates the flow of data between the main actors and services in the Sprinklr SaaS platform, including user authentication, chat session management, LLM context queries, S2S/API calls, and the secure delivery of the SDK via CloudFront and S3. It highlights the key integration points and the movement of sensitive data across trust boundaries.

```mermaid
flowchart TD
  EU["End User"]
  PWA["Partner Web App"]
  AGW["API Gateway"]
  ECS["App Services"]
  LLM["LLM Context Service"]
  RDS["Database"]
  S3["S3 Storage & SDK"]
  CF["CloudFront CDN"]
  PB["Partner Backend"]

  EU -- "Login/Chat" --> PWA
  PWA -- "API Request" --> AGW
  AGW -- "Routes" --> ECS
  ECS -- "Session/Chat Data" --> RDS
  ECS -- "Knowledge/Files" --> S3
  ECS -- "LLM Query" --> LLM
  LLM -- "Context/Data" --> RDS
  LLM -- "Knowledge" --> S3
  ECS -- "S2S/API Call" --> PB
  PWA -- "SDK Request" --> CF
  CF -- "Serves SDK" --> PWA
  CF -- "Fetches from" --> S3
```

## Key Points

- Sensitive data is protected at all stages with encryption in transit and at rest.
- All API and chat flows are authenticated and authorized.
- The SDK is delivered securely via CloudFront and S3, ensuring integrity and availability.
