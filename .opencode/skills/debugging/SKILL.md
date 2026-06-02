---
name: debugging
description: Systematic approach to finding and fixing bugs. Use when investigating issues, fixing errors, diagnosing problems, or understanding unexpected behavior.
license: MIT
---

## Debugging Principle

**Assume nothing is true until verified.**

The system is trying to tell you something. Listen to errors.

## Debugging Workflow

```
Bug Report → Reproduce → Isolate → Hypothesize → Test → Fix → Verify → Document
```

1. **Reproduce** - Can't fix what you can't reproduce
2. **Isolate** - Narrow down where the bug is
3. **Hypothesize** - Form a theory about the cause
4. **Test hypothesis** - Verify or falsify
5. **Fix** - Implement the fix
6. **Verify** - Ensure fix works and tests pass
7. **Document** - Explain fix in commit message

## Reproduction

**Critical:** If you can't reproduce it, you can't fix it.

- Get exact steps from user
- Match environment (browser, OS, version)
- Match data (specific user, specific record)
- Match timing (concurrent operations)

## Isolation Techniques

### Divide and Conquer

```typescript
// Is the bug in frontend or backend?
// 1. Test backend directly (curl, Postman)
// 2. If backend works → bug in frontend
// 3. If backend fails → continue isolating backend

// Is the bug in the route or controller?
// 1. Add log at route entry
// 2. Add log at controller entry
// 3. Which one is reached?
```

### Binary Search

```typescript
// Bug appears after processing 1000 items
// Test with 500 items → bug appears?
// - Yes → bug in first half
// - No → bug in second half
// Continue narrowing down
```

## Logging Strategy

### Structured Logging

```typescript
// Bad
console.log('User logged in', userId, timestamp);

// Good
logger.info('User logged in', {
  userId: 'user-123',
  timestamp: new Date().toISOString(),
  ip: '192.168.1.1'
});
```

### Log Levels

- `error` - Something broken (immediate attention)
- `warn` - Something unusual (investigation needed)
- `info` - Normal operations (significant events)
- `debug` - Detailed information (development)

## Common Bug Patterns

### 1. Null/Undefined Errors

```typescript
// Problem
const user = users.find(u => u.id === id);
console.log(user.name); // TypeError

// Debug - add null checks
const user = users.find(u => u.id === id);
if (!user) {
  console.error(`User not found for id: ${id}`);
  return null;
}
```

### 2. Race Conditions

```typescript
// Problem - multiple calls race to set cache
let cache = null;
async function getData() {
  if (cache) return cache;
  cache = await fetchUser(); // Race condition
  return cache;
}

// Fix - cache promise
const userPromise = new Map();
async function getData(id) {
  if (userPromise.has(id)) return userPromise.get(id);
  const promise = fetchUser(id).finally(() => userPromise.delete(id));
  userPromise.set(id, promise);
  return promise;
}
```

### 3. Off-by-One Errors

```typescript
// Problem
for (let i = 0; i <= array.length; i++) { // Should be <
  console.log(array[i]); // undefined on last iteration
}

// Debug - check boundaries
for (let i = 0; i < array.length; i++) {
  console.log(`[${i}]: ${array[i]}`);
  if (i === array.length - 1) console.log('Last element');
}
```

## Debugging Tools

### Console Debugging

```typescript
console.table(users, ['id', 'name', 'email']);
console.time('Processing');
// ... code
console.timeEnd('Processing');
console.trace('Called from');
```

### Node.js Debugging

```bash
node --inspect server.js
# Open chrome://inspect
```

## Debugging Checklist

Before starting:
- [ ] Can I reproduce the issue?
- [ ] What is the error message?
- [ ] When does it happen?
- [ ] What changed recently?

After fix:
- [ ] Fix resolves the issue
- [ ] Tests pass
- [ ] New test written for bug
- [ ] No side effects
- [ ] Fix documented

See `debugging-checklist.md` for comprehensive checklist.
See `debugging-patterns.md` for more patterns.

## Related Skills

- Use **testing** skill to write test that reproduces bug
- Use **refactoring** skill to clean up after fix
- Use **review** skill for code review of fix