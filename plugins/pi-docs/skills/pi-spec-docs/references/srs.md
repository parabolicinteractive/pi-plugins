# Software Requirements Specification

Standard: ISO/IEC/IEEE 29148:2018.

Path: `docs/srs.md`

A Software Requirements Specification is a structured collection of the essential
requirements of the software and its external interfaces (29148 3.1.27).

## Content Elements

29148 9.6 defines twenty required content elements for an SRS. The template below
covers all twenty. Each heading names the clause it satisfies so conformance can
be checked mechanically.

| Clause | Element | Template section |
|---|---|---|
| 9.6.1 | SRS overview | 1.1 |
| 9.6.2 | Purpose | 1.2 |
| 9.6.3 | Scope | 1.3 |
| 9.6.4 | Product perspective | 2.1 |
| 9.6.5 | Product functions | 2.2 |
| 9.6.6 | User characteristics | 2.3 |
| 9.6.7 | Limitations | 2.4 |
| 9.6.8 | Assumptions and dependencies | 2.5 |
| 9.6.9 | Apportioning of requirements | 2.6 |
| 9.6.10 | Specified requirements | 3 |
| 9.6.11 | External interfaces | 3.1 |
| 9.6.12 | Functions | 3.2 |
| 9.6.13 | Usability requirements | 3.3 |
| 9.6.14 | Performance requirements | 3.4 |
| 9.6.15 | Logical database requirements | 3.5 |
| 9.6.16 | Design constraints | 3.6 |
| 9.6.17 | Standards compliance | 3.7 |
| 9.6.18 | Software system attributes | 3.8 |
| 9.6.19 | Verification | 4 |
| 9.6.20 | Supporting information | 5 |

General content elements (identification, front matter, definitions, references,
acronyms) come from 29148 9.2 and appear in section 1 and the document header.

> Note on section numbering. 29148 8.5.2 contains an example SRS outline. That
> clause was not available in the publisher preview used to build this template.
> The numbering below is a document structure that carries all twenty 9.6 content
> elements; it is not a reproduction of the 8.5.2 example. If the full standard is
> obtained, reconcile this numbering against 8.5.2 and record any change as a
> tailoring entry.

## Requirement Format

Every requirement is a table plus a statement. Attributes follow 29148 5.2.8.

```markdown
#### FR-AUTH-001: Federated sign-in

**The system shall authenticate users against the organization's configured
OpenID Connect provider.**

| Attribute | Value |
|---|---|
| Identifier | FR-AUTH-001 |
| Rationale | Client mandates no separate credential store. |
| Source | S-001 (RFP 3.2) |
| Priority | Must |
| Verification method | Test |
| Verification reference | TBD |
| Allocated to | TBD |
| Depends on | - |
| Status | Proposed |
```

### Statement Rules

29148 5.2.7 governs requirement language.

- `shall` for mandatory, `should` for recommended, `may` for optional.
- One requirement per statement. A statement containing "and" that joins two
  capabilities is two requirements.
- Active voice. Name the actor.
- No implementation detail. "The system shall store sessions in Redis" is design,
  not requirement. State the behavior and constrain the implementation separately
  under 3.6 if the constraint is genuine.
- No unverifiable adjectives: fast, user-friendly, robust, flexible, intuitive,
  seamless, efficient. If the source used one, that source statement is a finding,
  not a requirement.
- No negative requirements where a positive form exists.

### Attribute Values

| Attribute | Values |
|---|---|
| Priority | Must, Should, Could, Won't. Project convention. 29148 requires a priority attribute but does not mandate this value set. |
| Verification method | Test, Analysis, Inspection, Demonstration |
| Status | Proposed, Approved, Baselined, Withdrawn |

`Source` cites an `S-NNN` from an analysis report, or a parent requirement for a
derived requirement (29148 3.1.10). A requirement with no source is unjustified.
If it genuinely originates from engineering judgment rather than a stakeholder,
say so explicitly: `Derived (engineering judgment)`.

