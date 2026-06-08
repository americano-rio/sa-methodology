# Package Diagram

Shows the modular organization structure of a system.

## Approach in Mermaid

Use **`classDiagram`** with `namespace` blocks to represent packages/modules.  
For higher-level architecture layers, use `flowchart TD` with nested subgraphs.

## classDiagram namespace Syntax

```
classDiagram
  namespace PackageName {
    class ComponentA
    class ComponentB
  }
  namespace AnotherPackage {
    class ComponentC
  }
  ComponentA ..> ComponentC : use
```

## flowchart Subgraph Syntax (for layered packages)

```
flowchart TD
  subgraph Layer["Layer Name"]
    subgraph Pkg["Package"]
      A["Module"]
    end
  end
```

## Dependency Stereotypes

| Stereotype | Arrow + Label | Description |
|---|---|---|
| `<<use>>` | `..>` | Usage dependency |
| `<<import>>` | `..>` | Package imports another |
| `<<access>>` | `..>` | Restricted access |
| `<<merge>>` | `..>` | Package merge |

## Recommended Colors (classDef / style)

| Layer | Fill | Stroke | Usage |
|---|---|---|---|
| Presentation | `#96CBFE` | `#6c8ebf` | UI layer |
| Application | `#A8D08D` | `#82b366` | Service/business layer |
| Domain | `#fff2cc` | `#d6b656` | Domain model |
| Infrastructure | `#e1d5e7` | `#9673a6` | Data/external access |
| Shared/Common | `#ffe6cc` | `#d79b00` | Cross-cutting concerns |

## Example 1

E-commerce system module structure with layered architecture (flowchart):

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial', 'lineColor': '#555555'}}}%%
flowchart TD
  classDef pres  fill:#96CBFE,stroke:#6c8ebf
  classDef app   fill:#A8D08D,stroke:#82b366
  classDef dom   fill:#fff2cc,stroke:#d6b656
  classDef infra fill:#e1d5e7,stroke:#9673a6
  classDef shared fill:#ffe6cc,stroke:#d79b00

  subgraph PL["Presentation Layer"]
    direction LR
    WEBUI["Web UI"]:::pres
    MOBILE["Mobile API"]:::pres
    ADMIN["Admin Console"]:::pres
  end

  subgraph AL["Application Layer"]
    direction LR
    ORDSVC["Order Service"]:::app
    CATSVC["Catalog Service"]:::app
    USRSVC["User Service"]:::app
    PAYSVC["Payment Service"]:::app
  end

  subgraph DL["Domain Layer"]
    direction LR
    ORDMOD["Order Model"]:::dom
    PRODMOD["Product Model"]:::dom
    USRMOD["User Model"]:::dom
  end

  subgraph IL["Infrastructure Layer"]
    direction LR
    PERSIST["Persistence"]:::infra
    MESSAGING["Messaging"]:::infra
    EXTAPI["External APIs"]:::infra
  end

  subgraph SL["Shared"]
    direction LR
    UTILS["Common Utils"]:::shared
    SEC["Security"]:::shared
  end

  WEBUI   -->|use| ORDSVC & CATSVC
  MOBILE  -->|use| ORDSVC
  ADMIN   -->|use| USRSVC

  ORDSVC  -->|import| ORDMOD
  CATSVC  -->|import| PRODMOD
  USRSVC  -->|import| USRMOD
  PAYSVC  -->|access| ORDMOD

  ORDMOD  -->|use| UTILS
  PERSIST -->|import| ORDMOD & PRODMOD
  MESSAGING -->|use| UTILS
  EXTAPI  -->|use| SEC
```

## Example 2

Clean Architecture using `classDiagram` with namespaces:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
classDiagram
  namespace Entities {
    class User {
      +id UUID
      +email String
    }
    class Invoice {
      +id UUID
      +amount Decimal
    }
  }

  namespace UseCases {
    class CreateUser {
      +execute(cmd) User
    }
    class GenerateInvoice {
      +execute(cmd) Invoice
    }
  }

  namespace Adapters {
    class UserController {
      +post(req) Response
    }
    class DBGateway {
      +save(entity) void
    }
  }

  namespace Frameworks {
    class ExpressApp {
      +listen(port) void
    }
    class PostgreSQLDriver {
      +query(sql) Result
    }
  }

  UserController ..> CreateUser       : use
  UserController ..> GenerateInvoice  : use
  CreateUser     ..> User             : import
  GenerateInvoice ..> Invoice         : import
  ExpressApp     ..> UserController   : use
  DBGateway      ..> User             : import
  PostgreSQLDriver ..> DBGateway      : use
```
