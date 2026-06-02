# Testing Strategy

## Overview

{{TESTING_OVERVIEW}}

**Testing Philosophy:** {{TESTING_PHILOSOPHY}}

**Coverage Goal:** `{{COVERAGE_THRESHOLD}}` minimum

## Testing Pyramid

```
        ╱╲
       ╱  ╲
      ╱ E2E╲      End-to-End Tests (Slow, Expensive)
     ╱──────╲     
    ╱        ╲
   ╱Integration╲   Integration Tests (Medium Speed)
  ╱────────────╲
 ╱              ╲
╱   Unit Tests   ╲  Unit Tests (Fast, Cheap)
╱──────────────────╲
```

**Distribution:**
- **Unit Tests:** 70% of tests
- **Integration Tests:** 20% of tests
- **E2E Tests:** 10% of tests

## Testing Types

### Unit Tests

**Purpose:** Test individual functions, classes, or modules in isolation.

**Characteristics:**
- Fast execution
- No external dependencies
- Mocked dependencies
- Tests single responsibility

**Example:**
```{{LANGUAGE}}
describe('UserService', () => {
  describe('createUser', () => {
    it('should create a new user', async () => {
      // Arrange
      const userData = {
        email: 'user@example.com',
        name: 'John Doe'
      };
      const mockRepo = {
        create: jest.fn().mockResolvedValue({ id: '1', ...userData })
      };
      const service = new UserService(mockRepo);
      
      // Act
      const result = await service.createUser(userData);
      
      // Assert
      expect(result).toMatchObject({
        id: '1',
        email: 'user@example.com',
        name: 'John Doe'
      });
      expect(mockRepo.create).toHaveBeenCalledWith(userData);
    });
    
    it('should throw error for invalid email', async () => {
      const service = new UserService(mockRepo);
      
      await expect(service.createUser({ email: 'invalid' }))
        .rejects.toThrow(ValidationError);
    });
  });
});
```

### Integration Tests

**Purpose:** Test how multiple units work together.

**Characteristics:**
- Tests real dependencies
- Uses test database
- May use test API server
- Slower than unit tests

**Example:**
```{{LANGUAGE}}
describe('User API Integration', () => {
  let app: Application;
  let db: Database;
  
  beforeAll(async () => {
    app = await createTestApp();
    db = await createTestDatabase();
  });
  
  afterAll(async () => {
    await db.close();
    await app.close();
  });
  
  beforeEach(async () => {
    await db.clear();
  });
  
  describe('POST /api/users', () => {
    it('should create user and return 201', async () => {
      const response = await request(app)
        .post('/api/users')
        .send({
          email: 'user@example.com',
          name: 'John Doe'
        });
      
      expect(response.status).toBe(201);
      expect(response.body.data).toMatchObject({
        email: 'user@example.com',
        name: 'John Doe'
      });
      
      // Verify in database
      const user = await db.users.findByEmail('user@example.com');
      expect(user).toBeDefined();
    });
    
    it('should return 400 for duplicate email', async () => {
      await db.users.create({ email: 'user@example.com' });
      
      const response = await request(app)
        .post('/api/users')
        .send({
          email: 'user@example.com',
          name: 'Jane Doe'
        });
      
      expect(response.status).toBe(400);
      expect(response.body.error.code).toBe('DUPLICATE_EMAIL');
    });
  });
});
```

### End-to-End Tests

**Purpose:** Test complete user flows from start to finish.

**Characteristics:**
- Tests real system
- No mocking
- Tests through UI or API
- Slowest tests
- Most realistic

**Example:**
```{{LANGUAGE}}
describe('User Registration Flow E2E', () => {
  let browser: Browser;
  let page: Page;
  
  beforeAll(async () => {
    browser = await launch({ headless: true });
    page = await browser.newPage();
  });
  
  afterAll(async () => {
    await browser.close();
  });
  
  it('should register new user successfully', async () => {
    // Navigate to registration page
    await page.goto('http://localhost:3000/register');
    
    // Fill form
    await page.type('#email', 'user@example.com');
    await page.type('#name', 'John Doe');
    await page.type('#password', 'SecurePass123!');
    
    // Submit
    await page.click('#submit');
    
    // Wait for redirect
    await page.waitForNavigation();
    
    // Verify at dashboard
    expect(page.url()).toContain('/dashboard');
    
    // Verify user created in database
    const user = await db.users.findByEmail('user@example.com');
    expect(user).toBeDefined();
  });
});
```

