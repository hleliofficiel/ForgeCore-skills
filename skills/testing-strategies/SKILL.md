# Testing Strategies & Quality Engineering

> Distilled engineering knowledge for writing effective tests, ensuring system reliability, and preventing regressions.

Testing is not about proving the code works. It is about proving the code does not break when it changes. A robust test suite is the safety net that enables rapid, fearless refactoring and continuous deployment.

---

# Core Principles

Every testing strategy should satisfy:

**Confidence:** A passing test suite must provide absolute confidence that the software is ready for production.
**Speed:** Fast tests run often. Slow tests are ignored. The feedback loop must be tight.
**Isolation:** Tests must not depend on each other. They must be able to run in any order, in parallel.
**Determinism:** Flaky tests are worse than no tests. If a test fails 1% of the time without code changes, delete it or fix it immediately.
**Behavior over Implementation:** Test what the code does (the public API), not how it does it (private methods).

---

# The Test Pyramid

Structure your test suite to balance speed, cost, and confidence.

**Unit Tests (The Base):**
- **Focus:** Single functions or classes.
- **Speed:** Milliseconds.
- **Volume:** Should make up the vast majority (70-80%) of your test suite.
- **Dependencies:** Fully mocked. No network, no database, no file system.

**Integration Tests (The Middle):**
- **Focus:** Interaction between components (e.g., repository layer hitting a real database).
- **Speed:** Seconds.
- **Volume:** About 15-20% of the suite.
- **Dependencies:** Use real databases (via Testcontainers or local instances). Mock external third-party APIs.

**End-to-End (E2E) Tests (The Peak):**
- **Focus:** Complete user journeys through the entire stack (UI to Database).
- **Speed:** Minutes.
- **Volume:** Only 5-10% of the suite. Cover the critical path (e.g., signup, checkout).
- **Dependencies:** Real environments, real networks.

---

# Test-Driven Development (TDD)

TDD is a design process, not just a testing process.

**Red, Green, Refactor:**
1.  **Red:** Write a failing test for a new feature.
2.  **Green:** Write the absolute minimum code to make the test pass.
3.  **Refactor:** Clean up the code while the test keeps you safe.

TDD forces you to design decoupled, testable code from the beginning. It prevents the "how do I test this?" problem.

---

# Mocking and Stubbing

Use test doubles judiciously. Over-mocking leads to tests that pass while the system is broken.

**Stubs:** Provide canned answers to calls made during the test (e.g., returning a hardcoded user object).
**Mocks:** Verify that a specific method was called with specific parameters (e.g., verifying `emailService.send()` was called).
**The Rule:** Mock external boundaries (third-party APIs, email providers). Do not mock internal business logic or data structures.

---

# UI & Component Testing

Frontend testing requires specific strategies.

**Component Tests:** Render a single UI component in isolation (e.g., using React Testing Library). Assert on accessibility roles and text content, not CSS classes or DOM structure.
**Visual Regression Testing:** Automatically compare screenshots of components to detect unintended visual changes.
**Interaction Testing:** Simulate user events (clicks, typing) and verify state changes.

---

# Advanced Testing Techniques

When basic pyramids are not enough.

**Property-Based Testing:** Instead of writing specific inputs/outputs, define the properties of a function and let the framework generate hundreds of random inputs to find edge cases.
**Mutation Testing:** The framework intentionally introduces bugs (mutations) into your code. If your tests still pass, your test suite is weak.
**Chaos Engineering:** Intentionally inject failures into a production or staging system (e.g., killing a database node, introducing network latency) to prove the system degrades gracefully.

---

# Technical Debt in Testing

Test code is production code. Treat it with the same respect.

**DRY Tests:** Avoid massive setup blocks duplicated across files. Use factory patterns to generate test data.
**Descriptive Names:** Test names should read like documentation. Use the `Should_DoX_When_Y` convention.
**Clean Teardown:** Tests must clean up their own state (e.g., truncating database tables after an integration test).

---

# The ForgeCore Testing Standard

A production-ready test suite is:
- **Trustworthy:** When it is red, the build stops. When it is green, you deploy.
- **Fast:** Developers run it locally without context switching.
- **Resilient:** It does not break when you refactor internal implementation details.
- **Readable:** A new engineer can read the tests to understand the business requirements.

Tests are the executable documentation of your system. Write them clearly.
