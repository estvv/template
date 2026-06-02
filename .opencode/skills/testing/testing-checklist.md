# Testing Checklist

Comprehensive checklist for writing and reviewing tests.

## Before Writing Tests

- [ ] Understand requirements completely
- [ ] Identify happy path scenarios
- [ ] Identify edge cases
- [ ] Identify error cases
- [ ] Know the expected behavior

## Unit Tests

### Structure
- [ ] Test file co-located with source file or in `__tests__/` directory
- [ ] Descriptive test names (should/when/then pattern)
- [ ] One concept per test
- [ ] AAA pattern (Arrange, Act, Assert)

### Coverage
- [ ] Happy path tested
- [ ] Edge cases tested (empty, null, undefined, extreme values)
- [ ] Error cases tested (invalid input, exceptions)
- [ ] Boundary conditions tested

### Quality
- [ ] Test is isolated (no dependencies on other tests)
- [ ] Test is deterministic (same result every time)
- [ ] Test is fast (< 100ms)
- [ ] Test is readable
- [ ] Mocks are used appropriately (mock external dependencies, not domain logic)

### Assertions
- [ ] Assertions are specific (not `expect(result).toBeTruthy()`)
- [ ] Assertions are meaningful (test the right thing)
- [ ] Assertions are complete (test all relevant properties)

## Integration Tests

### Scope
- [ ] Testing real interactions between components
- [ ] Using real dependencies (test database, etc.)
- [ ] Not testing implementation details
- [ ] Testing boundaries correctly

### Setup/Teardown
- [ ] Database is setup before tests
- [ ] Database is cleaned after tests
- [ ] Test data is loaded correctly
- [ ] Resources are released properly

### Quality
- [ ] Tests don't interfere with each other
- [ ] Tests can run in any order
- [ ] Tests can run in parallel (if designed for it)

## E2E Tests

### Scope
- [ ] Testing critical user flows
- [ ] Testing from user perspective
- [ ] Minimal tests (E2E tests are slow and brittle)

### Quality
- [ ] Tests are independent
- [ ] Tests wait for async operations properly
- [ ] Tests handle flakiness appropriately
- [ ] Tests have reasonable timeouts

## Test Data

- [ ] Test data is realistic (not "foo", "bar")
- [ ] Test data is meaningful
- [ ] Test data doesn't contain sensitive information
- [ ] Test data is created/managed correctly

## Mocking

- [ ] Mock at the right level (mock interfaces, not implementations)
- [ ] Mocks are verified if important (verify mock was called)
- [ ] Mocks don't over-specify (don't mock trivial behavior)
- [ ] Mocks are understandable

## Test Maintenance

- [ ] Tests fail for the right reason
- [ ] Tests don't fail randomly (flaky tests)
- [ ] Tests are easy to update when requirements change
- [ ] Tests don't break when implementation changes (only interface changes)

## Running Tests

### Pre-commit
- [ ] All tests pass locally
- [ ] Coverage threshold met
- [ ] Linting passes

### CI/CD
- [ ] Tests run in CI
- [ ] Tests pass in CI environment
- [ ] Test results are reported
- [ ] Coverage is tracked

## Anti-patterns to Avoid

- [ ] Testing private methods directly (test public API)
- [ ] Testing implementation details (test behavior)
- [ ] Overspecified tests (testing too much)
- [ ] Mystery guest (unexplained data)
- [ ] Test logic in production code
- [ ] Mocking everything (test should test real behavior)
- [ ] Shared mutable state between tests

## Test Smells

- [ ] Test names like "test1", "test2", etc.
- [ ] Tests with no assertions
- [ ] Tests that sometimes pass, sometimes fail (flaky)
- [ ] Tests that take > 1 second (slow)
- [ ] Tests that depend on each other
- [ ] Tests that require specific order
- [ ] Tests with hardcoded values and no explanation