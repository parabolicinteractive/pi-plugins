---
name: pi-spec-docs
description: >
  Creates and maintains formal software specification documents conforming to
  the ISO/IEC/IEEE harmonized standards family: SRS (29148), SAD and ADRs
  (42010), SDD (1016), and STP (29119-3). ALWAYS use this skill when the user
  wants to write, revise, or check a software requirements specification,
  software architecture document, software design description, software test
  plan, or architecture decision record. Also use when the user wants to
  analyze source material (client documents, RFPs, meeting notes, transcripts,
  an existing codebase) into requirements, when they ask what a spec document
  should contain, when they need to know whether their spec documents still
  agree with each other after a change, or when they need to report scope that
  arrived after a baseline was agreed. Trigger on mentions of SRS, SAD, SDD,
  STP, ADR, requirements specification, architecture document, design
  description, test plan, requirements traceability, or specification
  conformance.
version: 1.0.0
---

# Specification Documents

Produces specification documents that conform to a single standards family, trace
to each other by immutable identifier, and never silently lose intent from their
source material.

## Standards Family

Every document produced by this skill belongs to one interoperating family. Do not
introduce structures from outside it. If a needed structure appears to be missing,
say so rather than borrowing one from another standard.

| Artifact | Standard | Status |
|---|---|---|
| Source analysis, SRS | ISO/IEC/IEEE 29148:2018 | Implemented |
| SAD, ADR | ISO/IEC/IEEE 42010:2022 | Implemented, conformance not claimed |
| STP | ISO/IEC/IEEE 29119-3:2021 | Implemented |
| SDD | IEEE 1016-2009 | Not yet implemented |
| Baselines, change management | ISO/IEC/IEEE 12207:2017 | Referenced by 29148 6.6 |
| Information item content | ISO/IEC/IEEE 15289:2019 | Umbrella |

29148:2018 was revised specifically to harmonize with 15288 and 12207, and its
scope names 15289 conformance as an intended use. That is why these documents
interoperate rather than merely coexist.

For any document type marked "Not yet implemented", tell the user it is pending
rather than improvising a template.

## Directory Layout

```
docs/
  srs.md                                  Software Requirements Specification
  sad.md                                  Software Architecture Document
  sdd.md                                  Software Design Description
  stp.md                                  Software Test Plan
  adr/
    0001-<slug>.md                        Architecture Decision Records
  analysis/
    0001-<date>-<slug>.md                 Analysis reports
  changes/
    CR-001-<slug>.md                      Change Requests
  sources/                                Optional. Raw source material.
```

