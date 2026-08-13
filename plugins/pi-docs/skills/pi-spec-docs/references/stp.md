# Software Test Plan

Standard: ISO/IEC/IEEE 29119-3:2021.

Path: `docs/stp.md`

A test plan is a detailed description of test objectives to be achieved and the
means and schedule for achieving them, organized to coordinate testing activities
for some test item or set of test items (29119-3 3.19).

## Source Basis and Conformance Limits

| Element | Basis |
|---|---|
| Clause structure, every content element of clauses 5, 6, 7 and 8, all 26 clause 3 definitions, clause 1 scope, Figure 1 document hierarchy | Verified against the ISO publisher preview of 29119-3:2021 |
| Clause 4 conformance subclause titles (4.1.1 General, 4.1.2 Full conformance, 4.1.3 Tailored conformance) | Verified from the contents page |
| Clause 4 conformance normative text | Not available. Standard page 5, one page past where the preview ends |
| Annex A obligation levels (which items are shall, should, or may) | Not available. Annex A is normative and begins at standard page 32 |

29119-3 permits tailoring, like 29148 and unlike 42010.

**What the Annex A gap means in practice.** The complete list of information
items is verified; only their individual obligation levels are not. The template
below includes every item. A document containing every information item satisfies
any combination of shall, should and may, so `conformance: full` is defensible
when nothing is omitted.

The gap bites only on omission. Without Annex A you cannot tell whether leaving
something out is permitted or is a conformance failure. Therefore **every
omission is recorded as tailoring**, with a reason, rather than assumed
permissible. Never silently drop an element.

## Document Family

29119-3 defines a family of test documents. Figure 1 gives their hierarchy.

```
Test policy (6.2)
  Organizational test practices (6.3)
    Project test plan (7.2) ......... Test status report (7.3)
      Level test plan (7.2)  ........ Test completion report (7.4)
      Type test plan (7.2)
        Test model specification (8.2)
          Test case specification (8.3)
            Test procedure specification (8.4)
              Test data requirements (8.5) -> readiness report (8.7)
              Test environment requirements (8.6) -> readiness report (8.8)
              Actual results and test result (8.9)
              Test execution log (8.10)
              Test incident report (8.11)
```

`/pi-docs stp` produces the **project test plan** (clause 7.2) with common
information (clause 5). That is what "STP" means here.

The rest are execution-time artifacts produced while testing runs, not
specification-time documents. This package does not generate them. Say so plainly
if a user asks for one, and point at the clause so they know it exists.

A project may have more than one test plan (3.19 note 1): a project test plan,
also called a master test plan, covering all testing, plus level plans (system,
integration) or type plans (performance, security) beneath it. When generating a
level or type plan, the same template applies; set `plan-level` in frontmatter
and reference the parent plan.

## Relationship to the SRS

Two links are defined by the standard, not invented here.

**Test basis** (3.7) is the information used as the basis for designing and
implementing test cases, and the standard names a requirements specification as a
typical form. The SRS is the test basis. Name it as such in section 2.

**Test traceability matrix** (3.26) identifies related items in documentation and
software, such as requirements with associated tests. This is the same object as
the SRS's own Verification section (29148 9.6.19). They must agree. A requirement
whose verification approach in the SRS does not match its coverage here is a
Consistency finding.

## Identifiers

| Prefix | Element |
|---|---|
| `TP-NNN` | Test plan, when a project has level or type plans |
| `RSK-NNN` | Risk register entry |
| `TM-NNN` | Test model |
| `TCI-NNN` | Test coverage item |
| `TC-NNN` | Test case |

Same immutability rules as requirements. See `SKILL.md`.

`TC-NNN` identifiers are referenced from this plan but defined in the test case
specification (8.3), which this package does not generate. Referencing an
undefined `TC-NNN` is acceptable while the plan is a forward-looking document;
`/pi-docs-realign` reports them as unresolved rather than as dangling.

## Terminology

29119-3:2021 **replaced test conditions with test models** (3.16). Do not use
"test condition". A test model is a representation of the test item that lets
testing focus on particular characteristics; the test model plus the required
coverage identifies test coverage items.

Other terms to use precisely: incident (3.3) is the event, incident report (3.4)
is its documentation, and the standard notes it is also called an anomaly, bug,
defect, error, issue, problem or trouble report. Test result (3.22) is pass or
fail; actual results (3.1) are what was observed.

## Template

