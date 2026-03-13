---
name: pi-domain-model
description: >
  Guide to creating, maintaining, and enforcing consistent terminology
  (ubiquitous language) across code, documentation, and team communication.
  ALWAYS use this skill when the user mentions naming inconsistencies in
  their codebase, wants to define or standardize terms, needs help with
  domain modeling concepts (entities, aggregates, bounded contexts, domain
  events), is confused about what to call something, notices AI agents
  using wrong terminology, wants to create a glossary or shared vocabulary,
  or needs to handle terminology translation between their system and
  external APIs. Also use when the user mentions that different parts of
  their codebase use different names for the same concept, or when team
  members are using inconsistent language. This skill contains specific
  methodology for building domain models, entity definition worksheets,
  bounded context mapping patterns, and anti-drift techniques — do not
  answer domain terminology questions from general knowledge alone.
version: 0.1.0
---

# Domain Model & Ubiquitous Language

Guidance for creating, maintaining, and enforcing a shared vocabulary across all project artifacts. The domain model is the single most impactful document for AI-assisted development — inconsistent terminology is the primary cause of AI agent drift, hallucination, and wasted effort.

## Why This Matters

When code says `game`, the database says `frame`, the UI says `rack`, and the spec says `round` — all meaning the same thing — every participant (human and AI) wastes time translating. The domain model eliminates this by establishing one canonical term per concept.

## Creating a Domain Model

### Step 1: Identify Core Entities

Start with the nouns in your product vision and requirements. Ask:
- What are the "things" in this system?
- What do users create, read, update, or delete?
- What has a lifecycle (states, transitions)?

### Step 2: Define Each Entity

For each entity, capture:

**Name** — The canonical term. This exact word appears in code (class names, table names, API fields), documentation, UI copy, and conversation. Choose clarity over cleverness.

**Definition** — Plain-language description. A non-technical stakeholder should understand it. Avoid circular definitions ("A match is when two teams match up").

**Avoid list** — Synonyms and legacy terms that should NOT be used. This is critical for AI agents — without it, they'll use whatever synonym seems natural.

**Code mapping** — Where this entity lives in code: class name, database table, API resource path.

**Lifecycle** — Valid states and transitions. Use a state diagram if complex.

**Key attributes** — Important properties with types and business constraints.

**Relationships** — How it connects to other entities (has-many, belongs-to, references).

**Business rules** — Constraints, validations, invariants. "A tournament cannot be modified after its first match starts."

### Step 3: Identify Bounded Contexts

Not every part of the system uses the same model. A "user" in the auth context is different from a "player" in the tournament context, even if they're the same person. Map out:

- Where each context's boundary is (which services, modules, or features).
- Which entities are authoritative in which context.
- How contexts communicate (API calls, events, shared database).

### Step 4: Document Domain Events

Events are past-tense facts about state changes: `MatchConcluded`, `PlayerRanked`, `BracketGenerated`. They matter because they're often the triggers for side effects, notifications, and cross-context communication.

### Step 5: Establish Domain Rules

Business rules that span entities. These are the invariants that the system must enforce:
- "A player can only be on one team per tournament."
- "Scores are derived by counting frames, never stored directly."

## Enforcing Consistency

### In Code

- Variable names, class names, table names, and column names use the canonical term from the domain model.
- If a developer (or AI agent) introduces a new term, it gets added to the domain model first — not after the fact.
- Code review should flag terminology drift.

### In Documentation

- All spec documents reference the domain model as the glossary.
- Cross-link terms on first use: `[Frame](../domain-model.md#frame)`.
- The domain model's "Avoid" column helps catch inconsistencies in PR reviews.

### In AI Agent Context

When an AI agent works on the project:

1. **Load the domain model early.** It should be one of the first documents the agent reads, before writing any code or documentation.
2. **Reference it in prompts.** When asking an agent to implement a feature, include: "Use terminology as defined in docs/domain-model.md."
3. **Flag drift immediately.** If an agent introduces a synonym not in the domain model, correct it before it propagates.

### In Communication

- Use canonical terms in meetings, Slack, emails, and issue trackers.
- When a stakeholder uses a different term, gently redirect: "We call that a Frame in our system — it's documented in the domain model."

## Common Patterns

### Aggregate Roots

An aggregate root is the entry point for a cluster of related entities. External code interacts with the cluster only through the root. Example: a Tournament aggregate root owns its Matches, which own their Frames.

### Value Objects

Concepts that are defined by their attributes, not an identity. A score of "7-5" is a value object — there's no "score ID." If all attributes are equal, two value objects are equal.

### Domain Events as Documentation

Domain events double as living documentation of system behavior. The list of events tells you everything meaningful that can happen in the system.

## Evolving the Domain Model

The domain model is a living document. Update it when:

- A new concept enters the system.
- An existing term is discovered to be ambiguous.
- A bounded context boundary shifts.
- A business rule changes.

Track changes in the document's change log. When a term changes, update all code and documentation in the same PR to prevent drift.

## Anti-Patterns

- **Synonym tolerance**: Allowing multiple terms for the same concept "because everyone knows what we mean." They don't — and AI agents definitely don't.
- **Technical-only model**: A domain model that only covers database tables. It should cover the full business domain, including concepts that don't have a direct code representation yet.
- **Stale model**: A domain model that was written once and never updated. If the code has diverged from the model, the model is wrong — fix it.
- **Over-modeling**: Defining 200 terms on day one. Start with the core 15-20 entities and grow organically.

For the detailed domain model template, read `references/domain-model-guide.md` in this skill, or use the template from the `pi-doc-templates` skill at `references/domain-model-template.md`.
