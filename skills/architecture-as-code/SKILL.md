---
name: architecture-as-code
description: Use when scaffolding a new project or introducing architecture documentation and Architecture Decision Records (ADRs) into an existing one, or when a proposed change is architecturally significant and needs a decision recorded — and the architecture overview updated — before implementation. Also use when the user mentions ADRs, architecture decisions, an architecture overview or diagram, "should we write this down", or asks how to record a hard-to-reverse technical choice.
---

# Architecture as Code

A lightweight discipline for keeping a *living architecture overview* and
recording architecturally significant decisions as short, numbered Markdown
files — before the implementing code is written. The ADR format follows Michael
Nygard's [Lightweight Architecture Decision Records][nygard], as endorsed by the
[ThoughtWorks Technology Radar][twradar].

The practice has two coupled artifacts:

- **Architecture docs** (`docs/architecture/`) — a planning artifact that
  describes how the system is structured *right now* and why. At its centre is
  an `overview.md` with Mermaid diagrams. Read it before a structural change.
- **ADRs** (`docs/architecture/decisions/`) — numbered records of individual
  architecturally significant decisions.

The link between them is the heart of this skill:

> **ADRs are to the architecture overview as schema migrations are to a database
> schema.** Just as the schema always reflects the last applied migration, the
> architecture docs always reflect the state implied by the last accepted ADR.

This skill has two jobs:

1. **Bootstrap** the architecture-docs + ADR practice into a project that
   doesn't have it yet.
2. **Author** new ADRs — and keep the architecture overview in sync — as
   architecturally significant decisions arise.

## Bootstrapping the practice into a project

When scaffolding a new project — or introducing this practice into an existing
one — set up the architecture-docs directory and seed it. The templates named
below ship with this skill in its `templates/` directory.

1. Create `docs/architecture/` and `docs/architecture/decisions/` in the project
   root.
2. Copy `templates/architecture-readme.md` to `docs/architecture/README.md`.
   This describes how the project uses architecture docs and ADRs, and links
   prominently to the overview. Adjust the project name and any wording, but
   keep the "planning artifact" and "migration mechanism" sections intact.
3. Copy `templates/overview.md` to `docs/architecture/overview.md` and fill in
   what you already know about the system: its purpose, the major modules, and a
   first module-dependency diagram. For a brand-new project this can be a thin
   sketch — it grows with the system. Do not leave it empty; an honest skeleton
   is the point.
4. Copy `templates/template.md` to `docs/architecture/decisions/template.md`.
   This is the starting point every future ADR is copied from.
5. Copy `templates/0001-adopt-architecture-decision-records.md` to
   `docs/architecture/decisions/` unchanged, except set its `Date` field to
   today's date. This first ADR records the decision to adopt the practice.
6. Wire the practice into the project's contributor and agent guidance so it is
   actually followed (see "Wiring it into the project" below).

Do not invent a different directory layout or numbering scheme. Consistency is
the point — every Labs-style project should look the same here.

## What warrants an ADR

A change is architecturally significant — and needs an ADR — if it affects any
of the following:

- System structure or module boundaries (packages, layers, entry points)
- Addition or removal of an external dependency or service integration
- Transport layer, protocol, or external API contract
- The shape of a public interface
- Authentication or authorisation model
- Data formats exchanged across system boundaries
- A non-functional characteristic (security, performance, deployability,
  observability)
- A new convention that all contributors must follow
- Agent autonomy or AI-integration patterns
- Any decision that would be costly or disruptive to reverse

**If you are unsure, treat it as significant and write the ADR.** A
false-positive ADR costs one short review cycle. A missed architectural decision
can cost a rewrite.

For trivial, easily-reversible changes — renaming a local variable, fixing a
typo, adjusting a log message — no ADR is needed.

## The lifecycle

| Status     | Meaning |
|------------|---------|
| Draft      | Proposed; under discussion. No implementation work yet. |
| Accepted   | Approved; guides current implementation. |
| Deprecated | Still implemented but no longer recommended for new work. |
| Superseded | Replaced by a later ADR (link it). |

