---
description: Brainstorm improvements, scope advice, and ideas for a spec doc
allowed-tools: Read, Glob, Grep, Bash(ls:*,find:*,wc:*,git:*), AskUserQuestion
argument-hint: <doc-path>
---

Analyze a specification document in the context of the full project — codebase, other spec docs, vision, domain model, ADRs, and architecture — then provide strategic suggestions.

## Step 1: Gather Context

Before doing anything else, use AskUserQuestion to ask two questions:

**Question 1 — Thinking Mode:**
"What kind of thinking would be most helpful right now?"

Options:
- **Expand** — "What's missing? What features, edge cases, or opportunities haven't been considered?"
- **Reduce** — "What should we cut, defer, or move out of MVP? Where are we over-building?"
- **Refine** — "How do we make what's already here better? Sharpen the design, improve UX, strengthen the spec."
- **All three** — "Give me the full picture — expand, reduce, and refine."

**Question 2 — Save results:**
"Want me to save the brainstorm as a review document in docs/reviews/?"

Options:
- **Yes** — "Save it so I can reference it later."
- **No** — "Just conversation, don't save anything."

## Step 2: Read the Target Document

Read the document at $1 (the path provided as an argument). If no argument was provided, ask the user which document to brainstorm on.

Understand:
- What feature or system this document describes.
- Its current status (draft, review, accepted).
- What requirements it satisfies (check links in frontmatter).
- What decisions it references (ADR links).

## Step 3: Build Project Context

Read as much surrounding context as is available. Not all of these will exist — use what's there:

1. **Vision doc** (`docs/vision.md`) — success metrics, roadmap phases, target users, north star metric.
2. **Domain model** (`docs/domain-model.md`) — canonical terms, bounded contexts, domain rules.
3. **Requirements** (`docs/requirements.md`) — related requirements, their priority (Must/Should/Could/Won't).
4. **Architecture** (`docs/architecture.md`) — technical constraints, component boundaries, tech stack.
5. **ADRs** (`docs/adrs/`) — scan titles and read any that are referenced by or related to the target doc.
6. **Other design specs** (`docs/designs/`) — scan for related features to understand scope and dependencies.
7. **Codebase** — use Glob and Grep to understand the current implementation state. How much of this feature exists already? What's the code complexity? What would change?

## Step 4: Analyze and Generate Suggestions

Based on the thinking mode selected:

### If Expand:
- What features, capabilities, or edge cases are missing from this spec?
- What would make this feature delightful rather than merely functional?
- Are there adjacent features that would multiply the value of this one?
- What do competitors do here that's worth considering?
- Are there non-obvious user needs the spec doesn't address?
- What integrations or extensibility points would future-proof this?

### If Reduce:
- Which parts of this spec don't serve the north star metric from the vision doc?
- What's a Phase 2 or Phase 3 feature hiding inside the MVP scope?
- Where is the spec over-engineering — building for scale or edge cases that don't matter yet?
- What could be replaced with a simpler manual process for now?
- Are there features that sound important but won't be used in the first 6 months?
- What's the 80/20 cut — which 20% of this spec delivers 80% of the value?
- Does this spec introduce concepts not in the domain model? (That's a scope creep signal.)

### If Refine:
- How could the UX be improved for each user persona?
- Are the acceptance criteria specific enough to implement without ambiguity?
- Does the data model support all the described behaviors efficiently?
- Are there race conditions, edge cases, or failure modes not addressed?
- Could the API design be simpler or more consistent with existing patterns?
- Does the spec conflict with any existing ADRs or architecture decisions?
- Is the testing strategy sufficient? What's untested?

### If All Three:
Run all three analyses. Present them in separate sections.

## Step 5: Cross-Reference Analysis

Regardless of mode, always include:

- **Domain model alignment**: Flag any terms used in the spec that aren't in the domain model, or that use synonyms from the "Avoid" column.
- **ADR compliance**: Flag any design choices that contradict existing ADRs. Note if new ADRs should be created.
- **Requirement coverage**: Check whether all linked requirements are fully addressed. Flag requirements that are only partially covered.
- **Implementation complexity signal**: Based on codebase analysis, estimate relative effort. Flag anything that touches many files, services, or requires new infrastructure.
- **Investor lens**: Would this feature strengthen a demo? Does it support the stated value proposition?

## Step 6: Present Results

Structure the output as:

### Brainstorm: [Document Title]

**Context**: Brief summary of the doc's purpose and current state.

**[Mode] Suggestions**:
For each suggestion:
- **Suggestion title** — one-line summary.
- **Rationale** — why this matters, referencing specific project context.
- **Effort estimate** — Low / Medium / High relative to current spec scope.
- **Impact** — How this moves the needle on success metrics or user experience.
- **Recommendation** — Do it now / Defer to Phase N / Consider cutting / Needs ADR first.

**Cross-Reference Findings**:
- Domain model issues (if any).
- ADR conflicts or gaps (if any).
- Requirement coverage gaps (if any).
- Implementation complexity notes.

**Top 3 Recommendations**: The three highest-impact, most actionable suggestions distilled into a prioritized short list.

## Step 7: Save (If Requested)

If the user chose to save results:

1. Create `docs/reviews/` directory if it doesn't exist.
2. Save the brainstorm output as `docs/reviews/YYYY-MM-DD-doc-name-brainstorm.md` with frontmatter:

```yaml
---
title: "Brainstorm: [Document Title]"
status: accepted
created: [today]
updated: [today]
authors: [AI-assisted]
tags: [brainstorm, mode-used]
links:
  - type: reviews
    target: [relative path to the reviewed document]
---
```

3. Remind the user to run `/pi-doc-index` to update the master index.
