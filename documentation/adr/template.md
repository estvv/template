# [ADR-NNNN] Decision Title

## Metadata

| Field | Value |
|-------|-------|
| ADR Number | NNNN |
| Title | Decision Title |
| Status | Proposed / Accepted / Deprecated / Superseded |
| Date | YYYY-MM-DD |
| Decision Makers | Names or roles |
| Supersedes | ADR-XXXX (if applicable) |
| Superseded by | ADR-XXXX (if applicable) |

## Context

**Situation:** Describe the situation requiring a decision.

**Problem:** What problem are we trying to solve?

**Constraints:** What constraints exist?

**Requirements:** What requirements must be satisfied?

Example:
```
We need to choose a database for our application. The application has the 
following requirements:
- Handle 10,000 concurrent connections
- Support complex queries with JOINs
- Store and query JSON documents
- Full-text search on product descriptions
- ACID transactions for payment processing

We have a team of 5 developers with varying levels of database experience.
```

## Decision

**Decision:** State the decision clearly.

**Rationale:** Why this decision was made.

Example:
```
We will use PostgreSQL as our primary database.

Rationale:
- Proven reliability and scalability
- Strong community and ecosystem
- Built-in JSON support (JSONB)
- Full-text search capability
- ACID compliance
- Team familiarity with SQL databases
```

## Consequences

### Positive

- **Consequence 1:** Describe positive outcome
- **Consequence 2:** Describe positive outcome

Example:
- Strong data consistency guarantees
- Mature tooling and monitoring
- Excellent documentation
- Large talent pool for hiring

### Negative

- **Consequence 1:** Describe negative outcome
- **Consequence 2:** Describe negative outcome

Example:
- Requires more setup than SQLite
- Horizontal scaling requires additional tooling
- Less flexible schema than NoSQL options

### Neutral

- **Consequence 1:** Describe neutral outcome

Example:
- Requires SQL expertise for complex queries

## Alternatives Considered

### Alternative 1: [Name]

**Description:** Brief description.

**Pros:**
- Pro 1
- Pro 2

**Cons:**
- Con 1
- Con 2

**Why not chosen:** Reason for rejection.

Example:
```
### Alternative 1: MongoDB

**Description:** Document-based NoSQL database.

**Pros:**
- Flexible schema
- Built-in sharding
- Horizontal scaling

**Cons:**
- No ACID transactions (until recently)
- Limited JOIN capabilities
- Team lacks MongoDB experience

**Why not chosen:** ACID transactions are critical for payment processing. 
Team lacks NoSQL expertise.
```

### Alternative 2: [Name]

**Description:** Brief description.

**Pros:**
- Pro 1

**Cons:**
- Con 1

**Why not chosen:** Reason for rejection.

### Alternative 3: [Name]

**Description:** Brief description.

**Pros:**
- Pro 1

**Cons:**
- Con 1

**Why not chosen:** Reason for rejection.

## Implementation

**Steps:** How this decision will be implemented.

**Timeline:** When this will be implemented.

**Dependencies:** Dependencies on other decisions or systems.

**Migration:** How to migrate from previous solution.

Example:
```
1. Set up PostgreSQL instance (Week 1)
2. Design schema (Week 1-2)
3. Implement migrations (Week 2-3)
4. Migrate data from SQLite (Week 3)
5. Update application to use PostgreSQL (Week 3-4)
6. Add monitoring and backups (Week 4)

Dependencies: None
Migration: Data migration script from SQLite to PostgreSQL
```

## Validation

**How will we validate this decision?**

Example:
- Performance benchmarks meet requirements
- Load testing shows system can handle 10K concurrent connections
- Full-text search works as expected
- ACID transactions pass all tests

## Monitoring

**What metrics will we monitor?**

Example:
- Query latency (p50, p95, p99)
- Connection pool utilization
- Disk I/O
- Replication lag (if applicable)

## Related Decisions

Link to related ADRs that impact or are impacted by this decision.

- [ADR-0001: Use PostgreSQL](./0001-use-postgresql.md) - Implemented this decision
- [ADR-0003: Implement Read Replicas](./0003-read-replicas.md) - Built on this decision

## References

- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [PostgreSQL vs MySQL Comparison](https://example.com/comparison)
- [Team Discussion Thread](link-to-discussion)
- [Design Document](link-to-design-doc)

## Notes

Any additional notes or context.

## Questions

- Question 1?
- Question 2?

## History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial proposal | Name |
| YYYY-MM-DD | Updated with feedback | Name |
| YYYY-MM-DD | Accepted | Name |

---

## Template Usage

1. Copy this template
2. Rename to `NNNN-short-title.md` (e.g., `0001-use-postgresql.md`)
3. Fill in all sections
4. Remove instruction text (like this section)
5. Submit for review and discussion