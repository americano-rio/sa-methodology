---
name: uml_MD-Mermaid
description: Create UML and architecture diagrams using Mermaid syntax embedded in Markdown. Best for software modeling — Class, Sequence, Flowchart, State Machine, ER, Block, and more with concise text-based notation that renders in GitHub, GitLab, Notion, VS Code, and most Markdown viewers.
metadata:
  author: UML diagrams powered by Mermaid — the leading text-to-diagram tool for Markdown environments.
---

# UML Diagram Generator (Mermaid)
**Quick Start:** Choose diagram type → Write Mermaid text → Define elements and relationships → Wrap in ` ```mermaid ` fence.
> ⚠️ **IMPORTANT:** Always use ` ```mermaid ` code fence. NEVER use ` ```text ` — it will NOT render as a diagram.

## Critical Rules

- Every diagram starts with its **diagram type keyword** (e.g., `classDiagram`, `sequenceDiagram`, `flowchart TD`)
- No `@startuml` / `@enduml` — Mermaid uses the keyword alone
- Use `%%{init: {...}}%%` directive at the top for theming (optional)
- Comments use `%%` prefix: `%% this is a comment`
- Direction keywords: `TD` (top-down), `LR` (left-right), `BT` (bottom-top), `RL` (right-left)

## UML Diagram Types

| Type | Mermaid Keyword | Purpose | Example |
|------|----------------|---------|---------|
| Class | `classDiagram` | Class structure and relationships | [class-diagram.md](examples/class-diagram.md) |
| Sequence | `sequenceDiagram` | Message interactions over time | [sequence-diagram.md](examples/sequence-diagram.md) |
| Flowchart | `flowchart TD` | Workflow and process flow | [flowchart-diagram.md](examples/flowchart-diagram.md) |
| Swimlane Flowchart | `flowchart TD` + subgraph | Multi-role flowchart with lanes | [swimlane-flowchart.md](examples/swimlane-flowchart.md) |
| State Machine | `stateDiagram-v2` | Object lifecycle states | [state-machine-diagram.md](examples/state-machine-diagram.md) |
| Component | `block-beta` | System component organization | [component-diagram.md](examples/component-diagram.md) |
| Use Case | `flowchart LR` | User-system interactions | [use-case-diagram.md](examples/use-case-diagram.md) |
| Deployment | `block-beta` / C4 | Physical deployment architecture | [deployment-diagram.md](examples/deployment-diagram.md) |
| Object | `classDiagram` | Runtime object snapshot | [object-diagram.md](examples/object-diagram.md) |
| Package | `classDiagram` + namespace | Module organization | [package-diagram.md](examples/package-diagram.md) |
| ER Diagram | `erDiagram` | Database entity relationships | [er-diagram.md](examples/er-diagram.md) |

## SA 文件活動圖規範

### 活動圖（Activity Diagram）

- **一律使用 `flowchart TD`（直向，由上而下）**，禁止使用 `flowchart LR`
- 不使用泳道（subgraph swimlane），改以 classDef 色彩區分角色
- 角色色彩規範：

| 角色 | fill | stroke | classDef |
|------|------|--------|----------|
| 申請人 / 使用者 | `#d5e8d4` | `#82b366` | `green` |
| 系統 | `#dae8fc` | `#6c8ebf` | `blue` |
| 管理員 | `#e1d5e7` | `#9673a6` | `purple` |
| 判斷節點 | `#fff2cc` | `#d6b656` | `yellow` |
| 錯誤 / 失敗 | `#f8cecc` | `#b85450` | `red` |

- 圖前加顏色圖例說明，例如：
  `> 顏色圖例：🟢 申請人　🔵 系統　🟣 管理員　🟡 判斷　🔴 錯誤／失敗`

---

## Theming

Use `%%{init: ...}%%` directive to set themes or config:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#dae8fc', 'primaryBorderColor': '#6c8ebf', 'lineColor': '#333333'}}}%%
classDiagram
  class Example
```

Available themes: `default`, `base`, `dark`, `forest`, `neutral`

For custom colors on individual nodes, use `style` or `classDef` (flowchart) or inline `:::className`:

```mermaid
flowchart TD
  classDef green fill:#d5e8d4,stroke:#82b366
  classDef blue  fill:#dae8fc,stroke:#6c8ebf
  A[Start]:::green --> B[Process]:::blue
```

## Node Shapes (Flowchart)

| Shape | Syntax | Description |
|-------|--------|-------------|
| Rectangle | `[text]` | Default process step |
| Rounded | `(text)` | Soft process |
| Stadium/Pill | `([text])` | Start/End terminal |
| Subroutine | `[[text]]` | Pre-defined process |
| Cylinder | `[(text)]` | Database |
| Circle | `((text))` | Event/connector |
| Diamond | `{text}` | Decision |
| Hexagon | `{{text}}` | Preparation |
| Parallelogram | `[/text/]` | Input/Output |

## Relationship Syntax (Class Diagram)

| Relationship | Syntax | Description |
|---|---|---|
| Inheritance | `<\|--` | Hollow triangle (extends) |
| Realization | `..\|>` | Dashed + hollow triangle (implements) |
| Composition | `*--` | Filled diamond (owns) |
| Aggregation | `o--` | Hollow diamond (has-a) |
| Association | `-->` | Open arrow |
| Dependency | `..>` | Dashed open arrow |
| Link (solid) | `--` | Undirected |
| Link (dashed) | `..` | Undirected dashed |
