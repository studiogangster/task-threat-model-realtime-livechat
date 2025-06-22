# Threat Modeling: Group & Component Diagrams

This document provides Mermaid diagrams for each major group and component defined in the threat modeling report, visually clarifying trust boundaries and relationships.

---

## 1. High-Level Trust Boundaries

```mermaid
flowchart TD
  subgraph End_User ["End User (Outside Trust Boundary)"]
    EU["End User"]
  end

  subgraph Client_Org ["Client Organization (Outside Trust Boundary)"]
    CAU["Client Admin User"]
    CWA["Client Web App"]
    CB["Client Backend"]
  end

  subgraph Sprinklr_SaaS ["Sprinklr SaaS (Trust Boundary)"]
    SLC["Sprinklr Live Chat Service"]
    LLM["LLM Context Service"]
    SD["Sprinklr Dashboard"]
    IB["Integration Backend"]
  end

  EU -- "Uses" --> CWA
  CAU -- "Configures" --> SD
  CWA -- "Connects" --> SLC
  SLC -- "Queries" --> LLM
  LLM -- "S2S Callback" --> CB
  IB -- "Issues API Keys" --> CWA
```

---

## 2. End User Devices

```mermaid
flowchart TD
  subgraph End_User_Devices ["End User Devices"]
    WEB["Web Device"]
    MOBILE["Mobile Device"]
    OTHER["Other Device"]
  end
```

---

## 3. Client Organization

```mermaid
flowchart TD
  subgraph Client_Org ["Client Organization"]
    CAU["Client Admin User"]
    CWA["Client Web App"]
    CB["Client Backend"]
  end
```

---

## 4. Sprinklr SaaS

```mermaid
flowchart TD
  subgraph Sprinklr_SaaS ["Sprinklr SaaS"]
    SLC["Sprinklr Live Chat Service"]
    LLM["LLM Context Service"]
    SD["Sprinklr Dashboard"]
    IB["Integration Backend"]
  end
```

---

## 5. S2S LLM Callback Flow

```mermaid
flowchart TD
  subgraph Sprinklr_SaaS ["Sprinklr SaaS"]
    LLM["LLM Context Service"]
  end
  subgraph Client_Org ["Client Organization"]
    CB["Client Backend"]
  end
  LLM -- "S2S API Call" --> CB
```

---

## 6. Session Token Flow

```mermaid
flowchart TD
  EU["End User"]
  CWA["Client Web App"]
  SLC["Sprinklr Live Chat Service"]

  EU -- "Login" --> CWA
  CWA -- "Get Token" --> SLC
  EU -- "Connect (Token)" --> SLC
```

---

## 7. Trust Boundary Overview

```mermaid
flowchart TD
  subgraph Outside_Trust ["Outside Trust Boundary"]
    EU["End User"]
    CAU["Client Admin User"]
    CWA["Client Web App"]
    CB["Client Backend"]
  end
  subgraph Inside_Trust ["Sprinklr SaaS (Trust Boundary)"]
    SLC["Sprinklr Live Chat Service"]
    LLM["LLM Context Service"]
    SD["Sprinklr Dashboard"]
    IB["Integration Backend"]
  end
```

---

Each diagram above corresponds to a key group or component from the Definitions section, visually clarifying boundaries and relationships for threat modeling.
