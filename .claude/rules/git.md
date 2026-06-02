# Git Commit Conventions

## Commit Message Format

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

## Commit Types

Use these types consistently:

- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation only
- `style` - Formatting, missing semicolons, etc. (no code change)
- `refactor` - Code refactoring (no behavior change)
- `perf` - Performance improvement
- `test` - Adding/updating tests
- `chore` - Maintenance tasks (build, CI, dependencies)
- `ci` - CI/CD changes
- `build` - Build system changes
- `revert` - Revert a previous commit

## Scopes

Scopes are optional but recommended for larger projects:

```
feat(auth): add OAuth2 login
fix(api): resolve memory leak
docs(readme): update installation guide
```

Common scopes:
- `api` - API endpoints
- `ui` - User interface
- `db` - Database
- `auth` - Authentication
- `config` - Configuration
- `deps` - Dependencies
- `cli` - Command line interface

## Subject Rules

1. Use imperative mood ("add", not "added" or "adds")
2. Don't capitalize first letter
3. No period at the end
4. Maximum 50 characters
5. Be specific and descriptive

**Good:**
```
feat(auth): add JWT token refresh
fix(db): resolve connection timeout on high load
```

**Bad:**
```
Added JWT token refresh support.
Fixed the DB connection issue.
updates
```

## Body Rules

1. Use imperative mood
2. Separate from subject with blank line
3. Explain *what* and *why*, not *how*
4. Wrap at 72 characters
5. Can contain multiple paragraphs

**Example:**
```
feat(auth): add JWT token refresh

Users can now stay logged in indefinitely without needing to
re-authenticate. Access tokens expire after 15 minutes, but
refresh tokens allow obtaining new access tokens.

Closes #123
```

## Footer Rules

Use footer for:
- Breaking changes: `BREAKING CHANGE: <description>`
- Issue references: `Closes #123` or `Fixes #456`
- PR references: `Related to #789`

**Breaking Change Example:**
```
feat(api): migrate to v2 API endpoints

BREAKING CHANGE: All v1 endpoints are removed. Migrate to v2
endpoints by updating the base URL from /api/v1 to /api/v2.
```

## Examples

### Feature Addition
```
feat(dashboard): add real-time notifications

Implement WebSocket connection for real-time updates on the
dashboard. Shows new notifications immediately without page
refresh.

- Add WebSocket client connection
- Implement notification dispatcher
- Add UI toast notifications

Closes #234
```

### Bug Fix
```
fix(upload): handle large file uploads correctly

Files larger than 10MB were failing due to timeout. Increase
timeout and add progress indicator for better UX.

Fixes #456
```

### Refactoring
```
refactor(core): simplify user validation logic

Extract user validation into separate validator class. Improves
testability and reduces code duplication.

- Create UserValidator class
- Move validation logic from User model
- Add unit tests for validator

No behavior change.
```

### Documentation
```
docs(api): add authentication flow diagram

Add sequence diagram showing OAuth2 authentication flow with
all error cases documented.
```

### Performance
```
perf(search): optimize database queries

Add composite index on user_id and created_at columns.
Reduces query time from 2s to 50ms for user activity search.

Before: Sequential scan on activities table
After: Index scan using idx_user_created
```

## Pre-commit Checklist

Before committing, verify:

- [ ] Message follows format: `<type>(<scope>): <subject>`
- [ ] Type is one of: feat, fix, docs, style, refactor, perf, test, chore, ci, build, revert
- [ ] Subject is in imperative mood, lowercase, no period
- [ ] Body (if present) explains what and why
- [ ] Breaking changes documented with `BREAKING CHANGE:`
- [ ] Issue references added (Closes #123, Fixes #456)
- [ ] Scope matches the component being changed

## When to Use Each Type

| Type | When to Use | Example |
|------|-------------|---------|
| `feat` | New functionality | Adding login feature |
| `fix` | Bug fix | Fix login crash |
| `docs` | Documentation only | Update README |
| `style` | Code style only | Fix indentation |
| `refactor` | Code change, no behavior change | Simplify logic |
| `perf` | Performance improvement | Optimize query |
| `test` | Test changes | Add unit test |
| `chore` | Maintenance | Update dependencies |
| `ci` | CI/CD changes | Update GitHub Actions |
| `build` | Build system | Update webpack config |
| `revert` | Revert commit | Undo previous change |

## Commit Message Anti-Patterns

❌ **Avoid these:**

- Vague messages: "fix bugs", "updates", "changes"
- Mixed concerns: "add feature and fix bug and update docs"
- Unrelated scope: `feat(api): update landing page`
- Past tense: "fixed bug" instead of "fix bug"
- Period in subject: "fix bug."
- Too long subjects: "this is a very very very very long commit message that exceeds 50 characters"

✅ **Do this instead:**

- Specific: "fix null pointer exception in user service"
- Focused: One concern per commit
- Correct scope: `feat(ui): add login form`
- Imperative: "fix bug" not "fixed bug"
- Short subject: Maximum 50 characters
- Clear scope: Matches the component changed

## Multiple Commits

For larger changes, split into logical commits:

```bash
# Instead of one large commit:
git commit -m "add user authentication with JWT and tests and docs"

# Do multiple focused commits:
git commit -m "feat(auth): add JWT authentication"
git commit -m "test(auth): add JWT authentication tests"
git commit -m "docs(auth): document JWT authentication flow"
```

## Git Hooks

Consider using git hooks (via `husky`, `pre-commit`, etc.) to:

1. Validate commit message format
2. Run linters
3. Run tests
4. Check for secrets

**Example commit-msg hook:**
```bash
#!/bin/sh
# Validate commit message format

commit_msg=$(cat "$1")

if ! echo "$commit_msg" | grep -qE "^(feat|fix|docs|style|refactor|perf|test|chore|ci|build|revert)(\(.+\))?: .{1,50}"; then
  echo "Invalid commit message format!"
  echo "Format: <type>(<scope>): <subject>"
  echo "Types: feat, fix, docs, style, refactor, perf, test, chore, ci, build, revert"
  exit 1
fi
```

## TL;DR

1. **Format:** `<type>(<scope>): <subject>`
2. **Types:** feat, fix, docs, style, refactor, perf, test, chore, ci, build, revert
3. **Subject:** Imperative, lowercase, no period, max 50 chars
4. **Body:** Explain what and why, wrap at 72 chars
5. **Footer:** Breaking changes, issue references
6. **One concern per commit**

---

**References:**
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Best Practices](https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project)