`Withdrawn` requirements stay in the document. Never delete, never renumber,
never reuse the identifier.

## Template

```markdown
---
title: "<Project Name> - Software Requirements Specification"
type: srs
status: draft
version: "0.1"
baseline: null
conformance: full
standard: "ISO/IEC/IEEE 29148:2018"
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
---

# Software Requirements Specification

## Document Identification

| Field | Value |
|---|---|
| Project | |
| Document | Software Requirements Specification |
| Version | 0.1 |
| Baseline | Not baselined |
| Standard | ISO/IEC/IEEE 29148:2018 |
| Conformance | Full |
| Status | Draft |
| Date | YYYY-MM-DD |

## Revision History

| Version | Date | Baseline | Author | Change | Requirements affected | CR |
|---|---|---|---|---|---|---|
| 0.1 | YYYY-MM-DD | - | | Initial draft | - | - |

---

# 1. Introduction

## 1.1 SRS Overview

<!-- 29148 9.6.1 -->

What this document contains and how it is organized. State the software product
being specified and the edition of the standard followed.

## 1.2 Purpose

<!-- 29148 9.6.2 -->

Why this specification exists and who it is for. Name the intended readership:
the development team, the verification team, the acquirer, and any regulatory
reader.

## 1.3 Scope

<!-- 29148 9.6.3 -->

Name the software product. State what it does and, explicitly, what it does not
do. Boundaries stated here are load-bearing: anything outside them that arrives
later is a change request.

### 1.3.1 In Scope

### 1.3.2 Out of Scope

| Item | Reason | Source |
|---|---|---|

## 1.4 Definitions

<!-- 29148 9.2.3 -->

Terms whose meaning in this document differs from ordinary usage, or that the
project uses in a specific sense.

| Term | Definition |
|---|---|

## 1.5 References

<!-- 29148 9.2.4 -->

| Reference | Title | Version | Date |
|---|---|---|---|
| 29148 | ISO/IEC/IEEE 29148, Systems and software engineering, Life cycle processes, Requirements engineering | 2018 | 2018-11 |

## 1.6 Acronyms and Abbreviations

<!-- 29148 9.2.5 -->

| Acronym | Expansion |
|---|---|
| SRS | Software Requirements Specification |
| TBD | To Be Determined |

---

# 2. Overall Description

## 2.1 Product Perspective

<!-- 29148 9.6.4 -->

Where this software sits. Whether it is self-contained or a component of a larger
system. External systems it interacts with. Include a context diagram.

```mermaid
flowchart LR
  User --> System
  System --> ExternalService
