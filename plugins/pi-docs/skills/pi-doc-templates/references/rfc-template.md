# Request for Comments (RFC) Template

RFCs are longer explorations of complex design problems. They precede ADRs — write an RFC when the decision isn't obvious and needs broader input. Once consensus is reached, the RFC leads to one or more ADRs.

## Numbering Convention

Same as ADRs: zero-padded three-digit prefixes. `001-realtime-architecture.md`.

## Template

```markdown
---
title: "RFC-NNN: [Title]"
status: draft | review | accepted | rejected | withdrawn
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
reviewers: [names]
tags: [relevant, categories]
decision-adrs: [adrs/NNN-title.md]  # populated after decision is made
links:
  - type: satisfies
    target: path/to/requirement
---

# RFC-NNN: [Title]

## Motivation

What problem are we solving? Why does it need to be solved now? What happens if we don't solve it? Quantify the impact where possible.

## Proposal

Detailed description of the proposed solution. Enough detail for reviewers to evaluate feasibility and trade-offs, but not so much that it becomes an implementation guide (that's what design specs are for).

## Detailed Design

### [Aspect 1]

Technical details for the first major aspect of the proposal. Diagrams, state machines, data models, algorithm sketches as appropriate.

### [Aspect 2]

Technical details for the second major aspect.

<!-- Add subsections as needed for the complexity of the proposal. -->

## Drawbacks & Trade-Offs

Honest assessment of what this approach costs. What does it make harder? What technical debt does it introduce? Where might it not scale?

## Alternatives

### [Alternative A]

Full description of the alternative approach. Why the proposal is preferred over this alternative. Be specific — "simpler but doesn't handle edge case X" is better than "not as good."

### [Alternative B]

Same treatment for each alternative.

## Unresolved Questions

Numbered list of open questions that need input from reviewers. These should be specific enough to answer, not vague concerns.

1. Should we support [specific capability]? What are the implications?
2. How do we handle [specific edge case]?

## Implementation Plan

If accepted, high-level plan for how this gets built:
- Phase 1: ...
- Phase 2: ...
- Estimated effort: ...

## References

- Related RFCs: [RFC-NNN](NNN-title.md)
- Requirements: [REQ-NNN](../requirements.md#req-nnn)
- External references: technical papers, prior art, competitor analysis.
```

## RFC Lifecycle

1. **Draft**: Author writes the RFC. Status: `draft`.
2. **Review**: RFC circulated for feedback. Status: `review`. Reviewers listed in frontmatter.
3. **Decision**: After discussion, one of:
   - **Accepted**: Proposal adopted. Create ADR(s). Status: `accepted`. Link to ADRs in `decision-adrs`.
   - **Rejected**: Proposal declined. Document why. Status: `rejected`.
   - **Withdrawn**: Author withdraws. Status: `withdrawn`.
