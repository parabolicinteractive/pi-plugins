# Analysis Reports

Standard: ISO/IEC/IEEE 29148:2018.

An analysis report records what was found, when, and what was decided about it.
There are two types, and they are the standard's own two activities:

| Type | Definition | Command |
|---|---|---|
| `validation` | Confirming requirements define the right system as intended by stakeholders (29148 3.1.25) | `/pi-docs-analyze-sources` |
| `verification` | Confirming requirements are well-formed individually and as a set (29148 3.1.26) | `/pi-docs-realign` |

Path: `docs/analysis/NNNN-YYYY-MM-DD-<slug>.md`

Reports share one `NNNN` sequence and one project-global `F-NNN` finding
sequence, regardless of type.

## Mutability

Finding text is frozen once written. The report is a record of what was found on
a date.

The `Status` and `Rationale` fields of a finding are mutable. They are the only
mutable fields. Changing a finding's wording after the fact destroys the record.

`grep -rn "Status: Open" docs/analysis/` returns every live finding in the
project. This is the only finding register. Do not create a separate one.

## Source Statement Register

Produced by `validation` reports only. This is the mechanism that makes source
coverage checkable rather than aspirational.

Extract every atomic statement from the source material. An atomic statement is
one assertion, request, constraint, or piece of context. Split compound
sentences. A sentence containing three requests produces three statements.

Assign each an `S-NNN` and exactly one disposition.

```markdown
## Source Statement Register

| ID | Statement | Source | Disposition | Resolves To |
|---|---|---|---|---|
| S-001 | Users sign in with their corporate Google account. | RFP 3.2 | requirement | STR-003 |
| S-002 | The system should be fast. | RFP 3.4 | finding | F-002 |
| S-003 | We use Jira internally for ticketing. | Kickoff 2026-07-14 | context | - |
| S-004 | Reporting module, possibly phase two. | Kickoff 2026-07-14 | out-of-scope | See 3.2 |
| S-005 | Must retain audit logs for seven years. | RFP 5.1 | requirement | STR-011 |

Statements: 5. Dispositioned: 5. Undispositioned: 0.
```

The final line is a required invariant check. `Undispositioned` must be zero.

### Disposition Rules

- `requirement` - the statement became a requirement. Name it in `Resolves To`.
  When the project has a Stakeholder Requirements Specification, this resolves to
  an `STR-NNN`, not directly to a software requirement. The StRS then carries the
  derivation to `FR-NNN` and `NFR-NNN`. Resolve directly to a software
  requirement only when the project has no StRS.
- `finding` - the statement cannot become a requirement as written. It is
  ambiguous, contradictory, unverifiable, or requires client input. Name the
  `F-NNN`.
- `out-of-scope` - deliberately excluded. Record the reason in the out-of-scope
  section. Out-of-scope statements are change request candidates once a baseline
  exists.
- `context` - background, narrative, or organizational detail carrying no
  instruction. Use sparingly. When in doubt, raise a finding instead.

A statement that cannot be dispositioned becomes a finding. There is no fifth
option. Never omit a statement from the register because it seemed unimportant.

### Source Citation

Every statement cites where it came from, precisely enough to find again: a
document name plus section, a transcript name plus date, or a file path plus line
range. "The client said" is not a citation.

## Finding Format

```markdown
### F-002: Performance expectation is unverifiable

- **Type**: Conformance | Consistency | Currency | Coverage | Fidelity
- **Violates**: Verifiable (individual characteristic, 29148 5.2.5)
- **Severity**: High | Medium | Low
- **Class**: Mechanical | Judgment
- **Sources**: S-002 (RFP 3.4)
- **Affects**: srs.md 3.5
- **Trigger**: FR-012
- **Description**: The source states the system should be fast. No metric,
  target, or measurement method is given, so no test can confirm or refute it.
- **Recommendation**: Obtain a target response time and percentile from the
  client. Until then FR-012 carries a TBD.
- **Status**: Open | Fixed | Deferred | Accepted | Promoted
- **Rationale**:
- **Date**: 2026-08-12
```

Omit `Trigger` and `Affects` on validation findings where they do not apply.

### Finding Types

| Type | Question | Raised by |
|---|---|---|
| `Coverage` | Is something in the sources unaccounted for? | Coverage validator |
| `Fidelity` | Does a requirement misstate its source? | Fidelity validator |
| `Conformance` | Does a document lack required content, or a requirement lack a required characteristic? | Conformance validator, realign |
| `Consistency` | Do two documents or two requirements contradict? | Realign |
| `Currency` | Does a document cite something that has since changed? | Realign |

### Finding Class

`Mechanical` findings have a deterministic repair: a dangling identifier, a stale
quoted title, a version mismatch, a missing back-link.

`Judgment` findings require re-derivation: a requirement's behavior changed, so
dependent design or test content may now be wrong. Never auto-repair these. A
machine rewriting design prose from a requirement diff produces confident,
plausible, wrong output.

### Statuses