1. **Draft** — Copy `docs/architecture/decisions/template.md` to
   `docs/architecture/decisions/NNNN-short-title.md`, incrementing `NNNN` to the
   next unused sequential number (determine it by listing the directory). Fill
   in Context, Decision, and Consequences. Set `Status` to `Draft` and `Date` to
   today. **In the same change, update `overview.md` (and any other affected
   architecture docs) to reflect the _proposed_ state** — including the diagrams.
   Open a change (PR or equivalent) containing **only** the ADR and the
   architecture-doc updates — no test or implementation code.
2. **Review** — A human reviews the context, decision, consequences, and the
   proposed architecture-doc changes together. No implementation work proceeds
   during this phase.
3. **Accepted** (or rejected) — Once consensus is reached, update the `Status`
   field to `Accepted` and merge. The architecture docs now reflect the accepted
   decision. Implementation work may now begin.
4. **Superseded / Deprecated** — When a later ADR overturns this one, add a
   "Superseded by ADR NNNN" note and update the status; update the architecture
   docs to match the superseding decision. Never delete or rewrite the decision
   in an old record; supersede it with a new one.

ADRs are immutable once Accepted. A change of direction produces a *new* ADR,
not an edit to an old one.

**The hard rule that ties the two artifacts together:** never change an ADR's
status without updating the relevant architecture docs in the same commit. An
ADR accepted without a corresponding doc update leaves the docs stale — the
equivalent of a migration that was written but never applied.

## Diagrams: show current and proposed

Architecture docs use [Mermaid](https://mermaid.js.org/) so diagrams render
natively on GitHub and in editors with no build step.

- The diagrams in `overview.md` always depict the system **as it is now**.
- When proposing an architecturally significant change, edit those diagrams to
  depict the **proposed** state as part of the Draft ADR change. The reviewer
  sees the structural delta rendered directly in the diff.
- An ADR may embed its own focused diagram in the Context or Decision section to
  illustrate the specific change (for example, a before/after of two modules)
  without redrawing the whole system.

Keep diagrams legible: prefer a small diagram that makes one point over a sprawl
that makes none. Favour taller diagrams over very wide ones.

## What a good ADR change looks like

- The ADR is copied from `template.md` and keeps its four sections: Context,
  Decision, Consequences, References.
- The **Context** section makes the constraints legible to someone who wasn't in
  the room — what problem, what options, what criteria mattered.
- The **Decision** states what you're doing in one or two sentences, then the key
  reasoning.
- The **Consequences** section honestly names trade-offs and risks, not just
  benefits.
- The change contains exactly the ADR file plus the architecture-doc updates it
  implies (and a `template.md` update only if the template itself is changing).
  No source, no tests.

## What agents must NOT do

- Do **not** write test or implementation code for an architecturally
  significant change before its ADR is Accepted. Draft the ADR, update the
  architecture docs, and stop.
- Do **not** change an ADR's status without updating the relevant architecture
  docs in the same commit.
- Do **not** accept or merge your own ADR. Status promotion from Draft to
  Accepted requires a human reviewer.
- Do **not** edit an Accepted ADR to change its decision. Supersede it with a new
  ADR instead.

When you identify a needed architectural change mid-task: pause, draft the ADR,
update the affected architecture docs to the proposed state, surface it to the
human, and wait. Discovering an architectural decision in a diff is exactly what
this practice exists to prevent.

## Wiring it into the project

For the discipline to hold, the project's own guidance must point at it. When
bootstrapping, add a short architecture section to the files that direct
contributors and agents — for example a `CONTRIBUTING.md` (for humans) and an
`AGENTS.md` (for AI agents), if the project uses them. That section should state,
at minimum:

- Where the architecture docs and ADRs live, and the instruction to **read
  `docs/architecture/` before any structural change**.
- The "what warrants an ADR" list above.
- The hard rule: an architecturally significant change starts with a Draft ADR
  *and* proposed architecture-doc updates, and waits for Acceptance before any
  implementation.
- The migration rule: an ADR status change is incomplete without the
  corresponding architecture-doc update.

Keep it brief and link back to `docs/architecture/README.md` and ADR 0001 for
the full rationale rather than duplicating it.

[nygard]: https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
[twradar]: https://www.thoughtworks.com/radar/techniques/lightweight-architecture-decision-records
