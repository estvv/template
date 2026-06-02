---
description: Document architectural decisions with context and consequences. Use when making significant technical decisions that affect system architecture, technology choices, or design patterns.
disable-model-invocation: true
---

## What is an ADR?

**Architecture Decision Record:** A document that captures an important architectural decision along with its context and consequences.

**Purpose:**
- Record **why** a decision was made
- Provide context for future developers
- Prevent repeating past mistakes
- Enable informed re-evaluation

## When to Write an ADR

**Write ADR for:**
- Choosing a framework/library
- Changing architecture
- Choosing a database
- Making security decisions
- Decisions affecting multiple teams
- Hard-to-reverse decisions

**Don't write ADR for:**
- Minor implementation details
- Obvious decisions
- Reversible decisions with low impact

## ADR Template

```markdown
# ADR-NNNN: Title

Date: YYYY-MM-DD

## Status

[Proposed | Accepted | Deprecated | Superseded by ADR-XXXX]

## Context

What is the issue motivating this decision?

## Decision

What is the change being proposed/made?

## Consequences

What becomes easier or more difficult?

## Alternatives Considered

What other options were considered? Why not chosen?
```

## ADR Lifecycle

```
Proposed → Accepted → Deprecated → Superseded
              ↓
           Rejected
```

**Proposed:** Under discussion
**Accepted:** Agreed upon, should be followed
**Deprecated:** No longer recommended for new projects
**Superseded:** Replaced by new ADR (link to new ADR)
**Rejected:** Discussed but not accepted

## ADR Best Practices

### Do:
- Write when making the decision (not after)
- Keep concise (300-500 words)
- Include alternatives considered
- Acknowledge trade-offs
- Store in version control

### Don't:
- Delete or overwrite accepted ADRs
- Make it too vague
- Skip alternatives section
- Ignore consequences
- Write after implementation is done

## Directory Structure

```
docs/
  adr/
    0001-use-typescript.md
    0002-use-postgres.md
    0003-api-versioning.md
    template.md
    index.md
```

## Example ADR

```markdown
# ADR-0001: Use TypeScript for New Projects

Date: 2024-01-15

## Status

Accepted

## Context

We need to choose a language for new projects. Team has 
mixed TypeScript experience. Project will be maintained 
for 5+ years.

## Decision

Use TypeScript for all new JavaScript projects.

## Consequences

Positive:
- Compile-time type checking
- Better IDE support
- Self-documenting code

Negative:
- Learning curve
- Additional build step

## Alternatives Considered

1. JavaScript with JSDoc - No compilation, but less IDE support
2. Flow - Smaller community than TypeScript
```

See `adr-template.md` for complete template.
See `adr-examples.md` for more examples.

## Related Skills

- Use **documentation** skill for general documentation
- Reference ADRs in code comments when relevant