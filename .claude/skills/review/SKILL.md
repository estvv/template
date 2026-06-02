---
description: Review code changes for correctness, security, performance, and maintainability. Use when reviewing pull requests, conducting code reviews, or evaluating changes for merge.
disable-model-invocation: true
argument-hint: <branch-or-file>
---

## Review Workflow

For reviewing diffs or pull requests with dynamic code loading:

### Phase 1: Quick Scan (2 min)
- PR size (large PRs harder to review)
- PR description (is it clear?)
- Test coverage (are there tests?)
- CI status (passing?)

### Phase 2: Understanding (5-10 min)
- Read PR description
- Understand the goal
- Identify key changes
- Note complex areas

### Phase 3: Detailed Review (15-30 min)
- Review each file
- Check logic correctness
- Check error handling
- Check edge cases
- Check tests

### Phase 4: Final Thoughts (5 min)
- Overall assessment
- Security considerations
- Performance implications
- Maintainability

## Dynamic Diff Review

<!-- Uncomment to automatically load git diff -->
<!-- !`git diff $ARGUMENTS` -->

Review the changes above for:

### Correctness
- Does code do what it's supposed to?
- Are edge cases handled?
- Are error cases handled?

### Security
- SQL injection prevention
- XSS prevention
- CSRF protection
- Input validation
- No secrets in code
- Proper authentication/authorization

### Performance
- Time complexity (avoid O(n²) when O(n) possible)
- Database queries (N+1 problem)
- Caching opportunities
- Network calls minimized

### Maintainability
- Code is readable
- Good naming
- Appropriate comments
- Follows patterns
- No duplication

### Testing
- Tests for new code
- Tests for bug fixes
- Edge cases tested
- Tests are meaningful

## Comment Severity

### Blocking (must fix)
```typescript
// BLOCKING: Security issue
"This query is vulnerable to SQL injection. 
Use parameterized queries: db.query('SELECT * FROM users WHERE id = ?', [userId])"
```

### Important (should fix)
```typescript
// IMPORTANT: May cause bugs
"This function doesn't handle the case where 'users' is empty."
```

### Minor (nice to have)
```typescript
// MINOR: Style preference
"Consider using 'const' instead of 'let' since this isn't reassigned."
```

### Nit (trivial)
```typescript
// NIT: Very minor
"Extra space here. Not important."
```

## Security Checklist

- [ ] Authentication/authorization checks
- [ ] Input validation
- [ ] Output encoding
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] CSRF protection
- [ ] No secrets committed
- [ ] Dependencies secure

## Performance Checklist

- [ ] Appropriate algorithms
- [ ] No N+1 queries
- [ ] Database indexed
- [ ] Caching appropriate
- [ ] No memory leaks

See `review-checklist.md` for comprehensive checklist.

## Related Skills

- Use **testing** skill to verify test quality
- Use **refactoring** skill for suggested improvements
- Use **security** instructions from safety.md