## ADDED Requirements

### Requirement: CLAUDE.md SHALL provide vault identity and directory map
The root `CLAUDE.md` SHALL include a brief description of the vault's purpose and a table mapping each top-level directory to its role.

#### Scenario: Agent enters vault for the first time
- **WHEN** a Claude agent session starts in this vault
- **THEN** CLAUDE.md is loaded and the agent understands it's a Chinese personal knowledge management system with AI-driven pipeline

### Requirement: CLAUDE.md SHALL document core conventions
The root `CLAUDE.md` SHALL document naming conventions, template locations, link format rules, and the raw file tagging convention.

#### Scenario: Agent needs to create a new WIKI entry
- **WHEN** agent reads CLAUDE.md
- **THEN** agent knows to use templates in `Library/temples/`, follow naming conventions, and use `[[filename]]` wiki-links

### Requirement: CLAUDE.md SHALL reference the knowledge pipeline document
The root `CLAUDE.md` SHALL include a pointer to `Workflow/knowledge-pipeline.md` as the detailed operational guide for processing raw content.

#### Scenario: Agent needs to process raw content
- **WHEN** agent detects new files in `Library/raw/`
- **THEN** CLAUDE.md directs agent to read `Workflow/knowledge-pipeline.md` for the complete workflow

### Requirement: CLAUDE.md SHALL be concise
The root `CLAUDE.md` SHALL be no more than 50 lines, focusing on immediate context and pointers rather than full documentation.

#### Scenario: Agent session loads CLAUDE.md
- **THEN** the file is under 50 lines, providing enough context without excessive token consumption
