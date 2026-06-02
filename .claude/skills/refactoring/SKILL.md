---
description: Improve code structure while preserving behavior. Use when simplifying code, eliminating duplication, improving readability, or reducing technical debt.
disable-model-invocation: true
argument-hint: <file-or-pattern>
---

## Refactoring Principle

**Change internal structure without changing external behavior.**

Critical rule: Never refactor without tests. If tests are missing, write them first.

## Refactoring Workflow

1. **Verify tests pass** (all green)
2. **Make one small change**
3. **Run tests** (must still be green)
4. **Commit** if green
5. **Repeat** from step 2

## When to Refactor

- Adding a new feature (make it easier to add)
- Fixing a bug (understand the code better)
- Code review feedback
- Duplicate code found
- Long method (> 20 lines)
- Complex conditionals
- Code hard to understand

## Common Refactorings

### Extract Method

```typescript
// Before
function printOwing(invoice) {
  console.log('***********************');
  console.log('*** Customer Owes ***');
  console.log('***********************');
  let outstanding = 0;
  for (const order of invoice.orders) {
    outstanding += order.amount;
  }
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}

// After
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding(invoice);
  printDetails(invoice, outstanding);
}
```

### Rename Variable/Method

```typescript
// Before
function calc(a, b) {
  return a * b * 0.15;
}

// After
function calculateSubtotal(price: number, quantity: number): number {
  const TAX_RATE = 0.15;
  return price * quantity * (1 + TAX_RATE);
}
```

### Remove Duplication

```typescript
// Before
function calculateRectangleArea(width: number, height: number) {
  return width * height;
}

function calculateSquareArea(side: number) {
  return side * side;
}

// After
function calculateArea(width: number, height: number = width) {
  return width * height;
}
```

## Refactoring Checklist

Before starting:
- [ ] Tests exist and pass
- [ ] You understand the code
- [ ] Clear goal for improvement

During refactoring:
- [ ] One change at a time
- [ ] Tests pass after each change
- [ ] No behavior change
- [ ] Commit frequently

After refactoring:
- [ ] All tests pass
- [ ] Code is more readable
- [ ] No commented-out code
- [ ] No new warnings

See `refactoring-checklist.md` for comprehensive checklist.
See `refactoring-patterns.md` for more patterns.

## Code Smells to Refactor

| Smell | Refactoring |
|-------|-------------|
| Long method | Extract method |
| Large class | Extract class |
| Duplicate code | Extract method/class |
| Long parameter list | Use parameter object |
| Divergent change | Extract class |
| Shotgun surgery | Move method/class |
| Feature envy | Move method |
| Primitive obsession | Replace with object |

Use **review** skill to identify code smells.
Use **testing** skill to write tests before refactoring.