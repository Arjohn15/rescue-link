# Reusable Coding Standards

## 1. Purpose

This document defines a reusable set of coding standards intended for use across projects, teams, and codebases. The goal is to improve readability, maintainability, reliability, and collaboration without being overly language-specific.

These standards should be adopted as a baseline. Projects may add stricter rules when needed, but they should avoid weakening the principles defined here.

## 2. Core Principles

### 2.1 Optimize for readability
Code is read more often than it is written. Favor clarity over cleverness.

### 2.2 Prefer consistency over personal style
A consistent codebase is easier to navigate than one that reflects individual preferences.

### 2.3 Keep things simple
Choose the simplest design that correctly solves the problem.

### 2.4 Make change safe
Write code that is easy to test, review, and modify.

### 2.5 Design for maintainability
Assume someone unfamiliar with the code will need to understand and extend it later.

## 3. General Standards

### 3.1 Naming
- Use descriptive, intention-revealing names.
- Prefer full words over abbreviations unless the abbreviation is widely understood.
- Avoid vague names such as `data`, `temp`, `value`, `obj`, or `misc`.
- Name variables for what they represent, functions for what they do, and classes/modules for what they model.
- Use consistent naming patterns within a project.

Examples:
- Good: `customerEmail`, `retryCount`, `calculateInvoiceTotal`
- Poor: `x`, `tmp`, `doStuff`

### 3.2 Functions and methods
- Keep functions focused on one responsibility.
- Prefer small functions that are easy to understand and test.
- Limit parameter count where possible; group related parameters into structured objects when appropriate.
- Avoid hidden side effects.
- Prefer explicit inputs and outputs.
- Use early returns to reduce nesting when it improves clarity.

### 3.3 Complexity
- Avoid deeply nested logic.
- Break complex conditions into well-named intermediate variables or helper functions.
- Remove duplication when it improves maintainability, but do not abstract prematurely.
- Favor composition over inheritance unless inheritance is clearly justified.

### 3.4 Comments
- Write code that is clear enough to minimize the need for comments.
- Use comments to explain **why**, not **what**, unless the code cannot reasonably express intent on its own.
- Keep comments accurate and update them when behavior changes.
- Do not leave dead, misleading, or commented-out code in the codebase.

### 3.5 Formatting
- Use automated formatting tools whenever possible.
- Follow project-standard line length, indentation, spacing, and import ordering rules.
- Do not manually fight the formatter.
- Separate logical sections of code with whitespace to improve readability.

### 3.6 File and module organization
- Keep files focused and reasonably sized.
- Group related logic together.
- Avoid circular dependencies.
- Organize modules by domain or feature when practical, not only by technical layer.
- Keep public interfaces small and intentional.

## 4. Error Handling

- Handle errors explicitly and as close to the source as practical.
- Do not silently swallow exceptions or failures.
- Return or raise errors with useful context.
- Log errors at appropriate boundaries.
- Show safe, actionable messages to users; keep internal details out of user-facing errors.
- Use retries only when failure is expected to be transient and retry behavior is bounded.

## 5. Logging and Observability

- Log meaningful events, not noise.
- Use structured logging where supported.
- Do not log secrets, passwords, tokens, or sensitive personal data.
- Include enough context in logs to support debugging and tracing.
- Use appropriate log levels consistently.
- Prefer metrics and traces for operational insight where available.

## 6. Testing Standards

### 6.1 General expectations
- All non-trivial code should be covered by automated tests.
- Tests should be reliable, deterministic, and fast enough for regular execution.
- Every bug fix should include a test that would have caught the issue when practical.

### 6.2 Test design
- Test observable behavior, not implementation details, unless there is a strong reason.
- Keep tests clear and focused.
- Prefer one logical assertion per behavior under test, even if multiple checks are needed.
- Use descriptive test names that explain the scenario and expected outcome.
- Avoid brittle tests that fail due to unrelated refactoring.

### 6.3 Test pyramid
- Favor unit tests for core logic.
- Add integration tests for component interaction, data access, and external boundaries.
- Use end-to-end tests selectively for critical flows.

### 6.4 Test data
- Use minimal, readable fixtures.
- Avoid hidden dependencies between tests.
- Ensure tests can run independently and in any order.

## 7. Security Standards

