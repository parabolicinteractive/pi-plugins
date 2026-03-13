---
name: pi-doc-templates
description: >
  Contains specific templates with required sections, YAML frontmatter
  schemas, and structural conventions for 10 specification document types.
  ALWAYS use this skill when the user wants to create, write, or structure
  any of these: product vision, domain model, software requirements (SRS),
  technical architecture, ADRs, RFCs, technical design specs, executable
  specifications (BDD/Gherkin), API contracts, or operations guides. Also
  use when the user asks what sections a document should have, what
  template to follow, or how to structure any kind of spec document. This
  skill provides standardized templates — always load it before creating
  or advising on spec document structure. Trigger on any mention of
  writing, creating, or structuring a vision doc, requirements doc,
  architecture doc, design spec, ADR, RFC, operations guide, API docs,
  BDD specs, or domain model document.
version: 0.1.0
---

# Specification Document Templates

Templates and structural guidance for every document type in the spec system. Each template follows the convention of YAML frontmatter for metadata plus a defined section structure.

## How to Use These Templates

1. Identify the document type needed (see `pi-doc-system` skill for guidance on which type to create).
2. Read the appropriate template from `references/` — each document type has its own detailed template file.
3. Copy the template into the project's `docs/` directory at the correct path.
4. Fill in sections progressively. Not every section needs content on day one — mark empty sections as `[TBD]` and move on.
5. Set frontmatter status to `draft` initially.

## Template Index

| Document Type | Template File | Destination Path |
|--------------|---------------|-----------------|
| Product Vision & Charter | `references/vision-template.md` | `docs/vision.md` |
| Domain Model | `references/domain-model-template.md` | `docs/domain-model.md` |
| Software Requirements Spec | `references/requirements-template.md` | `docs/requirements.md` |
| Technical Architecture | `references/architecture-template.md` | `docs/architecture.md` |
| Architecture Decision Record | `references/adr-template.md` | `docs/adrs/NNN-title.md` |
| Request for Comments | `references/rfc-template.md` | `docs/rfcs/NNN-title.md` |
| Technical Design Spec | `references/design-spec-template.md` | `docs/designs/feature-name.md` |
| Executable Specification | `references/executable-spec-template.md` | `docs/specs/feature-name.feature` |
| API Contract | `references/api-contract-template.md` | `docs/api/service-name.yaml` |
| Operations Guide | `references/operations-template.md` | `docs/operations.md` |

## Writing Principles

These apply across all document types:

**For developers:** Be specific. Use code references, file paths, and concrete examples. Avoid hand-waving. If a section is speculative, label it.

**For investors:** Lead with impact. Every document should make it clear within the first two paragraphs why this matters to the business. Use the vision doc and requirements doc as investor entry points.

**For AI agents:** Be explicit about vocabulary (link to domain model), decisions (link to ADRs), and constraints (state them, don't imply them). Measurable acceptance criteria over vague descriptions. Include "Alternatives Considered" sections so agents don't suggest rejected approaches.

## Frontmatter Standard

Every document uses this frontmatter schema:

```yaml
---
title: string (required)
status: draft | review | accepted | superseded | deprecated (required)
created: YYYY-MM-DD (required)
updated: YYYY-MM-DD (required)
authors: [string] (required)
tags: [string] (optional)
superseded-by: relative path (only if status is superseded)
links: (optional)
  - type: satisfies | decided-by | implements | tested-by | supersedes
    target: relative path with optional anchor
---
```

## Section Conventions

- Use `##` for top-level sections, `###` for subsections.
- Mark incomplete sections with `<!-- TODO: description of what's needed -->`.
- Use `> **Note:**` callouts for important context that doesn't fit the main flow.
- Use `> **Decision:**` callouts to highlight key decisions (and link to the ADR).
- Code examples use fenced code blocks with language identifiers.
- Diagrams use Mermaid fenced blocks where possible for version-control friendliness.

Read the specific template files in `references/` for the detailed section structure of each document type.
