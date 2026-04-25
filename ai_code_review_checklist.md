# LLM Code Revision Failure Modes and Review Checklist

I. Correctness & Invariants
Violations of semantic correctness, completeness, consistency
Preconditions / postconditions broken
Cross-module invariants not preserved
State machine inconsistencies (missing transitions)
Implicit coupling violations (schema ↔ validation ↔ serialization)

II. Change Consistency & Propagation
Partial updates across call sites
Function signature drift
Inconsistent application of changes
Goal drift (spec no longer satisfied)

III. Redundancy & Code Evolution Pathologies
Duplicate implementations
Diverging helper functions
Dead code / stale branches
Old logic coexisting with new logic
Redundancy accumulation

IV. Abstraction & Design Errors
Premature abstraction
Incorrect factorization
Lossy generalization
Unnecessary indirection
“Locally elegant, globally incorrect” abstractions

V. API & Interface Hallucination
Non-existent APIs
Incorrect signatures
Phantom layers/functions
Mixed framework versions

VI. State, Memory, and Semantics Errors
Mutability / ownership violations
Lifecycle mismanagement
Copy vs reference errors
Eager vs lazy evaluation mistakes

VII. Non-Local Reasoning Failures
Cross-module constraints broken
Distributed invariants not preserved
Context corruption (assumptions drift)

VIII. Edge Cases & Robustness
Missing boundary conditions
Numerical instability
Concurrency issues
Error propagation gaps

IX. Regression Pathologies
Reintroduction of prior bugs
Silent degradation of behavior

X. Testing Failures
Tautological tests
Weak assertions
Missing adversarial cases
Spec–test mismatch

XI. Maintainability & Code Quality
Spaghetti code
Poor locality of logic
Comment–code divergence

## Canonical Examples 
1. Change Propagation Failure (Signature Drift)
Failure mode:
Inconsistent update across call sites

Before:
  def foo(a, b): ...

After:
  def foo(a, b, c): ...

Bug:
  foo(x, y)  # unchanged call site

Why this is wrong:
Interface change not propagated; violates consistency invariant.

2. Non-Local Invariant Violation (Schema Coupling)
Failure mode:
Implicit coupling broken (schema ↔ validation)

Before:
  schema: {age: int >= 0}
  validate(age >= 0)

After:
  schema: {age: int >= -1}  # updated
  validate(age >= 0)        # stale

Why this is wrong:
Cross-module invariant divergence.

3. Incorrect Factorization (Lossy Abstraction)
Failure mode:
Shared logic extracted incorrectly

Before:
  if (x == 0) return special();
  return f(x);

After:
  return f(x);

Why this is wrong:
Special-case semantics lost during abstraction.

4. Premature Abstraction
Failure mode:
Generalization for non-existent cases

Before:
  result = price * 1.2

After:
  result = apply_pricing_strategy(price, strategy="default")

Why this is wrong:
Introduces indirection without additional use cases; increases complexity.

5. API Hallucination
Failure mode:
Non-existent function assumed

Code:
  data = json.parse_file("file.json")

Why this is wrong:
`parse_file` does not exist in standard JSON libraries.

6. State / Semantics Error (Copy vs Reference)
Failure mode:
Incorrect assumption about mutability

Before:
  b = a
  b.append(1)

After expectation:
  a unchanged

Actual:
  a modified

Why this is wrong:
Aliasing not accounted for; shared reference mutated.

7. Partial State Machine Update
Failure mode:
Missing transitions

Before:
  A -> B -> C

After:
  A -> B
  # transition B -> C removed unintentionally

Why this is wrong:
State machine incomplete; violates reachability.

8. Dead Code Retention
Failure mode:
Old logic coexists with new logic

Before:
  if flag:
    new_logic()
  else:
    old_logic()

After:
  new_logic()
  # old_logic still defined and referenced elsewhere

Why this is wrong:
Stale branches create divergence and maintenance risk.

9. Regression (Error Reintroduction)
Failure mode:
Previously fixed bug reintroduced

Before:
  if x is None: return

After:
  process(x)  # guard removed

Why this is wrong:
Reintroduces null-handling bug.

10. Edge Case Omission
Failure mode:
Boundary condition not handled

Code:
  avg = sum(xs) / len(xs)

Bug:
  xs = []

Why this is wrong:
Division by zero; empty input not handled.

11. Concurrency Error
Failure mode:
Race condition

Code:
  if not exists(key):
    create(key)

Why this is wrong:
Non-atomic check-then-act; duplicate creation possible.

13. Weak Assertion
Failure mode:
Insufficient validation

Code:
  result = compute()
  assert result is not None

Why this is wrong:
Does not verify correctness of output.

14. Goal Drift
Failure mode:
Spec no longer satisfied

Spec:
  return sorted list

Code:
  return list(set(xs))

Why this is wrong:
Deduplicates but does not sort; deviates from requirement.

15. Context Corruption
Failure mode:
Earlier assumptions silently dropped

Before:
  # assumes normalized input

After:
  process(raw_input)

Why this is wrong:
Precondition no longer enforced.
