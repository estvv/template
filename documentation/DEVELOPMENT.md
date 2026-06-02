# Development Guide

## Overview

{{DEVELOPMENT_OVERVIEW}}

This guide covers local development setup, coding standards, and development workflow.

## Prerequisites

### Required Software

| Software | Version | Purpose |
|----------|---------|---------|
| {{LANGUAGE}} | {{LANGUAGE_VERSION}} | Runtime environment |
| {{PACKAGE_MANAGER}} | {{PM_VERSION}} | Dependency management |
| {{DATABASE}} | {{DB_VERSION}} | Database |
| Docker | 20.x | Containerization |
| Git | 2.x | Version control |

### Optional Tools

| Tool | Purpose |
|------|---------|
| nvm / pyenv / rustup | Version manager |
| {{IDE}} | Development environment |
| Postman / Insomnia | API testing |
| DBeaver / pgAdmin | Database client |

## Quick Start

### 1. Clone Repository

```bash
git clone {{REPO_URL}}
cd {{PROJECT_NAME}}
```

### 2. Install Dependencies

```bash
# Using npm
npm install

# Using yarn
yarn install

# Using pnpm
pnpm install
```

### 3. Environment Setup

```bash
# Copy environment template
cp .env.example .env

# Edit .env with your local values
{{EDITOR}} .env
```

Required environment variables:
```bash
{{ENV_VARS}}
DATABASE_URL=postgresql://localhost:5432/{{PROJECT_NAME}}_dev
API_KEY=your_api_key_here
SECRET_KEY=your_secret_key_here
```

### 4. Database Setup

```bash
# Create database
{{CREATE_DB_COMMAND}}

# Run migrations
{{MIGRATE_COMMAND}}

# Seed data (optional)
{{SEED_COMMAND}}
```

### 5. Start Development Server

```bash
# Start in development mode
{{DEV_COMMAND}}

# Start with watch mode
{{DEV_WATCH_COMMAND}}
```

### 6. Verify Setup

```bash
# Run tests
{{TEST_COMMAND}}

# Check linting
{{LINT_COMMAND}}

# Verify in browser
open http://localhost:{{PORT}}
```

## Project Structure

```
{{PROJECT_NAME}}/
├── src/
│   ├── {{MODULE_1}}/              # {{MODULE_1_DESCRIPTION}}
│   │   ├── controller.{{EXT}}      # Route handlers
│   │   ├── service.{{EXT}}         # Business logic
│   │   ├── model.{{EXT}}           # Data models
│   │   ├── repository.{{EXT}}      # Data access
│   │   └── test.{{EXT}}            # Module tests
│   ├── {{MODULE_2}}/              # {{MODULE_2_DESCRIPTION}}
│   ├── shared/                    # Shared utilities
│   │   ├── middleware/            # Middlewares
│   │   ├── utils/                 # Utilities
│   │   ├── types/                 # Type definitions
│   │   └── constants/             # Constants
│   ├── config/                    # Configuration
│   │   ├── database.{{EXT}}
│   │   ├── app.{{EXT}}
│   │   └── index.{{EXT}}
│   └── index.{{EXT}}              # Entry point
├── tests/
│   ├── unit/                      # Unit tests
│   ├── integration/              # Integration tests
│   └── e2e/                      # End-to-end tests
├── docs/
│   ├── ARCHITECTURE.md
│   ├── API.md
│   └── DATABASE.md
├── config/
│   ├── development.{{EXT}}
│   ├── production.{{EXT}}
│   └── test.{{EXT}}
├── scripts/
│   ├── setup.sh
│   ├── migrate.sh
│   └── deploy.sh
├── .env.example                   # Environment template
├── .gitignore                     # Git ignore rules
├── package.json / requirements.txt
└── README.md
```

## Development Workflow

### Branch Strategy

```
main
  └── develop
      ├── feature/user-auth
      ├── feature/api-endpoints
      └── fix/login-bug
```

**Branch Naming:**
- `feature/` - New features
- `fix/` - Bug fixes
- `refactor/` - Code refactoring
- `docs/` - Documentation updates
- `test/` - Test additions/updates
- `chore/` - Maintenance tasks

### Development Process

```bash
# 1. Create feature branch
git checkout develop
git pull origin develop
git checkout -b feature/my-feature

# 2. Make changes
{{EDITOR}} src/

# 3. Run tests
{{TEST_COMMAND}}

# 4. Commit changes
git add .
git commit -m "feat: add my feature"

# 5. Push to remote
git push origin feature/my-feature

# 6. Create pull request
# (via GitHub/GitLab UI)

# 7. After approval, merge via squash
# (Squash and merge preferred)
```

