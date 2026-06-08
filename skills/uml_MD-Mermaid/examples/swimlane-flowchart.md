# Swimlane Flowchart

Shows workflow partitioned by roles or services using subgraph lanes.

## Key Elements

- **Lane**: `subgraph LaneName[Lane Label]` — vertical or horizontal partition
- **Lane direction**: Set `flowchart LR` for vertical swimlanes, `flowchart TD` for horizontal
- All flowchart node shapes and edge styles apply inside lanes
- Arrows can cross lane boundaries freely

## Swimlane Syntax

```
flowchart LR
  subgraph Lane1[Role / Service]
    direction TB
    A[Step] --> B[Step]
  end
  subgraph Lane2[Another Role]
    direction TB
    C[Step]
  end
  B --> C
```

## Recommended Colors (classDef)

| Element | Fill | Stroke | Usage |
|---|---|---|---|
| Start/receive | `#d5e8d4` | `#82b366` | Input/receive actions |
| Process | `#dae8fc` | `#6c8ebf` | Processing steps |
| Decision | `#fff2cc` | `#d6b656` | Branch points |
| Error/Cancel | `#f8cecc` | `#b85450` | Error handling |
| Output | `#e1d5e7` | `#9673a6` | Results/output |

## Example 1

Employee onboarding across HR, IT, Manager, and New Employee lanes:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf', 'lineColor': '#333333', 'fontFamily': 'Arial'}}}%%
flowchart LR
  classDef green  fill:#d5e8d4,stroke:#82b366
  classDef blue   fill:#dae8fc,stroke:#6c8ebf
  classDef yellow fill:#fff2cc,stroke:#d6b656
  classDef red    fill:#f8cecc,stroke:#b85450
  classDef purple fill:#e1d5e7,stroke:#9673a6

  subgraph HR[HR]
    direction TB
    H1([Receive Signed Offer]):::green
    H2[Create Employee Record]:::blue
    H3[Send Welcome Email]:::blue
    H4[Prepare Onboarding Kit]:::blue
    H5[Schedule Orientation]:::blue
    H6[Verify Documents]:::blue
    H7{Documents Valid?}:::yellow
    H8[Finalize Enrollment]:::blue
    H9[Request Corrections]:::red
    H10[Update Status to Permanent]:::purple
  end

  subgraph IT[IT]
    direction TB
    I1[Provision Laptop]:::blue
    I2[Create Email Account]:::blue
    I3[Grant System Access]:::blue
  end

  subgraph EMP[New Employee]
    direction TB
    E1[Complete Tax & Benefits Forms]:::green
    E2[Upload ID Documents]:::blue
    E3[Resubmit Documents]:::red
    E4[Attend Orientation]:::blue
    E5[Complete Training Modules]:::blue
  end

  subgraph MGR[Manager]
    direction TB
    M1[Assign Mentor]:::blue
    M2[Set 30/60/90 Day Goals]:::blue
    M3[Schedule 1-on-1 Meetings]:::blue
    M4{Probation Review OK?}:::yellow
    M5[Confirm Employment]:::green
    M6[Extend Probation]:::red
  end

  H1 --> H2 --> H3
  H3 --> I1
  H3 --> H4
  I1 --> I2 --> I3
  H4 --> H5
  H5 --> E1 --> E2 --> H6
  H6 --> H7
  H7 -- Yes --> H8
  H7 -- No  --> H9 --> E3 --> H6
  H8 --> M1 --> M2 --> M3
  M3 --> E4 --> E5
  E5 --> M4
  M4 -- Yes --> M5 --> H10
  M4 -- No  --> M6
```
