# Test Strategy

## Goal and Scope

Demonstrate test design and reliable UI automation for the core money
transfer flow: validation, submission, balance updates, and transaction history.

The primary focus is Compose UI testing supported by exploratory testing.
Unit tests supplement selected business rules where they add value.

The app uses simulated banking data. Real banking integration, performance,
and comprehensive security testing are outside the initial scope.

## Behavior Scenarios

Describe test cases using Gherkin:

- **Given:** initial conditions and test data.
- **When:** a user action or event.
- **Then:** an observable expected result.
- **And:** an additional condition or result.

Expected results must follow documented requirements or explicitly accepted
project assumptions. Existing behavior alone does not prove correctness.

Scenarios currently serve as documentation, not executable tests.
Cucumber is not required. Future automated tests will reference scenario IDs.

Keep execution results separate from test case definitions.

## Priorities

Focus on user-visible risks:

- Incorrect balance changes after a transfer.
- Duplicate transfers caused by repeated submission.
- Incorrect validation or availability of the Proceed button.
- Missing or incorrect transaction history entries.
- Unclear confirmation and error messages.

## Test Approach

| Test type | Role |
|---|---|
| Compose UI tests — primary | Verify form behavior, navigation, validation, and selected complete user flows |
| Manual testing — supporting | Explore behavior, clarify requirements, and investigate failures |
| Unit tests — supplementary | Check selected calculations or validation rules that are difficult to exercise through the UI |

Cover successful operations, failures, and boundary values accessible
through the interface.

UI restrictions do not prove that the underlying logic rejects invalid data.
Document these coverage limits and add targeted unit tests where useful.

Keep feature-specific requirements and test cases in separate documents.

## Working Rules

- Each automated test prepares its own data and runs independently.
- Before each manual test, establish its stated preconditions.
- Use Compose semantics to locate elements; add test tags where necessary.
- Use controlled dependencies and proper synchronization instead of fixed sleeps.
- State which dependencies are replaced in each test suite.
- Do not claim full-flow coverage when the relevant business behavior is mocked.
- Record failures with reproduction steps and expected versus actual results.
- Add regression tests for fixed defects where practical.

## Execution and Completion

Run UI tests on an emulator, initially Pixel 6 with Android 15 / API 35.
Run supplementary unit tests locally.
Automate selected checks in CI during M4.

Testing is complete for the agreed scope when high-priority cases have
been executed, failures investigated, no unresolved defect blocks the
core flow, and remaining limitations are documented.

Update this strategy as the project evolves.