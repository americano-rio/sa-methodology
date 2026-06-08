# SA 文件工程方法論

以 AI 輔助撰寫系統分析（SA）文件的標準化方法，包含線框圖與 UML 圖表兩大技能包。

支援 Claude（`CLAUDE.md`）與 Gemini（`GEMINI.md`）。

## 目錄結構

```
sa-methodology/
├── CLAUDE.md            ← Claude Code 指令設定（放到專案根目錄）
├── GEMINI.md            ← Gemini 指令設定（放到專案根目錄）
└── skills/
    ├── wireframing/     ← 線框圖技能包
    │   ├── SKILL.md
    │   ├── TEMPLATE.md
    │   └── examples.yaml
    └── uml_MD-Mermaid/  ← UML / Mermaid 圖表技能包
        ├── SKILL.md
        └── examples/    ← 11 種圖表語法範例
```

## 使用方式

1. 複製 `CLAUDE.md`（或 `GEMINI.md`）到你的專案根目錄。
2. 複製 `skills/` 資料夾到你的專案 `.claude/skills/`（Claude）或對應的 AI 設定目錄。
3. 在對話中說「寫 SA」、「畫線框圖」、「畫 UML」即可觸發對應技能。

## 技能包說明

### wireframing — 線框圖

建立低至中保真介面線框圖，用 Markdown ASCII 呈現頁面骨架、資訊層級與互動標註。

觸發詞：`寫 SA`、`畫線框圖`、`wireframe`、`建立 SA`、`SA 文件`

### uml_MD-Mermaid — UML 圖表

使用 Mermaid 語法產生 UML 圖表，支援 GitHub、GitLab、Obsidian、VS Code 等環境直接渲染。

支援圖表類型：類別圖、循序圖、流程圖、泳道圖、狀態機、ER 圖、元件圖、部署圖、物件圖、套件圖、使用案例圖。

觸發詞：`畫 UML`、`循序圖`、`流程圖`、`狀態圖`、`ER 圖`
