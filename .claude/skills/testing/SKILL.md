---
description: Execute TDD workflow, write tests, verify coverage. Use when writing tests, improving coverage, implementing TDD, or fixing bugs with test-first approach.
disable-model-invocation: true
argument-hint: <file-or-pattern>
---

## Test-Driven Development (TDD)

Follow the Red-Green-Refactor cycle:

1. **Red:** Write a failing test
2. **Green:** Write minimal code to make it pass
3. **Refactor:** Clean up while keeping tests green

## Test Types

Choose the right test type:

- **Unit:** Isolated functions/classes (< 100ms, mock dependencies)
- **Integration:** Component interactions (use real dependencies)
- **E2E:** Complete user flows (critical paths only)

## Coverage Standards

- **Minimum:** 70% line coverage
- **Target:** 80% line coverage
- **Critical paths:** 90%+ coverage

Run coverage: `npm test -- --coverage` or equivalent for your framework.

## Testing Workflow

```
New Feature → Write failing test → Run test (red) → 
Implement → Run test (green) → Refactor → Run test (green) → Commit
```

## Bug Fix Workflow

```
Bug Report → Write test that reproduces bug → Verify test fails → 
Fix bug → Verify test passes → Commit (test + fix together)
```

## Test Structure

Use AAA pattern (Arrange, Act, Assert):

```typescript
it('should calculate total with tax', () => {
  // Arrange
  const items = [{ price: 10 }, { price: 20 }];
  const taxRate = 0.1;

  // Act
  const total = calculateTotal(items, taxRate);

  // Assert
  expect(total).toBe(33);
});
```

## Quick Checklist

Before committing:

- [ ] All tests pass
- [ ] New code has tests
- [ ] Bug fixes have reproducing tests
- [ ] Edge cases covered
- [ ] Coverage meets threshold

See `testing-checklist.md` for comprehensive checklist.
See `testing-patterns.md` for common patterns and examples.

## Related Skills

- Use **refactoring** skill when cleaning up code (keep tests green)
- Use **debugging** skill when tests fail unexpectedly
- Use **review** skill to review test quality