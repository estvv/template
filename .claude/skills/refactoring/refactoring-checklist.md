# Refactoring Checklist

Comprehensive checklist for safe refactoring.

## Before Starting Refactoring

### Have Tests
- [ ] Tests exist for code being refactored
- [ ] All tests pass
- [ ] Coverage is adequate (> 70%)

### Understand Code
- [ ] You understand what the code does
- [ ] You understand why it does it that way
- [ ] You have a clear goal for refactoring
- [ ] You've identified the code smell

### Prepare
- [ ] Branch is clean (no unrelated changes)
- [ ] You're not on a tight deadline
- [ ] You have time to do it right
- [ ] You have plan for incremental refactoring

## During Refactoring

### Make Small Changes
- [ ] One refactoring at a time
- [ ] Each change is atomic
- [ ] Each change is reversible
- [ ] You could stop at any point

### Verify Each Step
- [ ] Tests pass after each change
- [ ] Behavior unchanged
- [ ] No new warnings
- [ ] No new errors

### Commit Frequently
- [ ] Commit after each successful refactoring
- [ ] Commit message describes refactoring
- [ ] Easy to revert if needed
- [ ] Easy to review changes

### Don't Mix Concerns
- [ ] Not mixing refactoring with features
- [ ] Not mixing refactoring with bug fixes
- [ ] Not mixing refactorings together
- [ ] One concern per commit

## Common Refactorings

### Extract Method
- [ ] Method name clearly describes purpose
- [ ] Method is at the right level of abstraction
- [ ] Method has clear inputs and outputs
- [ ] Method is testable independently

### Rename Variable/Method
- [ ] Name accurately describes purpose
- [ ] Name follows project conventions
- [ ] Name is neither too short nor too long
- [ ] Name is easy to pronounce

### Remove Duplication
- [ ] Identified all duplicated code
- [ ] Extracted into appropriate abstraction
- [ ] No over-abstraction
- [ ] Tests still pass

### Simplify Conditional
- [ ] Logic is easier to understand
- [ ] No nested conditionals if possible
- [ ] Guard clauses used appropriately
- [ ] Boolean logic simplified

### Extract Class
- [ ] New class has single responsibility
- [ ] Clear interface defined
- [ ] Dependencies explicit
- [ ] Tests updated

### Introduce Parameter Object
- [ ] Logical grouping of parameters
- [ ] Object is well-named
- [ ] Used consistently
- [ ] Tests updated

## After Refactoring

### Verify
- [ ] All tests pass
- [ ] No behavior changed
- [ ] Code is more readable
- [ ] Code is more maintainable
- [ ] No commented-out code
- [ ] No new warnings

### Clean Up
- [ ] No TODO comments left
- [ ] No debug code left
- [ ] No commented-out code
- [ ] Imports/dependencies cleaned up

### Commit
- [ ] Commit message explains refactoring
- [ ] Commit is atomic (one refactoring)
- [ ] Easy to review
- [ ] Easy to revert

### Document
- [ ] Updated comments if needed
- [ ] Updated documentation if needed
- [ ] Updated ADR if architectural change
- [ ] Team notified of significant refactoring

## Code Smells to Address

### Bloaters
- [ ] Long method (> 20 lines)
- [ ] Large class (> 500 lines)
- [ ] Primitive obsession
- [ ] Long parameter list (> 3 parameters)
- [ ] Data clumps

### Object-Orientation Abusers
- [ ] Switch statements (use polymorphism)
- [ ] Temporary fields
- [ ] Refused bequest
- [ ] Alternative classes with different interfaces

### Change Preventers
- [ ] Divergent change (one class changes for multiple reasons)
- [ ] Shotgun surgery (one change requires many classes)
- [ ] Parallel inheritance hierarchies

### Dispensables
- [ ] Comments explaining bad code (refactor instead)
- [ ] Duplicate code
- [ ] Lazy class
- [ ] Data class
- [ ] Dead code
- [ ] Speculative generality

### Couplers
- [ ] Feature envy (method uses other class more)
- [ ] Inappropriate intimacy
- [ ] Message chains
- [ ] Middle man
- [ ] Incomplete library class

## Refactoring Anti-Patterns

### Don't
- [ ] Refactor without tests
- [ ] Refactor on a deadline
- [ ] Big bang refactoring
- [ ] Gold plating
- [ ] Mix refactoring with features

## Performance Considerations

### Don't Prematurely Optimize
- [ ] Profile before optimizing
- [ ] Refactor for clarity first
- [ ] Optimize only measured bottlenecks
- [ ] Keep readability until proven slow

### When Optimizing
- [ ] Have performance test
- [ ] Measure before and after
- [ ] Document optimization
- [ ] Consider trade-offs

## Legacy Code

### Characterization Tests
Before refactoring legacy code:
- [ ] Write tests that capture current behavior
- [ ] Test inputs and outputs
- [ ] Don't worry about code quality yet
- [ ] Focus on coverage

### Dependency Breaking
- [ ] Identify dependencies
- [ ] Introduce seams
- [ ] Use dependency injection
- [ ] Mock external dependencies

## Tools to Use

### IDE Refactoring Support
- Rename symbol
- Extract method/function
- Extract variable
- Inline variable/method
- Move symbol

### Linting Tools
- ESLint (JavaScript/TypeScript)
- Pylint (Python)
- Clippy (Rust)
- RuboCop (Ruby)

### Code Quality Tools
- SonarQube
- Code Climate
- ESLint complexity rules
- Cyclomatic complexity checkers