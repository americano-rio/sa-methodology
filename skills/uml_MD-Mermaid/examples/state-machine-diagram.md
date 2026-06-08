# State Machine Diagram

Shows state changes of an object during its lifecycle.

## Key Elements

- **State**: `StateName` or `state "Long Name" as alias` — rounded rectangle
- **Initial state**: `[*] -->` — solid black circle
- **Final state**: `--> [*]` — circle with outer ring
- **Transition**: `State1 --> State2 : event` — labeled arrow
- **Composite state**: `state StateName { ... }` — container with substates
- **Choice / Fork / Join**: `state id <<choice>>` / `<<fork>>` / `<<join>>`
- **Concurrency**: `--` inside composite state separates parallel regions
- **Notes**: `note right of StateName : text`
- **Direction**: `direction LR` inside composite state

## Recommended Colors

Mermaid `stateDiagram-v2` doesn't support per-state colors via inline syntax — use `%%{init}%%` theme variables or `classDef` with `:::` notation (experimental).

For color-coding, add comments to document intent:

| State Type | Theme suggestion | Usage |
|---|---|---|
| Pending/Idle | Blue tone | Waiting states |
| Active/Processing | Yellow tone | In-progress states |
| Success/Complete | Green tone | Successful outcomes |
| Error/Cancel | Red tone | Error/failure states |
| Final/Archive | Purple tone | Terminal states |

## Example 1

Order processing state machine with composite states and parallel regions:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
stateDiagram-v2
  [*] --> Pending

  Pending : Order received\nAwaiting review

  Pending --> Validating : Submit

  state Validating {
    direction LR
    state "Inventory & Payment" as inv_pay {
      [*] --> CheckingInventory
      CheckingInventory --> CheckingPayment : Items available
      CheckingPayment --> [*] : Payment verified
    }
    --
    state "Fraud Check" as fraud {
      [*] --> FraudCheck
      FraudCheck --> [*] : Passed
    }
  }

  Validating --> Processing : Validation OK
  Validating --> Cancelled : Validation failed

  state Processing {
    [*] --> Picking
    Picking --> Packing : Items picked
    Packing --> ReadyToShip : Packed
    ReadyToShip --> [*]
  }

  Processing --> Shipped : Dispatched

  Shipped : In transit\nTracking active

  Shipped --> Delivered : Received
  Shipped --> ReturnRequested : Customer return

  Delivered --> [*]

  Cancelled : Order cancelled\nRefund initiated
  Cancelled --> [*]

  ReturnRequested --> Refunded : Return approved
  Refunded --> [*]

  note right of Validating
    Parallel validation:
    inventory check and
    fraud detection run
    simultaneously
  end note
```

## Example 2

Traffic light state machine with choice and timed transitions:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'fontFamily': 'Arial'}}}%%
stateDiagram-v2
  [*] --> Red

  Red : 🔴 Red\nStop — 60s
  Yellow_Stop : 🟡 Yellow\nPrepare to go — 5s
  Green : 🟢 Green\nGo — 45s
  Yellow_Go : 🟡 Yellow\nPrepare to stop — 5s

  Red --> Yellow_Stop : timer expires
  Yellow_Stop --> Green : timer expires
  Green --> Yellow_Go : timer expires
  Yellow_Go --> Red : timer expires

  state EmergencyOverride {
    [*] --> FlashRed
    FlashRed --> FlashRed : 0.5s pulse
  }

  Red --> EmergencyOverride : emergency signal
  EmergencyOverride --> Red : emergency cleared
```
