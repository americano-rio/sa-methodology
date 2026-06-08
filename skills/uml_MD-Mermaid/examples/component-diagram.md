# Component Diagram

Shows system component organization, interfaces, and dependencies.

## Approach in Mermaid

Mermaid uses **`block-beta`** for component/architecture diagrams.  
For C4-style architecture, use `C4Component` or `C4Context` diagrams.

## block-beta Syntax

```
block-beta
  columns N
  A["Label"]:width   %% width in columns
  B["Label"]
  A --> B
```

- `columns N` — set grid column count
- `block:id ... end` — grouped sub-block
- `NodeID["Label"]:N` — node spanning N columns
- Arrow types: `-->`, `--`, `-.->`, `==>`
- Add `style NodeID fill:#color,stroke:#color` for colors

## Recommended Colors

| Element | Fill | Stroke | Usage |
|---|---|---|---|
| Core component | `#dae8fc` | `#6c8ebf` | Main business logic |
| Service component | `#d5e8d4` | `#82b366` | Service layer |
| Data component | `#fff2cc` | `#d6b656` | Data access/storage |
| External component | `#e1d5e7` | `#9673a6` | External/third-party |
| Package boundary | `#f5f5f5` | `#cccccc` | Subsystem boundaries |

## Example 1

E-commerce system with layered components and dependencies:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
block-beta
  columns 4

  block:presentation["Presentation Layer"]:4
    columns 2
    WebUI["Web UI"]
    MobileAPI["Mobile API"]
  end

  block:business["Business Layer"]:4
    columns 4
    OrderSvc["Order Service"]
    PaySvc["Payment Service"]
    CatalogSvc["Catalog Service"]
    UserSvc["User Service"]
  end

  block:data["Data Layer"]:4
    columns 3
    OrderRepo["Order Repository"]
    ProductRepo["Product Repository"]
    UserRepo["User Repository"]
  end

  block:external["External"]:4
    columns 2
    PayGW["Payment Gateway"]
    EmailProv["Email Provider"]
  end

  WebUI       --> OrderSvc
  WebUI       --> CatalogSvc
  MobileAPI   --> OrderSvc
  MobileAPI   --> UserSvc

  OrderSvc    --> PaySvc
  OrderSvc    --> OrderRepo
  PaySvc      --> PayGW
  CatalogSvc  --> ProductRepo
  UserSvc     --> UserRepo
  OrderSvc    --> EmailProv

  style WebUI       fill:#96CBFE,stroke:#6c8ebf
  style MobileAPI   fill:#96CBFE,stroke:#6c8ebf
  style OrderSvc    fill:#A8D08D,stroke:#82b366
  style PaySvc      fill:#A8D08D,stroke:#82b366
  style CatalogSvc  fill:#A8D08D,stroke:#82b366
  style UserSvc     fill:#A8D08D,stroke:#82b366
  style OrderRepo   fill:#fff2cc,stroke:#d6b656
  style ProductRepo fill:#fff2cc,stroke:#d6b656
  style UserRepo    fill:#fff2cc,stroke:#d6b656
  style PayGW       fill:#e1d5e7,stroke:#9673a6
  style EmailProv   fill:#e1d5e7,stroke:#9673a6
```

## Example 2

C4-style component diagram using flowchart:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial', 'primaryColor': '#dae8fc'}}}%%
flowchart TB
  classDef ui       fill:#96CBFE,stroke:#6c8ebf
  classDef service  fill:#A8D08D,stroke:#82b366
  classDef data     fill:#fff2cc,stroke:#d6b656
  classDef external fill:#e1d5e7,stroke:#9673a6

  subgraph PL[Presentation Layer]
    WEB["[Web UI]\nSPA / React"]:::ui
    MOB["[Mobile API]\nREST Adapter"]:::ui
  end

  subgraph BL[Business Layer]
    ORD["[Order Service]\nOrder management"]:::service
    PAY["[Payment Service]\nPayment processing"]:::service
    CAT["[Catalog Service]\nProduct catalog"]:::service
    USR["[User Service]\nAuthentication"]:::service
  end

  subgraph DL[Data Layer]
    OREP["[Order Repo]\nPostgreSQL"]:::data
    PREP["[Product Repo]\nPostgreSQL"]:::data
    UREP["[User Repo]\nPostgreSQL"]:::data
  end

  PGWS["[Payment Gateway]\nStripe API"]:::external
  MAIL["[Email Provider]\nSendGrid"]:::external

  WEB --> ORD & CAT
  MOB --> ORD & USR
  ORD --> PAY & OREP
  PAY --> PGWS
  CAT --> PREP
  USR --> UREP
  ORD --> MAIL
```
