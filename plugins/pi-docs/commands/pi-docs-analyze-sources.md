---
description: Analyze source material into a statement register and findings
argument-hint: [path or description of sources]
allowed-tools: Read, Write, Edit, Glob, Grep, Task, Bash(ls:*,mkdir:*,find:*,wc:*)
---

Analyze source material into a source statement register and a findings list,
producing a `validation` analysis report.

This is requirements validation in the sense of ISO/IEC/IEEE 29148 3.1.25:
confirming that requirements define the right system as intended by stakeholders.

## Purpose

This command exists to make source coverage arithmetic instead of judgment. Every
atomic statement in the source material receives an identifier and exactly one
disposition. Nothing is glossed over, because a gap in the register is countable
and grep-able.

Run this before writing an SRS. Run it again whenever new source material
arrives.

## Arguments

`$1` is a path, a glob, or a description of where the sources are. If empty, ask
what to analyze and offer to scan `docs/sources/`.

Sources may be documents (RFPs, proposals, decks, emails), transcripts (meetings,
calls, interviews), or an existing codebase to reverse-engineer requirements from.

## Procedure

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/pi-spec-docs/SKILL.md` and
   `${CLAUDE_PLUGIN_ROOT}/skills/pi-spec-docs/references/analysis.md`.

2. Inventory the sources. List every file, its type, its date, and where it came
   from. Confirm the list with the user before reading. If something is
   unreadable, say so rather than skipping it silently.

3. Determine the report number and the starting `S-NNN` and `F-NNN`. Scan all of
   `docs/analysis/` for the highest existing values of each. Never restart a
   sequence.

4. Determine whether a baseline exists. Read `docs/srs.md` frontmatter if present.
   Pre-baseline, unresolved statements become findings. Post-baseline, statements
   that expand scope become change request candidates.

5. Extract atomic statements. Read every source in full. Do not skim, do not
   sample, do not summarize before extracting. Split compound sentences: a
   sentence carrying three requests produces three statements. Cite each
   statement's origin precisely enough to find again.

6. Disposition every statement as `requirement`, `finding`, `out-of-scope`, or
   `context`. The undispositioned count must be zero. A statement that resists
   disposition becomes a finding.

7. Write findings for every statement that cannot become a requirement as
   written. Name the 29148 characteristic it violates, with clause. Ambiguity,
   contradiction between sources, unverifiable phrasing, and missing information
   are all findings, not omissions.

8. Write the report to `docs/analysis/NNNN-YYYY-MM-DD-<slug>.md` using the
   template in `analysis.md`, with `analysis-type: validation`.

9. Run three validators as parallel subagents. They are independent lenses, not
   redundant reviewers.

   **Coverage validator.** Give it the source file paths and nothing else. It must
   not see the register. Instruct it to read every source in full, extract its own
   list of atomic statements, and return them. Compare its list against the
   register. Anything it found that has no `S-NNN` is an escape and becomes a
   `Coverage` finding. Withholding the register is the entire point: an agent
   shown the register first will confirm its blind spots rather than find them.

   **Fidelity validator.** Give it the register and the sources. For every
   statement, it confirms the recorded wording faithfully represents the source.
   It reports drift, invention, and over-reach, where a hedge such as "we might
   want" has become a firm statement.

   **Conformance validator.** Give it the report and `analysis.md`. It runs the
   validation checklist and reports failures.

10. Merge validator findings into the report as new `F-NNN` entries. Record
    validator results in section 9.

11. Report to the user:
    - Path written
    - Statements extracted, and the disposition breakdown
    - Undispositioned count, which must be zero
    - Findings by severity
    - Escapes found by the coverage validator, stated first if there are any
    - What must be resolved before an SRS can be baselined, and who resolves it

Never present the analysis as complete while the undispositioned count is
non-zero or a coverage finding is open.
