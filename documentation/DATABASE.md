# Database Documentation

## Overview

{{DATABASE_OVERVIEW}}

**Database Type:** {{DATABASE_TYPE}} (Relational / Document / Key-Value / Graph)

**Database System:** `{{DATABASE_SYSTEM}}` (PostgreSQL / MySQL / MongoDB / Redis / etc.)

**Version:** `{{DATABASE_VERSION}}`

**ORM/Query Builder:** `{{ORM}}`

## Connection

### Connection String Format

```
{{CONNECTION_STRING_FORMAT}}
postgresql://user:password@host:port/database
mongodb://user:password@host:port/database
redis://user:password@host:port/database
```

### Environment Variables

```bash
DATABASE_URL={{DATABASE_URL}}
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=mydb
DATABASE_USER=myuser
DATABASE_PASSWORD=mypassword
```

### Connection Pooling

```{{LANGUAGE}}
{{CONNECTION_POOL_CONFIG}}
pool: {
  min: 2,
  max: 10,
  acquireTimeoutMillis: 30000,
  idleTimeoutMillis: 10000
}
```

## Schema

### Entity-Relationship Diagram

```
{{ER_DIAGRAM}}
┌─────────┐       ┌─────────┐       ┌─────────┐
│  User   │──────<│  Post   │>──────│ Comment │
└─────────┘       └─────────┘       └─────────┘
     │                                     │
     └─────────────────────────────────────┘
```

### Tables

#### Table: `users`

**Purpose:** Store user accounts and profiles

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| `id` | UUID | PRIMARY KEY, NOT NULL | Unique identifier |
| `email` | VARCHAR(255) | UNIQUE, NOT NULL | User email address |
| `password_hash` | VARCHAR(255) | NOT NULL | Hashed password |
| `username` | VARCHAR(50) | UNIQUE, NOT NULL | Username |
| `bio` | TEXT | NULLABLE | User biography |
| `avatar_url` | VARCHAR(500) | NULLABLE | Profile picture URL |
| `role` | ENUM('user', 'admin') | DEFAULT 'user' | User role |
| `is_active` | BOOLEAN | DEFAULT true | Active status |
| `created_at` | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| `updated_at` | TIMESTAMP | DEFAULT NOW() | Last update timestamp |

**Indexes:**
- `idx_users_email` on `email` (UNIQUE)
- `idx_users_username` on `username` (UNIQUE)
- `idx_users_created_at` on `created_at`

**Foreign Keys:** None

#### Table: `posts`

**Purpose:** Store user posts

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| `id` | UUID | PRIMARY KEY, NOT NULL | Unique identifier |
| `user_id` | UUID | FOREIGN KEY, NOT NULL | Author ID |
| `title` | VARCHAR(255) | NOT NULL | Post title |
| `content` | TEXT | NOT NULL | Post content |
| `status` | ENUM('draft', 'published') | DEFAULT 'draft' | Post status |
| `published_at` | TIMESTAMP | NULLABLE | Publication timestamp |
| `created_at` | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| `updated_at` | TIMESTAMP | DEFAULT NOW() | Last update timestamp |

**Indexes:**
- `idx_posts_user_id` on `user_id`
- `idx_posts_status` on `status`
- `idx_posts_published_at` on `published_at`

**Foreign Keys:**
- `fk_posts_user_id` REFERENCES `users(id)` ON DELETE CASCADE

#### Table: `comments`

**Purpose:** Store post comments

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| `id` | UUID | PRIMARY KEY, NOT NULL | Unique identifier |
| `post_id` | UUID | FOREIGN KEY, NOT NULL | Post ID |
| `user_id` | UUID | FOREIGN KEY, NOT NULL | Author ID |
| `content` | TEXT | NOT NULL | Comment content |
| `created_at` | TIMESTAMP | DEFAULT NOW() | Creation timestamp |
| `updated_at` | TIMESTAMP | DEFAULT NOW() | Last update timestamp |

**Indexes:**
- `idx_comments_post_id` on `post_id`
- `idx_comments_user_id` on `user_id`

**Foreign Keys:**
- `fk_comments_post_id` REFERENCES `posts(id)` ON DELETE CASCADE
- `fk_comments_user_id` REFERENCES `users(id)` ON DELETE CASCADE

### Relationships

```
users (1) ----< (N) posts
users (1) ----< (N) comments
posts (1) ----< (N) comments
```

## Migrations

### Migration Tool

**Tool:** `{{MIGRATION_TOOL}}` (e.g., Prisma Migrate, Alembic, Flyway, Knex)

### Migration Naming Convention

```
{{MIGRATION_NAMING}}
YYYYMMDDHHMMSS_description.sql
20240101000000_create_users_table.sql
20240101000001_create_posts_table.sql
```

### Creating a Migration

```bash
# {{MIGRATION_TOOL}} create migration command
{{CREATE_MIGRATION_COMMAND}}
```

### Migration Structure

