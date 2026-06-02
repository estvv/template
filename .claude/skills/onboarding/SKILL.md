---
description: Guide developers through project setup and understanding. Use when helping new team members understand a codebase, setting up development environment, or documenting project conventions.
disable-model-invocation: true
---

## Onboarding Workflow

### 1. Environment Setup (Day 1)
- [ ] Clone repository
- [ ] Install dependencies
- [ ] Setup environment variables
- [ ] Run local development server
- [ ] Run tests locally

### 2. Architecture Overview (Day 1-2)
- [ ] Understand high-level architecture
- [ ] Identify key components
- [ ] Understand data flow
- [ ] Review ADRs for major decisions

### 3. Code Walkthrough (Day 2-3)
- [ ] Understand project structure
- [ ] Review key modules
- [ ] Understand coding conventions
- [ ] Review testing strategy

### 4. First Contribution (Day 3-5)
- [ ] Pick a good first issue
- [ ] Make a small change
- [ ] Go through PR process
- [ ] Get code review

## Key Files to Review

Point new developers to:

```
README.md               # Project overview, setup
CONTRIBUTING.md        # Contribution guidelines
ARCHITECTURE.md        # Architecture overview
docs/adr/              # Architecture decisions
.env.example           # Configuration template
package.json           # Dependencies, scripts
```

## Project Structure Explanation

Help new developers understand:

```
src/
  components/       # UI components
  services/         # Business logic
  utils/            # Utility functions
  types/            # TypeScript types
tests/
  unit/             # Unit tests
  integration/      # Integration tests
  e2e/              # End-to-end tests
docs/
  adr/              # Architecture Decision Records
  api/              # API documentation
```

## Common Questions to Answer

1. **How do I run the app?**
   ```bash
   npm install
   npm run dev
   ```

2. **How do I run tests?**
   ```bash
   npm test
   npm run test:coverage
   ```

3. **How do I create a new feature?**
   - Read ARCHITECTURE.md
   - Check existing patterns
   - Follow directory structure
   - Write tests first (TDD)

4. **What are the code conventions?**
   - See `.prettierrc`, `.eslintrc`
   - Read existing code
   - Follow naming patterns

5. **How do I deploy?**
   - See `CONTRIBUTING.md`
   - Follow deployment guide

## Good First Issues

Characteristics:
- Clear problem definition
- Limited scope
- Existing tests
- Non-critical
- Good for learning codebase

Label: `good-first-issue` or `beginner-friendly`

## Onboarding Checklist for New Developers

### Week 1
- [ ] Development environment working
- [ ] Understand project structure
- [ ] Read architecture documentation
- [ ] Complete first small task
- [ ] Understand testing approach

### Week 2
- [ ] Understand key modules deeply
- [ ] Complete 2-3 small tasks
- [ ] Participate in code review
- [ ] Understand deployment process

### Month 1
- [ ] Comfortable with codebase
- [ ] Completing tasks independently
- [ ] Contributing to code review
- [ ] Understand architecture decisions
- [ ] Helping with onboarding docs

## Related Skills

- Use **documentation** skill to improve onboarding docs
- Use **review** skill to review new developer PRs
- Use **testing** skill to teach testing patterns