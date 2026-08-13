# pi-docs

Formal software specification documents that conform to one interoperating
standards family, trace to each other by immutable identifier, and never silently
lose intent from their source material.

## What It Does

Produces five document types plus the analysis artifacts that connect them:

| Document | Standard | Status |
|---|---|---|
| Software Requirements Specification (SRS) | ISO/IEC/IEEE 29148:2018 | Available, full conformance |
| Software Architecture Document (SAD) | ISO/IEC/IEEE 42010:2022 | Available, conformance not claimed |
| Architecture Decision Record (ADR) | ISO/IEC/IEEE 42010:2022 | Available, conformance not claimed |
| Software Test Plan (STP) | ISO/IEC/IEEE 29119-3:2021 | Available, full conformance |
| Software Design Description (SDD) | IEEE 1016-2009 | Available, conformance not claimed |

**On "conformance not claimed" for 42010.** Its clause 4 states that tailoring is
neither required nor permitted, so conformance is binary: a document either meets
Clause 6 or makes no claim. The clause structure, definitions, conformance rules
and conceptual model were verified against the ISO publisher preview, but the
normative text of Clause 6 begins at standard page 19, past where every free
preview stops. The individual requirements were recovered from the standard
editor's own CC-licensed 2011 template. That is good enough to structure a
document and not good enough to claim conformance, so generated SADs and ADRs say
"structured according to Clause 6" and assert nothing further. Purchasing
42010:2022 upgrades this to a real conformance claim.

## Why One Standards Family

The classic IEEE document standards (830, 1471, 829) are widely recognized but
were written independently and never designed to interoperate. Every link between
them has to be invented by whoever uses them, and all three are withdrawn.

The ISO/IEC/IEEE harmonized set was built to fit together. 29148:2018 states in
its foreword that it was revised specifically to harmonize with 15288 and 12207,
and its scope names 15289 conformance as an intended use. Concretely:

- 29148 clause 9.6.19 requires a Verification section that mirrors the
  requirements section. That is the interface to the test plan, defined by the
  standard rather than bolted on.
- 29148 clause 6.5.1 covers requirements activities in architecture definition.
  That is the interface to the architecture document.
- 29148 clause 6.6.2 covers change management. That is the basis for impact
  analysis when a baseline moves.

Traceability across documents is the reason this package exists. The harmonized
family supports it natively; the classic suite does not.

One caveat, stated plainly: IEEE 1016-2009 is an IEEE standard rather than a
joint ISO/IEC/IEEE publication like the others. It is the recognized design
description standard and belongs in the set, but the set is not perfectly uniform.

## Commands

### `/pi-docs <type> [name]`

Create or revise a specification document. Types: `srs`, `sad`, `sdd`, `stp`,
`adr`.

Create and revise are the same command. If the document exists it is revised in
place, the version is bumped, and a revision history row is appended. The file
path never changes; prior versions live in git history.

### `/pi-docs-analyze-sources [path]`

Analyze source material into a source statement register and a findings list.

Run this before writing an SRS. Sources may be client documents, RFPs, meeting
transcripts, or an existing codebase.

### `/pi-docs-realign [baseline]`

Check the whole document set for conformance, consistency, and currency. Reports
findings, then walks each one to a disposition.

## Source Coverage

The central guarantee: no instruction or intent from source material is lost.

This is enforced mechanically rather than hoped for. Every atomic statement
extracted from the sources receives an `S-NNN` identifier and exactly one
disposition:

| Disposition | Meaning |
|---|---|
| `requirement` | Became a requirement, named explicitly |
| `finding` | Ambiguous, contradictory, unverifiable, or needs client input |
| `out-of-scope` | Deliberately excluded, with a recorded reason |
| `context` | Background carrying no instruction |

The count of undispositioned statements must be zero. A statement that cannot be
dispositioned becomes a finding. There is no silent drop, because a gap in the
register is countable.

Three validators run after generation as independent subagents:

1. **Coverage** reads the sources without seeing the register, extracts its own
   statement list, and reports anything the register missed. It is denied the
   register deliberately, because an agent shown the register first confirms its
   blind spots rather than finding them.
2. **Fidelity** confirms each requirement states what its source stated. It
   catches drift, invention, and hedges promoted to mandates.
3. **Conformance** checks required content and requirement characteristics.

## Identifiers

Identifiers are the load-bearing structure. Everything else is prose around them.