## Test Structure

### Directory Structure

```
tests/
├── unit/
│   ├── services/
│   │   ├── user.service.test.ts
│   │   └── auth.service.test.ts
│   ├── repositories/
│   │   ├── user.repository.test.ts
│   │   └── post.repository.test.ts
│   └── utils/
│       ├── validation.test.ts
│       └── helpers.test.ts
├── integration/
│   ├── api/
│   │   ├── users.test.ts
│   │   ├── posts.test.ts
│   │   └── auth.test.ts
│   └── database/
│       ├── user-queries.test.ts
│       └── migrations.test.ts
├── e2e/
│   ├── user-registration.test.ts
│   ├── post-creation.test.ts
│   └── checkout.test.ts
├── fixtures/
│   ├── users.fixture.ts
│   └── posts.fixture.ts
├── helpers/
│   ├── db.helper.ts
│   └── api.helper.ts
└── setup.ts
```

### Test File Naming

- Unit tests: `{{name}}.test.{{ext}}` or `{{name}}.spec.{{ext}}`
- Integration tests: `{{name}}.integration.test.{{ext}}`
- E2E tests: `{{name}}.e2e.test.{{ext}}`

### Test Organization

```{{LANGUAGE}}
describe('{{MODULE_NAME}}', () => {
  describe('{{METHOD_NAME}}', () => {
    describe('when {{CONDITION}}', () => {
      it('should {{EXPECTED_BEHAVIOR}}', () => {
        // Test
      });
    });
    
    describe('when {{ERROR_CONDITION}}', () => {
      it('should throw {{ERROR_TYPE}}', () => {
        // Test
      });
    });
  });
});
```

## Test Data

### Fixtures

```{{LANGUAGE}}
// tests/fixtures/users.fixture.ts
export const userFixture = {
  valid: {
    email: 'user@example.com',
    name: 'John Doe',
    password: 'SecurePass123!'
  },
  
  invalid: {
    emptyEmail: { email: '', name: 'John Doe' },
    invalidEmail: { email: 'invalid', name: 'John Doe' },
    missingName: { email: 'user@example.com', name: '' }
  },
  
  admin: {
    email: 'admin@example.com',
    name: 'Admin User',
    role: 'admin'
  }
};
```

### Factories

```{{LANGUAGE}}
// tests/helpers/user.factory.ts
export class UserFactory {
  static create(overrides = {}) {
    return {
      id: uuid(),
      email: `user-${Date.now()}@example.com`,
      name: 'Test User',
      ...overrides
    };
  }
  
  static createMany(count: number, overrides = {}) {
    return Array.from({ length: count }, () => this.create(overrides));
  }
  
  static async createAndSave(db: Database, overrides = {}) {
    const user = this.create(overrides);
    return db.users.create(user);
  }
}
```

### Seed Data

```{{LANGUAGE}}
// tests/helpers/seed.ts
export async function seedDatabase(db: Database) {
  await db.users.createMany([
    UserFactory.create({ role: 'admin' }),
    UserFactory.create({ role: 'user' }),
    UserFactory.create({ role: 'user' })
  ]);
  
  await db.posts.createMany([
    PostFactory.create({ userId: '1', status: 'published' }),
    PostFactory.create({ userId: '2', status: 'draft' })
  ]);
}
```

## Mocking

### Function Mocks

```{{LANGUAGE}}
// Mock external dependency
const mockEmailService = {
  sendEmail: jest.fn().mockResolvedValue({ success: true })
};

// Use in test
const service = new UserService(mockEmailService);
```

### Module Mocks

```{{LANGUAGE}}
// Mock entire module
jest.mock('../services/email.service', () => ({
  sendEmail: jest.fn().mockResolvedValue({ success: true })
}));

// Partial mock
jest.mock('../services/email.service', () => ({
  ...jest.requireActual('../services/email.service'),
  sendEmail: jest.fn().mockResolvedValue({ success: true })
}));
```

### Database Mocks

```{{LANGUAGE}}
// In-memory database for tests
import { createConnection } from 'typeorm';
import { SqliteConnection } from 'typeorm-driver-sqlite';

export async function createTestDatabase() {
  return createConnection({
    type: 'sqlite',
    database: ':memory:',
    entities: [/* ... */],
    synchronize: true
  });
}
```

