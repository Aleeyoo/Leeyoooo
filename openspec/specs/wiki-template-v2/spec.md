# WIKI Template v2

## ADDED Requirements

### Requirement: WIKI entry SHALL have AI retrieval index
The WIKI template frontmatter SHALL include a `concepts` field containing 3-8 core concept keywords that enable AI semantic retrieval across projects. Concepts MUST be distinct, self-contained terms directly discussed in the WIKI entry.

#### Scenario: AI generates concepts from article
- **WHEN** AI processes a raw article about the 18-month trust window in the AI era
- **THEN** the generated WIKI frontmatter includes `concepts: ["信任红利", "人格资产", "内容边际成本", "AI检索排名", "作者性"]`

#### Scenario: Concepts are limited in number
- **WHEN** AI fills the `concepts` field
- **THEN** the array contains between 3 and 8 entries

### Requirement: WIKI entry SHALL self-report confidence level
The WIKI template frontmatter SHALL include a `confidence` field with one of three values: `high` (factual claims, primary sources, established knowledge), `medium` (reasoned arguments, logical deductions), or `low` (speculation, predictions, unverified claims).

#### Scenario: Marking factual content as high confidence
- **WHEN** the raw source describes historically documented events (e.g., "Amazon launched in July 1995")
- **THEN** the corresponding WIKI entry sets `confidence: high`

#### Scenario: Marking predictions as medium or low confidence
- **WHEN** the raw source makes a time-bound prediction (e.g., "the window will close in 18 months")
- **THEN** the corresponding WIKI entry sets `confidence: medium` or `confidence: low`

### Requirement: WIKI body SHALL have a scan zone
The WIKI body SHALL include a scan zone at the top containing: a one-sentence core claim (callout `💡`), up to 3 key conclusions (`📌`), and actionable items if present (`🎬`). This zone enables readers to decide whether to deep-read within 5 seconds.

#### Scenario: Reader scans WIKI entry
- **WHEN** a reader opens a WIKI entry
- **THEN** the first section they see contains the one-sentence claim, key conclusions, and optional action items in separate callout blocks

### Requirement: WIKI body SHALL have a deep-read zone
The WIKI body SHALL include a deep-read zone after the scan zone containing: an argument chain (claim → evidence → case), key quotations from the source, and a limitations/blind-spots section that identifies what the source does NOT cover.

#### Scenario: AI identifies blind spots in source
- **WHEN** AI processes a raw article about personal branding in the AI era
- **THEN** the WIKI entry's "局限/盲区" section identifies uncovered aspects (e.g., "未讨论非文字创作者如何建立人格资产", "未涉及非中文市场的情况")

### Requirement: WIKI entry SHALL support multi-project attribution
A single WIKI entry's `Belongs to` field SHALL support linking to multiple projects via YAML array, without requiring content duplication across projects.

#### Scenario: Article spans two projects
- **WHEN** a raw article covers both personal growth and AI trends
- **THEN** the WIKI entry sets `Belongs to: ["[[个人成长]]", "[[AI趋势]]"]` and appears in both projects' `.base` views
