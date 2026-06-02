# Security Documentation

## Overview

{{SECURITY_OVERVIEW}}

This document outlines security practices, vulnerabilities, and mitigation strategies for {{PROJECT_NAME}}.

**Security Contact:** {{SECURITY_CONTACT_EMAIL}}

**Last Security Audit:** {{LAST_AUDIT_DATE}}

## Security Principles

1. **Defense in Depth** - Multiple layers of security
2. **Least Privilege** - Minimum necessary permissions
3. **Fail Securely** - Deny by default
4. **Keep It Simple** - Complexity introduces vulnerabilities
5. **Security by Design** - Build security in from the start
6. **Transparency** - No security through obscurity

## Threat Model

### Assets

**Critical Assets:**
- User credentials (passwords, tokens)
- Personal identifiable information (PII)
- Financial data
- API keys and secrets
- Business logic

**Secondary Assets:**
- Application logs
- Metadata
- Configuration files

### Attack Surface

**External Facing:**
- Public API endpoints
- Web application (frontend)
- Database connections
- Third-party integrations
- File uploads

**Internal Facing:**
- Admin panel
- Internal APIs
- Background jobs
- CI/CD pipeline
- Developer tools

### Potential Attackers

- **External Attackers:** Script kiddies, hacktivists, organized crime
- **Internal Threats:** Disgruntled employees, compromised accounts
- **Insider Threats:** Malicious insiders
- **Advanced Persistent Threats (APTs):** State-sponsored actors

## Authentication

### Authentication Method

**Method:** {{AUTH_METHOD}} (OAuth2 / JWT / Session / API Key)

### Password Security

**Password Requirements:**
- Minimum 12 characters
- At least 1 uppercase, 1 lowercase, 1 number, 1 symbol
- Not in common password lists
- Different from previous passwords

**Password Storage:**
```{{LANGUAGE}}
// Use strong hashing (bcrypt, argon2)
const hashedPassword = await bcrypt.hash(password, 12);

// NEVER store plain text passwords
```

### Session Management

**Session Configuration:**
```{{LANGUAGE}}
{
  cookie: {
    httpOnly: true,        // Prevent XSS access
    secure: true,          // HTTPS only
    sameSite: 'strict',     // CSRF protection
    maxAge: 3600000        // 1 hour
  },
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false
}
```

### JWT Security

**JWT Best Practices:**
```{{LANGUAGE}}
// Use short expiration
const token = jwt.sign(payload, secret, {
  expiresIn: '15m',        // Access token: 15 minutes
  algorithm: 'HS256'       // Use strong algorithm
});

// Implement refresh tokens
const refreshToken = jwt.sign(payload, refreshSecret, {
  expiresIn: '7d'         // Refresh token: 7 days
});

// Verify tokens properly
try {
  const decoded = jwt.verify(token, secret, {
    algorithms: ['HS256']  // Specify allowed algorithms
  });
} catch (error) {
  // Handle invalid/expired tokens
}
```

### OAuth2 Security

- Use PKCE for public clients
- Validate state parameter
- Store tokens securely
- Implement token rotation

## Authorization

### Role-Based Access Control (RBAC)

**Roles:**
- `admin` - Full access
- `user` - Standard access
- `guest` - Limited access

**Permissions:**
```{{LANGUAGE}}
const permissions = {
  admin: {
    users: ['create', 'read', 'update', 'delete'],
    posts: ['create', 'read', 'update', 'delete'],
    settings: ['read', 'update']
  },
  user: {
    users: ['read:self', 'update:self'],
    posts: ['create', 'read', 'update:self', 'delete:self'],
    settings: []
  },
  guest: {
    users: [],
    posts: ['read'],
    settings: []
  }
};
```

### Authorization Checks