| Status | Meaning | Re-raised next run |
|---|---|---|
| `Open` | Not yet dispositioned | Yes |
| `Fixed` | Repaired. Mechanical findings repaired directly, judgment findings by regenerating the affected section | No |
| `Deferred` | Real, not now. Record why and when it will be revisited | Yes, as known-deferred |
| `Accepted` | Intentional divergence or false positive. Record why | No |
| `Promoted` | Genuine new scope. A change request was raised. Record the `CR-NNN` | No |

`Deferred` and `Accepted` differ only in whether the finding comes back. That
difference is the reason both exist.

## Report Template

```markdown
---
title: "<Project> - <Source Intake | Realignment> Analysis"
type: analysis
analysis-type: validation
status: draft
version: "1.0"
baseline: null
conformance: full
standard: "ISO/IEC/IEEE 29148:2018"
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
---

# <Title>

## 1. Identification

| Field | Value |
|---|---|
| Report | NNNN |
| Type | validation |
| Date | YYYY-MM-DD |
| Baseline analysed | 1.0 or "pre-baseline" |
| Analyst | name |

## 2. Scope

What was analysed and what was not. For validation reports, list every source
consumed with its identifier, date, and provenance. For verification reports,
list every document inspected and the baseline or commit it was compared
against.

### 2.1 Sources Consumed

| Source | Type | Date | Provenance |
|---|---|---|---|
| RFP | Document | 2026-06-30 | Client, via email |
| Kickoff transcript | Transcript | 2026-07-14 | Recorded call |

### 2.2 Not Analysed

Anything deliberately excluded, and why. State explicitly if nothing was
excluded.

## 3. Source Statement Register

<!-- Validation reports only. See the register format above. -->

## 4. Findings

<!-- One subsection per finding, in the finding format above. -->

## 5. Out of Scope

Statements deliberately excluded from the requirements, with reasons. After a
baseline exists, each entry here is a change request candidate.

| ID | Statement | Reason | CR |
|---|---|---|---|
| S-004 | Reporting module, possibly phase two. | Client marked as later phase. Not in current scope. | - |

## 6. Results

| Metric | Count |
|---|---|
| Source statements extracted | |
| Became requirements | |
| Became findings | |
| Declared out of scope | |
| Classified as context | |
| Undispositioned | 0 |
| Findings open | |
| Findings by severity (H/M/L) | |

## 7. Conformance

Full | Tailored | Non-conformant

State the basis. If tailored, list each omission and its reason.

## 8. Recommendations

Ordered actions. What must be resolved before the SRS can be baselined, and who
must resolve it.

## 9. Validator Results

| Validator | Findings raised | Escapes found |
|---|---|---|
| Coverage | | |
| Fidelity | | |
| Conformance | | |
```

## Change Requests

A finding promoted to a change request moves out of the analysis stream.

Path: `docs/changes/CR-NNN-<slug>.md`

```markdown
---
title: "<Short description>"
type: change-request
status: open
version: "1.0"
baseline: "1.0"
conformance: full
standard: "ISO/IEC/IEEE 29148:2018 6.6.2"
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
---

# CR-NNN: <Title>

## Origin

| Field | Value |
|---|---|
| Raised by | F-023 |
| Baseline in effect | 1.0 |
| Requested by | Client, Acme Corp |
| Date received | YYYY-MM-DD |

## Description

What is being requested, in the requester's terms.

## Requirements Affected

| Requirement | Change |
|---|---|
| FR-012 | Modified |
| FR-048 | New |

## Documents Affected

| Document | Sections |
|---|---|
| srs.md | 3.3 |
| sad.md | 4.3 |

## Impact Analysis

Effect on scope, schedule, and cost. State unknowns as unknowns.

## Disposition

Approved | Rejected | Deferred, with date and approver.
```

The `baseline` field is what makes scope reporting possible. Every change request
carrying `baseline: 1.0` arrived after that baseline was agreed. That set is the
scope expansion report.

## Validation Checklist

Run against a completed analysis report.

### Register Integrity

- [ ] Every source listed in 2.1 has at least one `S-NNN` derived from it
- [ ] `Undispositioned` count is zero
- [ ] Every `S-NNN` with disposition `requirement` names a real requirement in `Resolves To`
- [ ] Every `S-NNN` with disposition `finding` names a real `F-NNN`
- [ ] Every `S-NNN` with disposition `out-of-scope` appears in section 5
- [ ] No `S-NNN` is duplicated
- [ ] No `S-NNN` has been renumbered or reused
- [ ] Every statement cites a locatable source, not a paraphrase of who said it
- [ ] Compound statements were split, not recorded as one

### Finding Integrity

- [ ] Every finding names the characteristic it violates, with clause
- [ ] Every finding has a type, severity, and class
- [ ] Every finding has a recommendation, not just a complaint
- [ ] Every non-Open finding has a rationale
- [ ] Every `Promoted` finding names an existing `CR-NNN`
- [ ] No `F-NNN` reused across reports

### Report Integrity

- [ ] Section 2.2 states what was excluded, or states that nothing was
- [ ] Results counts reconcile with the register
- [ ] Conformance is declared, and tailoring is itemised if claimed
- [ ] All three validators ran, and section 9 records their results
- [ ] No coverage findings remain Open if the report is being treated as complete

### Output Rules

- [ ] No emojis
- [ ] No em-dashes
