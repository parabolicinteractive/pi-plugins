# pi-docs

A structured specification documentation system for software projects. Create, manage, audit, and cross-link spec documents following modern best practices for developers, investors, and AI agents.

## What It Does

pi-docs provides a complete documentation architecture for software projects — from product vision through operational runbooks. It establishes a consistent `docs/` directory structure, templates for 10 document types, cross-linking conventions, and tools to keep everything healthy.

The system is designed for three audiences simultaneously:
- **Developers** get precise, actionable specs with code references and acceptance criteria.
- **Investors** get clear product vision, market context, and business model documentation.
- **AI agents** get explicit terminology, decision history, and measurable requirements — the three things that prevent agent drift and wasted effort.

## Components

### Skills

| Skill | Triggers On |
|-------|-------------|
| **pi-doc-system** | "documentation structure", "what documents do we need", "where should I document this" |
| **pi-doc-templates** | "create a spec document", "what should go in a vision doc", "template for ADR" |
| **pi-domain-model** | "domain model", "ubiquitous language", "naming conventions", "what should we call this" |

### Commands

| Command | Description |
|---------|-------------|
| `/pi-doc-create <type> [name]` | Create a new spec document from template |
| `/pi-doc-audit [path]` | Audit docs for coverage gaps, broken links, stale content, terminology drift |
| `/pi-doc-index [path]` | Generate or update the master document index |
| `/pi-adr <title>` | Quick-create an Architecture Decision Record |
| `/pi-doc-brainstorm <doc-path>` | Brainstorm improvements, scope advice, and ideas for a spec doc |

### Document Types Supported

| Type | File | Description |
|------|------|-------------|
| Product Vision & Charter | `docs/vision.md` | Market opportunity, users, business model |
| Domain Model | `docs/domain-model.md` | Shared vocabulary, entities, bounded contexts |
| Software Requirements Spec | `docs/requirements.md` | Functional & non-functional requirements |
| Technical Architecture | `docs/architecture.md` | System design, components, tech stack |
| Architecture Decision Records | `docs/adrs/NNN-*.md` | One decision per file, never deleted |
| Requests for Comments | `docs/rfcs/NNN-*.md` | Design explorations before decisions |
| Technical Design Specs | `docs/designs/*.md` | Per-feature implementation designs |
| Executable Specifications | `docs/specs/*.feature` | BDD scenarios (Gherkin format) |
| API Contracts | `docs/api/*` | OpenAPI, GraphQL, webhook specs |
| Operations Guide | `docs/operations.md` | Deployment, monitoring, incident response |

## Setup

No configuration required. Install the plugin and use the commands.

The plugin expects spec documents to live in a `docs/` directory at the project root. The `/pi-doc-create` command will create this directory if it doesn't exist.

## Usage

**Starting a new project's documentation:**
```
/pi-doc-create vision
/pi-doc-create domain-model
/pi-doc-create requirements
/pi-doc-create architecture
```

**Recording a decision:**
```
/pi-adr Use Supabase for backend
```

**Checking documentation health:**
```
/pi-doc-audit
```

**Updating the master index:**
```
/pi-doc-index
```

**Brainstorming improvements to a spec:**
```
/pi-doc-brainstorm docs/designs/homepage.md
```
You'll be prompted to choose a thinking mode (expand, reduce, refine, or all three) and whether to save results to `docs/reviews/`.

## Philosophy

- **Docs as code** — everything in Git, in markdown, alongside the source.
- **Cross-linked, not flat** — documents form a graph with explicit relationships.
- **Measurable over vague** — every requirement is testable, every metric has a number.
- **Decision history matters** — ADRs prevent AI agents from suggesting approaches you've already rejected.
- **One term per concept** — the domain model is the contract that keeps humans and AI agents aligned.

## Author

Parabolic Interactive