```{{LANGUAGE}}
// Always authenticate first, then authorize
function requireAuth(handler) {
  return async (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Unauthorized' });
    }
    next();
  };
}

function requireRole(role) {
  return async (req, res, next) => {
    if (!req.user.roles.includes(role)) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    next();
  };
}

// Usage
app.delete('/api/users/:id',
  requireAuth,
  requireRole('admin'),
  deleteUser
);
```

### Resource Ownership

```{{LANGUAGE}}
// Check resource ownership
async function deletePost(req, res) {
  const post = await Post.findById(req.params.id);

  // Verify ownership
  if (post.userId !== req.user.id && !req.user.roles.includes('admin')) {
    return res.status(403).json({ error: 'Forbidden' });
  }

  await post.delete();
  res.json({ success: true });
}
```

## Input Validation

### Validation Principles

1. **Whitelist allowed values** (not blacklist)
2. **Validate on server side** (client validation is not enough)
3. **Sanitize all inputs**
4. **Use parameterized queries**
5. **Validate before processing**

### Input Validation

```{{LANGUAGE}}
import { z } from 'zod';

// Define schema
const UserSchema = z.object({
  email: z.string().email().max(255),
  name: z.string().min(1).max(100),
  age: z.number().int().min(0).max(150).optional(),
  role: z.enum(['user', 'admin']).default('user')
});

// Validate
function validateUser(data) {
  const result = UserSchema.safeParse(data);

  if (!result.success) {
    throw new ValidationError(result.error);
  }

  return result.data;
}
```

### SQL Injection Prevention

**Always use parameterized queries:**

```{{LANGUAGE}}
// BAD: SQL injection vulnerable
const query = `SELECT * FROM users WHERE email = '${email}'`;
await db.query(query);

// GOOD: Parameterized query
const query = 'SELECT * FROM users WHERE email = ?';
await db.query(query, [email]);

// GOOD: ORM
const user = await User.findByEmail(email);
```

### XSS Prevention

```{{LANGUAGE}}
// Escape output based on context
import { escape } from 'html-escaper';

// HTML context
const safeHTML = escape(userInput);

// URL context
const safeURL = encodeURIComponent(userInput);

// JavaScript context
const safeJS = JSON.stringify(userInput);

// CSS context
const safeCSS = /* use proper escaping for CSS */;
```

### Content Security Policy

```{{LANGUAGE}}
// Set CSP headers
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy',
    "default-src 'self'; " +
    "script-src 'self' 'unsafe-inline' https://cdn.example.com; " +
    "style-src 'self' 'unsafe-inline'; " +
    "img-src 'self' data: https:; " +
    "font-src 'self' https://fonts.example.com; " +
    "connect-src 'self' https://api.example.com; " +
    "frame-ancestors 'none';"
  );
  next();
});
```

## Data Protection

### Data Classification

**Public:** No restrictions
**Internal:** Employees only
**Confidential:** Authorized personnel only
**Restricted:** Highest security, encryption required

### Encryption at Rest

**Implementation:**
```{{LANGUAGE}}
// Encrypt sensitive data before storage
import { encrypt, decrypt } from './crypto';

async function saveUser(user) {
  // Encrypt PII
  user.ssn = await encrypt(user.ssn);
  user.creditCard = await encrypt(user.creditCard);

  await db.users.create(user);
}

async function getUser(id) {
  const user = await db.users.findById(id);

  // Decrypt when retrieving
  return {
    ...user,
    ssn: await decrypt(user.ssn),
    creditCard: await decrypt(user.creditCard)
  };
}
```

### Encryption in Transit

- Use TLS 1.2+ for all connections
- Enforce HTTPS
- Use HSTS headers
- Verify certificates

### Data Masking

**Log Masking:**
```{{LANGUAGE}}
// Mask sensitive data in logs
function maskEmail(email: string): string {
  const [local, domain] = email.split('@');
  return `${local[0]}***@${domain}`;
}

function maskPhone(phone: string): string {
  return phone.replace(/(\d{3})\d{4}(\d{4})/, '$1****$2');
}

logger.info('User created', {
  email: maskEmail(user.email),
  phone: maskPhone(user.phone),
  password: '[REDACTED]'
});
```