### External API Mocks

```{{LANGUAGE}}
// Mock fetch/API calls
import { rest } from 'msw';
import { setupServer } from 'msw/node';

const server = setupServer(
  rest.get('https://api.example.com/users', (req, res, ctx) => {
    return res(ctx.json({ data: [{ id: '1', name: 'John' }] }));
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

## Running Tests

### Commands

```bash
# Run all tests
{{TEST_COMMAND}}

# Run unit tests
{{TEST_UNIT_COMMAND}}

# Run integration tests
{{TEST_INTEGRATION_COMMAND}}

# Run e2e tests
{{TEST_E2E_COMMAND}}

# Run specific test file
{{TEST_FILE_COMMAND}} path/to/file.test.ts

# Run with pattern
{{TEST_PATTERN_COMMAND}} "UserService"

# Run in watch mode
{{TEST_WATCH_COMMAND}}

# Run with coverage
{{TEST_COVERAGE_COMMAND}}

# Run in parallel
{{TEST_PARALLEL_COMMAND}}

# Update snapshots
{{TEST_UPDATE_SNAPSHOTS_COMMAND}}
```

### Test Configuration

```{{LANGUAGE}}
// jest.config.js / vitest.config.ts / pytest.ini
export default {
  testEnvironment: 'node',
  roots: ['<rootDir>/tests'],
  testMatch: ['**/*.test.ts'],
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts'
  ],
  coverageThreshold: {
    global: {
      branches: {{COVERAGE_THRESHOLD}},
      functions: {{COVERAGE_THRESHOLD}},
      lines: {{COVERAGE_THRESHOLD}},
      statements: {{COVERAGE_THRESHOLD}}
    }
  },
  setupFilesAfterEnv: ['<rootDir>/tests/setup.ts']
};
```

## Test Coverage

### Coverage Metrics

```bash
# Generate coverage report
{{TEST_COVERAGE_COMMAND}}

# View report
open coverage/lcov-report/index.html
```

### Coverage Thresholds

```json
{
  "coverageThreshold": {
    "global": {
      "branches": 80,
      "functions": 80,
      "lines": 80,
      "statements": 80
    }
  }
}
```

### What to Test

**Always Test:**
- Business logic
- Edge cases
- Error handling
- State transitions
- Permission checks
- Validation logic

**May Test:**
- Complex algorithms
- Calculations
- Data transformations

**Don't Test:**
- Framework code
- Third-party libraries
- Trivial getters/setters
- Simple configuration

## Test Best Practices

### 1. AAA Pattern

```{{LANGUAGE}}
it('should {{EXPECTED_BEHAVIOR}}', () => {
  // Arrange
  const input = {{TEST_DATA}};
  const expected = {{EXPECTED_OUTPUT}};
  
  // Act
  const result = {{FUNCTION}}(input);
  
  // Assert
  expect(result).toEqual(expected);
});
```

### 2. One Assertion Per Test (Usually)

```{{LANGUAGE}}
// Good: One clear assertion
it('should return user by ID', async () => {
  const user = await getUserById('1');
  expect(user.id).toBe('1');
});

// Sometimes okay: Related assertions
it('should create user with correct properties', async () => {
  const user = await createUser({ name: 'John' });
  expect(user.id).toBeDefined();
  expect(user.name).toBe('John');
  expect(user.createdAt).toBeInstanceOf(Date);
});
```

### 3. Descriptive Test Names

```{{LANGUAGE}}
// Bad
it('test1', () => {});
it('works', () => {});

// Good
it('should return 404 when user not found', async () => {});
it('should throw ValidationError for invalid email', async () => {});
it('should create user with hashed password', async () => {});
```

### 4. Test Edge Cases

```{{LANGUAGE}}
describe('divide', () => {
  it('should divide two numbers', () => {
    expect(divide(10, 2)).toBe(5);
  });
  
  // Edge case
  it('should throw error for division by zero', () => {
    expect(() => divide(10, 0)).toThrow(DivisionByZeroError);
  });
  
  // Edge case
  it('should handle negative numbers', () => {
    expect(divide(-10, 2)).toBe(-5);
  });
});
```

### 5. Isolate Tests

```{{LANGUAGE}}
// Bad: Tests depend on each other
let user;
it('test1', () => { user = createUser(); });
it('test2', () => { expect(user.name).toBe('John'); });

