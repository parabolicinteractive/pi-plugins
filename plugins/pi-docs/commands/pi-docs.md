---
description: Create or revise a specification document
argument-hint: <srs|sad|sdd|stp|adr> [name]
allowed-tools: Read, Write, Edit, Glob, Grep, Task, Bash(ls:*,mkdir:*,find:*,git log:*,git diff:*,git tag:*)
---

Create or revise a specification document conforming to the standards family
declared in the `pi-spec-docs` skill.

## Arguments

`$1` is the document type. `$2` is an optional name, used only for ADRs.

| Type | Standard | Path | Status |
|---|---|---|---|
| `srs` | ISO/IEC/IEEE 29148:2018 | `docs/srs.md` | Available |
| `sad` | ISO/IEC/IEEE 42010:2022 | `docs/sad.md` | Available, conformance not claimed |
| `adr` | ISO/IEC/IEEE 42010:2022 | `docs/adr/NNNN-<slug>.md` | Available, conformance not claimed |
| `stp` | ISO/IEC/IEEE 29119-3:2021 | `docs/stp.md` | Available |
| `sdd` | IEEE 1016-2009 | `docs/sdd.md` | Not yet implemented |

`stp` produces the project test plan (29119-3 clause 7.2). 29119-3 defines a
wider family of test documents, including test case and test procedure
specifications, incident reports, and status and completion reports. Those are
execution-time artifacts and this package does not generate them. If a user asks
for one, say so and name the clause so they know it exists.

If `$1` is empty, list the types and ask which one.

If `$1` names a type marked "Not yet implemented", say so plainly and stop. Do
not improvise a template from general knowledge. Offer to research the standard
first.

## Procedure

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/pi-spec-docs/SKILL.md` for the shared
   conventions: identifier scheme, frontmatter, traceability, output rules.

2. Read the reference file for the requested type from
   `${CLAUDE_PLUGIN_ROOT}/skills/pi-spec-docs/references/`.

3. Locate the project root. Look for `.git` first, then `package.json`,
   `pyproject.toml`, `go.mod`, `Cargo.toml`. Create `docs/` and any needed
   subdirectory only if the document requires it.

4. Determine whether this is a create or a revise.

### Create

5a. Check for prior analysis. Glob `docs/analysis/*.md`. If a `validation` report
    exists, read it and build the document from its source statement register.
    Every `S-NNN` dispositioned as `requirement` must resolve to a requirement in
    the new document.

    If no analysis report exists and the user has source material, stop and tell
    them to run `/pi-docs-analyze-sources` first. Building an SRS directly from
    raw sources skips the coverage register, which is the only mechanism that
    prevents silent loss of intent.

    If there is no source material at all, interview the user. Ask one question
    at a time. Record their answers as source statements in a new analysis report
    before writing the document, so coverage is checkable either way.

6a. Scan for the highest existing identifier of each prefix across all of `docs/`
    before assigning any new one.

7a. Write the document. Set `status: draft`, `version: 0.1`, `baseline: null`.
    Every section from the reference template appears. Unknown content becomes a
    `TBD` with a register entry, never an omission.

### Revise

**ADRs are the exception.** If the type is `adr` and the target's status is
`accepted`, `rejected`, or `superseded`, do not revise it. Create a new ADR that
supersedes it, set the old one's `status` and `superseded-by` fields, and change
nothing else in the old file. Editing an accepted decision record destroys the
only thing it exists to preserve. A `proposed` ADR may be edited freely. After
superseding, update the SAD decision register in section 6.1.

5b. Read the existing document. Note its current version and baseline.

6b. Identify what is changing and why. If the change originates from a change
    request, read it and record the `CR-NNN`.

7b. Apply the change. Never renumber, never reuse a withdrawn identifier. A
    dropped requirement becomes `Withdrawn` and moves to the withdrawn list; it
    is not deleted.

8b. Bump the version and append a revision history row naming the requirements
    affected and the `CR-NNN` if any. The requirements-affected column is what
    makes impact analysis possible without git.

### Both

9. Run the three validators described in the Validation Protocol section of
   `SKILL.md`, as parallel subagents. Merge their findings into the current
   analysis report as new `F-NNN` entries.

10. Report to the user:
    - Path written
    - Counts: requirements by type, TBDs, open findings
    - Conformance: full, tailored, or non-conformant, with the basis
    - Any coverage finding still open, stated prominently

Never report the document as complete while coverage findings are open. A
coverage finding means something from the sources is unaccounted for.