### Data Retention

| Data Type | Retention Period | Auto-delete |
|-----------|-----------------|-------------|
| User accounts | Until deletion request | Yes, after 30 days |
| Activity logs | 1 year | Yes |
| Audit logs | 7 years | No |
| Session data | 24 hours | Yes |
| Temporary files | 1 hour | Yes |

## API Security

### Rate Limiting

```{{LANGUAGE}}
import rateLimit from 'express-rate-limit';

// General rate limit
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,    // 15 minutes
  max: 100,                     // 100 requests per window
  message: 'Too many requests, please try again later.',
  headers: true
});

// Strict rate limit for auth
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,                       // 5 attempts per window
  message: 'Too many login attempts.'
});

app.use('/api/', limiter);
app.use('/api/auth/', authLimiter);
```

### API Keys

**Never expose API keys in client-side code.**

```{{LANGUAGE}}
// Server-side only
const API_KEY = process.env.API_KEY;

// Request with API key
fetch('/api/data', {
  headers: {
    'Authorization': `Bearer ${API_KEY}`
  }
});
```

### CORS Configuration

```{{LANGUAGE}}
import cors from 'cors';

const corsOptions = {
  origin: process.env.ALLOWED_ORIGINS.split(','),
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true,
  maxAge: 86400  // 24 hours
};

app.use(cors(corsOptions));
```

### Security Headers

```{{LANGUAGE}}
import helmet from 'helmet';

app.use(helmet());

// Or configure individual headers
app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'"],
    styleSrc: ["'self'"]
  }
}));

app.use(helmet.xssFilter());
app.use(helmet.noSniff());
app.use(helmet.frameguard({ action: 'deny' }));
```

## Dependency Security

### Vulnerability Scanning

```bash
# Node.js
npm audit
npm audit fix

# Python
pip-audit
safety check

# Continuous scanning
snyk test
```

### Dependency Updates

```bash
# Check for outdated
npm outdated

# Update dependencies
npm update

# Update major versions
npm install package@latest
```

### Security Policy

**Dependency Acceptance Criteria:**
- No critical/high vulnerabilities
- Active maintenance (commits in last 12 months)
- Compatible license
- Reasonable bundle size
- Sufficient community/popularity

## Logging & Monitoring

### What to Log

**Log These:**
- Authentication events (success/failure)
- Authorization failures
- Input validation errors
- Rate limit breaches
- Unusual activity
- Error events
- Configuration changes

**Never Log These:**
- Passwords (even hashed)
- API keys
- Session tokens
- Credit card numbers
- SSN
- Personal health information

### Log Format

```json
{
  "timestamp": "2024-01-01T00:00:00Z",
  "level": "info",
  "event": "user.login",
  "userId": "123",
  "ip": "192.168.1.1",
  "userAgent": "Mozilla/5.0...",
  "success": true,
  "requestId": "abc-123"
}
```

### Monitoring Alerts

**Critical Alerts:**
- Multiple failed login attempts from same IP
- Unusual traffic patterns
- Error rate spike
- Unauthorized access attempts
- Database connection failures

### Incident Response

**Severity Levels:**

| Level | Description | Response Time |
|-------|-------------|----------------|
| P1 - Critical | Active breach, data loss | Immediate |
| P2 - High | Vulnerability exploited | Within 2 hours |
| P3 - Medium | Vulnerability discovered | Within 24 hours |
| P4 - Low | Security improvement | Within 1 week |

**Response Procedure:**
1. Acknowledge alert
2. Assess severity
3. Contain incident
4. Investigate root cause
5. Remediate
6. Postmortem

## Third-Party Security

### External APIs

- Use API keys securely (server-side only)
- Validate all responses
- Implement circuit breakers
- Monitor for changes
- Have fallback mechanisms

