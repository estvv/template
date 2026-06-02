# Debugging Checklist

Systematic checklist for finding and fixing bugs.

## Initial Investigation

### Reproduce the Bug
- [ ] Can reproduce consistently
- [ ] Know exact steps to reproduce
- [ ] Verified in production-like environment
- [ ] Tested with same data/user
- [ ] Tested with same timing conditions

### Gather Information
- [ ] Captured error message
- [ ] Captured stack trace
- [ ] Captured logs before/during/after
- [ ] Know when it happens (always/sometimes/specific conditions)
- [ ] Know what changed recently (code/config/data)
- [ ] Checked if related issues exist

### Document Findings
- [ ] Created issue/ticket
- [ ] Documented reproduction steps
- [ ] Documented error details
- [ ] Added screenshots/screen recordings if helpful

## Form Hypothesis

### Understand the System
- [ ] Understand what the code should do
- [ ] Understand what it's actually doing
- [ ] Identified where the problem could be
- [ ] Listed possible causes

### Create Hypothesis
- [ ] Formulated specific hypothesis
- [ ] Identified what would prove it true
- [ ] Identified what would prove it false
- [ ] Prioritized hypotheses by likelihood

## Test Hypothesis

### Isolate the Problem
- [ ] Narrowed down location (frontend/backend/database)
- [ ] Narrowed down to specific file/module
- [ ] Narrowed down to specific function/line
- [ ] Created minimal reproduction

### Use Debugging Tools
- [ ] Added logging strategically
- [ ] Used debugger/breakpoints
- [ ] Checked logs (application/database/server)
- [ ] Used network inspector (for API calls)
- [ ] Used memory profiler (for memory issues)
- [ ] Used CPU profiler (for performance issues)

### Verify Hypothesis
- [ ] Tested hypothesis with targeted change
- [ ] Did hypothesis prove true/false?
- [ ] If false, formed new hypothesis
- [ ] If true, identified root cause

## Implement Fix

### Write Test
- [ ] Wrote test that reproduces bug
- [ ] Test fails without fix
- [ ] Test passes with fix
- [ ] Test covers edge cases
- [ ] Test added to test suite

### Implement Fix
- [ ] Minimal change to fix bug
- [ ] No unrelated changes
- [ ] Code is clean and maintainable
- [ ] No hidden side effects
- [ ] Handles edge cases

### Code Review
- [ ] Self-reviewed fix
- [ ] Got code review from teammate
- [ ] Addressed all review comments
- [ ] All tests pass (including new test)

## Verify Fix

### Test Locally
- [ ] Ran test suite - all pass
- [ ] Tested fix manually - bug fixed
- [ ] Tested edge cases
- [ ] Tested related functionality
- [ ] No other behavior changed

### Test in Staging
- [ ] Deployed to staging
- [ ] Tested in staging environment
- [ ] Verified with production-like data
- [ ] Verified with production-like config
- [ ] No other functionality broken

### Test in Production
- [ ] Deployed to production (or will deploy)
- [ ] Monitored for errors
- [ ] Verified fix in production
- [ ] No new errors introduced
- [ ] User confirmed fix works

## Document Prevention

### Document
- [ ] Updated commit message with bug details
- [ ] Updated ticket/issue with resolution
- [ ] Explained root cause
- [ ] Explained fix
- [ ] Listed affected components

### Prevent Recurrence
- [ ] Added test for bug
- [ ] Added validation if input error
- [ ] Added assertion if logic error
- [ ] Added logging for better debugging
- [ ] Added/updated documentation
- [ ] Considered if similar bugs exist elsewhere

## Common Bug Patterns Checklist

### Null/Undefined Issues
- [ ] Added null checks
- [ ] Used optional chaining
- [ ] Provided default values
- [ ] Handled undefined in array functions

### Type Errors
- [ ] Added type validation
- [ ] Used TypeScript properly
- [ ] Checked runtime types

### Async/Await Issues
- [ ] Proper error handling (try/catch)
- [ ] Awaited all promises
- [ ] Handled promise rejections
- [ ] No race conditions

### Logic Errors
- [ ] Off-by-one errors fixed
- [ ] Conditionals correct (&& vs ||)
- [ ] Comparison correct (=== vs ==)
- [ ] Loop conditions correct

### State Issues
- [ ] No unintended mutations
- [ ] State properly initialized
- [ ] State properly reset
- [ ] No stale closure issues

### Performance Issues
- [ ] Identified bottleneck
- [ ] Optimized algorithm
- [ ] Added caching if needed
- [ ] Reduced unnecessary work

### Security Issues
- [ ] Input validated
- [ ] Output encoded
- [ ] SQL injection prevented
- [ ] XSS prevented
- [ ] CSRF token validated

## Debugging Questions to Ask

### What?
- What is the symptom?
- What is the error message?
- What is the expected behavior?
- What is the actual behavior?

### When?
- When does it happen? (always/sometimes)
- When did it start happening?
- When doesn't it happen?

### Where?
- Where in the code does the error occur?
- Where in the system (frontend/backend/db)?
- Where in the process flow?

### Who?
- Who is affected? (all users/some users)
- Who can reproduce it?
- Who knows about this area?

### Why?
- Why is this happening?
- Why did it work before?
- Why is it failing now?

## After Fix

### Reflect
- [ ] Understood root cause
- [ ] Learned something new
- [ ] Improved debugging skills
- [ ] Documented for future reference

### Share
- [ ] Shared findings with team
- [ ] Updated documentation
- [ ] Added to knowledge base
- [ ] Prevented similar bugs elsewhere