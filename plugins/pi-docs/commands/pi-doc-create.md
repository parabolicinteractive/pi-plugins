---
description: Create a new spec document from template
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(ls:*,mkdir:*,find:*)
argument-hint: <type> [name]
---

Create a new specification document using the pi-docs template system.

## Argument Parsing

The first argument ($1) is the document type. The second argument ($2) is an optional name (used for ADRs, RFCs, design specs, and executable specs).

Valid document types:
- `vision` — Product Vision & Charter → `docs/vision.md`
- `domain-model` — Domain Model & Ubiquitous Language → `docs/domain-model.md`
- `requirements` — Software Requirements Specification → `docs/requirements.md`
- `architecture` — Technical Architecture → `docs/architecture.md`
- `adr` — Architecture Decision Record → `docs/adrs/NNN-title.md` (requires name)
- `rfc` — Request for Comments → `docs/rfcs/NNN-title.md` (requires name)
- `design` — Technical Design Specification → `docs/designs/name.md` (requires name)
- `spec` — Executable Specification (BDD) → `docs/specs/name.feature` (requires name)
- `api` — API Contract → `docs/api/name.yaml` (requires name)
- `operations` — Operations Guide → `docs/operations.md`

If no arguments are provided, list the available document types and ask the user which one to create.

## Procedure

1. Read the appropriate template from the `pi-doc-templates` skill at `${CLAUDE_PLUGIN_ROOT}/skills/pi-doc-templates/references/`.

2. Determine the destination path:
   - Find the project root (look for `docs/` directory, or common root markers like `package.json`, `.git`, `Cargo.toml`, `go.mod`, `pyproject.toml`).
   - If no `docs/` directory exists, create it and inform the user.
   - For numbered documents (ADRs, RFCs): scan existing files to determine the next sequence number.

3. Create the file from the template:
   - Replace placeholder values with today's date, the provided name, and the next sequence number (if applicable).
   - Set frontmatter status to `draft`.
   - Set frontmatter created/updated to today's date.

4. For ADRs and RFCs, also create the `_template.md` file in the directory if it doesn't already exist.

5. Create any missing parent directories (`docs/adrs/`, `docs/rfcs/`, `docs/designs/`, `docs/specs/`, `docs/api/`).

6. After creating the file, tell the user:
   - The file path.
   - A brief description of the sections they should fill in.
   - Which other documents this one should cross-reference (based on the linking rules in the pi-doc-system skill).

7. If a `docs/INDEX.md` exists, remind the user to run `/pi-doc-index` to update it.
