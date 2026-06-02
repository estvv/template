# Code Review Checklist

Comprehensive checklist for reviewing pull requests.

## Before Reviewing

### Author Checklist (for reference)
- [ ] Code compiles/builds
- [ ] All tests pass
- [ ] Self-reviewed own code
- [ ] PR description explains what/why
- [ ] Linked to relevant issue
- [ ] Screenshots for UI changes
- [ ] No secrets in code

### Reviewer Preparation
- [ ] Understand PR description
- [ ] Understand the goal
- [ ] Check PR size (large PRs may need more time)
- [ ] Check if CI passed
- [ ] Allocate enough time (15-30 min for medium PR)

## Code Quality

### Correctness
- [ ] Code does what it's supposed to
- [ ] Edge cases handled
- [ ] Error cases handled
- [ ] Logic is correct

### Readability
- [ ] Code is easy to understand
- [ ] Good naming conventions
- [ ] Appropriate comments (why, not what)
- [ ] Consistent style
- [ ] Follows project conventions

### Maintainability
- [ ] Code is easy to modify
- [ ] No unnecessary complexity
- [ ] Follows patterns
- [ ] No duplication (DRY)
- [ ] Functions/methods not too long
- [ ] Classes/modules not too big

### Performance
- [ ] Time complexity acceptable
- [ ] Space complexity acceptable
- [ ] No N+1 queries
- [ ] Appropriate caching
- [ ] Network calls minimized
- [ ] Algorithms efficient

## Testing

### Test Coverage
- [ ] New code has tests
- [ ] Bug fixes have reproducing tests
- [ ] Tests cover happy path
- [ ] Tests cover edge cases
- [ ] Tests cover error cases

### Test Quality
- [ ] Tests are meaningful
- [ ] Tests are deterministic
- [ ] Tests are fast
- [ ] Tests are readable
- [ ] Tests use appropriate mocking

## Security

### Input/Output
- [ ] All input validated
- [ ] All output encoded
- [ ] No SQL injection
- [ ] No XSS vulnerabilities
- [ ] No CSRF vulnerabilities
- [ ] File uploads validated

### Authentication/Authorization
- [ ] Authentication checks in place
- [ ] Authorization checks in place
- [ ] Proper session management
- [ ] Tokens have reasonable expiration

### Data Protection
- [ ] No secrets in code
- [ ] No secrets in logs
- [ ] Sensitive data encrypted
- [ ] HTTPS used
- [ ] Proper error handling (no sensitive data in errors)

### Dependencies
- [ ] Dependencies up-to-date
- [ ] No known vulnerabilities
- [ ] Dependencies from trusted sources

## Error Handling

### Error Messages
- [ ] Errors are caught and handled
- [ ] Error messages are informative
- [ ] Error messages don't expose sensitive data
- [ ] User-facing errors are friendly
- [ ] Errors are logged appropriately

### Edge Cases
- [ ] Null/undefined handled
- [ ] Empty inputs handled
- [ ] Invalid inputs handled
- [ ] Network failures handled
- [ ] Timeouts handled

## Documentation

### Code Documentation
- [ ] Complex logic explained
- [ ] Public APIs documented
- [ ] Comments help, not clutter

### Project Documentation
- [ ] README updated if needed
- [ ] API documentation updated if needed
- [ ] Architecture docs updated if needed
- [ ] CHANGELOG updated
- [ ] Migration guides if needed

## Design

### Architecture
- [ ] Follows project architecture
- [ ] Appropriate patterns used
- [ ] Dependencies appropriate
- [ ] Coupling minimized
- [ ] Cohesion maximized

### API Design
- [ ] Endpoints follow conventions
- [ ] Request/response format appropriate
- [ ] Error responses consistent
- [ ] Status codes correct
- [ ] API versioning considered

### Database
- [ ] Schema changes appropriate
- [ ] Indexes added where needed
- [ ] Query performance acceptable
- [ ] Data integrity maintained
- [ ] Migration backward-compatible (if applicable)

## Git Practices

### Commit
- [ ] Commit messages clear
- [ ] One concern per commit
- [ ] Appropriate commit size
- [ ] No unrelated changes

### Branch
- [ ] Branch up-to-date with main
- [ ] No merge conflicts
- [ ] Appropriate branch name

## Review Comments

### Tone
- [ ] Focus on code, not person
- [ ] Ask questions, don't demand
- [ ] Explain the "why"
- [ ] Acknowledge good work
- [ ] Use "we" not "you"

### Content
- [ ] Comments are specific
- [ ] Comments provide alternatives
- [ ] Comments explain reasoning
- [ ] Severity clearly marked (blocking/important/minor/nit)

## After Review

### Approving
- [ ] All concerns addressed
- [ ] All discussions resolved
- [ ] Approved with appropriate status
- [ ] Any follow-up items noted

### Requesting Changes
- [ ] All blocking issues clearly marked
- [ ] Clear instructions for resolution
- [ ] Offered to help or discuss
- [ ] Being constructive

## PR Size Guidelines

- **Small (< 200 lines):** Easy review, ~15-30 min
- **Medium (200-400 lines):** Manageable, ~30-60 min
- **Large (400-1000 lines):** Difficult, consider splitting
- **Huge (> 1000 lines):** Split into multiple PRs

## Review Time Guidelines

| PR Size | Review Time | Priority |
|---------|-------------|----------|
| Small | < 30 min | Same day |
| Medium | < 60 min | Within 24 hours |
| Large | 1-2 hours | Within 48 hours |

## Anti-Patterns to Avoid

- [ ] Rubber stamping (approving without reviewing)
- [ ] Nitpicking (focusing on minors)
- [ ] Blocking on style preferences
- [ ] Delaying reviews unnecessarily
- [ ] Being harsh or condescending
- [ ] Not explaining the "why"
- [ ] Not reading PR description
- [ ] Not considering context