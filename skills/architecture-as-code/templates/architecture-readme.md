# Architecture Documentation

This directory holds the architecture-as-code documentation for this project:
a living description of how the system is structured and why, plus the decision
records that shaped it. It renders natively on GitHub and in most editors — no
build step required.

**Start here:**

- **[`overview.md`](./overview.md)** — the high-level architecture: purpose,
  module structure, dependencies, and key design decisions.
- **[`decisions/`](./decisions/)** — the Architecture Decision Records (ADRs).
  See [ADR 0001](./decisions/0001-adopt-architecture-decision-records.md) for
  why this practice exists.

---

## Architecture docs are planning artifacts

These docs describe *how the system works* and *why it is structured that way*.
They are meant to be consulted **before** a change, not written up afterward.

Before making any change that touches the structure of the system, a
contributor (human or AI agent) should:

1. Read the relevant files here to understand the current state.
2. Write an ADR (see [`decisions/`](./decisions/)) if the change is
   architecturally significant.
3. Update the relevant architecture files — including any diagrams — to reflect
   the *proposed* state. This happens in the same change as the ADR, before any
   implementation code.
4. Implement the code only after the ADR has been accepted.

A draft pull request that contains only updated architecture docs and a Draft
ADR — with no implementation code — is a valid and encouraged state.

---

## ADRs are the migration mechanism

The relationship between ADRs and the architecture overview mirrors the
relationship between schema migrations and a database schema:

> Just as no migration is ever skipped and the schema always reflects the last
> applied migration, the architecture docs always reflect the state implied by
> the last accepted ADR.

| ADR status change | Architecture doc action required |
|-------------------|----------------------------------|
| → **Accepted** | Update the relevant architecture file(s) to reflect the decision — in the same commit as the status change |
| → **Deprecated** | Note the deprecation in the relevant architecture file(s) |
| → **Superseded by ADR NNNN** | Update the relevant architecture file(s) to reflect the superseding decision |

Reviewing an ADR change means reviewing both the ADR text and the accompanying
architecture-doc updates. If an ADR is accepted but the architecture docs are
not updated, the docs are stale.

---

## Files in this directory

| File | Contents |
|------|----------|
| [`README.md`](./README.md) | This file — navigation, conventions, and the ADR lifecycle |
| [`overview.md`](./overview.md) | The high-level architecture: purpose, module structure, dependency diagram, key decisions |
| [`decisions/`](./decisions/) | Architecture Decision Records, `NNNN-short-title.md`, numbered sequentially from 0001 |

_Add a row for each new architecture document (e.g. a deep-dive on a subsystem,
a call graph, or a set of sequence diagrams) as the project grows._

---

## Rendering Mermaid diagrams

Diagrams in these files use [Mermaid](https://mermaid.js.org/) fenced code
blocks (` ```mermaid `).

| Environment | How diagrams render |
|-------------|---------------------|
| **GitHub** | Rendered automatically in the web UI and in PR diffs |
| **VS Code** | Install the [Markdown Preview Mermaid Support](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid) extension |
| **CLI / CI** | `npx @mermaid-js/mermaid-cli` (`mmdc`) for SVG/PNG export |

No build step is required to read or share these diagrams.
