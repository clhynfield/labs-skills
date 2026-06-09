---
name: ears-specifications
description: Use when writing formal specifications using the EARS (Easy Approach to Requirements Syntax) patterns — converting natural-language requirements, Gherkin scenarios, or user stories into testable, atomic specs with stable semantic IDs.
---

# EARS Specifications

EARS (Easy Approach to Requirements Syntax) is a lightweight requirements format: every spec follows one of five patterns. Each spec should be small enough for a single red-green-refactor cycle.

## The Five Patterns

1. **Ubiquitous** (always true): "The system shall [behavior]."
2. **Event-driven** (triggered by action): "When [trigger], the system shall [response]."
3. **State-driven** (while condition holds): "While [state], the system shall [behavior]."
4. **Optional** (feature-dependent): "Where [feature is enabled], the system shall [behavior]."
5. **Unwanted** (error handling): "If [unwanted condition], then the system shall [response]."

## Semantic IDs

Use the format `{FEATURE}-{TYPE}-{NNN}`:

- **FEATURE**: 2–4 letter prefix derived from the feature area (e.g., `AUTH`, `CART`, `DASH`)
- **TYPE**: Component type — `UI`, `API`, `DATA`, `NAV`, `BE`, `PROC`
- **NNN**: Sequential number, zero-padded (`001`, `002`, ...)

Keep IDs stable. Don't renumber when inserting — use gaps or sub-numbers like `AUTH-API-001a`.

## Converting from Gherkin

One Gherkin scenario often yields multiple EARS specs. Decompose each `Given/When/Then` into atomic, testable assertions.

Gherkin:

```gherkin
Scenario: Successful login
  Given a registered user
  When they submit valid credentials
  Then a session is created
  And they are redirected to the dashboard
```

EARS:

- `AUTH-API-001`: When valid credentials are submitted, the system shall create a session.
- `AUTH-NAV-001`: When a session is created, the system shall navigate to the dashboard.

## Common Mistakes

- **Spec too big.** If you can't write a single test that proves it, split it.
- **Two specs in one sentence.** "When X, the system shall A and B." Split.
- **Renumbering on insertion.** Once issued, an ID is permanent. Use gaps or `001a`.
- **Compound TYPE.** `AUTH-UI-API-001` is two specs masquerading as one. Pick the dominant type, or split.