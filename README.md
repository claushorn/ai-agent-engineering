# AI-Assisted Trading Research & Execution Framework

## 1. Overview

This repository implements a structured framework for **research, validation, and deployment of algorithmic trading strategies**, with explicit support for **AI-assisted code generation and review**.

The system is designed around a core principle:

> **All changes must preserve global system invariants, not just local correctness.**

It integrates:
- Research pipelines (data → features → models → evaluation)
- Live execution systems
- AI-assisted development workflows with formalized failure modes

---

## 2. System Philosophy

The repository enforces a set of non-negotiable principles:

### 2.1 Invariant-Driven Development
All components must preserve:
- **Correctness** — outputs match specification
- **Consistency** — no divergence across modules
- **Completeness** — no missing logic or transitions
- **Single source of truth** — no duplicated logic

---

### 2.2 No Silent Failure

The system must never degrade behavior implicitly.

- No silent fallbacks
- No hidden defaults that change semantics

If an operation cannot be performed:
- **Fail explicitly with a clear error**

---

### 2.3 Global Consistency Over Local Fixes

A correct local change is invalid if it:
- breaks cross-module invariants
- introduces inconsistency elsewhere
- leaves partial updates

---

### 2.4 Explicit Design Decisions

All non-trivial architectural choices must be:
- documented
- justified
- preserved across changes

---

## 3. System Structure

The system is organized around the following conceptual layers:

### 3.1 Research Pipeline

A composable pipeline of transformations:

raw data → features → signals → evaluation → artifacts


Each step:
- consumes defined inputs
- produces typed outputs
- satisfies pre/postconditions

---

### 3.2 Execution System

Responsible for:
- real-time strategy execution
- order generation
- state management

Key properties:
- deterministic behavior
- versioned deployment
- auditable logs

---

### 3.3 Shared Logic

Common components:
- feature computation
- strategy logic
- utilities

Constraint:
> A concept must have exactly one implementation.

---

### 3.4 AI-Assisted Development Layer

The system includes formal constraints for AI-generated code:

- failure mode taxonomy
- invariant checks
- structured review process

---

## 4. System Invariants

The following invariants must always hold:

### 4.1 Interface Consistency
All API changes must be propagated to:
- all call sites
- all dependent modules

---

### 4.2 Data Contract Integrity
Schemas, validation, and serialization must agree.

---

### 4.3 State Machine Completeness
- No missing transitions
- No unreachable states

---

### 4.4 No Duplicate Logic
- No competing implementations
- No diverging helpers

---

### 4.5 Deterministic Behavior
Given the same inputs and state:
- outputs must be reproducible

---

## 5. AI Failure Modes (Summary)

AI-generated code is prone to systematic errors:

### 5.1 Non-Local Failures
- cross-module invariants broken
- partial updates

### 5.2 Abstraction Errors
- premature generalization
- loss of special-case logic

### 5.3 API Hallucination
- non-existent functions
- incorrect signatures

### 5.4 Regression Introduction
- previously fixed bugs reintroduced

### 5.5 Testing Failures
- tautological tests
- weak assertions

A full specification is provided in:

ai_code_review_checklist.yaml 


---

## 6. Development Workflow

### 6.1 Before Implementing

- Read relevant design decisions
- Identify invariants affected
- Verify consistency with system architecture

---

### 6.2 During Implementation

- Avoid duplication
- Avoid premature abstraction
- Update all dependent components

---

### 6.3 After Implementation

- Run tests
- Review full diff
- Check against failure modes

---

### 6.4 Before Commit

Validate:

- no invariant violations
- no dead code
- no partial updates
- no missing edge cases

---

## 7. Testing Principles

Tests must validate **behavior**, not implementation.

### Required:
- correctness of outputs
- edge cases (empty, null, boundary)
- failure modes

### Forbidden:
- tautological tests
- assertions that only check existence

---

## 8. Debugging Protocol

When investigating issues:

1. Identify the exact version that produced the behavior
2. Inspect code at that version
3. Compare against current state
4. Do not assume HEAD reflects runtime behavior

---

## 9. Task Management

Long-running work must be tracked explicitly:

- current state
- decisions made
- remaining work

Work must be:
- resumable
- auditable

---

## 10. Environment & Execution

Use the project’s standard tooling consistently.

Example:

uv run <script>


Environment must be:
- reproducible
- deterministic

---

## 11. Contribution Guidelines

All contributions must:

- preserve system invariants
- include tests
- avoid duplication
- avoid silent behavior changes

If a change conflicts with existing design:
- explicitly surface the conflict
- do not resolve implicitly

---

## 12. Known Limitations

- AI-assisted code may violate non-local constraints
- Static checks cannot capture all semantic errors
- Some invariants require human validation

---

## 13. Guiding Principle

> The system is correct only if all invariants hold globally.

Local correctness is insufficient.