### Commit Guidelines

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Code Review Process

1. **Author:**
   - Create PR with clear description
   - Link related issues
   - Add screenshots (if UI changes)
   - Self-review changes

2. **Reviewer:**
   - Review within 24 hours
   - Provide constructive feedback
   - Approve when ready
   - Request changes if needed

3. **Merge:**
   - Squash and merge
   - Delete feature branch
   - Update changelog (if needed)

## Coding Standards

### Style Guide

**Linter:** `{{LINTER}}`

**Formatter:** `{{FORMATTER}}`

```bash
# Run linter
{{LINT_COMMAND}}

# Fix linting issues
{{LINT_FIX_COMMAND}}

# Format code
{{FORMAT_COMMAND}}
```

### File Naming

- **Files:** `kebab-case.{{EXT}}` (e.g., `user-service.ts`)
- **Classes:** `PascalCase` (e.g., `UserService`)
- **Functions:** `camelCase` (e.g., `getUserById`)
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `MAX_RETRIES`)
- **Components:** `PascalCase.{{EXT}}` (e.g., `Button.tsx`)

### Import Order

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

### Code Conventions

**Functions:**
```{{LANGUAGE}}
/**
 * {{FUNCTION_DESCRIPTION}}
 * @param {{PARAM_NAME}} - {{PARAM_DESCRIPTION}}
 * @returns {{RETURN_DESCRIPTION}}
 */
function {{FUNCTION_NAME}}({{PARAMS}}): {{RETURN_TYPE}} {
  // Implementation
}
```

**Classes:**
```{{LANGUAGE}}
class {{CLASS_NAME}} {
  // Properties
  private {{PROPERTY}}: {{TYPE}};

  // Constructor
  constructor({{PARAMS}}) {
    // Initialization
  }

  // Methods
  public {{METHOD}}(): {{RETURN_TYPE}} {
    // Implementation
  }
}
```

**Error Handling:**
```{{LANGUAGE}}
try {
  // Operation
} catch (error) {
  logger.error('Operation failed', { error, context });
  throw new {{ERROR_CLASS}}('User-friendly message');
}
```

### Documentation

**Code Comments:**
```{{LANGUAGE}}
// Good: Explain why, not what
// Calculate total including tax (8.5%) and shipping
const total = subtotal * 1.085 + shipping;

// Bad: Obvious comment
// Add shipping to total
const total = subtotal + shipping;
```

**JSDoc/TSDoc:**
```{{LANGUAGE}}
/**
 * Calculate total order price
 * @param items - Array of order items
 * @param options - Calculation options
 * @returns Total price including tax and shipping
 * @throws {ValidationError} If items array is empty
 * @example
 * const total = calculateTotal([{ price: 10, quantity: 2 }], { tax: 8.5 });
 */
function calculateTotal(items: Item[], options: Options): number {
  // Implementation
}
```

## Testing

### Test Structure

```
tests/
├── unit/
│   ├── {{MODULE_1}}.test.{{EXT}}
│   └── {{MODULE_2}}.test.{{EXT}}
├── integration/
│   ├── api.test.{{EXT}}
│   └── database.test.{{EXT}}
└── e2e/
    ├── user-flow.test.{{EXT}}
    └── checkout.test.{{EXT}}
```

### Running Tests

```bash
# Run all tests
{{TEST_COMMAND}}

# Run unit tests
{{TEST_UNIT_COMMAND}}

# Run integration tests
{{TEST_INTEGRATION_COMMAND}}

# Run e2e tests
{{TEST_E2E_COMMAND}}

# Run with coverage
{{TEST_COVERAGE_COMMAND}}

# Run in watch mode
{{TEST_WATCH_COMMAND}}
```

### Writing Tests

**Unit Test Example:**
```{{LANGUAGE}}
describe('{{FUNCTION_NAME}}', () => {
  it('should {{EXPECTED_BEHAVIOR}}', () => {
    // Arrange
    const input = {{INPUT}};

    // Act
    const result = {{FUNCTION_NAME}}(input);

    // Assert
    expect(result).toBe({{EXPECTED}});
  });

  it('should throw error when {{CONDITION}}', () => {
    expect(() => {{FUNCTION_NAME}}({{INVALID_INPUT}}))
      .toThrow({{ERROR_TYPE}});
  });
});
```

