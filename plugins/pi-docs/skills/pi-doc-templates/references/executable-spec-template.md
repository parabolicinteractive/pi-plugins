# Executable Specification Template

Executable specs are BDD scenarios written in Gherkin format. They serve as both documentation and automated tests. One `.feature` file per feature area.

## File Convention

- Location: `docs/specs/feature-name.feature`
- Naming: kebab-case matching the feature area.
- These files should also be referenced from (or symlinked into) the test suite so they execute in CI.

## Template

```gherkin
# docs/specs/feature-name.feature

Feature: [Feature Name]
  [One-line description of what this feature does]

  As a [persona from the domain model]
  I want [capability]
  So that [business value]

  Background:
    # Shared setup for all scenarios in this feature.
    # Use sparingly — only for truly universal preconditions.
    Given [common precondition]

  # --- Happy Path ---

  Scenario: [Descriptive name of the success case]
    Given [initial context]
    And [additional context if needed]
    When [action taken]
    Then [expected outcome]
    And [additional expected outcome]

  # --- Edge Cases ---

  Scenario: [Descriptive name of edge case]
    Given [context that creates the edge case]
    When [action taken]
    Then [expected behavior under this condition]

  # --- Error Cases ---

  Scenario: [Descriptive name of error case]
    Given [context]
    When [action that should fail]
    Then [expected error behavior]
    And [system should remain in valid state]

  # --- Data-Driven Scenarios ---

  Scenario Outline: [Parameterized scenario name]
    Given [context with <variable>]
    When [action with <input>]
    Then [expected <result>]

    Examples:
      | variable | input | result |
      | value1   | x     | y      |
      | value2   | a     | b      |
```

## Writing Guidelines

**Scenario names** should describe the behavior, not the implementation: "Player sees updated score after frame" not "WebSocket pushes score update to client."

**Given/When/Then** should use domain language from the [Domain Model](../domain-model.md), not technical jargon: "Given a match is in progress" not "Given the match row has started_at not null."

**One behavior per scenario.** If a scenario has more than 5-6 steps, it's probably testing multiple things. Split it.

**Background** is for universal preconditions only. If only some scenarios need it, move it into those scenarios' Given steps.

**Scenario Outlines** are for testing the same behavior with different data. Don't use them to test fundamentally different behaviors.

## Linking to Design Specs

Add a comment at the top of the feature file linking to the design spec and requirements it verifies:

```gherkin
# Verifies: REQ-042, REQ-043
# Design: docs/designs/scoring.md
# ADR: docs/adrs/005-frame-based-scoring.md
```

## Anti-Patterns to Avoid

- **Testing implementation, not behavior**: Scenarios should describe what the user experiences, not how the code works.
- **Brittle data**: Don't hardcode IDs or timestamps. Use descriptive names and relative time.
- **God scenarios**: A single scenario that tests an entire workflow. Break it up.
- **Missing error cases**: Happy paths are easy. The real value is in documenting what should happen when things go wrong.