```markdown
---
title: "<Project Name> - Software Test Plan"
type: stp
plan-level: project
parent-plan: null
status: draft
version: "0.1"
baseline: null
conformance: full
standard: "ISO/IEC/IEEE 29119-3:2021"
test-basis: [srs.md]
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
---

# Software Test Plan

## 1. Common Information

<!-- 29119-3 clause 5.2 -->

### 1.1 Unique Identifier

<!-- 5.2.1 -->

| Field | Value |
|---|---|
| Document identifier | |
| Version | 0.1 |
| Plan level | Project |
| Parent plan | Not applicable |

### 1.2 Issuing Organization

<!-- 5.2.2 -->

### 1.3 Approval Authority

<!-- 5.2.3 -->

| Name | Role | Approval date |
|---|---|---|

### 1.4 Change History

<!-- 5.2.4 -->

| Version | Date | Baseline | Author | Change | CR |
|---|---|---|---|---|---|
| 0.1 | YYYY-MM-DD | - | | Initial draft | - |

### 1.5 Status

<!-- 5.2.5 -->

Draft

### 1.6 Introduction

<!-- 5.2.6 -->

Purpose of this plan and its intended readership.

### 1.7 Scope

<!-- 5.2.7 -->

What testing this plan covers and, explicitly, what it does not. Name the test
items in scope and out of scope.

#### 1.7.1 Out of Scope

| Item | Reason |
|---|---|

### 1.8 References

<!-- 5.2.8 -->

| Reference | Title | Version | Date |
|---|---|---|---|
| srs.md | Software Requirements Specification | | |
| 29119-3 | ISO/IEC/IEEE 29119-3, Software and systems engineering, Software testing, Part 3: Test documentation | 2021 | 2021-10 |

### 1.9 Glossary

<!-- 5.2.9 -->

Terms specific to this plan. Link to the SRS for shared vocabulary.

| Term | Definition |
|---|---|

---

# 2. Context of Testing

<!-- 29119-3 7.2.2 -->

## 2.1 Project or Product

What is being tested and why testing is being performed.

## 2.2 Test Items

| Test item | Description | Version |
|---|---|---|

## 2.3 Test Basis

<!-- 29119-3 3.7 -->

The documentation used as the basis for designing and implementing test cases.

| Test basis | Role |
|---|---|
| `docs/srs.md` | Functional and non-functional requirements under verification |
| `docs/sad.md` | Architecture, for integration and deployment test design |

## 2.4 Test Levels and Types

Which levels (unit, integration, system, acceptance) and types (functional,
performance, security, usability) are in scope for this plan.

---

# 3. Assumptions and Constraints

<!-- 29119-3 7.2.3 -->

## 3.1 Assumptions

| ID | Assumption | Impact if false |
|---|---|---|

## 3.2 Constraints

| ID | Constraint | Source | Effect on testing |
|---|---|---|---|

---

# 4. Stakeholders

<!-- 29119-3 7.2.4 -->

| Stakeholder | Role in testing | Responsibilities | Interest |
|---|---|---|---|

---

# 5. Testing Communication

<!-- 29119-3 7.2.5 -->

How test information flows: reporting lines, cadence, escalation, and the
artifacts each stakeholder receives.

| Communication | From | To | Frequency | Format |
|---|---|---|---|---|
| Test status report | Test lead | Project stakeholders | Weekly | 29119-3 7.3 |

---

# 6. Risk Register

<!-- 29119-3 7.2.6 -->

Both product risks (what could be wrong with the thing) and project risks (what
could go wrong with the testing).

| ID | Risk | Type | Likelihood | Impact | Exposure | Mitigation | Owner |
|---|---|---|---|---|---|---|---|
| RSK-001 | | Product | | | | | |

Risk-based prioritization drives the test strategy below. A risk with no
mitigating test activity, and a test activity mitigating no risk, are both
findings.

---

# 7. Test Strategy

<!-- 29119-3 7.2.7. Test strategy is part of the test plan (3.25). -->

## 7.1 Test Levels and Test Types

| Level or type | In scope | Entry criteria | Exit criteria |
|---|---|---|---|

## 7.2 Test Design Techniques

Techniques used to derive test cases, and why. See 29119-4 for the catalogue.

| Technique | Applied to | Rationale |
|---|---|---|

## 7.3 Test Completion Criteria

Measurable conditions under which testing stops. Coverage targets, defect
thresholds, residual risk tolerance.

| Criterion | Target | Measurement |
|---|---|---|

## 7.4 Retesting and Regression Testing

Approach to confirming fixes and to detecting regressions.

## 7.5 Test Data

Approach to obtaining, managing, and disposing of test data. Detailed
requirements belong in a test data requirements document (29119-3 8.5).

## 7.6 Test Environment

Environments required and how they are provisioned. Detailed requirements belong
in a test environment requirements document (29119-3 8.6).

| Environment | Purpose | Owner |
|---|---|---|

## 7.7 Tooling

| Tool | Purpose | Owner |
|---|---|---|

## 7.8 Test Deliverables

What testing will produce, and who receives each.

| Deliverable | 29119-3 clause | Recipient |
|---|---|---|
| Test status report | 7.3 | |
| Test completion report | 7.4 | |

## 7.9 Metrics

| Measure | Purpose | Collection method |
|---|---|---|

---

# 8. Testing Activities and Estimates

<!-- 29119-3 7.2.8 -->

| Activity | Description | Estimate | Dependencies | Risk mitigated |
|---|---|---|---|---|

---

# 9. Staffing

<!-- 29119-3 7.2.9 -->

## 9.1 Roles and Responsibilities

| Role | Responsibility | Named |
|---|---|---|

## 9.2 Skills and Training Needs

| Skill required | Gap | Training plan |
|---|---|---|

---

# 10. Schedule

<!-- 29119-3 7.2.10 -->

| Milestone | Activity | Start | End | Depends on |
|---|---|---|---|---|

---

# 11. Test Traceability Matrix

<!-- 29119-3 3.26. Must agree with the SRS Verification section, 29148 9.6.19. -->

Every requirement in the SRS appears here. A requirement with no test coverage is
unverified, which means it fails the 29148 5.2.5 verifiable characteristic.

| Requirement | Verification method (SRS 4) | Test model | Test coverage item | Test cases | Level |
|---|---|---|---|---|---|
| FR-AUTH-001 | Test | TM-001 | TCI-003 | TC-014, TC-015 | System |

Unverified requirements: none.

Methods must match the SRS Verification section exactly: Test, Analysis,
Inspection, Demonstration.

---

# 12. Supporting Information

## 12.1 TBD Register

| TBD | Location | Blocks | Finding | Owner | Needed by |
|---|---|---|---|---|---|

## 12.2 Tailoring Record

<!-- 29119-3 4.1.3. Required if conformance is declared as tailored. -->

Every information item omitted from clauses 5 and 7.2, with a reason.

| Clause | Element omitted | Reason |
|---|---|---|
```

