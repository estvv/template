# Technology Stack Rules for Claude Code

## Language & Runtime

**Language:** `{{LANGUAGE}}`

**Version:** `{{LANGUAGE_VERSION}}`

**Runtime Requirements:**
```
{{RUNTIME_REQUIREMENTS}}
```

## Framework & Libraries

### Backend
```
{{BACKEND_FRAMEWORK}} {{FRAMEWORK_VERSION}}
Key libraries: {{KEY_LIBRARIES}}
```

### Frontend (if applicable)
```
{{FRONTEND_FRAMEWORK}}
UI Library: {{UI_LIBRARY}}
State Management: {{STATE_MANAGEMENT}}
```

## Project Structure

```
{{PROJECT_NAME}}/
├── src/
│   ├── {{MODULE_1}}/
│   ├── {{MODULE_2}}/
│   └── index.{{EXT}}
├── tests/
├── docs/
├── config/
└── package.json / requirements.txt / Cargo.toml
```

## Naming Conventions

### Files
`{{FILE_NAMING_CONVENTION}}` (e.g., `kebab-case.ts`, `snake_case.py`)

### Classes/Components
`{{CLASS_NAMING_CONVENTION}}` (e.g., `PascalCase`)

### Functions/Variables
`{{FUNCTION_NAMING_CONVENTION}}` (e.g., `camelCase`)

### Constants
`{{CONSTANT_NAMING_CONVENTION}}` (e.g., `UPPER_SNAKE_CASE`)

### Interfaces/Types
`{{TYPE_NAMING_CONVENTION}}` (e.g., `IInterfaceName` or just `InterfaceName`)

## Import Order

```{{LANGUAGE}}
// 1. Standard library
import {{STDLIB_IMPORTS}};

// 2. Third-party libraries
import {{THIRD_PARTY_IMPORTS}};

// 3. Internal modules
import {{INTERNAL_IMPORTS}};

// 4. Types/interfaces
import type { {{TYPE_IMPORTS}} } from '{{TYPE_MODULE}}';
```

## Data Layer

**Database:** `{{DATABASE}}`

**ORM/Query Builder:** `{{ORM}}`

**Migration Tool:** `{{MIGRATION_TOOL}}`

### Model Pattern

```{{LANGUAGE}}
{{MODEL_EXAMPLE}}
```

## API Design

### RESTful Endpoints

```
GET    /{{RESOURCE}}           # List all
GET    /{{RESOURCE}}/:id       # Get one
POST   /{{RESOURCE}}           # Create one
PUT    /{{RESOURCE}}/:id       # Replace one
PATCH  /{{RESOURCE}}/:id       # Update one
DELETE /{{RESOURCE}}/:id       # Delete one
```

### Request/Response Format

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

### Error Response

```json
{
  "success": false,
  "data": null,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {}
  }
}
```

## Testing

**Framework:** `{{TEST_FRAMEWORK}}`

**Coverage:** `{{COVERAGE_THRESHOLD}}`

### Test Structure

```{{LANGUAGE}}
describe('{{COMPONENT_NAME}}', () => {
  describe('{{METHOD_NAME}}', () => {
    it('should {{EXPECTED_BEHAVIOR}}', () => {
      {{TEST_CODE}}
    });
  });
});
```

### Test Commands

```bash
# Run tests
{{TEST_COMMAND}}

# Run tests in watch mode
{{TEST_WATCH_COMMAND}}

# Generate coverage report
{{COVERAGE_COMMAND}}
```

## Build & Development

### Commands

```bash
# Install dependencies
{{INSTALL_COMMAND}}

# Run development server
{{DEV_COMMAND}}

# Build for production
{{BUILD_COMMAND}}

# Run linter
{{LINT_COMMAND}}

# Format code
{{FORMAT_COMMAND}}
```

## Linting & Formatting

**Linter:** `{{LINTER}}`

**Formatter:** `{{FORMATTER}}`

**Pre-commit:** `{{PRE_COMMIT_TOOL}}`

## Configuration

### Environment Files

- `.env` - Local secrets (gitignored)
- `.env.example` - Template (committed)
- `.env.development` - Dev defaults
- `.env.production` - Production defaults
- `.env.test` - Test environment

### Config Access

```{{LANGUAGE}}
{{CONFIG_ACCESS_EXAMPLE}}
```

## Logging

**Library:** `{{LOGGING_LIBRARY}}`

### Log Levels (use appropriately)

- `error` - Critical failures, immediate attention needed
- `warn` - Potential issues, recoverable errors
- `info` - Important business events
- `debug` - Detailed flow for debugging
- `trace` - Very detailed debugging

### Log Format

```{{LANGUAGE}}
logger.info('Event occurred', {
  context: 'value',
  userId: '123',
  duration: '45ms'
});
```

## Performance

### Caching

**Cache:** `{{CACHE}}`

```{{LANGUAGE}}
{{CACHE_EXAMPLE}}
```

### Database Optimization

- Use indexes appropriately
- Avoid N+1 queries
- Implement pagination
- Use connection pooling
- Cache frequent queries

## Documentation

### Code Documentation

```{{LANGUAGE}}
/**
 * {{FUNCTION_DESCRIPTION}}
 * @param {{PARAM_TYPE}} {{PARAM_NAME}} - {{PARAM_DESCRIPTION}}
 * @returns {{RETURN_TYPE}} {{RETURN_DESCRIPTION}}
 * @throws {{ERROR_TYPE}} {{ERROR_DESCRIPTION}}
 * @example
 * {{USAGE_EXAMPLE}}
 */
```

### API Documentation

**Tool:** `{{API_DOC_TOOL}}`

Document:
- Endpoint paths and methods
- Request parameters and bodies
- Response schemas
- Error codes
- Authentication requirements
- Rate limits

## Deployment

### Containerization

**Tool:** `{{CONTAINER_TOOL}}`

```dockerfile
{{DOCKERFILE_TEMPLATE}}
```

### Build Artifacts

- `dist/` or `build/` directory
- Source maps enabled
- Minified for production
- Tree-shaking enabled

## Monitoring

### APM Tool

`{{APM_TOOL}}`

### Key Metrics to Track

- Response time (p50, p95, p99)
- Error rate
- Throughput
- Resource utilization (CPU, memory, disk)
- Business metrics

## Quick Reference

```bash
# Install dependencies
{{INSTALL_COMMAND}}

# Run dev server
{{DEV_COMMAND}}

# Run tests
{{TEST_COMMAND}}

# Build production
{{BUILD_COMMAND}}

# Lint code
{{LINT_COMMAND}}

# Format code
{{FORMAT_COMMAND}}
```

## Placeholders to Replace

When setting up a new project:

```
{{LANGUAGE}} -> Your language (TypeScript, Python, etc.)
{{LANGUAGE_VERSION}} -> Runtime version (Node 20, Python 3.11)
{{FRAMEWORK}} -> Your framework (Express, FastAPI, etc.)
{{TEST_FRAMEWORK}} -> Testing library (Jest, pytest)
{{LINTER}} -> Linting tool (ESLint, pylint)
{{FORMATTER}} -> Formatting tool (Prettier, Black)
{{DATABASE}} -> Your database (PostgreSQL, MongoDB)
{{ORM}} -> ORM/Query builder (Prisma, SQLAlchemy)
```