**Integration Test Example:**
```{{LANGUAGE}}
describe('API /{{ENDPOINT}}', () => {
  beforeAll(async () => {
    await setupDatabase();
  });

  afterAll(async () => {
    await teardownDatabase();
  });

  it('should return {{EXPECTED}}', async () => {
    const response = await request(app)
      .get('/{{ENDPOINT}}')
      .set('Authorization', `Bearer ${token}`);

    expect(response.status).toBe(200);
    expect(response.body).toMatchObject({{EXPECTED_BODY}});
  });
});
```

### Test Coverage

**Coverage Threshold:** `{{COVERAGE_THRESHOLD}}` (e.g., 80%)

```bash
# Generate coverage report
{{TEST_COVERAGE_COMMAND}}

# View coverage report
open coverage/index.html
```

### Test Best Practices

1. **AAA Pattern:** Arrange, Act, Assert
2. **One assertion per test** (usually)
3. **Descriptive test names**
4. **Test edge cases**
5. **Mock external dependencies**
6. **Keep tests isolated**

## Debugging

### Logging

**Log Levels:**
- `ERROR` - Critical failures
- `WARN` - Potential issues
- `INFO` - Business events
- `DEBUG` - Detailed flow
- `TRACE` - Very detailed debugging

**Log Format:**
```{{LANGUAGE}}
logger.info('Operation completed', {
  context: 'UserService',
  userId: '123',
  duration: '45ms'
});
```

### Debug Tools

```bash
# Start in debug mode
{{DEBUG_COMMAND}}

# Node.js
node --inspect src/index.js

# Python
python -m pdb src/main.py
```

### IDE Debug Configuration

**VS Code:**
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}/src/index.js",
      "envFile": "${workspaceFolder}/.env"
    }
  ]
}
```

### Common Issues

**Port Already in Use:**
```bash
# Find process using port
lsof -i :{{PORT}}

# Kill process
kill -9 {{PID}}
```

**Database Connection Failed:**
```bash
# Check database is running
{{DB_STATUS_COMMAND}}

# Verify connection string
echo $DATABASE_URL

# Test connection
{{DB_TEST_CONNECTION}}
```

**Module Not Found:**
```bash
# Clear cache and reinstall
rm -rf node_modules
rm package-lock.json
npm install
```

## Database Management

### Migrations

```bash
# Create migration
{{CREATE_MIGRATION_COMMAND}}

# Run migrations
{{MIGRATE_COMMAND}}

# Rollback last migration
{{MIGRATE_ROLLBACK_COMMAND}}

# Check migration status
{{MIGRATE_STATUS_COMMAND}}
```

### Seeding

```bash
# Run seeders
{{SEED_COMMAND}}

# Run specific seeder
{{SEED_SPECIFIC_COMMAND}} --name=UserSeeder
```

### Querying

```bash
# Connect to database
{{DB_CONNECT_COMMAND}}

# Run query
{{DB_QUERY_COMMAND}}
```

## API Development

### Testing Endpoints

**cURL:**
```bash
curl -X POST http://localhost:{{PORT}}/api/endpoint \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer {{TOKEN}}" \
  -d '{"key": "value"}'
```

**Postman Collection:**
Import `postman_collection.json` into Postman.

**HTTP Client (VS Code):**
```http
### Get Users
GET http://localhost:{{PORT}}/api/users
Authorization: Bearer {{TOKEN}}

### Create User
POST http://localhost:{{PORT}}/api/users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com"
}
```

### Mocking External APIs

```{{LANGUAGE}}
// Use environment variables for API URLs
const API_BASE_URL = process.env.{{API_URL_VAR}};

// Mock in development
if (process.env.NODE_ENV === 'development') {
  mockExternalAPI();
}
```

## Performance

### Profiling

```bash
# CPU profiling
{{PROFILING_COMMAND}}

# Memory profiling
{{MEMORY_PROFILE_COMMAND}}
```

### Optimization Tips

1. **Database:**
   - Use indexes
   - Avoid N+1 queries
   - Use connection pooling
   - Implement caching

2. **API:**
   - Implement pagination
   - Use compression
   - Cache responses
   - Rate limiting

3. **Code:**
   - Avoid premature optimization
   - Profile before optimizing
   - Use efficient algorithms
   - Minimize dependencies

## Environment Variables

### Development (.env)

```bash
# Environment
NODE_ENV=development
LOG_LEVEL=debug

