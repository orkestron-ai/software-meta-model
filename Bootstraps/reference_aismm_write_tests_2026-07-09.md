# AISMM Canonical Write Tests Bootstrap

## Purpose

This bootstrap defines the **standard pass for writing unit and functional/integration tests** for changed code.

Use it:
- After finishing a code change
- On direct request ("write tests", "add tests", "cover with tests")

This bootstrap is language- and framework-agnostic. Concrete commands, paths, and class names below are **placeholders** — substitute the actual language, test framework, and directory conventions of the product.

---

## Step 0: Understand What Was Changed

1. Identify which class(es), module(s), or function(s) were added or modified in this task.
2. Determine where the change lives in the product's own module/package structure.
3. Decide **which type of test to write** using the rules below.

---

## Step 1: Choose Test Type

**Write a unit test** when the changed code is:
- A service, factory, decorator, resolver, validator, or handler
- Logic that can be exercised without a database, HTTP, or external services

**Write a functional/integration test** when the changed code is:
- An HTTP endpoint / controller action
- A flow that depends on persisted state or an external system
- Logic that requires fixtures, a real (or test) database, or stubbed external services

If both could apply, **prefer the unit test** — it runs faster and is more stable.

---

## Step 2: Find Neighboring Tests (mandatory — do not skip)

Look at tests that already exist **in the same module/package** as the change:

- Unit tests: the product's unit-test directory, mirroring the changed module's path
- Functional/integration tests: the product's functional-test directory, mirroring the changed module's path

Read 1-2 neighboring test files and copy their:
- Namespace/package layout, class/file modifiers, annotation or decorator style
- Naming conventions for constants/fixtures, mock/stub wiring pattern, data-provider/parametrization structure

**Do not invent a new pattern if an existing one is nearby.** Consistency with neighboring tests matters more than any single "best practice" the agent might otherwise apply.

---

## Step 3: Unit Test Conventions

Match whatever the codebase already does for:
- Namespace/package/module placement (often mirrors the production code's own namespace, not just the test-directory path)
- Base class / test harness usage
- Test data declaration style (constants, fixtures, builders)
- Per-dependency mock/stub construction (e.g. one private helper per dependency)
- Any product-specific test utilities (e.g. helpers that construct domain value objects like money/currency) — use them rather than constructing raw values by hand.

---

## Step 4: Functional/Integration Test Conventions

Match whatever the codebase already does for:
- Namespace/package/module placement of functional tests
- Base test class per test category (e.g. one base class for HTTP/controller flows, another for generic functional tests)
- Naming style for test methods/datasets (human-readable dataset labels are preferred when the framework supports them)

---

## Step 5: Avoid Deprecated Code

Before using any class, method, or service from the project:
- Check whether it is marked deprecated (docblock annotation, comment, or equivalent convention used by the codebase).
- If it is — find the replacement it points to and use that instead.
- Never introduce a new usage of a deprecated symbol.

---

## Step 6: Run the Tests

Determine how the project runs its test suite (a task runner, a containerized environment, a plain CLI test runner) and use that mechanism directly — do not ask whether the environment is available if the project's own convention says it is always running.

**Always run the tests after writing them.** If they fail:
1. Read the error output carefully.
2. If an expected value doesn't match — fix the expected value in the test, not the application code (unless the test correctly caught a real bug).
3. If a fixture or service is missing — check neighboring passing tests for how they set it up.
4. Re-run after each fix until green.

---

## Checklist Before Finishing

- [ ] Correct test type chosen (unit vs functional/integration)
- [ ] Read neighboring tests and matched their style
- [ ] Namespace/module placement follows the project's own convention
- [ ] Project's own style rules followed (strict typing, visibility, naming, etc.)
- [ ] No deprecated classes or methods used
- [ ] No leftover generator/scaffold artifacts in created code
- [ ] Tests run and are green
- [ ] Run command included in the response