// Good: Each test is isolated
beforeEach(() => {
  user = UserFactory.create();
});

it('test1', () => {
  expect(user.name).toBeDefined();
});

it('test2', () => {
  expect(user.email).toBeDefined();
});
```

### 6. Use Test Helpers

```{{LANGUAGE}}
// Bad: Repeated setup code
it('test1', async () => {
  const app = await createTestApp();
  const db = await createTestDatabase();
  const user = await UserFactory.createAndSave(db);
  // ...
});

// Good: Extract helper
async function setupTest() {
  const app = await createTestApp();
  const db = await createTestDatabase();
  const user = await UserFactory.createAndSave(db);
  return { app, db, user };
}

it('test1', async () => {
  const { app, user } = await setupTest();
  // ...
});
```

## Test-Driven Development (TDD)

### TDD Cycle

```
1. Write a failing test (Red)
2. Write minimal code to pass (Green)
3. Refactor code (Refactor)
4. Repeat
```

### TDD Example

```{{LANGUAGE}}
// 1. Write failing test (Red)
it('should calculate total with tax', () => {
  const items = [{ price: 100, quantity: 2 }];
  const total = calculateTotal(items, { tax: 10 });
  expect(total).toBe(220); // 100 * 2 * 1.10
});

// 2. Write minimal code (Green)
function calculateTotal(items, options) {
  const subtotal = items.reduce((sum, item) => 
    sum + item.price * item.quantity, 0);
  const tax = options.tax / 100;
  return subtotal * (1 + tax);
}

// 3. Refactor (if needed)
function calculateTotal(items, options = {}) {
  const subtotal = items.reduce(
    (sum, { price, quantity }) => sum + price * quantity,
    0
  );
  
  const taxRate = (options.tax || 0) / 100;
  return subtotal * (1 + taxRate);
}
```

## Continuous Integration

### CI Test Pipeline

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [18, 20]
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linter
        run: npm run lint
      
      - name: Run unit tests
        run: npm run test:unit
      
      - name: Run integration tests
        run: npm run test:integration
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage/lcov.info
```

### Pre-commit Hooks

```bash
# Run linting and tests before commit
npm run lint
npm run test:unit
```

## Performance Testing

### Load Testing

```{{LANGUAGE}}
// Using Artillery / k6 / JMeter
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  vus: 100,
  duration: '30s'
};

export default function() {
  const res = http.get('http://localhost:3000/api/users');
  
  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time < 200ms': (r) => r.timings.duration < 200
  });
  
  sleep(1);
}
```

### Benchmark Tests

```{{LANGUAGE}}
describe('Performance', () => {
  it('should handle 1000 requests per second', async () => {
    const requests = Array(1000).fill(null).map(() => 
      request(app).get('/api/health')
    );
    
    const start = Date.now();
    await Promise.all(requests);
    const duration = Date.now() - start;
    
    expect(duration).toBeLessThan(1000);
  });
});
```

## Testing Checklist

### Before Committing

- [ ] All tests pass
- [ ] New code has tests
- [ ] Coverage threshold met
- [ ] No skipped tests
- [ ] Linting passes
- [ ] No console.logs

### Test Quality Checklist

- [ ] Tests are isolated
- [ ] Tests are deterministic
- [ ] Edge cases covered
- [ ] Error cases tested
- [ ] Mocks are appropriate
- [ ] Test names are descriptive
- [ ] No test interdependencies

## Troubleshooting

### Common Issues

**Tests pass locally but fail in CI:**
- Check environment variables
- Verify test database state
- Check timezones/dates
- Verify file paths

**Flaky tests:**
- Remove time-dependent assertions
- Use proper async/await
- Add retry mechanisms
- Isolate shared state

**Slow tests:**
- Use mocks instead of real dependencies
- Run tests in parallel
- Optimize database queries
- Use in-memory databases

**Memory issues:**
- Close database connections
- Clear mocks between tests
- Use `--runInBand` for memory-intensive tests

## Related Documentation

- [DEVELOPMENT.md](./DEVELOPMENT.md) - Development setup
- [API.md](./API.md) - API documentation
- [DATABASE.md](./DATABASE.md) - Database schema
- [SECURITY.md](./SECURITY.md) - Security considerations