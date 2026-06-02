# Security Guidelines

## Secrets Management

**NEVER commit secrets to the repository.**

### Allowed Secret Locations

- Environment variables (`.env` files in `.gitignore`)
- Secret managers (Vault, AWS Secrets Manager, etc.)
- CI/CD secret injection
- Local secure storage (keychain, etc.)

### Forbidden Secret Locations

- Source code
- Configuration files committed to repo
- Comments in code
- Documentation files
- Test files (use mocks/stubs instead)

## Environment Variables

Use `.env.example` to document required variables:

```bash
# .env.example (committed to repo)
DATABASE_URL=postgresql://user:password@host:port/db
API_KEY=your-api-key-here
SECRET_KEY=your-secret-key-here
```

```bash
# .env (in .gitignore, NEVER commit)
DATABASE_URL=postgresql://actual:credentials@localhost:5432/mydb
API_KEY=sk_live_actualKey123
SECRET_KEY=superSecretKey456
```

## Dependency Security

### Regular Updates

- Review and update dependencies regularly
- Use `npm audit` / `yarn audit` / `pip audit` regularly
- Fix critical/high vulnerabilities immediately
- Monitor dependency security advisories

### Dependency Review

Before adding new dependencies:
- Check for known vulnerabilities
- Verify active maintenance
- Review license compatibility
- Assess bundle size impact (for frontend)

## Code Security

### Input Validation

Always validate and sanitize:
- User input in forms
- API parameters
- URL parameters
- File uploads
- Environment variables

```{{LANGUAGE}}
// {{VALIDATION_EXAMPLE}}
```

### SQL Injection Prevention

- Use parameterized queries
- Never concatenate user input into SQL
- Use ORM/query builder where possible

```{{LANGUAGE}}
// {{SQL_SAFETY_EXAMPLE}}
```

### XSS Prevention

- Escape output based on context
- Use CSP (Content Security Policy)
- Sanitize HTML input
- Use framework-provided escaping

### Authentication & Authorization

- Use established auth libraries
- Implement proper session management
- Enforce strong password policies
- Use MFA where appropriate
- Log authentication events

## API Security

### Rate Limiting

- Implement rate limiting on all endpoints
- Use sliding window algorithm
- Return proper 429 responses
- Document rate limits

### HTTPS Everywhere

- Enforce HTTPS in production
- Use HSTS headers
- Never transmit secrets over HTTP
- Proper certificate management

### CORS Configuration

```{{LANGUAGE}}
// {{CORS_EXAMPLE}}
```

- Whitelist specific origins
- Never use `*` in production
- Limit allowed methods
- Set proper max-age

## File Security

### File Uploads

- Validate file type (magic bytes, not just extension)
- Limit file size
- Scan for malware
- Store outside web root
- Never execute uploaded files

### Path Traversal

- Validate and sanitize file paths
- Use `basename()` or equivalent
- Never use user input directly in paths
- Implement proper permissions

## Logging & Monitoring

### What to Log

- Authentication events (success/failure)
- Authorization failures
- Input validation errors
- Rate limit breaches
- Unexpected errors

### What NOT to Log

- Passwords
- API keys
- Access tokens
- Personal identifiable information (PII)
- Credit card numbers
- Any sensitive user data

### Log Security

- Use structured logging
- Implement log rotation
- Secure log storage
- Monitor for anomalies

## Security Headers

```{{LANGUAGE}}
// {{SECURITY_HEADERS_EXAMPLE}}
```

Set headers:
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `X-XSS-Protection: 1; mode=block`
- `Content-Security-Policy: default-src 'self'`
- `Strict-Transport-Security: max-age=31536000`

## Third-Party Integrations

### API Keys

- Use environment variables
- Rotate keys regularly
- Use least-privilege keys
- Monitor usage
- Document required scopes

### Webhooks

- Verify signatures
- Use HTTPS endpoints
- Implement retry logic
- Log all webhook events

## CI/CD Security

### Secrets in CI

- Use CI/CD secret management
- Never echo secrets in logs
- Mask sensitive output
- Rotate CI secrets regularly

### Build Security

- Pin dependency versions
- Verify checksums
- Scan built artifacts
- Sign releases

## Incident Response

### If You Find a Vulnerability

1. **Do NOT commit fixes to public repo initially**
2. Report privately to maintainers
3. Provide detailed reproduction steps
4. Allow time for fix before disclosure

### Security Advisory Process

{{SECURITY_ADVISORY_PROCESS}}

## Security Checklist

Before each release:

- [ ] All secrets are in environment variables
- [ ] `.env` is in `.gitignore`
- [ ] Dependencies are up-to-date and audited
- [ ] Input validation in place
- [ ] Security headers configured
- [ ] HTTPS enforced
- [ ] Rate limiting enabled
- [ ] Authentication/authorization tested
- [ ] Logs don't contain sensitive data
- [ ] File upload security implemented

## Additional Resources

{{SECURITY_RESOURCES}}