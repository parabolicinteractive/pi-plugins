---
description: Audit spec docs for coverage gaps and issues
allowed-tools: Read, Glob, Grep, Bash(ls:*,find:*,wc:*)
argument-hint: [docs-path]
---

Audit the project's specification documents for completeness, consistency, and cross-linking integrity.

## Procedure

1. **Locate the docs directory.** If $1 is provided, use it. Otherwise, find the `docs/` directory relative to the project root.

2. **Inventory all documents.** Scan the docs directory and categorize every file by document type:
   - `vision.md` — Product Vision & Charter
   - `domain-model.md` — Domain Model
   - `requirements.md` — Software Requirements Specification
   - `architecture.md` — Technical Architecture
   - `operations.md` — Operations Guide
   - `adrs/*.md` — Architecture Decision Records (exclude `_template.md`)
   - `rfcs/*.md` — Requests for Comments (exclude `_template.md`)
   - `designs/*.md` — Technical Design Specifications (exclude `_template.md`)
   - `specs/*.feature` — Executable Specifications
   - `api/*` — API Contracts
   - `INDEX.md` — Document Index
   - `README.md` — Doc system entry point

3. **Check coverage gaps.** Report which core documents are missing:
   - vision.md (essential)
   - domain-model.md (essential)
   - requirements.md (essential)
   - architecture.md (essential)
   - At least one ADR (expected for any non-trivial project)
   - operations.md (important for production systems)
   - INDEX.md (recommended)

4. **Check frontmatter health.** For each document found, verify:
   - Has YAML frontmatter with required fields (title, status, created, updated, authors).
   - Status is a valid value (draft, review, accepted, superseded, deprecated).
   - Updated date is not more than 6 months old (flag as potentially stale).
   - Superseded documents link to their successor.

5. **Check cross-references.** Scan for markdown links between documents:
   - Verify linked files exist (detect broken links).
   - Flag documents with zero outbound links (isolated — not connected to the doc graph).
   - Check that design specs reference requirement IDs.
   - Check that ADRs have an "Alternatives Considered" section.

6. **Check terminology consistency.** If a domain model exists:
   - Extract terms from the "Avoid" column.
   - Grep other documents for those avoided terms.
   - Report any terminology drift.

7. **Generate the audit report.** Present findings organized as:
   - **Coverage Summary**: Table showing each document type, whether it exists, its status, and last updated date.
   - **Missing Documents**: List of recommended documents not yet created.
   - **Stale Documents**: Documents not updated in 6+ months.
   - **Broken Links**: Cross-references that point to non-existent files.
   - **Isolated Documents**: Documents with no cross-references.
   - **Terminology Drift**: Avoided terms found in other documents.
   - **Recommendations**: Prioritized list of actions to improve doc health.

8. Suggest using `/pi-doc-create` to fill any gaps found.