```

## 2.2 Product Functions

<!-- 29148 9.6.5 -->

Summary of major capabilities. This is the map, not the territory. Detailed
requirements live in section 3.

| Function | Description | Requirements |
|---|---|---|
| F-01 | | FR-... |

## 2.3 User Characteristics

<!-- 29148 9.6.6 -->

| User class | Description | Technical level | Frequency of use | Accessibility needs |
|---|---|---|---|---|

## 2.4 Limitations

<!-- 29148 9.6.7 -->

Regulatory, hardware, licensing, interface, and technology limitations that bound
the solution space. These are imposed from outside. Distinguish them from design
constraints in 3.6, which are choices made within those bounds.

## 2.5 Assumptions and Dependencies

<!-- 29148 9.6.8 -->

### 2.5.1 Assumptions

| ID | Assumption | Impact if false | Validation plan |
|---|---|---|---|
| ASM-001 | | | |

### 2.5.2 Dependencies

| ID | Dependency | Type | Impact if unavailable | Fallback |
|---|---|---|---|---|
| DEP-001 | | | | |

## 2.6 Apportioning of Requirements

<!-- 29148 9.6.9 -->

Requirements deferred to future versions, and requirements allocated to other
system elements rather than this software.

### 2.6.1 Deferred to Later Releases

| Requirement | Target release | Reason |
|---|---|---|

### 2.6.2 Allocated Elsewhere

| Requirement | Allocated to | Reason |
|---|---|---|

---

# 3. Specific Requirements

<!-- 29148 9.6.10 -->

How the requirements below are organized, and the identifier scheme in use. State
the organizing principle explicitly: by feature area, by user class, by mode of
operation, or by stimulus and response.

## 3.1 External Interfaces

<!-- 29148 9.6.11 -->

### 3.1.1 User Interfaces

### 3.1.2 Hardware Interfaces

### 3.1.3 Software Interfaces

### 3.1.4 Communications Interfaces

<!-- Requirements here use the FR-INT-NNN namespace. -->

## 3.2 Functions

<!-- 29148 9.6.12 -->

### 3.2.1 <Feature Area>

<!-- Requirements in the FR-<AREA>-NNN namespace, in the requirement format. -->

## 3.3 Usability Requirements

<!-- 29148 9.6.13. NFR-USE-NNN. -->

Measurable usability targets. Effectiveness, efficiency, and satisfaction
criteria. Accessibility standard and conformance level.

## 3.4 Performance Requirements

<!-- 29148 9.6.14. NFR-PERF-NNN. -->

Every entry states a metric, a target, a condition, and a measurement method. A
performance requirement without a measurement method is not verifiable.

## 3.5 Logical Database Requirements

<!-- 29148 9.6.15. NFR-DB-NNN. -->

Information to be retained, retention periods, integrity constraints, and access
requirements. Logical only. Physical schema belongs in the SDD.

## 3.6 Design Constraints

<!-- 29148 9.6.16. CON-NNN. -->

Constraints on the solution that are genuinely required, with the reason each is
imposed. A constraint without a rationale is an unexamined assumption.

| ID | Constraint | Rationale | Source |
|---|---|---|---|
| CON-001 | | | |

## 3.7 Standards Compliance

<!-- 29148 9.6.17. -->

Standards, regulations, and policies the software must comply with, and what
compliance requires in practice.

| Standard | Applies to | Compliance requirement | Evidence |
|---|---|---|---|

## 3.8 Software System Attributes

<!-- 29148 9.6.18 -->

### 3.8.1 Reliability

<!-- NFR-REL-NNN -->

### 3.8.2 Availability

<!-- NFR-REL-NNN -->

### 3.8.3 Security

<!-- NFR-SEC-NNN -->

### 3.8.4 Maintainability

<!-- NFR-MAINT-NNN -->

### 3.8.5 Portability

<!-- NFR-PORT-NNN -->

---

# 4. Verification

<!-- 29148 9.6.19 -->

How each requirement in section 3 will be confirmed. This section mirrors section
3 requirement for requirement, and it is the interface to the Software Test Plan.

Every requirement in section 3 appears here exactly once. A requirement with no
verification approach is not verifiable and therefore does not conform to 29148
5.2.5.

| Requirement | Method | Approach | Acceptance criterion | Test reference |
|---|---|---|---|---|
| FR-AUTH-001 | Test | Automated integration test against a staging OIDC provider | Valid credentials yield a session; invalid yield HTTP 401 | TBD |

Methods: Test, Analysis, Inspection, Demonstration.

---

# 5. Supporting Information

<!-- 29148 9.6.20 -->

## 5.1 TBD Register

<!-- 29148 3.2 defines TBD as To Be Determined. -->

Every unresolved item in this document, with an owner and a resolution path. A
document with declared TBDs is conformant. A document with undeclared gaps is not.

| TBD | Location | Blocks | Finding | Owner | Needed by |
|---|---|---|---|---|---|
| Response time target | 3.4 | NFR-PERF-001 | F-002 | Client | Baseline 1.0 |

## 5.2 Traceability

### 5.2.1 Upward, Source to Requirement

| Source statement | Requirement |
|---|---|
| S-001 | FR-AUTH-001 |

### 5.2.2 Downward, Requirement to Allocation and Verification

| Requirement | Allocated to (SAD) | Detailed in (SDD) | Verified by (STP) |
|---|---|---|---|
| FR-AUTH-001 | TBD | TBD | TBD |

### 5.2.3 Requirement Dependencies

| Requirement | Depends on | Depended on by |
|---|---|---|

## 5.3 Tailoring Record

<!-- 29148 Annex C. Required only if conformance is declared as tailored. -->

| Clause | Element omitted | Reason |
|---|---|---|

## 5.4 Withdrawn Requirements

Retained for citation resolution. Never delete, never reuse the identifier.

| Requirement | Withdrawn in version | Reason | Superseded by |
|---|---|---|---|

## 5.5 Analysis Reports

| Report | Type | Date |
|---|---|---|
```