### Webhooks

```{{LANGUAGE}}
// Verify webhook signature
function verifyWebhook(payload: string, signature: string): boolean {
  const expected = crypto
    .createHmac('sha256', WEBHOOK_SECRET)
    .update(payload)
    .digest('hex');

  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expected)
  );
}

app.post('/webhook', (req, res) => {
  const signature = req.headers['x-webhook-signature'];

  if (!verifyWebhook(req.body, signature)) {
    return res.status(401).send('Invalid signature');
  }

  // Process webhook
});
```

## Security Checklist

### Development

- [ ] No secrets in code
- [ ] Input validation on all endpoints
- [ ] Parameterized queries for all database access
- [ ] Output encoding for all user data
- [ ] Authentication on all sensitive endpoints
- [ ] Authorization checks on all resources
- [ ] CSRF protection enabled
- [ ] CORS properly configured
- [ ] Security headers set
- [ ] Error messages don't leak information

### Testing

- [ ] Security tests in test suite
- [ ] Penetration testing performed
- [ ] Dependency vulnerability scan
- [ ] OWASP Top 10 tested
- [ ] Authentication flow tested
- [ ] Authorization tested
- [ ] Input validation tested
- [ ] Rate limiting tested

### Deployment

- [ ] HTTPS enforced
- [ ] Secrets in secrets manager
- [ ] Database access restricted
- [ ] Firewall configured
- [ ] Rate limiting enabled
- [ ] Logging configured
- [ ] Monitoring set up
- [ ] Backup encryption enabled
- [ ] Security patches applied

### Monitoring

- [ ] Failed login alerting
- [ ] Error rate monitoring
- [ ] Traffic anomaly detection
- [ ] Dependency monitoring
- [ ] Certificate expiration monitoring
- [ ] Log aggregation

## Vulnerability Disclosure

### Reporting Vulnerabilities

**Responsible Disclosure Process:**

1. **Report:** Email {{SECURITY_EMAIL}} with:
   - Vulnerability description
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

2. **Acknowledgment:** We'll respond within 48 hours

3. **Investigation:** We'll validate and assess

4. **Fix:** We'll develop and test a fix

5. **Disclosure:** Coordinated public disclosure after fix deployed

**What We Promise:**
- No legal action for good-faith reports
- Credit in security advisories (if desired)
- Timely response and fixes

### Security Advisories

All security advisories published at: `{{SECURITY_ADVISORIES_URL}}`

## Security Training

### Required Training for All Developers

- OWASP Top 10
- Secure coding practices
- Security awareness
- Incident response procedures

### Additional Training for Security Team

- Advanced penetration testing
- Forensics
- Threat modeling
- Security architecture

## Compliance

### Relevant Standards

- **GDPR** - European data protection
- **SOC 2** - Service organization controls
- **PCI DSS** - Payment card industry
- **HIPAA** - Healthcare information
- **ISO 27001** - Information security

### Compliance Checklist

**GDPR:**
- [ ] Data processing agreement
- [ ] Privacy policy
- [ ] Data subject rights
- [ ] Data breach notification process
- [ ] Data Protection Officer

**SOC 2:**
- [ ] Security policies
- [ ] Access controls
- [ ] Encryption
- [ ] Monitoring
- [ ] Incident response

## Security Contacts

**Security Team:** {{SECURITY_EMAIL}}

**Security Lead:** {{SECURITY_LEAD}}

**Bug Bounty Program:** {{BUG_BOUNTY_URL}}

## Related Documentation

- [ARCHITECTURE.md](./ARCHITECTURE.md) - System architecture
- [DEPLOYMENT.md](./DEPLOYMENT.md) - Deployment security
- [API.md](./API.md) - API security considerations
- [DATABASE.md](./DATABASE.md) - Database security

## Last Updated

**Date:** {{LAST_UPDATED}}

**Next Review:** {{NEXT_REVIEW_DATE}}