| Prefix | Meaning |
|---|---|
| `S-NNN` | Source statement |
| `F-NNN` | Finding |
| `FR-<AREA>-NNN` | Functional requirement |
| `NFR-<CAT>-NNN` | Non-functional requirement |
| `CON-NNN` | Constraint |
| `ASM-NNN` / `DEP-NNN` | Assumption / dependency |
| `CR-NNN` | Change request |
| `STK-NNN` / `PER-NNN` | Stakeholder / stakeholder perspective |
| `CNC-NNN` / `ASP-NNN` | Concern / architecture aspect |
| `VP-NNN` / `VW-NNN` / `MK-NNN` | Viewpoint / view / model kind |
| `COR-NNN` / `INC-NNN` | Correspondence / known inconsistency |
| `ADR-NNNN` | Architecture decision record |
| `RSK-NNN` | Risk register entry |
| `TM-NNN` / `TCI-NNN` / `TC-NNN` | Test model / test coverage item / test case |
| `DSTK-NNN` / `DCN-NNN` | Design stakeholder / design concern |
| `DVP-NNN` / `DVW-NNN` | Design viewpoint / design view |
| `DE-NNN` / `DR-NNN` / `DCON-NNN` | Design entity / relationship / constraint |

Three absolute rules:

1. Never renumber.
2. Never reuse a withdrawn identifier.
3. Never delete a withdrawn item. Mark it `Withdrawn` and leave it in place.

Downstream documents, test cases, and code comments cite these identifiers. A
reused number turns every one of those citations into a silent lie.

## Document Lifecycle

**Revised in place:** SRS, SAD, SDD, STP. These are baselined configuration items
(29148 3.1.3). Changes bump the version and append to the revision history.

**Superseded, never edited:** ADRs. An accepted decision record is immutable. When
a decision changes, a new ADR supersedes the old one. The record of a reversed
decision keeps its value.

## Conformance Reporting

Conformance uses the standard's own vocabulary (29148 clause 4):

- **Full conformance** - all required content present.
- **Tailored conformance** - content omitted, each omission declared with a
  reason. Tailoring is a normative, deliberate act (29148 Annex C).
- **Non-conformant** - required content missing and undeclared.

An unanswered question is not an omission. It is a `TBD` (29148 3.2) with a
register entry naming an owner. A document full of declared TBDs conforms. A
document with gaps nobody noticed does not.

## Layout

```
docs/
  srs.md
  sad.md
  sdd.md
  stp.md
  adr/
    0001-<slug>.md
  analysis/
    0001-<date>-<slug>.md
  changes/
    CR-001-<slug>.md
  sources/
```

Analysis reports are the finding register. There is no separate register file;
`grep -rn "Status: Open" docs/analysis/` is the query.

Change requests carry the baseline they arrived after, which makes scope
expansion reportable: everything with `baseline: 1.0` came in after the
specification was agreed.

## Standards Referenced

| Standard | Title | Role |
|---|---|---|
| ISO/IEC/IEEE 29148:2018 | Systems and software engineering, Life cycle processes, Requirements engineering | SRS content and requirement characteristics |
| ISO/IEC/IEEE 42010:2022 | Software, systems and enterprise, Architecture description | SAD and ADR |
| IEEE 1016-2009 | Information technology, Systems design, Software design descriptions | SDD |
| ISO/IEC/IEEE 29119-3:2021 | Software and systems engineering, Software testing, Test documentation | STP |
| ISO/IEC/IEEE 12207:2017 | Systems and software engineering, Software life cycle processes | Baselines and change management |
| ISO/IEC/IEEE 15289:2019 | Systems and software engineering, Content of life-cycle information items | Information item content |

Verification status of the citations used:

| Standard | Verified | Not verified |
|---|---|---|
| 29148:2018 | Clause structure, all clause 3 definitions, clause 4 conformance, 5.2.5 to 5.2.8, 6.5.x, 6.6.2, 9.2.x, 9.6.1 to 9.6.20, Annex C | The example SRS outline at 8.5.2 |
| 29119-3:2021 | Clause structure, every content element of clauses 5 to 8, all 26 clause 3 definitions, clause 1 scope, Figure 1 document hierarchy | Clause 4 normative text, and the Annex A obligation levels |
| 42010:2022 | Clause structure, all 19 definitions, clause 4 conformance, 5.1 to 5.2.3, core ontology and cardinalities | Normative text of clauses 6, 7 and 8 |
| 1016-2009 | Nothing first-party. Clause structure, clause 4 required contents, the twelve viewpoints and the design element model are corroborated across three independent secondary sources | Everything else, including whether the standard defines conformance rules at all |

Where a citation is unverified, the affected template says so in place rather
than asserting a conformance claim it cannot support.

**1016-2009 is the weak link.** IEEE is not ISO, so there is no publisher sample
in the format that worked for the three ISO standards; Accuris and ANSI refused
the request and IEEE Xplore blocked it. Its template was built from secondary
sources and says so. If you buy one standard, buy this one.

## Attribution

Requirement content for the SAD and ADR templates was recovered from the
architecture description template and architecture viewpoint template for use
with ISO/IEC/IEEE 42010:2011 by Rich Hilliard, licensed under Creative Commons
Attribution 3.0 Unported. http://www.iso-architecture.org/42010/templates/

## Skill

`pi-spec-docs` holds the shared conventions: identifier scheme, frontmatter,
traceability rules, lifecycle, conformance vocabulary, and the validation
protocol. Per-document templates and checklists live in its `references/`
directory, one file per document type, each containing the template and its
checklist and nothing else.
