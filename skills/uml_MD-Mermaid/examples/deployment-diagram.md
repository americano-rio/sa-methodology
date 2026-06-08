# Deployment Diagram

Shows physical deployment architecture of the system.

## Approach in Mermaid

Use **`block-beta`** for clean deployment boxes, or **`flowchart TD`** with subgraphs for named environments. C4 `C4Deployment` is also available for formal deployment diagrams.

## Recommended Colors

| Element | Fill | Stroke | Usage |
|---|---|---|---|
| Load balancer | `#dae8fc` | `#6c8ebf` | Entry point / routing |
| Web/App server | `#d5e8d4` | `#82b366` | Application servers |
| Database | `#fff2cc` | `#d6b656` | Data storage |
| Cache | `#ffe6cc` | `#d79b00` | Caching layer |
| External service | `#e1d5e7` | `#9673a6` | Third-party |
| Cloud boundary | `#f5f5f5` | `#cccccc` | Cloud/environment boundary |

## Example 1

AWS deployment with load balancer, app servers, and database cluster (flowchart):

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial', 'lineColor': '#555555'}}}%%
flowchart TD
  classDef lb     fill:#dae8fc,stroke:#6c8ebf
  classDef web    fill:#d5e8d4,stroke:#82b366
  classDef db     fill:#fff2cc,stroke:#d6b656
  classDef cache  fill:#ffe6cc,stroke:#d79b00
  classDef ext    fill:#e1d5e7,stroke:#9673a6

  Users(["👤 Users"])

  subgraph External["External"]
    CDN["CDN\nCloudFront"]:::ext
    Email["Email Service\nSES"]:::ext
  end

  subgraph AWS["AWS Cloud"]
    LB["Load Balancer\nNginx / ALB"]:::lb

    subgraph WebTier["Web Tier"]
      WEB1["Web Server 1\nApp v2.1 / Node.js 20"]:::web
      WEB2["Web Server 2\nApp v2.1 / Node.js 20"]:::web
    end

    subgraph DataTier["Data Tier"]
      DB_PRI[("PostgreSQL\nPrimary")]:::db
      DB_REP[("PostgreSQL\nReplica")]:::db
      REDIS[("Redis Cache")]:::cache
    end

    subgraph Storage["Storage"]
      S3[("S3 Bucket")]:::db
    end
  end

  Users      -->|HTTPS| CDN
  CDN        -->|HTTPS| LB
  LB         -->|HTTP| WEB1 & WEB2
  WEB1 & WEB2 -->|JDBC| DB_PRI
  DB_PRI     -->|Replication| DB_REP
  WEB1 & WEB2 -->|TCP| REDIS
  WEB1       -->|S3 API| S3
  WEB1       -.->|SMTP| Email
```

## Example 2

Kubernetes deployment with block-beta:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
block-beta
  columns 3

  Internet["🌐 Internet"]:3

  block:k8s["Kubernetes Cluster"]:3
    columns 3

    block:ingress_block["Ingress"]:3
      INGRESS["Nginx Ingress\n:443"]
    end

    block:svc_block["Services"]:3
      columns 3
      SVC_WEB["svc/web\nClusterIP"]
      SVC_API["svc/api\nClusterIP"]
      SVC_DB["svc/db\nClusterIP"]
    end

    block:pod_block["Pods"]:3
      columns 3
      POD_WEB["pod: web\nx3 replicas"]
      POD_API["pod: api\nx3 replicas"]
      POD_DB["pod: postgres\nx1 StatefulSet"]
    end

    block:cm_block["Config"]:3
      columns 3
      CM["ConfigMap"]
      SEC["Secret"]
      PVC["PersistentVolumeClaim"]
    end
  end

  Internet --> INGRESS
  INGRESS  --> SVC_WEB & SVC_API
  SVC_WEB  --> POD_WEB
  SVC_API  --> POD_API
  SVC_DB   --> POD_DB
  POD_API  --> SVC_DB

  style INGRESS  fill:#dae8fc,stroke:#6c8ebf
  style SVC_WEB  fill:#d5e8d4,stroke:#82b366
  style SVC_API  fill:#d5e8d4,stroke:#82b366
  style SVC_DB   fill:#d5e8d4,stroke:#82b366
  style POD_WEB  fill:#A8D08D,stroke:#82b366
  style POD_API  fill:#A8D08D,stroke:#82b366
  style POD_DB   fill:#fff2cc,stroke:#d6b656
```
