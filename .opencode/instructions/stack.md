# Technology Stack Guidelines

## Language & Runtime

### Primary Language

**Language:** `{{LANGUAGE}}` (e.g., TypeScript, Python, Rust, Go)

**Version:** `{{LANGUAGE_VERSION}}` (e.g., Node 20 LTS, Python 3.11)

**Runtime Requirements:**
- {{RUNTIME_REQUIREMENT_1}}
- {{RUNTIME_REQUIREMENT_2}}

## Framework & Libraries

### Backend Framework

**Framework:** `{{BACKEND_FRAMEWORK}}` (e.g., Express, FastAPI, Actix)

**Version:** `{{FRAMEWORK_VERSION}}`

**Key Libraries:**
```
{{KEY_LIBRARIES_LIST}}
```

### Frontend Framework (if applicable)

**Framework:** `{{FRONTEND_FRAMEWORK}}` (e.g., React, Vue, Svelte)

**UI Library:** `{{UI_LIBRARY}}` (e.g., Tailwind, Material-UI)

**State Management:** `{{STATE_MANAGEMENT}}` (e.g., Redux, Zustand, Jotai)

## Project Structure

```
{{PROJECT_NAME}}/
├── src/
│   ├── {{MODULE_1}}/
│   │   ├── {{MODULE_1_FILES}}
│   ├── {{MODULE_2}}/
│   │   ├── {{MODULE_2_FILES}}
│   └── index.{{EXT}}
├── tests/
│   ├── {{TEST_STRUCTURE}}
├── docs/
│   ├── {{DOC_STRUCTURE}}
├── config/
│   ├── {{CONFIG_FILES}}
├── package.json / requirements.txt / Cargo.toml
└── README.md
```

## Code Organization

### Module Structure

Each module should contain:
- Core logic
- Types/interfaces
- Tests
- Documentation
- Index file (public API)

```{{LANGUAGE}}
// {{MODULE_STRUCTURE_EXAMPLE}}
```

### Naming Conventions

**Files:**
- `{{FILE_NAMING_CONVENTION}}` (e.g., `user-service.ts`, `user_service.py`)

**Classes/Components:**
- `{{CLASS_NAMING_CONVENTION}}` (e.g., `UserService`, `PascalCase`)

**Functions/Variables:**
- `{{FUNCTION_NAMING_CONVENTION}}` (e.g., `getUserById`, `camelCase`)

**Constants:**
- `{{CONSTANT_NAMING_CONVENTION}}` (e.g., `MAX_RETRIES`, `UPPER_SNAKE_CASE`)

**Interfaces/Types:**
- `{{TYPE_NAMING_CONVENTION}}` (e.g., `User`, `IUserService`)

### Import Organization

```{{LANGUAGE}}
// {{IMPORT_EXAMPLE}}
// 1. Standard library
// 2. Third-party libraries
// 3. Internal modules
// 4. Types/interfaces
```

## Data Layer

### Database

**Database:** `{{DATABASE}}` (e.g., PostgreSQL, MongoDB, SQLite)

**ORM/Query Builder:** `{{ORM}}` (e.g., Prisma, SQLAlchemy, Diesel)

**Migrations:** `{{MIGRATION_TOOL}}` (e.g., Prisma Migrate, Alembic)

### Models

```{{LANGUAGE}}
// {{MODEL_EXAMPLE}}
```

### Repository Pattern (if used)

```{{LANGUAGE}}
// {{REPOSITORY_EXAMPLE}}
```

## API Design

### RESTful Conventions

- Use plural nouns for resources: `/users`, `/posts`
- Use HTTP methods correctly:
  - `GET` - Retrieve
  - `POST` - Create
  - `PUT` - Replace
  - `PATCH` - Partial update
  - `DELETE` - Remove

### Endpoints

```
{{API_ENDPOINTS}}
```

### Response Format

```json
{
  "success": true,
  "data": {},
  "error": null,
  "meta": {
    "page": 1,
    "total": 100
  }
}
```

### Error Handling

```{{LANGUAGE}}
// {{ERROR_HANDLING_EXAMPLE}}
```

## Testing

### Testing Framework

**Framework:** `{{TEST_FRAMEWORK}}` (e.g., Jest, pytest, cargo test)

**Coverage Tool:** `{{COVERAGE_TOOL}}`

**Coverage Threshold:** `{{COVERAGE_THRESHOLD}}` (e.g., 80%)

### Test Structure

```{{LANGUAGE}}
// {{TEST_EXAMPLE}}
// describe/it pattern or equivalent
```