## Validation Checklist

Run against a completed SRS. Findings go into an analysis report of type
`verification` using the finding format in `analysis.md`.

### Content Conformance (29148 9.6)

- [ ] 1.1 SRS overview present
- [ ] 1.2 Purpose present, readership named
- [ ] 1.3 Scope present, with an explicit out-of-scope list
- [ ] 2.1 Product perspective present, with a context diagram
- [ ] 2.2 Product functions present
- [ ] 2.3 User characteristics present
- [ ] 2.4 Limitations present
- [ ] 2.5 Assumptions and dependencies present, each assumption with an impact-if-false
- [ ] 2.6 Apportioning of requirements present, or explicitly stated as none
- [ ] 3 Specified requirements present, organizing principle stated
- [ ] 3.1 External interfaces present
- [ ] 3.2 Functions present
- [ ] 3.3 Usability requirements present
- [ ] 3.4 Performance requirements present
- [ ] 3.5 Logical database requirements present
- [ ] 3.6 Design constraints present, each with a rationale
- [ ] 3.7 Standards compliance present
- [ ] 3.8 Software system attributes present
- [ ] 4 Verification present
- [ ] 5 Supporting information present
- [ ] Any omitted element appears in the tailoring record with a reason

### Individual Requirement Characteristics (29148 5.2.5)

For each requirement:

- [ ] **Necessary**: removing it would leave a deficiency
- [ ] **Appropriate**: correct level of abstraction, no implementation detail
- [ ] **Unambiguous**: one interpretation only
- [ ] **Complete**: needs no further information to be actioned
- [ ] **Singular**: states one capability, characteristic, constraint, or quality factor
- [ ] **Feasible**: achievable within technology, cost, and schedule
- [ ] **Verifiable**: measurable, with a method named in section 4
- [ ] **Correct**: accurately represents the need it came from
- [ ] **Conforming**: follows the statement rules above

### Requirement Set Characteristics (29148 5.2.6)

- [ ] **Complete**: the set covers everything the solution needs, with no undeclared gaps
- [ ] **Consistent**: no two requirements contradict, terminology is uniform throughout
- [ ] **Feasible**: the set as a whole fits the real budget, schedule, and technical limits
- [ ] **Comprehensible**: a competent reader can understand it without the authors present
- [ ] **Able to be validated**: it can be checked against genuine stakeholder intent

### Identifier Integrity

- [ ] Every requirement has a unique identifier
- [ ] No identifier has been renumbered since the last baseline
- [ ] No withdrawn identifier has been reused
- [ ] Withdrawn requirements are present and listed in 5.4
- [ ] Namespaces match the section the requirement lives in

### Traceability

- [ ] Every requirement cites a source statement, a parent requirement, or an explicit derivation
- [ ] Every source statement dispositioned as `requirement` resolves to a requirement in this document
- [ ] Every requirement appears exactly once in section 4
- [ ] Every requirement has a downward allocation, or `TBD` with a register entry
- [ ] No citation points to a non-existent identifier

### TBD Discipline

- [ ] Every `TBD` in the document appears in the 5.1 register
- [ ] Every register entry names an owner and what it blocks
- [ ] Every register entry linked to a finding names a real `F-NNN`
- [ ] No section is silently empty. Empty means either a TBD or a tailoring entry

### Output Rules

- [ ] No emojis
- [ ] No em-dashes
- [ ] Diagrams are Mermaid
- [ ] No unverifiable adjectives in requirement statements
