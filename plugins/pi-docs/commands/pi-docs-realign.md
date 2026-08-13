---
description: Check the spec document set for conformance, consistency, and currency
argument-hint: [baseline, tag, or commit to compare against]
allowed-tools: Read, Write, Edit, Glob, Grep, Task, AskUserQuestion, Bash(ls:*,find:*,git log:*,git diff:*,git tag:*,git show:*)
---

Check every specification document in the project, report what is wrong, and walk
each finding to a disposition.

This is requirements verification in the sense of ISO/IEC/IEEE 29148 3.1.26:
confirming that requirements are well-formed individually and as a set. Change
management follows 29148 6.6.2.

Despite the name, this command reports and then acts on your instruction. It does
not rewrite documents on its own.

## Arguments

`$1` is an optional baseline, git tag, or commit to compare against. If empty,
compare against the most recent git tag; if the repository has no tags, compare
against `HEAD`. If the project is not a git repository, fall back to the
requirements-affected column of each document's revision history.

## Three Classes of Check

**Conformance.** Does each document contain the content its standard requires,
and does each requirement meet the individual characteristics (29148 5.2.5)? Does
the set meet the set characteristics (29148 5.2.6)?

**Consistency.** Do cross-document citations resolve? Report orphan requirements
that nothing satisfies or verifies, orphan components that satisfy nothing, and
citations pointing at identifiers that do not exist.

**Currency.** Has anything a document cites changed since that document was last
updated? This is the cascade: when a requirement moves, every document citing it
is suspect.

**Terminology.** Is a term defined two ways across documents, used without being
defined anywhere, or listed as rejected but still in use? Skip this class if
`docs/glossary.md` does not exist, and say that it was skipped rather than
reporting a clean result.

## Procedure

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/pi-spec-docs/SKILL.md` and
   `${CLAUDE_PLUGIN_ROOT}/skills/pi-spec-docs/references/analysis.md`.

2. Inventory `docs/`. Note which documents exist, their versions, baselines, and
   declared conformance. A missing downstream document is itself a finding when
   an upstream one is baselined.

3. Read the existing findings register. `grep -rn "Status:" docs/analysis/`.
   Findings already dispositioned as `Accepted` or `Promoted` are not re-raised.
   Findings dispositioned as `Deferred` are reported as known-deferred, not as
   new.

4. Run the conformance check. For each document, read its reference file and run
   its validation checklist. Read only the reference files for documents that
   exist.

5. Run the consistency check. Build the identifier graph: every identifier
   defined, and every citation of one. Report both directions of orphan and every
   dangling citation.

6. Run the currency check. Diff the SRS against the comparison point from `$1`.
   Extract the identifiers of requirements whose statement, attributes, or status
   changed. For each changed identifier, find every downstream citation and raise
   a `Currency` finding against that section.

6b. Run the terminology check, if `docs/glossary.md` exists. Read it, then for
    each document compare its local definitions section against the glossary and
    scan its body for rejected terms and for domain terms defined nowhere. Raise
    `Terminology` findings per `references/glossary.md`. Undefined-term detection
    is imprecise, so prefer a candidate list the user can dismiss over silently
    missing drift; `Accepted` dispositions persist so a dismissed term is not
    raised again.

    If the glossary does not exist, report the class as skipped. Do not report it
    as clean.

7. Classify every finding as `Mechanical` or `Judgment`.

   `Mechanical` findings have a deterministic repair: a dangling identifier, a
   stale quoted title, a version mismatch, a missing back-link.

   `Judgment` findings require re-derivation: a requirement's behavior changed, so
   dependent design or test content may now be wrong. Repairing one means
   regenerating that section, not editing a string.

8. Write the report to `docs/analysis/NNNN-YYYY-MM-DD-realign-<baseline>.md` with
   `analysis-type: verification`, before walking dispositions. The report must
   survive the session regardless of what is decided next.

9. Present the report to the user: findings grouped by class and severity, counts,
   and the declared conformance of each document.

10. Walk the findings using `AskUserQuestion`, four at a time. For each finding
    offer exactly these four dispositions:

    | Option | Action |
    |---|---|
    | **Fix now** | Mechanical: apply the deterministic repair. Judgment: regenerate the affected section from the current upstream document. Set status `Fixed`. |
    | **Defer** | Record the reason and set status `Deferred`. Nothing is written to the document. Re-raised next run as known-deferred. |
    | **Promote to CR** | Genuine new scope. Create `docs/changes/CR-NNN-<slug>.md` with the current `baseline`. Set status `Promoted` and name the CR. |
    | **Accept** | Intentional divergence or false positive. Record the reason and set status `Accepted`. Not re-raised. |

    State each finding's class in the question so the user knows what "Fix now"
    costs on that item. Never auto-repair a `Judgment` finding without an explicit
    choice.

11. Apply the chosen dispositions. Update the report's `Status` and `Rationale`
    fields. These are the only mutable fields in a finding; never alter finding
    text after it is written.

12. Emit a Scope Since Baseline summary: every change request carrying the
    current `baseline` value, with its status. This is the record of what arrived
    after the specification was agreed.

13. Report to the user:
    - Report path
    - Findings by type, severity, and disposition
    - Documents whose conformance changed
    - Scope since baseline: change request count and their status
    - Findings still open

## Notes

Do not create a separate finding register. `docs/analysis/` is the register, and
`grep "Status: Open"` is the query.

Do not write markers into specification documents. Findings live in analysis
reports, which are readable, diffable, and citable. A comment buried in a
deliverable is not a work item.