## Validation Checklist

Findings go into an analysis report of type `verification`.

### Common Information (29119-3 clause 5.2)

- [ ] 5.2.1 Unique identifier
- [ ] 5.2.2 Issuing organization
- [ ] 5.2.3 Approval authority
- [ ] 5.2.4 Change history
- [ ] 5.2.5 Status
- [ ] 5.2.6 Introduction
- [ ] 5.2.7 Scope, with an explicit out-of-scope list
- [ ] 5.2.8 References
- [ ] 5.2.9 Glossary

### Test Plan Content (29119-3 clause 7.2)

- [ ] 7.2.2 Context of testing, including test items and test basis
- [ ] 7.2.3 Assumptions and constraints, each assumption with an impact-if-false
- [ ] 7.2.4 Stakeholders, with testing responsibilities
- [ ] 7.2.5 Testing communication
- [ ] 7.2.6 Risk register, covering product and project risks
- [ ] 7.2.7 Test strategy
- [ ] 7.2.8 Testing activities and estimates
- [ ] 7.2.9 Staffing, including skills and training needs
- [ ] 7.2.10 Schedule
- [ ] Any omitted element appears in the tailoring record with a reason

### Traceability

- [ ] The test basis is named and points at documents that exist
- [ ] **Every requirement in the SRS appears in the traceability matrix**
- [ ] Every requirement has at least one test coverage item or a recorded reason it has none
- [ ] Verification methods here match the SRS Verification section exactly
- [ ] No test activity exists that mitigates no risk and covers no requirement
- [ ] No risk in the register lacks a mitigating test activity
- [ ] Every `TC-NNN` referenced is either defined or recorded as not yet specified

### Coherence

- [ ] Test completion criteria are measurable, with a stated measurement method
- [ ] Entry and exit criteria defined for each test level in scope
- [ ] Estimates in section 8 reconcile with the schedule in section 10
- [ ] Every deliverable named in 7.8 has a recipient

### Terminology

- [ ] The document uses "test model", never "test condition". The 2021 edition replaced the concept
- [ ] Incident, incident report, actual results, and test result are used per clause 3

### Identifier Integrity

- [ ] Every identifier unique
- [ ] No identifier renumbered since the last baseline
- [ ] No withdrawn identifier reused

### Output Rules

- [ ] No emojis
- [ ] No em-dashes
- [ ] Diagrams are Mermaid
