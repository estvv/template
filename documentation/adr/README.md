# Architecture Decision Records

This directory contains Architecture Decision Records (ADRs) for {{PROJECT_NAME}}.

## What is an ADR?

An Architecture Decision Record (ADR) is a document that captures an important architectural decision made along with its context and consequences.

## Why ADRs?

- **Document decisions** - Keep track of why decisions were made
- **Share knowledge** - Help team members understand architecture
- **Onboarding** - Faster onboarding for new team members
- **Avoid repeating discussions** - Refer back to previous decisions
- **Continuous learning** - Learn from past decisions

## ADR Template

Use [template.md](./template.md) to create new ADRs.

## Creating an ADR

1. Copy [template.md](./template.md) to `NNNN-short-title.md`
   - `NNNN` is a sequential number (e.g., 0001, 0002)
   - `short-title` is a brief description (e.g., `use-postgresql`)

2. Fill in the sections:
   - **Title** - Descriptive title
   - **Date** - When decision was made
   - **Status** - Proposed, Accepted, Deprecated, Superseded
   - **Context** - Situation prompting the decision
   - **Decision** - The decision made
   - **Consequences** - Resulting impact
   - **Alternatives** - Other options considered

3. Submit for review and discussion

4. Update status to "Accepted" after approval

## ADR Index

| Number | Title | Status | Date |
|--------|-------|--------|------|
| 0001 | Use PostgreSQL as Primary Database | Accepted | 2024-01-01 |
| 0002 | Implement JWT Authentication | Accepted | 2024-01-15 |
| 0003 | Use Microservices Architecture | Proposed | 2024-02-01 |

## Status Definitions

- **Proposed** - Decision is being discussed
- **Accepted** - Decision has been approved
- **Deprecated** - Decision is no longer recommended
- **Superseded** - Decision has been replaced by another ADR

## Updating ADRs

ADRs should rarely be changed once accepted. If context changes:

1. Create a new ADR superseding the old one
2. Link to the new ADR from the old one
3. Set old ADR status to "Superseded"

## Examples

**Good ADR:**
```
# 0001 - Use PostgreSQL as Primary Database

## Date
2024-01-01

## Status
Accepted

## Context
We need a reliable, scalable database for storing user data, orders, and 
product information. The database must support:
- Complex queries with JOINs
- ACID transactions
- Full-text search
- JSON data for flexible schema

## Decision
We will use PostgreSQL as our primary database.

## Consequences
- Good: ACID compliance, mature ecosystem, strong community
- Bad: Requires more setup than SQLite, horizontal scaling requires more effort
- Neutral: Team has experience with PostgreSQL

## Alternatives Considered
- MySQL: Less robust JSON support
- MongoDB: No ACID transactions
- SQLite: Not suitable for production scale
```

**Bad ADR:**
```
# 0001 - Database Decision

We chose PostgreSQL because it's good.
```

## Best Practices

1. **One decision per ADR** - Keep it focused
2. **Write for future readers** - Assume no context
3. **Include alternatives** - Show due diligence
4. **Be honest about consequences** - Good and bad
5. **Keep concise** - 1-2 pages max
6. **Link to related ADRs** - Build decision tree

## When to Create an ADR

Create an ADR when you make a decision that:

- Affects system architecture
- Impacts multiple components
- Has significant consequences
- Involves trade-offs
- Changes existing architecture
- Introduces new technology
- Has long-term implications

## Example Decisions Requiring ADRs

- Database selection (SQL vs. NoSQL)
- Authentication/authorization approach
- API style (REST vs. GraphQL vs. gRPC)
- Deployment strategy (monolith vs. microservices)
- Caching strategy
- Message queue selection
- Logging/monitoring approach
- Testing strategy
- Security architecture

## Reference

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) - Michael Nygard
- [Architecture Decision Records](https://adr.github.io/) - ADR GitHub Organization

## Related Documentation

- [ARCHITECTURE.md](../ARCHITECTURE.md) - Overall architecture overview
- [DEVELOPMENT.md](../DEVELOPMENT.md) - Development guidelines