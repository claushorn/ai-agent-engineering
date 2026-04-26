# CLAUDE.md — Project Guidelines for AI-Assisted Development

## 1. System Awareness Before Action

Before executing any non-trivial task, identify:

- Available actions, scripts, and entry points
- Data artifacts and their formats
- Preconditions and postconditions for pipeline steps

If a registry or schema exists (e.g., `registry/`, `schemas/`, `README.md`), read it first.

**Rule:** Do not execute or modify pipelines without understanding how components compose.

---

## 2. Change Tracking & Auditability

All changes affecting production or critical systems MUST be logged.

- Create a changelog entry for every meaningful modification
- Use a consistent, timestamped format
- Ensure entries explain *what changed* and *why*

**Rule:** No silent changes to live or critical paths.

---

## 3. Testing as a First-Class Constraint

Every code change MUST be accompanied by:

- A new or updated test
- Tests that validate behavior, not just execution

Avoid:
- Tautological tests (mirroring implementation)
- Weak assertions (e.g., only checking non-null)

**Rule:** If behavior is not tested, it is not guaranteed.

---

## 4. Design Decision Consistency

Maintain a central record of design decisions.

Before implementing:
- Verify consistency with existing decisions

When conflicts arise:
- Explicitly surface them
- Do not resolve implicitly

When introducing new decisions:
- Record: rule + rationale (“Why”)

**Rule:** Local changes must not violate global design invariants.

---

## 5. Debugging Protocol (Critical Systems)

When investigating errors in deployed systems:

1. Identify the exact version/commit that produced the behavior
2. Inspect the code at that version (not current HEAD)
3. Compare against current state to detect drift
4. Avoid reasoning about logs without version alignment

**Rule:** Never debug against the wrong code version.

---

## 6. Change Discipline

After completing any task:

- Review diffs for unintended changes
- Ensure no unrelated files were modified
- Check for regression risks

Before committing:

- Validate against the failure-mode checklist (`llm_revision_failure_modes.md`)
- Explicitly check:
  - invariant violations
  - partial updates
  - dead code
  - regression introduction
  - missing edge cases

**Rule:** Every change must be globally consistent, not just locally correct.

---

## 7. Commit & Deployment Protocol

- Propose commits only after:
  - tests pass
  - diffs are clean
  - no unresolved issues remain

- Never push without explicit approval (if human-in-the-loop)

**Rule:** Commits represent verified system states, not intermediate work.

---

## 8. Task Memory

Multi-step tasks must maintain an explicit, persistent state in `task_memory/<task-name>.md`.

### Purpose

Task memory preserves **continuity across plan–implement–revise cycles**.

It is the primary mechanism to prevent:
- context loss
- repeated re-explanation
- inconsistent decisions

---

### Required Structure

Each task file must contain:

- **Objective**
  - What is the task trying to achieve?

- **Current State**
  - What has been implemented so far?
  - What is known to work / not work?

- **Decisions**
  - Key choices made and why

- **Open Issues**
  - Unresolved questions or uncertainties

- **Next Steps**
  - Concrete, minimal actions to continue

---

### Usage Protocol

**At task start:**
- Check if a relevant task file exists
- If yes: read it before planning
- If no: propose creating one for non-trivial tasks

---

**During execution:**
- Keep plans small (single logical step)
- After each step:
  - update Current State
  - update Decisions if applicable
  - update Open Issues

---

**At task boundaries (end of plan):**
- Ensure the file reflects the true system state
- Remove outdated or contradictory information
- Add clear Next Steps

---

### Consistency Rules

- Task memory must reflect **actual code state**, not intended state
- Do not leave stale or conflicting entries
- Do not duplicate information already captured in:
  - changelog (why)
  - design decisions (global constraints)

---

### When to Use

Create or use task memory if:

- the task requires more than one implementation step
- the task spans multiple files or modules
- the task involves uncertainty or investigation

---

### When to Close

- Move completed tasks to `task_memory/done/`
- Do not delete task history

---

### Key Principle

- Keep steps small, but keep state persistent.

Task memory enables reliable long-horizon reasoning with limited context.

**Rule:** Work must be resumable without loss of context.

---

## 9. End-of-Session Hygiene

At the end of each session:

- Run a full diff review
- Identify unintended changes
- Ensure no silent regressions

**Rule:** No session ends with an unknown system state.

---

## 10. System Invariants

The following must always hold:

- Interfaces are consistent across all call sites
- Data contracts are respected across modules
- State machines are complete and reachable
- No duplicate sources of truth exist

All changes must be validated against these invariants.

---

## 11. Non-Local Effects Warning

Changes may have effects outside the edited file.

Always check:
- dependent modules
- shared utilities
- data pipelines

Do not assume locality of impact.

---

## 12. Diff Interpretation Rule

A diff is not just a change—it is a transformation.

Always ask:
- What invariant might this break?
- What assumptions depended on the old behavior?
