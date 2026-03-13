---
description: Quick-create an Architecture Decision Record
allowed-tools: Read, Write, Edit, Glob, Bash(ls:*,mkdir:*,find:*)
argument-hint: <decision-title>
---

Quick-create a new Architecture Decision Record (ADR). This is a shortcut for the most frequently created document type.

## Procedure

1. **Parse the title.** Use $ARGUMENTS as the decision title. If no arguments provided, ask the user what decision they're documenting.

2. **Find the ADR directory.** Look for `docs/adrs/` relative to the project root. If it doesn't exist, create it.

3. **Determine the sequence number.** Scan existing ADR files for the highest number. The new ADR gets the next number, zero-padded to three digits.

4. **Generate the filename.** Convert the title to kebab-case: lowercase, spaces to hyphens, remove special characters. Example: "Use PostgreSQL for primary data" → `007-use-postgresql-for-primary-data.md`.

5. **Read the ADR template** from `${CLAUDE_PLUGIN_ROOT}/skills/pi-doc-templates/references/adr-template.md`.

6. **Create the ADR file** with:
   - Frontmatter populated: title as "ADR-NNN: [Title]", status as `proposed`, today's date.
   - All template sections present with placeholder guidance.

7. **Also create `docs/adrs/_template.md`** if it doesn't already exist, containing the raw ADR template for future manual use.

8. **Assist with content.** After creating the file, ask the user:
   - "What's the context? What situation triggered this decision?"
   - "What did you decide?"
   - "What alternatives did you consider?"

   Then fill in the Context, Decision, and Alternatives Considered sections based on their answers. Leave Consequences for the user to fill in after they've had time to think about implications.

9. **Report the result:**
   - File path of the new ADR.
   - Remind the user to update the ADR status from `proposed` to `accepted` once reviewed.
   - Suggest running `/pi-doc-index` to update the master index.
   - If the ADR relates to architecture choices, suggest updating `docs/architecture.md` to reference it.
