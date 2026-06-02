# Skills Directory

This directory contains reusable instruction modules (skills) that help AI assistants perform specific tasks effectively.

## What are Skills?

Skills are modular, focused instruction sets that:
- Guide specific workflows (testing, refactoring, debugging, etc.)
- Use progressive disclosure (brief overview + detailed checklists)
- Are optimized for token efficiency
- Work across AI platforms (OpenCode, Claude Code)

## Directory Structure

```
skills/
├── testing/
│   ├── SKILL.md              # Main skill file (concise)
│   ├── testing-checklist.md  # Detailed checklist
│   └── testing-patterns.md   # Patterns and examples
├── refactoring/
│   ├── SKILL.md
│   └── refactoring-checklist.md
├── debugging/
│   ├── SKILL.md
│   └── debugging-checklist.md
├── review/
│   ├── SKILL.md
│   └── review-checklist.md
├── adr/
│   └── SKILL.md
├── onboarding/
│   └── SKILL.md
└── documentation/
    ├── SKILL.md
    └── documentation-checklist.md
```

## Available Skills

| Skill | Description | Use When |
|-------|-------------|----------|
| **testing** | TDD workflow, write tests, verify coverage | Writing tests, improving coverage, TDD |
| **refactoring** | Improve code structure, eliminate duplication | Simplifying code, reducing tech debt |
| **debugging** | Find and fix bugs systematically | Investigating issues, fixing errors |
| **review** | Code review for correctness, security, performance | Reviewing PRs, evaluating changes |
| **adr** | Document architectural decisions | Making technical decisions |
| **onboarding** | Guide developers through project setup | New team members, environment setup |
| **documentation** | Write clear, comprehensive docs | Creating README, API docs, guides |

## How Skills Work

### Progressive Disclosure

Skills use a two-level approach:

1. **SKILL.md** (20-80 lines): Concise workflow, core patterns, quick checklist
2. **Supporting files**: Detailed checklists, patterns, examples

This allows the AI to:
- Load lightweight overview first
- Load detailed content only when needed
- Minimize token usage

### Platform Compatibility

**OpenCode:**
```yaml
---
name: skill-name
description: What this skill does
license: MIT
---
```

**Claude Code:**
```yaml
---
description: What this skill does
disable-model-invocation: true
argument-hint: <file-or-pattern>
---
```

Both platforms use the same skill content, just different frontmatter.

## Using Skills

### OpenCode

Skills are automatically discovered and loaded when relevant:

```typescript
// The AI agent sees available skills
<available_skills>
  <skill>
    <name>testing</name>
    <description>Execute TDD workflow...</description>
  </skill>
</available_skills>

// When needed, AI invokes the skill
skill({ name: "testing" })
```

### Claude Code

Skills can be invoked explicitly:

```bash
# Review changes in current branch vs main
claude --skill review --argument "main"

# Test specific files
claude --skill testing --argument "src/**/*.test.ts"

# Debug specific error
claude --skill debugging --argument "TypeError: Cannot read property 'id' of undefined"
```

Claude Code can also dynamically load content:
```markdown
<!-- !`git diff $ARGUMENTS` -->
```

## Creating New Skills

### 1. Create Directory

```bash
mkdir .opencode/skills/my-skill
mkdir .claude/skills/my-skill
```

### 2. Create SKILL.md

**OpenCode version** (`.opencode/skills/my-skill/SKILL.md`):
```markdown
---
name: my-skill
description: What this skill does. Use when [specific situations].
license: MIT
---

## Skill Workflow

1. Step one
2. Step two
3. Step three

## Core Concepts

Brief explanation of key concepts.

## Quick Checklist

- [ ] Item one
- [ ] Item two
- [ ] Item three

See `my-skill-checklist.md` for detailed checklist.
```

**Claude Code version** (`.claude/skills/my-skill/SKILL.md`):
```markdown
---
description: What this skill does. Use when [specific situations].
disable-model-invocation: true
argument-hint: <file-or-pattern>
---

[Same content as OpenCode version]
```

### 3. Create Supporting Files

```markdown
# my-skill-checklist.md

## Comprehensive Checklist

- [ ] Detailed item one
- [ ] Detailed item two
- [ ] Detailed item three
```

### 4. Follow Best Practices

**Keep SKILL.md concise:**
- 20-80 lines recommended
- Focus on workflow, not reference
- Point to supporting files

**Use progressive disclosure:**
- Brief overview in SKILL.md
- Detailed checklists in separate files
- Patterns/examples in separate files

**Be action-oriented:**
- Focus on "how to" not "what is"
- Provide concrete steps
- Include checklists

**Cross-reference:**
- Link to related skills
- Reference external documentation

## Skill Design Principles

### 1. Assume Intelligence

The AI is already intelligent. Skills should provide:
- Specific workflows
- Checklists
- Patterns
- Examples

Not:
- Explanations of basic concepts
- Excessive descriptions
- Unnecessary context

### 2. Be Token-Efficient

Every token costs context window space:
- Use concise language
- Prefer checklists over prose
- Use tables for comparisons
- Link to detailed files instead of inlining

### 3. Progressive Disclosure

```
SKILL.md (lightweight overview)
    ↓ Load when needed
checklist.md (detailed checklist)
    ↓ Load when needed
patterns.md (examples and patterns)
```

### 4. Platform Agnostic

Same content, different frontmatter:
- **OpenCode:** `name`, `description`, `license`
- **Claude Code:** `description`, `disable-model-invocation`, `argument-hint`

## Examples

### Minimal Skill (20 lines)

```markdown
---
name: git-commit
description: Write conventional commit messages. Use when committing changes.
---

## Commit Message Format

\`\`\`
<type>(<scope>): <subject>

<body>

<footer>
\`\`\`

## Types

- feat: New feature
- fix: Bug fix
- docs: Documentation
- style: Formatting
- refactor: Code refactoring
- test: Tests
- chore: Maintenance

## Examples

\`\`\`
feat(auth): add JWT refresh token rotation

Implement refresh token rotation for better security.
Refresh tokens expire after 7 days and are rotated on use.

Closes #123
\`\`\`
```

### Comprehensive Skill (80 lines + supporting files)

```markdown
---
name: testing
description: Execute TDD workflow, write tests, verify coverage. Use when writing tests, improving coverage, implementing TDD.
---

## TDD Workflow

1. Red: Write failing test
2. Green: Write minimal code
3. Refactor: Clean up

## Coverage Standards

- Minimum: 70%
- Target: 80%
- Critical paths: 90%+

## Quick Checklist

- [ ] Tests pass
- [ ] Coverage meets threshold
- [ ] Edge cases covered

See `testing-checklist.md` for comprehensive checklist.
See `testing-patterns.md` for common patterns.
```

## Further Reading

- [Skill Authoring Best Practices](https://docs.anthropic.com/claude/docs/skills)
- [OpenCode Documentation](https://opencode.ai/docs/skills/)
- [Progressive Disclosure Pattern](https://docs.anthropic.com/claude/docs/skills#progressive-disclosure-patterns)