```sql
-- Migration: Create users table
-- Created at: 2024-01-01 00:00:00

-- Up Migration
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  username VARCHAR(50) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_username ON users(username);

-- Down Migration
DROP TABLE users;
```

### Running Migrations

```bash
# Apply all pending migrations
{{MIGRATE_UP_COMMAND}}

# Rollback last migration
{{MIGRATE_DOWN_COMMAND}}

# Rollback all migrations
{{MIGRATE_RESET_COMMAND}}

# Check migration status
{{MIGRATE_STATUS_COMMAND}}
```

### Migration Best Practices

1. **Always test migrations**
   - Test on local database
   - Test on staging environment
   - Test rollback

2. **Avoid data loss**
   ```sql
   -- Good: Add column with default
   ALTER TABLE users ADD COLUMN bio TEXT DEFAULT '';

   -- Bad: Drop column without backup
   ALTER TABLE users DROP COLUMN bio;
   ```

3. **Large tables**
   ```sql
   -- Add index concurrently to avoid locking
   CREATE INDEX CONCURRENTLY idx_users_email ON users(email);
   ```

4. **Data migrations**
   - Separate schema and data migrations
   - Run data migrations in batches
   - Add rollback plan

## Models

### Model Definition

```{{LANGUAGE}}
{{MODEL_DEFINITION_EXAMPLE}}
// Example model definition
interface User {
  id: string;
  email: string;
  username: string;
  passwordHash: string;
  bio?: string;
  avatarUrl?: string;
  role: 'user' | 'admin';
  isActive: boolean;
  createdAt: Date;
  updatedAt: Date;
}
```

### Repository Pattern

```{{LANGUAGE}}
{{REPOSITORY_PATTERN_EXAMPLE}}
interface UserRepository {
  findById(id: string): Promise<User | null>;
  findByEmail(email: string): Promise<User | null>;
  create(data: CreateUserData): Promise<User>;
  update(id: string, data: UpdateUserData): Promise<User>;
  delete(id: string): Promise<void>;
}
```

### Query Optimization

#### Use Indexes

```sql
-- Query without index (slow)
SELECT * FROM users WHERE email = 'user@example.com';

-- Add index
CREATE INDEX idx_users_email ON users(email);

-- Query with index (fast)
SELECT * FROM users WHERE email = 'user@example.com';
```

#### Avoid N+1 Queries

```{{LANGUAGE}}
// Bad: N+1 queries
const posts = await getPosts();
for (const post of posts) {
  post.comments = await getComments(post.id); // N queries
}

// Good: Single query with join
const posts = await getPostsWithComments();
```

#### Use Pagination

```sql
-- Bad: Fetch all rows
SELECT * FROM posts;

-- Good: Paginate
SELECT * FROM posts
LIMIT 20 OFFSET 0;
```

## Seeding

### Seed Data Structure

```javascript
// seeds/users.seed.js
module.exports = [
  {
    id: '00000000-0000-0000-0000-000000000001',
    email: 'admin@example.com',
    username: 'admin',
    passwordHash: 'hashed_password',
    role: 'admin'
  },
  {
    id: '00000000-0000-0000-0000-000000000002',
    email: 'user@example.com',
    username: 'user',
    passwordHash: 'hashed_password',
    role: 'user'
  }
];
```

### Running Seeds

```bash
# Run all seeds
{{SEED_COMMAND}}

# Run specific seed
{{SEED_SPECIFIC_COMMAND}}
```

## Backup & Restore

### Backup Strategy

**Backup Type:** {{BACKUP_TYPE}} (Full / Incremental / Differential)

**Backup Frequency:** {{BACKUP_FREQUENCY}}

**Retention Policy:** {{RETENTION_POLICY}}

### Backup Commands

```bash
# PostgreSQL
pg_dump -h localhost -U user -d mydb > backup_$(date +%Y%m%d).sql

# MongoDB
mongodump --host localhost --db mydb --out ./backup

# MySQL
mysqldump -h localhost -u user -p mydb > backup_$(date +%Y%m%d).sql
```

### Restore Commands

```bash
# PostgreSQL
psql -h localhost -U user -d mydb < backup_20240101.sql

# MongoDB
mongorestore --host localhost --db mydb ./backup/mydb

# MySQL
mysql -h localhost -u user -p mydb < backup_20240101.sql
```

### Automated Backups

{{AUTOMATED_BACKUP_SETUP}}

## Performance

### Indexing Strategy

**When to Index:**
- Columns in WHERE clauses
- Columns in JOIN conditions
- Columns in ORDER BY
- Columns in GROUP BY

**When NOT to Index:**
- Small tables
- High-frequency writes, low-frequency reads
- Columns with low cardinality (boolean, gender)

### Query Analysis

```sql
-- PostgreSQL: EXPLAIN ANALYZE
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'user@example.com';

-- MySQL: EXPLAIN
EXPLAIN SELECT * FROM users WHERE email = 'user@example.com';
```

