# Project Glossary

Path: `docs/glossary.md`

One canonical definition per term, for the whole document set.

## Standing of This Document

**This is a project convention, not a standard-defined information item.** Unlike
the other six document types, no ISO or IEEE standard specifies a project
glossary as an artifact. Do not cite one as though it did.

It has a defensible justification rather than a citation. 29148 5.2.6 names
`consistent` as a characteristic of a requirement set and defines it to include
uniform terminology across the set. A set of documents that use one word for
three things is not consistent. This file is how that characteristic is made
checkable.

Every document type already has a definitions slot: 29148 9.2.3 definitions and
9.2.5 acronyms, 29119-3 5.2.9 glossary, IEEE 1016 clause 2. Those slots stay.
What changes is that they carry only terms specific to that document, and defer
to this file for anything shared. Six local glossaries with no reconciliation is
the drift problem, not the solution to it.

## No Identifiers

Terms are keyed by the term itself. There is no `GL-NNN` prefix, because a
numeric identifier for a word adds a layer of indirection that buys nothing:
findings cite the term, documents use the term, and the term is already unique.

This is the one place in the package where the identifier discipline does not
apply, and it is deliberate.

Meaning changes are handled by the superseded list in section 4, not by
versioned identifiers.

## What Belongs Here

A term belongs in the glossary when **any** of these hold:

- It appears in more than one document
- It names something in the problem domain rather than the solution
- A stakeholder and an engineer would plausibly mean different things by it
- It is a word in common English that the project uses in a narrower sense
- An external system uses a different word for the same thing

A term does not belong here when it is understood identically by everyone, is
standard industry vocabulary used in its ordinary sense, or appears in exactly
one document and is defined there.

Resist completeness. A glossary containing every noun in the project is one
nobody reads, and an unread glossary enforces nothing.

## Template

```markdown
---
title: "<Project Name> - Glossary"
type: glossary
status: draft
version: "0.1"
baseline: null
conformance: not-applicable
standard: "Project convention. See 29148 5.2.6 consistent terminology."
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
---

# Glossary

The canonical vocabulary for this project. Where a document's own definitions
section and this file disagree, this file wins.

## 1. Terms

### Customer

An organization that holds a contract with us. Distinct from a **User**, who is
a person who signs in.

| Field | Value |
|---|---|
| Kind | Entity |
| Source | S-004 (Kickoff transcript 2026-07-14) |
| Also known as | Account (in the client's Salesforce) |
| Do not use | Client, in the sense of an organization. Reserved for software that calls an API |
| Used in | strs.md, srs.md, sad.md, sdd.md |

### User

A person who signs in. A User belongs to exactly one **Customer**.

| Field | Value |
|---|---|
| Kind | Role |
| Source | S-001 (RFP 3.2) |
| Also known as | - |
| Do not use | End user, when User is meant. Reserve for the person who consumes output without signing in |
| Used in | strs.md, srs.md, stp.md |

<!-- Repeat. Alphabetical within kind, or alphabetical throughout. Pick one and
     say which in section 5. -->

## 2. Rejected Terms

Words the project deliberately does not use, and what to use instead. This
section stops rejected vocabulary from creeping back in through a new document or
a new contributor.

| Rejected term | Use instead | Reason |
|---|---|---|
| Client | Customer, or API client | Ambiguous between the organization and calling software |
| Tenant | Customer | Client-facing documents should use the client's own word |

## 3. External Terminology

Where an external system, API, or client organization uses a different word for
the same concept. Record the mapping so nobody "corrects" the boundary code to
match our vocabulary, or our documents to match theirs.

| Our term | External term | System | Notes |
|---|---|---|---|
| Customer | Account | Client Salesforce | Field mapping in sdd.md 4.3 |

## 4. Superseded Terms

When a term's meaning changes, record it here rather than editing history. A
document written before the change may still use the old sense.

| Term | Old meaning | New meaning | Changed in version | CR |
|---|---|---|---|---|

## 5. Conventions

State the ordering rule used in section 1, and any project rules about
capitalization of glossary terms in prose.
```

## Terminology Findings

`/pi-docs-realign` raises `Terminology` findings. Four kinds:

| Finding | Meaning | Default severity |
|---|---|---|
| Conflicting definition | A term is defined differently in two documents, or a document's local definition contradicts the glossary | High |
| Undefined term | A domain term is used in a document and defined nowhere | Medium |
| Rejected term in use | A term from section 2 appears in a document | Medium |
| Orphan term | The glossary defines a term no document uses | Low |

Orphan terms are low severity on purpose. A term may be defined ahead of the
document that will use it. Repeated orphans across several runs suggest the
glossary is growing faster than the work, which is worth mentioning but is not a
defect.

Detecting undefined terms is inherently imprecise. Prefer reporting a candidate
list the user can dismiss over silently missing drift. `Accepted` dispositions
persist, so a term dismissed once is not raised again.

## Validation Checklist

### Content

- [ ] Every term has a definition that does not use the term being defined
- [ ] Every term states its kind
- [ ] Every term cites a source, or is marked as project-coined
- [ ] Terms that are easily confused with each other say so explicitly
- [ ] Rejected terms name what to use instead and why
- [ ] External terminology mappings name the system

### Consistency With the Document Set

- [ ] No document defines a glossary term differently in its own definitions section
- [ ] No document uses a rejected term
- [ ] Every term marked `Used in` actually appears in those documents
- [ ] Terms used across two or more documents appear here rather than only locally

### Discipline

- [ ] The glossary is not a dump of every noun in the project
- [ ] Superseded meanings are recorded rather than overwritten
- [ ] The ordering convention in section 5 matches the actual order

### Output Rules

- [ ] No emojis
- [ ] No em-dashes
