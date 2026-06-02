# Security Rules for Claude Code

## Critical: Secrets Management

**NEVER commit secrets to version control.**

### Allowed Secret Locations
- Environment variables (`.env` files in `.gitignore`)
- Secret managers (Vault, AWS Secrets Manager, Azure Key Vault)
- CI/CD secret injection
- Local secure storage

### Forbidden Secret Locations
- Source code (even commented out)
- Configuration files committed to repo
- Test files
- Documentation
- Issues/PRs

## Environment Variables

```bash
# .env.example (commit this)
DATABASE_URL=postgresql://user:password@host:port/db
API_KEY=your-api-key-here
SECRET_KEY=your-secret-key-here

# .env (in .gitignore, NEVER commit)
DATABASE_URL=postgresql://real:creds@localhost:5432/mydb
API_KEY=sk_live_actualKey123
SECRET_KEY=superSecretKey456
```

## Input Validation Rules

Always validate/sanitize:
- User input from forms
- API request parameters
- URL parameters
- File uploads (name, type, size)
- Environment variables

```{{LANGUAGE}}
// {{VALIDATION_PATTERN}}
```

## SQL Injection Prevention

**Never concatenate user input into SQL queries.**

```{{LANGUAGE}}
// BAD - SQL injection vulnerable
const query = "SELECT * FROM users WHERE id = " + userId;

// GOOD - Use parameterized queries
const query = "SELECT * FROM users WHERE id = ?";
db.query(query, [userId]);
```

## XSS Prevention

- Escape output based on context (HTML, JS, CSS, URL)
- Use CSP headers
- Sanitize HTML input
- Use framework-provided escaping

## Authentication & Authorization

### Must Have
- Use established authentication libraries
- Implement proper session management
- Enforce strong password policies
- Log authentication events (success/failure)
- Implement rate limiting on auth endpoints

### Never Do
- Roll your own crypto
- Store passwords in plain text
- Send passwords via email
- Use weak hashing algorithms
- Hardcode credentials

## API Security

### Rate Limiting

- Apply to all endpoints
- Use sliding window algorithm
- Return proper `429 Too Many Requests`
- Document rate limits in API docs

### CORS Configuration

```{{LANGUAGE}}
// Whitelist specific origins
// Never use '*' in production
const corsOptions = {
  origin: ['https://example.com'],
  methods: ['GET', 'POST'],
  credentials: true
};
```

### HTTPS Enforcement

- Enforce HTTPS in production
- Use HSTS headers
- Never transmit secrets over HTTP
- Proper certificate management

## File Upload Security

### Validation Checklist

- [ ] Validate file type via magic bytes, not extension
- [ ] Limit file size
- [ ] Scan for malware
- [ ] Store outside web root
- [ ] Generate random filenames
- [ ] Never execute uploaded files
- [ ] Implement proper permissions

### Path Traversal Prevention

```{{LANGUAGE}}
// BAD - path traversal vulnerable
const filePath = '/uploads/' + userInput;

// GOOD - sanitize and validate
const fileName = path.basename(userInput);
const filePath = path.join('/uploads', fileName);
```

## Logging Rules

### Log These
- Authentication events (success/failure)
- Authorization failures
- Input validation errors
- Rate limit breaches
- Unexpected errors
- Business-critical operations

### Never Log These
- Passwords (even hashed)
- API keys
- Access tokens
- Credit card numbers
- Personal identifiable information (PII)
- Session tokens

## Dependency Security

### Regular Tasks
- Update dependencies regularly
- Run vulnerability scans (`npm audit`, `pip audit`)
- Review security advisories
- Check license compatibility
- Assess bundle size impact

### Before Adding Dependencies
- Check for known vulnerabilities
- Verify active maintenance
- Review popularity and community
- Check license compatibility
- Assess size/performance impact

## Security Headers

Always set these headers:

```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Content-Security-Policy: default-src 'self'
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

## CI/CD Security

### Secrets in CI
- Use CI/CD platform secret management
- Never echo secrets in logs
- Mask sensitive output
- Rotate CI secrets regularly

### Build Security
- Pin dependency versions
- Verify checksums
- Scan built artifacts
- Sign releases

## Pre-commit Checklist

Never commit files containing:
- [ ] API keys or tokens
- [ ] Passwords
- [ ] Private keys
- [ ] `.env` files
- [ ] Database credentials
- [ ] OAuth secrets
- [ ] Certificate files

## Incident Response

### If You Find a Vulnerability

1. **Do NOT commit fixes to public repo initially**
2. Report privately to maintainers
3. Provide detailed reproduction steps
4. Allow time for fix before disclosure

### Security Contact

{{SECURITY_CONTACT}}

## Additional Resources

{{SECURITY_RESOURCES}}