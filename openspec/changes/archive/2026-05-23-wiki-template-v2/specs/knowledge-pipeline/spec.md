## MODIFIED Requirements

### Requirement: Agent SHALL generate WIKI entries following the v2 template structure
When generating a WIKI entry from raw content, the agent SHALL follow the v2 template at `Library/temples/WIKI.md`, which includes the following frontmatter fields and body structure:

**Frontmatter fields:**
- `Belongs to`: Project attribution (YAML array for multi-project)
- `aliases`: Search aliases for different keyword lookups
- `tags`: Sub-topic tags within the project namespace
- `created`: Date in YYYY-MM-DD format
- `source`: Set to `ai-generated`
- `source_url`: Source URL from raw content
- `concepts`: 3-8 core concept keywords for AI semantic retrieval
- `confidence`: One of `high`, `medium`, or `low`

**Body structure (scan zone → deep-read zone → related):**
- Scan zone: `💡` one-sentence core claim, `📌` up to 3 key conclusions, `🎬` actionable items (if any)
- Deep-read zone: argument chain (claim → evidence → case), key quotations, limitations/blind-spots
- Related zone: wiki-links to related entries

#### Scenario: Generating a WIKI entry from raw article
- **WHEN** AI processes a new raw article in `Library/raw/`
- **THEN** the generated WIKI file in `WIKI/` includes all frontmatter fields from the v2 template and the three-zone body structure

#### Scenario: AI reports confidence correctly
- **WHEN** the raw source contains factual claims about historical events
- **THEN** the WIKI entry frontmatter sets `confidence: high`
- **WHEN** the raw source contains reasoned arguments and opinions
- **THEN** the WIKI entry frontmatter sets `confidence: medium`
- **WHEN** the raw source contains unverified predictions
- **THEN** the WIKI entry frontmatter sets `confidence: low`

#### Scenario: AI identifies limitations and blind spots
- **WHEN** AI generates the deep-read zone
- **THEN** the "局限/盲区" section identifies at least one aspect the raw source does not cover or discuss