# Database
DATABASE_URL=postgresql://localhost:5432/{{PROJECT_NAME}}_dev

# API Keys
API_KEY=dev_api_key
SECRET_KEY=dev_secret_key

# External Services
EXTERNAL_API_URL=https://api.example.com
```

### Test (.env.test)

```bash
# Environment
NODE_ENV=test
LOG_LEVEL=error

# Database
DATABASE_URL=postgresql://localhost:5432/{{PROJECT_NAME}}_test

# API Keys
API_KEY=test_api_key
SECRET_KEY=test_secret_key
```

### Production (.env.production)

```bash
# Environment
NODE_ENV=production
LOG_LEVEL=info

# Database (from secrets manager)
DATABASE_URL=${PROD_DATABASE_URL}

# API Keys (from secrets manager)
API_KEY=${PROD_API_KEY}
SECRET_KEY=${PROD_SECRET_KEY}
```

## Git Hooks

### Pre-commit

```bash
#!/bin/sh
# Run linting
{{LINT_COMMAND}}

# Run tests
{{TEST_COMMAND}}

# Check for secrets
git diff --cached | grep -i "password\|secret\|api.key" && exit 1
```

### Setup Hooks

```bash
# Using Husky (npm)
npm install husky --save-dev
npx husky add .husky/pre-commit "npm run lint && npm test"

# Using pre-commit (Python)
pip install pre-commit
pre-commit install
```

## IDE Setup

### VS Code

**Recommended Extensions:**
```json
{
  "recommendations": [
    "{{LANG_EXTENSION}}",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "ms-python.python",
    "rust-lang.rust-analyzer"
  ]
}
```

**Settings:**
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "{{FORMATTER}}",
  "editor.codeActionsOnSave": {
    "source.fixAll": true
  },
  "{{LANG}}.format.enable": true
}
```

### IntelliJ IDEA

**Settings:**
- Enable `{{LANG}}` plugin
- Configure code style
- Set up run configurations

## Scripts

### package.json Scripts

```json
{
  "scripts": {
    "dev": "{{DEV_COMMAND}}",
    "build": "{{BUILD_COMMAND}}",
    "start": "{{START_COMMAND}}",
    "test": "{{TEST_COMMAND}}",
    "test:watch": "{{TEST_WATCH_COMMAND}}",
    "test:coverage": "{{TEST_COVERAGE_COMMAND}}",
    "lint": "{{LINT_COMMAND}}",
    "lint:fix": "{{LINT_FIX_COMMAND}}",
    "format": "{{FORMAT_COMMAND}}",
    "migrate": "{{MIGRATE_COMMAND}}",
    "seed": "{{SEED_COMMAND}}",
    "db:reset": "{{DB_RESET_COMMAND}}"
  }
}
```

### Custom Scripts

```bash
# scripts/setup.sh
#!/bin/bash
set -e

echo "Setting up development environment..."
cp .env.example .env
npm install
npm run migrate
npm run seed

echo "Setup complete! Run 'npm run dev' to start."
```

## Docker Development

### Development with Docker

```bash
# Start all services
docker-compose up

# Start specific service
docker-compose up app db

# Run command in container
docker-compose exec app {{COMMAND}}

# View logs
docker-compose logs -f app

# Stop all services
docker-compose down
```

### Docker Compose Override

```yaml
# docker-compose.override.yml
version: '3.8'

services:
  app:
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DEBUG=*
    command: npm run dev
```

## Troubleshooting

### Common Issues

**Node modules issues:**
```bash
rm -rf node_modules package-lock.json
npm install
```

**Port conflicts:**
```bash
# Find process
lsof -i :3000
# Kill process
kill -9 <PID>
```

**Database connection:**
```bash
# Check database is running
docker ps | grep db

# Test connection
psql -h localhost -U user -d mydb
```

**Permission denied:**
```bash
# Make scripts executable
chmod +x scripts/*.sh
```

## Resources

### Documentation
- [Architecture](./ARCHITECTURE.md) - System architecture
- [API](./API.md) - API documentation
- [Database](./DATABASE.md) - Database schema
- [Testing](./TESTING.md) - Testing strategy

### External Links
- [Language Docs]({{LANG_DOCS_URL}})
- [Framework Docs]({{FRAMEWORK_DOCS_URL}})
- [Best Practices]({{BEST_PRACTICES_URL}})

### Getting Help
- Check existing issues on GitHub
- Ask in team chat: {{SLACK_CHANNEL}}
- Office hours: {{OFFICE_HOURS}}
