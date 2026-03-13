# Domain Model — Extended Guide

## Entity Definition Worksheet

Use this worksheet when defining a new entity for the domain model. Copy this block into the domain model document and fill it in.

```markdown
### [Entity Name]

- **Definition**: [Plain-language description]
- **Avoid**: [Do not call it: synonym1, synonym2, legacy-term]
- **Code mapping**:
  - Class/Type: `EntityName`
  - Database table: `entity_names`
  - API resource: `/entity-names`
- **Lifecycle states**: [State1] → [State2] → [State3]
- **Key attributes**:
  - `attribute_name` (type) — Description. Constraints.
- **Relationships**:
  - Has many [OtherEntity] (one-to-many)
  - Belongs to [ParentEntity] (many-to-one)
- **Business rules**:
  - [Rule description. What the system enforces.]
- **Domain events**:
  - `EntityCreated` — When [trigger].
  - `EntityUpdated` — When [trigger].
- **Notes**: [Edge cases, historical context, common misconceptions]
```

## Bounded Context Mapping Patterns

### Shared Kernel

Two contexts share a subset of the domain model. Changes require coordination.

Use when: Two closely collaborating teams need identical definitions for a few key entities.

Risk: Tight coupling between contexts. Changes in the kernel affect both.

### Customer-Supplier

One context (supplier) provides data to another (customer). The supplier defines the interface.

Use when: One context clearly depends on another's data, but they evolve independently.

### Anticorruption Layer

A translation layer that converts one context's model into another's terms.

Use when: Integrating with external systems or legacy code whose model doesn't match yours.

### Published Language

A shared, documented format for exchanging data between contexts (like an API schema or event format).

Use when: Multiple contexts need to consume the same data independently.

## Domain Event Catalog Template

```markdown
## Domain Events

### [Context Name] Events

#### [EventName]

- **Trigger**: What causes this event.
- **Producer**: Which component/service emits it.
- **Consumers**: Which components/services react to it.
- **Payload**:
  ```json
  {
    "event_type": "event.name",
    "timestamp": "ISO-8601",
    "data": {
      "entity_id": "uuid",
      "relevant_field": "value"
    }
  }
  ```
- **Ordering guarantees**: Are events for the same entity ordered?
- **Idempotency**: Can consumers safely process the same event twice?
- **Related events**: Events that typically precede or follow this one.
```

## Terminology Audit Checklist

Run this audit periodically (or when onboarding AI agents) to catch drift:

1. **Code vs. Domain Model**: Grep the codebase for terms in the "Avoid" column. Any hits are inconsistencies to fix.
2. **Database vs. Domain Model**: Compare table and column names against canonical terms.
3. **API vs. Domain Model**: Check API resource names and field names.
4. **UI vs. Domain Model**: Check user-facing labels, error messages, and help text.
5. **Documentation vs. Domain Model**: Search spec docs for avoided synonyms.
6. **Tests vs. Domain Model**: Test descriptions and fixture names should use canonical terms.

## Working with AI Agents

### Prompt Pattern for Domain Awareness

When giving an AI agent a task, include:

```
Before implementing, read docs/domain-model.md and use the exact terminology
defined there. Key terms for this task: [Term1], [Term2], [Term3].
Do not introduce synonyms. If you need a new term, propose it for the
domain model before using it in code.
```

### Detecting Agent Drift

Signs that an AI agent is drifting from the domain model:
- Introduces variable names not in the glossary.
- Uses a synonym from the "Avoid" column.
- Creates a new concept without adding it to the domain model.
- Describes behavior using different terms than the executable specs.

Correction: point the agent back to the domain model. Don't just fix the symptom — have the agent re-read the model and acknowledge the correct terminology.
