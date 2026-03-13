# Architecture Decision Record Template

ADRs are the most frequently created document type. Keep them short (under one page). One decision per ADR. Never delete an ADR — supersede it.

## Numbering Convention

Files are numbered sequentially with zero-padded three-digit prefixes: `001-use-postgres.md`, `002-adopt-svelte.md`, etc. The number is assigned at creation time and never changes, even if the ADR is superseded.

## Template

```markdown
---
title: "ADR-NNN: [Decision Title]"
status: proposed | accepted | deprecated | superseded
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
tags: [relevant, categories]
superseded-by: adrs/NNN-new-decision.md  # only if superseded
links:
  - type: implements
    target: path/to/related/requirement/or/design
---

# ADR-NNN: [Decision Title]

## Context

What is the situation that requires a decision? What forces are at play (technical constraints, business requirements, team capabilities, timeline pressure)? Be specific about what triggered this decision.

## Decision

State the decision clearly and specifically. Use active voice: "We will use X" not "X was chosen." Be precise enough that someone (or an AI agent) could implement this without further clarification.

## Consequences

### Positive

What improves as a result of this decision.

### Negative

What trade-offs are accepted. What becomes harder or more expensive.

### Risks

What could go wrong. Under what conditions would this decision need to be revisited.

## Alternatives Considered

### [Alternative A]

Brief description. Why it was rejected.

### [Alternative B]

Brief description. Why it was rejected.

<!-- This section is critical for AI agents. Without it, agents will suggest
     alternatives you've already evaluated and rejected. Be explicit about
     WHY each alternative was rejected. -->

## References

- Related ADRs: [ADR-NNN](NNN-title.md)
- RFC (if applicable): [RFC-NNN](../rfcs/NNN-title.md)
- PR/commit implementing this: [link]
- External references: [link]
```

## Writing Tips

- **Context**: Describe the situation, not the solution. A reader should understand the problem before seeing the decision.
- **Decision**: Be specific. "Use PostgreSQL 16 with pgvector extension for vector search" not "Use a relational database."
- **Alternatives Considered**: This is the most valuable section for AI agents. Document at least two alternatives and the specific reasons they were rejected. "We considered MongoDB but rejected it because our data is heavily relational and we need ACID transactions across multiple tables."
- **Keep it short**: If the ADR is longer than one page, the decision might need to be split into multiple ADRs, or the exploration belongs in an RFC.
