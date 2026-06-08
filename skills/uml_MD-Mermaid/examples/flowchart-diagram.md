# Flowchart Diagram (Activity)

Shows workflow with activities, decisions, loops, and parallel processing.

## Key Elements

- **Start/End**: `([text])` — stadium/pill shape
- **Activity**: `[text]` — rectangle
- **Decision**: `{text}` — diamond
- **Database/Cylinder**: `[(text)]` — cylinder
- **Parallel / Fork**: Use `subgraph` blocks side-by-side
- **Direction**: `TD` (top-down), `LR` (left-right), `BT` (bottom-top)

## Node Shapes

| Shape | Syntax | Usage |
|---|---|---|
| Rectangle | `[text]` | Process step |
| Rounded rect | `(text)` | Soft action |
| Stadium/Pill | `([text])` | Start / End |
| Diamond | `{text}` | Decision |
| Cylinder | `[(text)]` | Database |
| Circle | `((text))` | Connector / Event |
| Hexagon | `{{text}}` | Preparation |
| Parallelogram | `[/text/]` | Input / Output |
| Subroutine | `[[text]]` | Pre-defined process |

## Edge Styles

| Type | Syntax | Description |
|---|---|---|
| Solid arrow | `-->` | Default flow |
| Labeled arrow | `-- label -->` or `-->|label|` | Arrow with guard label |
| Thick arrow | `==>` | Emphasized flow |
| Dotted arrow | `-.->` | Optional/async flow |
| No arrowhead | `---` | Link without direction |

## Recommended Colors (classDef)

| Element | Fill | Stroke | Usage |
|---|---|---|---|
| Start/receive | `#d5e8d4` | `#82b366` | Input/receive actions |
| Process | `#dae8fc` | `#6c8ebf` | Processing steps |
| Decision | `#fff2cc` | `#d6b656` | Branch points |
| Error/Cancel | `#f8cecc` | `#b85450` | Error handling |
| Output | `#e1d5e7` | `#9673a6` | Results/output |

## Example 1

CI/CD pipeline with decisions, parallel stages, and loops:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf', 'lineColor': '#333333', 'fontFamily': 'Arial'}}}%%
flowchart TD
  classDef green  fill:#d5e8d4,stroke:#82b366
  classDef blue   fill:#dae8fc,stroke:#6c8ebf
  classDef yellow fill:#fff2cc,stroke:#d6b656
  classDef red    fill:#f8cecc,stroke:#b85450
  classDef purple fill:#e1d5e7,stroke:#9673a6

  START([Developer pushes code]):::green --> LINT[Run Linter & Static Analysis]:::blue

  LINT --> LINT_OK{Lint OK?}:::yellow
  LINT_OK -- Yes --> COMPILE[Compile Project]:::blue
  LINT_OK -- No  --> LINT_FAIL[Notify: Fix lint errors]:::red
  LINT_FAIL --> STOP1([Stop])

  COMPILE --> TEST[Run Unit Tests]:::blue
  TEST --> TEST_OK{Tests Pass?}:::yellow
  TEST_OK -- Yes --> DOCKER[Build Docker Image]:::blue
  TEST_OK -- No  --> TEST_FAIL[Notify: Tests failed]:::red
  TEST_FAIL --> STOP2([Stop])

  DOCKER --> PARALLEL_START{ }

  subgraph parallel [Parallel Checks]
    direction LR
    SEC[Security Scan]:::blue
    INT[Integration Tests]:::blue
    PERF[Performance Tests]:::blue
  end

  PARALLEL_START --> SEC
  PARALLEL_START --> INT
  PARALLEL_START --> PERF

  SEC  --> PARALLEL_END{ }
  INT  --> PARALLEL_END
  PERF --> PARALLEL_END

  PARALLEL_END --> ALL_OK{All checks pass?}:::yellow
  ALL_OK -- No  --> BLOCK[Block deploy pipeline]:::red
  ALL_OK -- Yes --> STAGING[Deploy to Staging]:::blue

  STAGING --> SMOKE[Run Smoke Tests]:::blue
  SMOKE --> SMOKE_OK{Smoke OK?}:::yellow
  SMOKE_OK -- Yes --> APPROVE[Await Manual Approval]:::purple
  SMOKE_OK -- No  --> RETRY{Retries < 3?}:::yellow
  RETRY -- Yes --> REDEPLOY[Retry deploy]:::blue
  REDEPLOY --> SMOKE
  RETRY -- No  --> ROLLBACK[Rollback Staging]:::red

  APPROVE --> APPROVED{Approved?}:::yellow
  APPROVED -- Yes --> PROD[Deploy to Production]:::green
  APPROVED -- No  --> ROLLBACK
  PROD --> NOTIFY[Send Release Notification]:::purple
  NOTIFY --> END([End])
```