`docs/` sits at the project root. Find the root by looking for `.git`, then for
common markers (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`).
Create `docs/` and its subdirectories only when a document needs them.

## Identifier Scheme

Identifiers are the load-bearing structure of this system. Everything else is
prose around them.

| Prefix | Meaning | Assigned by |
|---|---|---|
| `S-NNN` | Source statement | Source analysis |
| `F-NNN` | Finding | Source analysis, realign |
| `FR-NNN` | Functional requirement | SRS |
| `FR-<AREA>-NNN` | Functional requirement, namespaced | SRS |
| `NFR-<CAT>-NNN` | Non-functional requirement | SRS |
| `CON-NNN` | Constraint | SRS |
| `ASM-NNN` | Assumption | SRS |
| `DEP-NNN` | Dependency | SRS |
| `CR-NNN` | Change request | Realign |
| `STK-NNN` | Stakeholder | SAD |
| `PER-NNN` | Stakeholder perspective | SAD |
| `CNC-NNN` | Concern | SAD |
| `ASP-NNN` | Architecture aspect | SAD |
| `VP-NNN` | Architecture viewpoint | SAD |
| `VW-NNN` | Architecture view | SAD |
| `MK-NNN` | Model kind | SAD |
| `COR-NNN` | Correspondence | SAD |
| `INC-NNN` | Known inconsistency | SAD |
| `ADR-NNNN` | Architecture decision record | ADR |
| `RSK-NNN` | Risk register entry | STP |
| `TM-NNN` | Test model | STP |
| `TCI-NNN` | Test coverage item | STP |
| `TC-NNN` | Test case | STP |

`<CAT>` for non-functional requirements is one of `USE`, `PERF`, `DB`, `SEC`,
`REL`, `MAINT`, `PORT`, matching the 29148 9.6 content elements they derive from.

### Identifier Rules

These are absolute. Violating them silently breaks every downstream citation.

1. **Never renumber.** Once assigned, an identifier is permanent.
2. **Never reuse.** A withdrawn `FR-012` does not free the number. The next
   requirement takes the next unused number.
3. **Never delete a withdrawn item.** Set its status to `Withdrawn` and leave it
   in place. Downstream documents, test cases, and code comments cite it.
4. **Numbering is per-prefix and project-global.** `F-042` means one thing across
   every analysis report in the project.
5. **Before assigning a new identifier, scan for the highest existing one** of
   that prefix across all of `docs/`. Do not assume the next number.

## Frontmatter

Every document produced by this skill carries YAML frontmatter.

```yaml
---
title: string
type: srs | sad | sdd | stp | adr | analysis | change-request
status: draft | review | baselined | superseded | withdrawn
version: string
baseline: string or null
conformance: full | tailored | none
standard: string
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [string]
---
```

`baseline` is null until the document is first approved. After that it names the
baseline the current version belongs to. A baseline is a formally approved,
fixed version of a configuration item (29148 3.1.3).

## Conformance Vocabulary

Report conformance using each standard's own terms, not invented verdicts. **The
vocabulary differs per standard.** Do not apply one standard's rules to another.

### Tailoring permitted: analysis reports, SRS (29148), STP (29119-3)

29148 clause 4 with Annex C, and 29119-3 clause 4.1, both make tailoring a
normative, deliberate act.

- **Full conformance** - all required content of the information item is present.
- **Tailored conformance** - content is omitted, and each omission is declared
  with a reason in the document's tailoring record.
- **Non-conformant** - required content is missing and undeclared.

For the STP there is an added rule. The obligation level of each information item
lives in 29119-3 Annex A, which is unverified. Every content element is therefore
included by default, and **every omission is recorded as tailoring** rather than
assumed permissible. See `references/stp.md`.

### Tailoring prohibited: SAD, ADR (42010)

42010 clause 4 states that tailoring is **neither required nor permitted** when a
conformance claim is made. Conformance is binary. There is no tailored option and
no tailoring record.

- **Conformant** - the specification meets the requirements of Clause 6.
- **Not claimed** - no conformance claim is made.

The normative text of 42010 Clause 6 has not been verified against the published
standard. Until it is, every generated SAD and ADR declares `not-claimed` and
states that it is structured according to Clause 6. Never assert 42010
conformance on inferred requirements. See `references/sad.md`.

### TBDs are not omissions

An unanswered question is not missing content. Mark it `TBD` (29148 3.2) and
register it with an owner. A document full of declared TBDs conforms. A document
missing sections nobody noticed does not.

## Traceability

Traceability runs in two directions (29148 3.1.23). Both must hold.

**Upward, derivation.** Every requirement names the source statement or parent
requirement it came from. A requirement with no upward trace is unjustified.

**Downward, allocation.** Every requirement is allocated to something that
satisfies it and something that verifies it. A requirement with no downward trace
is unimplemented or untested.

```
sources -> S-NNN -> FR-NNN / NFR-NNN -> VW-NNN view -> SDD module
                         |                    |             |
                         |               ADR-NNNN           |
                         +---------> STP test case <--------+
```

Inside the SAD, traceability is expressed as 42010 **correspondences**
(`COR-NNN`), which are named relations between AD elements. An AD can itself be
an AD element in another AD (42010 3.11 note 1), so the link from the SAD to the
SRS and to the SDD is a correspondence, not an ad hoc reference. Correspondence
methods record whether each rule holds or list its violations.

Findings that do not become requirements are still traced. `S-NNN` may resolve to
a requirement, a finding, an out-of-scope declaration, or a context
classification, but never to nothing.

## Document Lifecycle

**Revise in place:** SRS, SAD, SDD, STP. These are baselined configuration items.
Changes bump the version and append to the revision history. The file path never
changes. Prior versions live in git history, not as parallel files.

**Supersede, never edit:** ADRs. An accepted decision record is immutable. When a
decision changes, write a new ADR and set the old one to `superseded`. The record
of a reversed decision retains its value.

## Source Coverage

No instruction or intent from source material may be lost. This is not a quality
goal, it is an invariant with a mechanical check.

Every atomic statement extracted from source material receives an `S-NNN`
identifier and exactly one disposition:

| Disposition | Meaning |
|---|---|
| `requirement` | Became `FR-NNN`, `NFR-<CAT>-NNN`, `CON-NNN`, `ASM-NNN`, or `DEP-NNN` |
| `finding` | Became `F-NNN`. Ambiguous, contradictory, unverifiable, or needs client input |
| `out-of-scope` | Deliberately excluded, with a recorded reason |
| `context` | Background or narrative. Not a requirement and not actionable |

The count of `S-NNN` entries with no disposition must be zero. If a statement
cannot be dispositioned, it becomes a finding. There is no fifth option and no
silent drop.

## Validation Protocol

After generating or revising a document from source material, run three
validators as subagents. They are independent lenses, not redundant reviewers.

**1. Coverage.** Give this agent the *source material only*. Do not give it the
statement register. It independently extracts its own list of atomic statements,
then compares against the register. Anything it finds that has no `S-NNN` is an
escape. Withholding the register is the entire point: an agent shown the register
first inherits its blind spots and confirms them.

**2. Fidelity.** Give this agent the statement register and the generated
document. For each `S-NNN` to requirement mapping, it confirms the requirement
states what the source stated. It reports drift (meaning changed), invention
(requirement asserts what no source said), and over-reach (a hedge became a
mandate).

**3. Conformance.** Give this agent the generated document and the relevant
reference file. It checks required content elements are present and that each
requirement meets the individual characteristics (29148 5.2.5) and the set meets
the set characteristics (29148 5.2.6).

Each validator returns findings in the standard finding format. Merge them into
the analysis report as new `F-NNN` entries. Report the counts to the user. Never
report a document as complete while coverage findings are open.

## Output Rules

All generated document content, including filenames, headings, body text,
frontmatter values, and cross-references:

- **No emojis.** Plain text only.
- **No em-dashes.** Use hyphens.
- Diagrams use Mermaid fenced blocks.
- Requirement statements use `shall` for mandatory, `should` for recommended,
  `may` for optional (29148 5.2.7 requirement language criteria).
- One requirement per statement. Never combine.
- No implementation detail in requirements. That belongs in the SDD.

## Reference Files

Read the reference file for the document type in question before writing
anything. Each contains the template and its validation checklist.

| Document | Reference |
|---|---|
| Analysis reports, source statement register, findings | `references/analysis.md` |
| Software Requirements Specification | `references/srs.md` |
| Software Architecture Document | `references/sad.md` |
| Architecture Decision Record | `references/adr.md` |
| Software Test Plan | `references/stp.md` |
| Software Design Description | Pending |
