# Pivotal XP — Claude Code Skills Package

A Claude Code skills package for running solo or small-team coding sessions with Pivotal Labs–style Extreme Programming discipline. You and Claude pair on epics, slice them into stories, write EARS specifications, and TDD each spec one behavior at a time — all with explicit developer approval at every gate.

## What's in the box

Four skills, designed to ship together but usable independently:

- **`pivotal-xp`** — the workflow. Five phases from epic refinement to story completion, with developer-approval gates at every TDD transition.
- **`test-driven-development`** — a standalone RED-GREEN-REFACTOR discipline skill.
- **`ears-specifications`** — a reusable reference for EARS (Easy Approach to Requirements Syntax) patterns and the semantic-ID convention. Useful outside Pivotal-XP for any Spec-Driven-Development flow.
- **`architecture-as-code`** — bootstraps a project's architecture-docs and ADR practice, keeps a living architecture overview (with Mermaid diagrams) in sync with decisions, and guides authoring lightweight, numbered records for architecturally significant decisions before the implementing code is written.

## Install

This is a bare skills directory — no plugin manifest. Two ways to use it:

**Per-project** — clone into a project and Claude Code picks up the `skills/` directory automatically when you work in that repo:

```bash
git clone git@github.com:clhynfield/labs-skills.git
```

**Globally** — symlink the individual skills into your user skills directory so they're available across all your projects:

```bash
ln -s "$(pwd)/skills/pivotal-xp"              ~/.claude/skills/pivotal-xp
ln -s "$(pwd)/skills/test-driven-development" ~/.claude/skills/test-driven-development
ln -s "$(pwd)/skills/ears-specifications"     ~/.claude/skills/ears-specifications
ln -s "$(pwd)/skills/architecture-as-code"    ~/.claude/skills/architecture-as-code
```

## First session

The skill triggers on phrasings like:

- "Let's pair on this epic"
- "I want to work this in Pivotal XP style"
- "Help me slice this into stories"
- "Let's TDD this spec"

A typical opener:

> **You:** I have a feature idea — let's work it as a Pivotal XP epic.
>
> **Claude:** Let's start with epic refinement. Share the rough idea and I'll help shape it into an epic doc under `/docs/epics/`. I'll ask clarifying questions in batches, then move us into story slicing.

## The workflow at a glance

Five phases, with developer approval at every gate:

1. **Epic Refinement** — Iterate on a markdown epic; clarify scope, users, acceptance criteria.
2. **Story Slicing** — Decompose the epic into small, independently testable stories.
3. **EARS Specifications + TDD Plan** — Convert Gherkin acceptance criteria into EARS specs; sequence them by dependency.
4. **Test-Driven Development** — One spec at a time. Strict RED → GREEN → REFACTOR per spec, with developer approval at every transition.
5. **Story Completion** — Run the full suite, update the parent epic doc, move on.

All artifacts live in `/docs/epics/` alongside your code — no Jira, no Linear, no external store.

## Works with

`pivotal-xp` Phase 4 uses the bundled `test-driven-development` skill for its RED-GREEN-REFACTOR mechanics.

The package is fully self-contained — no other plugins required.

## Tips

- **Model choice.** Phases 1–3 reward Opus (epic refinement, story design, and EARS authoring are high-judgment). Phase 4 (mechanical RED-GREEN-REFACTOR) runs well on Sonnet. Switch with `/model` between phases if you want the split.
- **File ordering.** Epics and stories use 4-digit prefixes (`0001-`, `0010-`) with gaps, so you can insert later items without renumbering.
- **Resumable across sessions.** Because all spec state lives in the story doc with `@spec` annotations in the code, work picks back up cleanly across long sessions or after a context reset.
- **EARS standalone.** If you only want lightweight Spec-Driven-Dev (no Pivotal workflow), use the `ears-specifications` skill on its own — it's not coupled to `pivotal-xp`.

## Background

Pivotal Labs was a software consultancy known for rigorous Extreme Programming practice: 100% pair programming, test-first discipline, small vertical slices, and ruthless simplicity. This package adapts that discipline to a human-and-agent pair. The agent proposes, you refine and approve, you both stay focused on the spec in front of you.

For more on XP: Kent Beck's *Extreme Programming Explained* and Martin Fowler's writing on XP cover the underlying methodology.