- Treat all external input as untrusted.
- Validate and sanitize inputs at system boundaries.
- Use parameterized queries or safe ORM patterns to avoid injection risks.
- Apply least privilege for services, credentials, and access control.
- Never hardcode secrets in source code.
- Store secrets in approved secret-management systems.
- Keep dependencies current and remove unused dependencies.
- Review authentication, authorization, and data exposure paths carefully.
- Avoid leaking sensitive information through logs, errors, or APIs.

## 8. Performance Standards

- Make correctness and clarity the default; optimize when measurement shows a need.
- Consider performance implications for hot paths, large datasets, and repeated operations.
- Avoid unnecessary allocations, blocking operations, and redundant work in performance-sensitive code.
- Use caching deliberately and define invalidation behavior clearly.
- Measure before and after optimization changes.
- Document non-obvious performance tradeoffs.

## 9. API and Interface Design

- Make interfaces easy to understand and hard to misuse.
- Prefer explicit contracts over implicit behavior.
- Keep APIs consistent in naming, error handling, and response structure.
- Version public APIs when breaking changes are possible.
- Validate inputs and fail clearly on invalid usage.
- Document assumptions, side effects, and edge cases.

## 10. Data and State Management

- Keep state changes explicit and predictable.
- Minimize mutable shared state.
- Prefer immutability where practical.
- Define ownership and lifecycle for important data.
- Be careful with concurrency, asynchronous flows, and race conditions.
- Use transactions or equivalent safeguards for multi-step critical updates.

## 11. Dependency Management

- Add dependencies only when they provide clear value.
- Prefer well-maintained, trusted libraries over niche or unreviewed packages.
- Remove unused dependencies promptly.
- Pin or constrain versions based on project requirements.
- Understand the security, licensing, and operational impact of third-party packages.

## 12. Documentation Standards

- Document public modules, APIs, commands, and major workflows.
- Keep setup, run, test, and deployment instructions current.
- Capture important architectural decisions in lightweight design records when needed.
- Write README files for humans new to the project.
- Include examples for non-obvious usage.

## 13. Code Review Standards

- All production code changes should go through review unless an emergency process is explicitly defined.
- Reviews should focus on correctness, readability, maintainability, security, and test coverage.
- Keep pull requests reasonably scoped.
- Provide context in pull request descriptions, including purpose, approach, risks, and test evidence.
- Review feedback should be specific, respectful, and actionable.
- Resolve comments clearly rather than silently.

## 14. Version Control Standards

- Commit related changes together.
- Write clear commit messages that explain intent.
- Avoid mixing refactoring, formatting-only changes, and functional changes unless necessary.
- Keep the main branch releasable when possible.
- Rebase or merge according to project conventions.
- Do not commit generated files, secrets, or local environment artifacts unless intentionally required.

## 15. Configuration and Environment

- Store configuration outside code where practical.
- Use environment-specific configuration safely and consistently.
- Provide sensible defaults for local development.
- Validate required configuration at startup where possible.
- Keep local development, CI, and production behavior aligned as much as practical.

## 16. CI/CD Expectations

- Automated checks should run on every change set.
- At minimum, CI should run formatting checks, static analysis, and relevant tests.
- Broken builds should be treated as a priority.
- Deployment automation should be repeatable, observable, and reversible.
- Prefer incremental, low-risk releases where possible.

## 17. Language-Specific Extensions

Each project should supplement this standard with language-specific rules covering topics such as:
- naming conventions
- formatter and linter choices
- type system usage
- package structure
- framework patterns
- test tooling
- dependency management

Examples:
- TypeScript: strict typing, avoid `any`, enable strict compiler settings
- Python: use type hints for public functions, follow formatter/linter standards
- Java: package conventions, null handling, immutability guidance
- Go: idiomatic error handling, package boundaries, interface usage

## 18. Definition of Done

A change is complete only when:
- the code is understandable and follows project standards
- tests are added or updated as needed
- documentation is updated where relevant
- security and operational concerns are considered
- reviewers can understand the purpose and impact of the change
- the change passes required automated checks

## 19. Exceptions

Exceptions to these standards should be rare, intentional, and documented when they matter. When deviating, explain the reason and the tradeoff.

## 20. Recommended Enforcement

To keep these standards practical and sustainable, each project should define:
- a formatter
- a linter or static analysis toolset
- a test command for local and CI use
- a pull request template
- a lightweight architecture/documentation convention

## 21. Quick Summary

Write code that is easy to read, easy to test, safe to change, and hard to misuse. Prefer consistency, explicitness, and simplicity. Use automation to enforce style, and use reviews and tests to protect quality.
