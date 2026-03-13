# Software Requirements Specification Template

Based on IEEE 29148 / ISO/IEC/IEEE 29148:2018, adapted for modern agile delivery and AI-assisted development.

```markdown
---
title: "[Project Name] — Software Requirements Specification"
status: draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
authors: [name]
tags: [requirements, srs, specification]
---

# Software Requirements Specification

## 1. Introduction

### 1.1 Purpose

What this document covers and why it exists. State the system or subsystem being specified.

### 1.2 Scope

What is included and what is explicitly excluded. Boundaries of the system.

### 1.3 Intended Audience

Who should read this and what they should get from it:
- Developers: implementation guidance and acceptance criteria.
- Investors: product scope and capability overview.
- AI agents: precise, testable requirements for code generation and verification.

### 1.4 Definitions & Acronyms

Link to the [Domain Model](domain-model.md) for the canonical glossary. Only list terms specific to this document here.

## 2. System Overview

### 2.1 Product Perspective

Where this system sits in the larger context. External systems it integrates with. Hardware/platform constraints. Diagram recommended.

### 2.2 User Characteristics

Reference the personas from the [Product Vision](vision.md). For each persona, note:
- Technical proficiency level.
- Frequency and context of use.
- Accessibility needs.

### 2.3 Constraints

Regulatory, licensing, hardware, performance, and technology constraints that bound the solution space.

### 2.4 Assumptions & Dependencies

Numbered list. Each assumption should have a validation plan. Each dependency should have a fallback.

## 3. Functional Requirements

Organize by feature area or user journey. Each requirement follows this format:

### [Feature Area]

#### REQ-NNN: [Requirement Title]

- **Description**: What the system must do. Use precise language: "shall" for mandatory, "should" for recommended, "may" for optional.
- **Actor**: Which user persona or system triggers this.
- **Preconditions**: What must be true before this can happen.
- **Input**: What data or action initiates this.
- **Processing**: What the system does (business logic, calculations, state changes).
- **Output**: What the user sees or what state changes result.
- **Postconditions**: What must be true after successful completion.
- **Acceptance Criteria**: Testable conditions (map to executable specs where possible).
  - Given [context], when [action], then [expected result].
  - Given [context], when [action], then [expected result].
- **Priority**: Must / Should / Could / Won't (MoSCoW).
- **Links**: Related ADRs, design specs, executable specs.

<!-- Repeat for each requirement. Number sequentially within feature areas. -->

## 4. Non-Functional Requirements

### 4.1 Performance

| Metric | Target | Measurement Method |
|--------|--------|--------------------|
| Response time (95th percentile) | | |
| Throughput (requests/sec) | | |
| Concurrent users supported | | |
| Cold start time | | |

### 4.2 Reliability & Availability

- Uptime target (e.g., 99.9%).
- Mean time to recovery (MTTR).
- Data durability guarantees.
- Graceful degradation strategy.

### 4.3 Security

- Authentication requirements.
- Authorization model.
- Data encryption (at rest, in transit).
- Compliance requirements (GDPR, SOC2, HIPAA, etc.).
- Vulnerability management process.

### 4.4 Usability

- Target user proficiency (novice, intermediate, expert).
- Accessibility standard (WCAG level).
- Supported platforms/browsers.
- Internationalization requirements.

### 4.5 Scalability

- Expected growth trajectory.
- Horizontal vs. vertical scaling strategy.
- Data volume projections.

### 4.6 Maintainability

- Code coverage targets.
- Documentation requirements.
- Deployment frequency target.
- Technical debt management approach.

## 5. Interface Requirements

### 5.1 User Interfaces

Key screens or interaction patterns. Reference wireframes or design system. Do not duplicate the design system — link to it.

### 5.2 Software Interfaces

External APIs consumed, data formats expected, protocol requirements.

### 5.3 Hardware Interfaces

Physical devices, sensors, displays, or peripherals the system interacts with.

### 5.4 Communication Interfaces

Network protocols, message formats, real-time requirements.

## 6. Traceability Matrix

| Requirement | Design Spec | ADR | Executable Spec | Status |
|------------|-------------|-----|-----------------|--------|
| REQ-001 | designs/auth.md | ADR-003 | specs/auth.feature | Implemented |
| REQ-002 | | | | Draft |

## Appendices

- Links to user research backing these requirements.
- Related documents: [Vision](vision.md), [Architecture](architecture.md), [Domain Model](domain-model.md).
```