### Test Types

- **Unit Tests** - Test individual functions/classes
- **Integration Tests** - Test module interactions
- **E2E Tests** - Test complete user flows

### Running Tests

```bash
{{TEST_COMMAND}}
{{TEST_WATCH_COMMAND}}
{{COVERAGE_COMMAND}}
```

## Build & Bundling

### Build Tool

**Tool:** `{{BUILD_TOOL}}` (e.g., webpack, vite, cargo build)

**Build Command:** `{{BUILD_COMMAND}}`

**Dev Server:** `{{DEV_COMMAND}}`

### Output

- `dist/` or `build/` directory
- Source maps for debugging
- Minified for production
- Tree-shaking enabled

## Linting & Formatting

### Linter

**Tool:** `{{LINTER}}` (e.g., ESLint, pylint, clippy)

**Config:** `{{LINTER_CONFIG}}`

```bash
{{LINT_COMMAND}}
{{LINT_FIX_COMMAND}}
```

### Formatter

**Tool:** `{{FORMATTER}}` (e.g., Prettier, Black, rustfmt)

**Config:** `{{FORMATTER_CONFIG}}`

```bash
{{FORMAT_COMMAND}}
```

### Pre-commit Hooks

Use `{{PRE_COMMIT_TOOL}}` (e.g., husky, pre-commit) to:
- Run linter
- Run tests
- Check formatting
- Validate commit messages

## Dependency Management

### Package Manager

**Tool:** `{{PACKAGE_MANAGER}}` (e.g., npm, yarn, pnpm, pip, poetry, cargo)

### Adding Dependencies

```bash
{{INSTALL_DEPENDENCY}}
{{INSTALL_DEV_DEPENDENCY}}
```

### Updating Dependencies

```bash
{{UPDATE_DEPENDENCIES}}
```

## Environment Configuration

### Configuration Files

```
.env                # Local secrets (gitignored)
.env.example        # Template (committed)
.env.development    # Dev environment
.env.production     # Production defaults
.env.test           # Test environment
```

### Config Access

```{{LANGUAGE}}
// {{CONFIG_ACCESS_EXAMPLE}}
```

## Logging

### Logger

**Library:** `{{LOGGING_LIBRARY}}` (e.g., winston, logging, log)

**Log Levels:**
- `error` - Critical failures
- `warn` - Potential issues
- `info` - Important events
- `debug` - Detailed flow
- `trace` - Very detailed debugging

### Log Format

```{{LANGUAGE}}
// {{LOG_FORMAT_EXAMPLE}}
```

## Performance

### Caching

**Cache:** `{{CACHE}}` (e.g., Redis, Memcached)

**Strategy:** {{CACHE_STRATEGY}}

```{{LANGUAGE}}
// {{CACHE_EXAMPLE}}
```

### Query Optimization

- Use indexes appropriately
- Avoid N+1 queries
- Implement pagination
- Use connection pooling
- Cache frequent queries

## Deployment

### Containerization

**Container:** `{{CONTAINER_TOOL}}` (e.g., Docker, Podman)

```dockerfile
# {{DOCKERFILE_EXAMPLE}}
```

### Orchestration (if applicable)

**Platform:** `{{ORCHESTRATION_PLATFORM}}` (e.g., Kubernetes, ECS)

**Config:** `{{ORCHESTRATION_CONFIG}}`

## Monitoring & Observability

### APM

**Tool:** `{{APM_TOOL}}` (e.g., Datadog, New Relic, Prometheus)

### Metrics

- Response time
- Error rate
- Throughput
- Resource utilization
- Business metrics

### Tracing

**Tool:** `{{TRACING_TOOL}}` (e.g., Jaeger, Zipkin)

## Documentation

### Code Documentation

```{{LANGUAGE}}
// {{DOCUMENTATION_EXAMPLE}}
```

### API Documentation

**Tool:** `{{API_DOC_TOOL}}` (e.g., Swagger/OpenAPI, GraphQL Docs)

### Architecture Documentation

- `docs/architecture.md` - System architecture
- `docs/api.md` - API documentation
- `docs/database.md` - Database schema
- `docs/deployment.md` - Deployment guide

## Quick Reference

```bash
# Install dependencies
{{INSTALL_COMMAND}}

# Run development server
{{DEV_COMMAND}}

# Run tests
{{TEST_COMMAND}}

# Build for production
{{BUILD_COMMAND}}

# Lint code
{{LINT_COMMAND}}

# Format code
{{FORMAT_COMMAND}}
```