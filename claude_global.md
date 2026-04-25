# CLAUDE.md — Global Rules for Code Generation and Analysis

## 1. No Silent Fallbacks

Never introduce fallback behavior that changes semantics.
Fail fast with explicit errors. Do not exit successfully (`exit(0)`) on failure conditions.

- If the intended logic cannot be executed:
  - Emit a clear error
  - Fail explicitly

**Rule:** Explicit failure is always preferred over implicit deviation.

---

## 2. No Code Duplication

- Reuse existing functions where possible
- If reuse is not possible:
  - Refactor into shared utilities

Avoid:
- Copy-paste implementations
- Slightly divergent duplicates

**Rule:** One concept → one implementation.

---

## 3. Semantic Understanding Over Plausibility

When analyzing code:

- Do not provide plausible explanations
- Derive behavior from:
  - actual code paths
  - execution flow
  - data transformations

Use:
- instrumentation
- measurement
- tracing

**Rule:** Explanations must be grounded in verified behavior.

---

## 4. Preserve System Invariants

All changes must preserve:

- correctness
- consistency
- completeness

Check explicitly:
- cross-module invariants
- state transitions
- data contracts

**Rule:** Local improvements must not break global properties.

---

## 5. No Partial Changes

When modifying:

- APIs
- schemas
- state machines

Ensure:
- all dependent code is updated

**Rule:** Changes must be applied transitively across the system.

---

## 6. Edge Case Completeness

Always account for:

- empty inputs
- null values
- boundary conditions
- concurrency scenarios

**Rule:** Correctness includes edge cases, not just nominal paths.

---

## 7. Avoid Premature Abstraction

Only abstract when:

- multiple real use cases exist

Avoid:
- unnecessary indirection
- speculative generalization

**Rule:** Prefer concrete correctness over abstract elegance.

---

## 8. Tooling Discipline

Use the project’s standard tooling consistently.

- Use designated runtime/runner
- Use standard dependency management

**Rule:** Execution environment must be consistent and reproducible.