### Slow Query Log

```bash
# Enable slow query log
{{SLOW_QUERY_LOG_CONFIG}}

# Query threshold (e.g., 2 seconds)
long_query_time = 2
```

### Connection Pooling

```{{LANGUAGE}}
{{POOLING_EXAMPLE}}
const pool = createPool({
  min: 2,
  max: 10,
  acquireTimeoutMillis: 30000,
  idleTimeoutMillis: 10000
});
```

## Security

### Data Encryption

**At Rest:**
- Use database-level encryption
- Encrypt sensitive columns (PII, financial data)
- Use application-level encryption for highly sensitive data

**In Transit:**
- Use TLS/SSL connections
- Enforce SSL certificate verification

### Access Control

**Principle of Least Privilege:**

```sql
-- Create application user with minimal permissions
CREATE USER app_user WITH PASSWORD 'secure_password';
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_user;

-- Admin user with full permissions
CREATE USER admin_user WITH PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE mydb TO admin_user;
```

### SQL Injection Prevention

```{{LANGUAGE}}
// BAD: SQL injection vulnerable
const query = `SELECT * FROM users WHERE email = '${email}'`;

// GOOD: Use parameterized queries
const query = 'SELECT * FROM users WHERE email = ?';
connection.query(query, [email]);
```

### Sensitive Data Handling

**Never Store:**
- Plain text passwords
- Credit card numbers (use tokenization)
- SSN (unless encrypted)
- API keys

**Always Hash/Salt:**
- Passwords (use bcrypt, argon2)
- Password reset tokens
- Security questions answers

## Monitoring

### Database Metrics

**Key Metrics:**
- Connection count
- Query latency (p50, p95, p99)
- Query throughput
- Lock wait time
- Cache hit ratio
-Disk I/O
- Replication lag (if applicable)

### Monitoring Tools

**Tool:** `{{DB_MONITORING_TOOL}}`

- **PostgreSQL:** pg_stat_statements, pg_stat_activity
- **MySQL:** Performance Schema, Slow Query Log
- **MongoDB:** Database Profiler, Ops Manager

### Health Checks

```sql
-- PostgreSQL: Check long-running queries
SELECT pid, now() - pg_stat_activity.query_start AS duration, query
FROM pg_stat_activity
WHERE (now() - pg_stat_activity.query_start) > interval '5 minutes';

-- Check table sizes
SELECT
  schemaname,
  tablename,
  pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;
```

## Troubleshooting

### Common Issues

#### Connection Pool Exhausted

**Symptoms:**
- `Connection timeout` errors
- `Too many connections` errors

**Solution:**
```{{LANGUAGE}}
// Increase pool size
pool: { max: 20 }

// Long-running queries blocking connections
// Optimize queries or use read replicas
```

#### Slow Queries

**Diagnosis:**
```sql
-- Find slow queries
SELECT * FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;
```

**Solution:**
- Add missing indexes
- Optimize query
- Use query caching

#### Lock Contention

**Diagnosis:**
```sql
-- Check for locks
SELECT * FROM pg_locks WHERE NOT granted;

-- Blocking queries
SELECT blocked_locks.pid AS blocked_pid,
       blocking_locks.pid AS blocking_pid
FROM pg_catalog.pg_locks blocked_locks
JOIN pg_catalog.pg_locks blocking_locks
  ON blocking_locks.locktype = blocked_locks.locktype
 WHERE blocked_locks.granted = false;
```

**Solution:**
- Reduce transaction scope
- Avoid long-running transactions
- Use appropriate isolation level

## Best Practices

1. **Use Migrations**
   - All schema changes via migrations
   - Version control migrations
   - Test migrations before production

2. **Index Strategically**
   - Index query patterns, not guess
   - Monitor and adjust indexes
   - Use composite indexes wisely

3. **Normalize Appropriately**
   - Avoid over-normalization (too many joins)
   - Avoid under-normalization (data duplication)
   - Use denormalization for read-heavy queries

4. **Handle Transactions**
   - Keep transactions short
   - Use appropriate isolation level
   - Handle deadlocks properly

5. **Backup Regularly**
   - Automated backups
   - Test restore process
   - Off-site backup storage

6. **Monitor Performance**
   - Query performance metrics
   - Connection pool usage
   - Lock contention

## Development Workflow

### Local Setup

```bash
# Start database
{{START_DB_COMMAND}}

# Create database
{{CREATE_DB_COMMAND}}

# Run migrations
{{MIGRATE_COMMAND}}

# Seed data
{{SEED_COMMAND}}
```

### Testing

```bash
# Run tests with test database
{{TEST_COMMAND}}

# Reset test database
{{TEST_RESET_COMMAND}}
```

## Related Documentation

- [Architecture](./ARCHITECTURE.md) - System architecture
- [API](./API.md) - API documentation
- [Testing](./TESTING.md) - Testing strategy