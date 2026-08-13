# Architecture Decision Record

Standard: ISO/IEC/IEEE 42010:2022, clause 6.10 (recording of architecture
decisions and rationale) and 5.2.12.

Path: `docs/adr/NNNN-<slug>.md`

An ADR is one architecture decision, recorded permanently. Decisions and
rationale are AD elements (42010 3.4), so ADRs are part of the architecture
description, not a practice bolted alongside it.

## Immutability

**An accepted ADR is never edited.** This is the one rule that matters.

When a decision changes, write a new ADR that supersedes the old one. Set the old
one's status to `superseded` and name its successor. That is the only permitted
modification to an accepted record.

The reason: the value of a decision record is that it preserves why a choice was
made at a point in time, with the information available then. Editing it to match
current reality destroys the only thing it was for. This is why decisions live in
separate files rather than inside the SAD, which is revised in place.

Consequences:

- Never renumber an ADR.
- Never reuse a number, including from a rejected or superseded ADR.
- Never delete an ADR.
- Correct typographical errors only. Never revise reasoning, alternatives, or
  outcomes.

## Is It ADR-Worthy

42010 guidance on selecting key decisions. A decision qualifies if it is any of:

- Affecting key stakeholders, or many stakeholders
- Essential to project planning and management
- Expensive to enforce or implement
- Highly sensitive to change, or costly to change later
- Involving intricate or non-obvious reasoning
- Pertaining to architecturally significant requirements
- Requiring major expenditure of time or effort to decide
- Resulting in capital expenditure or indirect costs

If a decision meets none of these, it is an implementation choice. Put it in the
SDD.

## Required Information Items

42010 names the information items to consider when recording a decision. All of
them appear in the template below.

| Item | Field |
|---|---|
| Unique identifier | Frontmatter `id` |
| Statement of the decision | Decision |
| Linkages to concerns it pertains to | Concerns |
| Owner of the decision | Owner |
| Linkages to affected AD elements | Affects |
| Rationale linked to the decision | Rationale |
| Forces and constraints on the decision | Forces and Constraints |
| Assumptions influencing the decision | Assumptions |
| Considered alternatives and their potential consequences | Alternatives Considered |

**Alternatives are not optional.** 42010 requires the AD to give evidence that
multiple architectures were considered. An ADR listing no alternatives fails that
requirement and is also useless: it records an outcome without a decision.

## Statuses

| Status | Meaning | Editable |
|---|---|---|
| `proposed` | Under discussion, not yet decided | Yes |
| `accepted` | Decided and in force | No |
| `rejected` | Considered and declined. Retained so the option is not re-proposed | No |
| `superseded` | Replaced by a later ADR, named in `superseded-by` | Status field only |
| `deprecated` | No longer relevant, not replaced | Status field only |

A `rejected` ADR is as valuable as an accepted one. It is the record that stops
an agent or a new engineer from re-proposing something already ruled out.

## Template

```markdown
---
title: "<Decision title, stated as the decision>"
type: adr
id: ADR-0001
status: proposed
version: "1.0"
baseline: null
conformance: not-claimed
standard: "ISO/IEC/IEEE 42010:2022 6.10"
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
owner: name
concerns: [CNC-002]
affects: [VW-002, VW-004]
supersedes: null
superseded-by: null
---

# ADR-0001: <Title>

## Status

proposed

<!-- If superseded: "superseded by [ADR-0042](0042-slug.md) on YYYY-MM-DD" -->

## Context

The situation that forced a decision. What is true, what is constrained, what is
at stake. Written so a reader two years from now understands the problem without
having been in the room.

State the problem, not the answer.

## Decision

**The single sentence statement of what was decided.**

Then any elaboration needed to make the decision unambiguous.

## Concerns Addressed

Which stakeholder concerns from the SAD this decision pertains to.

| Concern | Stakeholders |
|---|---|
| CNC-002 | STK-001, STK-003 |

## AD Elements Affected

Which parts of the architecture description this decision touches.

| Element | Effect |
|---|---|
| VW-002 | Process view restructured around the queue |
| VW-004 | Deployment view gains a broker node |

## Forces and Constraints

What pushed on this decision. Regulatory limits, budget, schedule, existing
commitments, technical bounds, team capability. Constraints are externally
imposed; forces are competing pressures.

| Force or constraint | Type | Effect on the decision |
|---|---|---|

## Assumptions

What was believed true when deciding. Each assumption is a tripwire: if it turns
out false, this decision should be revisited.

| ID | Assumption | Impact if false |
|---|---|---|
| A-1 | | |

## Alternatives Considered

Required. At least one real alternative, considered seriously.

### Alternative 1: <name>

- **Description**:
- **Consequences if chosen**:
- **Why not chosen**:

### Alternative 2: <name>

- **Description**:
- **Consequences if chosen**:
- **Why not chosen**:

## Rationale

Why the chosen option beat the alternatives, given the forces, constraints, and
assumptions above. This is the reasoning, not a restatement of the decision.

## Consequences

What becomes true because of this decision. Include the costs, not only the
benefits. A decision with no negative consequences was not a real tradeoff and
the analysis is probably incomplete.

**Positive**

**Negative**

**Neutral**

## Related

| Relation | Target |
|---|---|
| Supersedes | - |
| Superseded by | - |
| Relates to | - |
| Satisfies requirement | FR-012 |
```

## Superseding

When writing an ADR that supersedes another:

1. Create the new ADR with the next unused number. Set `supersedes` to the old
   identifier.
2. In the new ADR's Context, state what changed since the original decision. That
   is the whole point of the pair.
3. In the old ADR, change only two things: `status` to `superseded`, and
   `superseded-by` to the new identifier. Add a status line naming the successor
   and the date. Change nothing else.
4. Update the SAD's decision register in section 6.1.

## Validation Checklist

### Record Integrity

- [ ] Identifier is unique and unreused
- [ ] Title states the decision, not the topic
- [ ] Status is one of the five defined values
- [ ] Context states a problem, not a solution
- [ ] Decision is a single unambiguous statement
- [ ] Owner named

### 42010 Information Items

- [ ] Concerns named, and each exists in the SAD
- [ ] AD elements affected named, and each exists in the SAD
- [ ] Forces and constraints recorded
- [ ] Assumptions recorded, each with an impact-if-false
- [ ] **At least one real alternative considered, with consequences and reason for rejection**
- [ ] Rationale explains why the choice beat the alternatives
- [ ] Consequences include negatives, not only benefits

### Immutability

- [ ] No accepted ADR has been edited except status and supersession fields
- [ ] Every `superseded` ADR names its successor
- [ ] Every `supersedes` reference points to an ADR that exists and is marked superseded
- [ ] No ADR deleted
- [ ] No number reused

### Register Consistency

- [ ] Every ADR in `docs/adr/` appears in the SAD decision register
- [ ] Every register entry points to an ADR that exists
- [ ] Register statuses match the ADR files

### Output Rules

- [ ] No emojis
- [ ] No em-dashes

## Attribution

Decision information items and key-decision selection criteria recovered from the
architecture description template for use with ISO/IEC/IEEE 42010:2011 by Rich
Hilliard, licensed CC BY 3.0. http://www.iso-architecture.org/42010/templates/
