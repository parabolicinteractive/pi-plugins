---
description: Freeze the document set as an approved baseline
argument-hint: "[label], for example 1.0"
allowed-tools: Read, Write, Edit, Glob, Grep, AskUserQuestion, Bash(ls:*,find:*,mkdir:*,git log:*,git diff:*,git tag:*,git status:*,git show:*)
---

Freeze the specification document set as a formally approved baseline.

A baseline is a formally approved version of a configuration item, formally
designated and fixed at a specific time (ISO/IEC/IEEE 29148 3.1.3). Change
management follows 29148 6.6.2.

This command is the operation the rest of the package depends on. Version
history, impact analysis, and scope-since-baseline reporting are all measured
from a baseline. Without one they degrade silently.

## Arguments

`$1` is the baseline label, for example `1.0`. If omitted, propose the next label
and confirm it:

- **Major** when scope changed: requirements added or withdrawn.
- **Minor** when existing requirements were clarified without scope change.
- `1.0` when no baseline exists yet.

## Procedure

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/pi-spec-docs/SKILL.md`.

2. Inventory `docs/`. Record every document, its type, version, current baseline,
   status, and declared conformance.

3. Determine the previous baseline. Read the highest existing record in
   `docs/baselines/`, or fall back to the most recent git tag. State explicitly
   if this is the first baseline.

4. Run the gate.

### Hard failures, which block

Do not baseline while any of these hold. Each means intent may have been lost,
and a baseline is a claim that it has not.

- Any `Coverage` finding is `Open` in `docs/analysis/`
- Any analysis report has a non-zero undispositioned statement count
- Any citation points to an identifier that does not exist
- Any source statement dispositioned as `requirement` resolves to nothing
- Any `STR-NNN` has no child requirement and no recorded reason
- Any document has uncommitted changes in git

### Warnings, which do not block

Report these, then ask whether to proceed. Baselining with declared unknowns is a
legitimate business decision; baselining with undeclared gaps is not.

- Open `TBD` register entries in any document
- Documents declaring `not-claimed` or `tailored` conformance
- Open findings of class `Conformance`, `Consistency`, `Currency`, `Terminology`
- Requirements with no downward allocation or no verification approach
- Documents that do not exist yet (for example no STP while the SRS is being frozen)

5. Present the gate result.

   If any hard failure holds, stop. List each with the file and identifier
   involved, and what would resolve it. Do not offer to override. The fix is to
   run `/pi-docs-realign` or `/pi-docs-analyze-sources` and disposition the
   findings.

   If only warnings hold, list them grouped by kind and ask whether to proceed.
   Anything the user accepts is recorded in the baseline record as an accepted
   warning, so the decision is preserved rather than forgotten.

6. Collect approval. Ask who approved the baseline and on what date. For client
   work, record the client-side approver by name and role. This is the record
   that a specification was agreed, not merely written.

7. Compute the measurement snapshot per 29148 6.6.3. See the Measurement section
   of `${CLAUDE_PLUGIN_ROOT}/commands/pi-docs-realign.md` for the definitions.

8. Write the baseline record to `docs/baselines/<label>.md`.

```markdown
---
title: "Baseline <label>"
type: baseline
label: "<label>"
status: approved
previous: "<previous label or null>"
created: YYYY-MM-DD
approved-by: [name]
approved-on: YYYY-MM-DD
git-tag: "baseline-<label>"
---

# Baseline <label>

## Documents Frozen

| Document | Type | Version | Conformance |
|---|---|---|---|

## Approval

| Name | Role | Organization | Date |
|---|---|---|---|

## Measurement Snapshot

<!-- 29148 6.6.3 -->

| Measure | Value | Previous baseline | Change |
|---|---|---|---|

## Accepted Warnings

Conditions knowingly accepted when this baseline was approved.

| Warning | Detail | Accepted by | Reason |
|---|---|---|---|

## Open TBDs at Baseline

| TBD | Document | Blocks | Owner | Needed by |
|---|---|---|---|---|

## Change Requests Since Previous Baseline

| CR | Description | Status | Requirements affected |
|---|---|---|---|
```

9. Stamp every frozen document's frontmatter: `status: baselined`,
   `baseline: <label>`, `updated` to today. Bump `version` if it is still a draft
   number.

10. Append a revision history row to each frozen document naming the baseline.

11. Regenerate the index with `/pi-docs index`, so `docs/README.md` reflects the
    new state.

12. Offer to tag git as `baseline-<label>`. **Ask before tagging.** A tag is a
    shared, visible repository mutation. If the user declines, note in the record
    that no tag was created, since realign falls back to the record when no tag
    exists.

13. Report:
    - Baseline label and record path
    - Documents frozen, with versions
    - Measurement snapshot, highlighting requirements volatility
    - Accepted warnings
    - Open TBDs carried into the baseline
    - Whether a git tag was created

## Notes

Baselines are never edited. A baseline record is what was true on a date. To
change what is frozen, create the next baseline.

Post-baseline, a finding that requires a requirement change becomes a change
request rather than a direct edit. That is what makes scope reporting possible:
every change request carrying `baseline: <label>` arrived after that baseline was
agreed.
