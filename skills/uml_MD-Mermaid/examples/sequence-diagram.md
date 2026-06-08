# Sequence Diagram

Shows message interactions between objects in chronological order.

## Key Elements

- **Participant**: `participant Name as alias` — rectangle lifeline
- **Actor**: `actor Name as alias` — stick figure lifeline
- **Box group**: `box "Label" #color ... end box` — group participants
- **Activation**: `activate` / `deactivate` or `+` / `-` shorthand
- **Create**: `create participant Name` — creates new object
- **Destroy**: `destroy Name` — destroys object (X mark)
- **Note**: `Note right of A: text` or `Note over A,B: text`

## Message Types

| Message | Syntax | Description |
|---|---|---|
| Synchronous | `->>` | Solid line, filled arrowhead |
| Synchronous (no arrow) | `->` | Solid line, open arrowhead |
| Return / async reply | `-->>` | Dashed line, filled arrowhead |
| Return (no arrow) | `-->` | Dashed line, open arrowhead |
| Cross (lost) | `-x` | Solid line with X |
| Cross dashed | `--x` | Dashed line with X |
| Open async | `-)` | Solid line, open circle arrow |
| Open async dashed | `--)` | Dashed line, open circle arrow |

## Combined Fragments

| Fragment | Syntax | Description |
|---|---|---|
| alt/else | `alt ... else ... end` | Alternative (if-else) |
| opt | `opt ... end` | Optional (if) |
| loop | `loop ... end` | Loop iteration |
| par | `par ... and ... end` | Parallel execution |
| critical | `critical ... option ... end` | Critical section |
| break | `break ... end` | Break out |

## Recommended Colors (box backgrounds)

| Element | Color | Usage |
|---|---|---|
| Frontend | `#e8f4fd` (sky blue) | UI components |
| Backend | `#e8f6e8` (sage green) | Server/service |
| Data | `#fef9e7` (peach/yellow) | Data storage |
| External | `#f4ecf7` (lavender) | Third-party services |

## Example 1

E-commerce order processing with multiple participants and fragments:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial', 'activationBorderColor': '#6c8ebf', 'activationBkgColor': '#dae8fc'}}}%%
sequenceDiagram
  actor Customer

  box rgb(245, 245, 245) Frontend
    participant Web as Web Store
  end

  box rgb(245, 245, 245) Backend Services
    participant Order as Order Service
    participant Payment as Payment Service
    participant Inv as Inventory
  end

  participant DB as Orders DB
  participant Email as Email Service

  rect rgb(232, 244, 253)
    Note over Customer, Inv: Place Order
    Customer->>Web: Browse & Add to Cart
    activate Web
    Web->>Order: POST /orders
    activate Order
    Order->>Inv: Check stock
    activate Inv
    Inv-->>Order: Stock confirmed
    deactivate Inv
    Order->>DB: Create order (PENDING)
    activate DB
    DB-->>Order: Order #12345
    deactivate DB
    Order-->>Web: Order created
    deactivate Order
    Web-->>Customer: Show order summary
    deactivate Web
  end

  rect rgb(232, 244, 253)
    Note over Customer, Email: Process Payment
    Customer->>Web: Submit payment
    activate Web
    Web->>Payment: POST /payments
    activate Payment

    alt Payment Success
      Payment->>Order: Payment confirmed
      activate Order
      Order->>DB: Update order (PAID)
      Order->>Email: Send confirmation
      activate Email
      Email-->>Customer: Order confirmation email
      deactivate Email
      Order-->>Payment: ACK
      deactivate Order
      Payment-->>Web: Payment OK
    else Payment Failed
      Payment-->>Web: Payment declined
      Web-->>Customer: Show error, retry
    end

    deactivate Payment
    deactivate Web
  end
```

## Example 2

AWS serverless API flow with activation bars and loop:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
sequenceDiagram
  actor Client as Mobile App

  box rgb(227, 242, 253) AWS API Layer
    participant APIGW as API Gateway
  end

  box rgb(232, 245, 233) Compute
    participant Auth as Auth Lambda
    participant Proc as Process Lambda
  end

  box rgb(255, 253, 231) Data
    participant Dynamo as DynamoDB
    participant S3
    participant SQS
  end

  Client->>APIGW: POST /process
  activate APIGW

  APIGW->>Auth: Verify token
  activate Auth
  Auth-->>APIGW: 200 OK
  deactivate Auth

  APIGW->>Proc: Process request
  activate Proc
  Proc->>Dynamo: Save record
  Proc->>SQS: Enqueue task
  Proc->>S3: Upload file
  Proc-->>APIGW: 202 Accepted
  deactivate Proc

  APIGW-->>Client: 202 Accepted
  deactivate APIGW
```
