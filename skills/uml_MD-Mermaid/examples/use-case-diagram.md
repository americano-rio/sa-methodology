# Use Case Diagram

Describes system functional requirements and user interactions.

## Approach in Mermaid

Mermaid does not have a native use case diagram type.  
Use **`flowchart LR`** to approximate use case diagrams with:
- Actor shapes: `Actor(["👤 Actor Name"])` — stadium shape
- Use case shapes: `UC("Use Case")` — rounded rect
- System boundary: `subgraph System[System Name]`
- Associations: `-->` (solid line)
- Include/Extend: `-->|<<include>>|` / `-->|<<extend>>|`
- Generalization: `-->` with label

## Recommended Colors (classDef)

| Element | Fill | Stroke | Usage |
|---|---|---|---|
| Primary actor | `#ffffff` | `#333333` | Main users |
| System actor | `#e1d5e7` | `#9673a6` | External systems |
| Core use case | `#dae8fc` | `#6c8ebf` | Primary functions |
| Secondary use case | `#d5e8d4` | `#82b366` | Supporting functions |
| Admin use case | `#fff2cc` | `#d6b656` | Management functions |

## Example 1

E-commerce system use cases with multiple actor types and relationships:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial', 'lineColor': '#333333'}}}%%
flowchart LR
  classDef actor    fill:#ffffff,stroke:#333333
  classDef sysActor fill:#e1d5e7,stroke:#9673a6
  classDef core     fill:#dae8fc,stroke:#6c8ebf
  classDef support  fill:#d5e8d4,stroke:#82b366
  classDef admin    fill:#fff2cc,stroke:#d6b656

  customer(["👤 Customer"]):::actor
  regcust(["👤 Registered\nCustomer"]):::actor
  adm(["👤 Admin"]):::actor
  pg(["⚙️ Payment\nGateway"]):::sysActor

  regcust -->|extends| customer

  subgraph System["E-Commerce System"]
    direction TB
    UC1("Browse Catalog"):::core
    UC2("Search Products"):::core
    UC3("View Product"):::core
    UC4("Add to Cart"):::support
    UC5("Checkout"):::support
    UC6("Process Payment"):::support
    UC7("Track Order"):::support
    UC8("Write Review"):::support
    UC9("Manage Products"):::admin
    UC10("Manage Orders"):::admin
    UC11("View Reports"):::admin
    UC12("Apply Coupon"):::support
  end

  customer  --> UC1 & UC2 & UC3
  regcust   --> UC4 & UC5 & UC7 & UC8
  adm       --> UC9 & UC10 & UC11

  UC5 -->|"<<include>>"| UC6
  UC5 -->|"<<extend>>"| UC12

  UC6 --> pg
```

## Example 2

Banking system with actor generalization and include/extend relationships:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
flowchart LR
  classDef actor   fill:#ffffff,stroke:#333333
  classDef sysact  fill:#e1d5e7,stroke:#9673a6
  classDef core    fill:#dae8fc,stroke:#6c8ebf
  classDef support fill:#d5e8d4,stroke:#82b366
  classDef admin   fill:#fff2cc,stroke:#d6b656

  user(["👤 User"]):::actor
  customer(["👤 Bank\nCustomer"]):::actor
  teller(["👤 Teller"]):::actor
  auditor(["👤 Auditor"]):::actor
  auth_svc(["⚙️ Auth Service"]):::sysact

  customer -->|extends| user
  teller   -->|extends| user

  subgraph Bank["Banking System"]
    direction TB
    LOGIN("Login"):::core
    VIEW_BAL("View Balance"):::core
    TRANSFER("Transfer Funds"):::core
    PAY_BILL("Pay Bill"):::support
    OPEN_ACC("Open Account"):::support
    CLOSE_ACC("Close Account"):::admin
    GEN_REPORT("Generate Report"):::admin
    VERIFY_2FA("2FA Verification"):::support
    AUDIT_LOG("Audit Log"):::admin
  end

  user      --> LOGIN
  customer  --> VIEW_BAL & TRANSFER & PAY_BILL
  teller    --> OPEN_ACC & CLOSE_ACC
  auditor   --> GEN_REPORT & AUDIT_LOG

  LOGIN     -->|"<<include>>"| VERIFY_2FA
  TRANSFER  -->|"<<include>>"| VERIFY_2FA
  VERIFY_2FA --> auth_svc
```
