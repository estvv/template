# Testing Patterns

Common testing patterns and examples.

## Parametrized Tests

Test multiple inputs with one test body:

```typescript
describe('validateEmail', () => {
  const testCases = [
    { input: 'test@example.com', expected: true },
    { input: 'invalid-email', expected: false },
    { input: 'test+spam@example.com', expected: true },
    { input: '@example.com', expected: false },
  ];

  testCases.forEach(({ input, expected }) => {
    it(`should return ${expected} for "${input}"`, () => {
      expect(validateEmail(input)).toBe(expected);
    });
  });
});
```

## Test Fixtures

Reuse test data across tests:

```typescript
// fixtures/users.ts
export const validUser = {
  name: 'John Doe',
  email: 'john@example.com',
  age: 30,
};

export const invalidUser = {
  name: '',
  email: 'invalid',
  age: -5,
};

// tests/user.test.ts
import { validUser, invalidUser } from './fixtures/users';

describe('User validation', () => {
  it('should accept valid user', () => {
    expect(validateUser(validUser)).toBe(true);
  });
});
```

## Mock Patterns

### Dependency Injection

```typescript
class UserService {
  constructor(private repository: UserRepository) {}
  
  async getUser(id: string) {
    return this.repository.findById(id);
  }
}

// Test with mock
const mockRepository = {
  findById: jest.fn().mockResolvedValue({ id: '123', name: 'John' }),
};

const service = new UserService(mockRepository);
```

### Mock External APIs

```typescript
// Mock fetch globally
global.fetch = jest.fn(() =>
  Promise.resolve({
    ok: true,
    json: () => Promise.resolve({ data: 'mock data' }),
  })
);

// Test
const result = await fetchData();
expect(result).toEqual({ data: 'mock data' });
expect(fetch).toHaveBeenCalledWith('https://api.example.com/data');
```

## Async Testing

### Promises

```typescript
it('should fetch user', async () => {
  const user = await fetchUser('123');
  expect(user.name).toBe('John');
});

it('should reject for invalid id', async () => {
  await expect(fetchUser('invalid')).rejects.toThrow('User not found');
});
```

### Callbacks (prefer Promises)

```typescript
it('should call callback with result', (done) => {
  fetchData((result) => {
    expect(result).toBe('data');
    done();
  });
});
```

## Test Organization

### Directory Structure

```
src/
  services/
    user.service.ts
    user.service.test.ts    # Co-located tests
tests/
  integration/
    user.integration.test.ts
  e2e/
    registration.e2e.test.ts
  fixtures/
    users.ts
  helpers/
    setup.ts
```

### Naming Conventions

```typescript
// What vs. How
it('should return user name', () => {});           // Good - describes behavior
it('should get name property', () => {});          // Bad - describes implementation

// Should / When
it('should throw error when user not found', () => {});  // Good
it('throws error', () => {});                             // Bad - incomplete
```

## Edge Cases

```typescript
describe('divide', () => {
  it('should divide positive numbers', () => {
    expect(divide(10, 2)).toBe(5);
  });

  it('should handle zero numerator', () => {
    expect(divide(0, 5)).toBe(0);
  });

  it('should throw error for zero denominator', () => {
    expect(() => divide(10, 0)).toThrow('Division by zero');
  });

  it('should handle decimal results', () => {
    expect(divide(10, 3)).toBeCloseTo(3.333, 2);
  });
});
```

## Error Testing

```typescript
describe('createUser', () => {
  it('should throw error for invalid email', () => {
    expect(() => createUser('invalid-email')).toThrow('Invalid email');
  });

  it('should throw error for duplicate email', async () => {
    await expect(createUser('existing@example.com')).rejects.toThrow('Email already exists');
  });

  it('should include error details', async () => {
    try {
      await createUser('invalid');
      fail('Should have thrown');
    } catch (error) {
      expect(error.message).toBe('Invalid email');
      expect(error.code).toBe('INVALID_EMAIL');
    }
  });
});
```

## Snapshot Testing

```typescript
it('should match snapshot', () => {
  const component = render(<UserProfile user={validUser} />);
  expect(component).toMatchSnapshot();
});

// Update snapshots: npm test -- -u
```

## Performance Testing

```typescript
describe('performance', () => {
  it('should process 10k items in under 100ms', () => {
    const items = Array(10000).fill(null).map((_, i) => ({ id: i }));

    const start = performance.now();
    processItems(items);
    const duration = performance.now() - start;

    expect(duration).toBeLessThan(100);
  });
});
```