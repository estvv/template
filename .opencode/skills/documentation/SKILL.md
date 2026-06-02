---
name: documentation
description: Guidelines for writing clear, comprehensive, and maintainable documentation. Use when creating README files, API docs, architecture docs, user guides, or any technical documentation.
license: MIT
---

# Documentation Best Practices

Guidelines for writing high-quality technical documentation that is clear, comprehensive, and maintainable.

## Core Principles

1. **Clarity Over Cleverness** - Write to be understood, not to impress
2. **Audience-First** - Write for your readers, not yourself
3. **Living Documents** - Keep docs updated with code changes
4. **Show, Don't Just Tell** - Use examples and code snippets
5. **Structure for Discovery** - Make information easy to find

## Documentation Types

### README.md

**Purpose:** Quick introduction and getting started guide

**Essential Sections:**
- Project name and description
- Quick start / Installation
- Basic usage
- License
- Contributing link

**Template:**
```markdown
# Project Name

Brief description of what the project does.

## Quick Start

\`\`\`bash
npm install package-name
\`\`\`

\`\`\`javascript
const package = require('package-name');
package.doSomething();
\`\`\`

## Features

- Feature 1
- Feature 2

## Installation

\`\`\`bash
npm install package-name
\`\`\`

## Usage

Basic usage example.

## Documentation

- [API Documentation](./docs/API.md)
- [Architecture](./docs/ARCHITECTURE.md)

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md)

## License

MIT
```

### API Documentation

**Purpose:** Document all endpoints, request/response formats

**Essential Sections:**
- Authentication
- Base URL
- Request/Response format
- Endpoints (for each: method, path, parameters, body, response, errors)
- Rate limiting
- Webhooks (if applicable)
- Examples in multiple languages

**Best Practices:**
- Include request/response examples
- Document all error codes
- Show realistic data (not "foo", "bar")
- Keep language-agnostic
- Provide SDK examples

### Architecture Documentation

**Purpose:** Explain system design and decisions

**Essential Sections:**
- High-level overview
- Component diagram
- Data flow
- Key decisions and rationale
- Trade-offs
- Future considerations

### ADR (Architecture Decision Record)

**Purpose:** Document why architectural decisions were made

**Structure:**
1. Title
2. Status
3. Context
4. Decision
5. Consequences
6. Alternatives Considered

### User Guide

**Purpose:** Help users accomplish specific tasks

**Structure:**
- Getting started
- Common tasks
- Advanced usage
- Troubleshooting
- FAQ

## Writing Guidelines

### Use Active Voice

**Bad:** The configuration file should be edited by you.
**Good:** Edit the configuration file.

**Bad:** Users can create accounts.
**Good:** Create an account.

### Be Concise

**Bad:** In order to install the package, you will need to run the installation command.
**Good:** Install the package:
```bash
npm install package-name
```

### Front-Load Important Information

**Bad:** There are many configuration options available. Before we discuss those, let's talk about installation.
**Good:** Install the package in 30 seconds:
```bash
npm install package-name
```
Then configure it with these essential options...

### Use Consistent Terminology

- Pick one term and use it consistently
- Define terms on first use
- Create a glossary for complex domains

### Show Examples Early

**Bad:** The function accepts three parameters: name (string), age (number), and email (string). Here's how to use it...
**Good:** 
```javascript
createUser('John Doe', 30, 'john@example.com');
```

Parameters:
- `name` (string): User's full name
- `age` (number): User's age
- `email` (string): User's email

### Write for Scanning

Use:
- Headings and subheadings
- Bullet lists
- Code blocks
- Tables
- Bold for key terms
- Short paragraphs (3-4 sentences)

### Avoid Assumptions

**Bad:** Obviously, you need Node.js installed.
**Good:** Prerequisites: Node.js 18+ is required. Install it from [nodejs.org](https://nodejs.org/).

## Documentation Structure

### Directory Layout

```
docs/
├── ARCHITECTURE.md      # System architecture
├── API.md               # API documentation
├── DATABASE.md          # Database schema
├── DEPLOYMENT.md        # Deployment guide
├── DEVELOPMENT.md       # Development setup
├── TESTING.md           # Testing guide
├── SECURITY.md          # Security practices
└── adr/                 # Architecture Decision Records
    ├── README.md
    ├── template.md
    └── 0001-decision.md
```

### Cross-Linking

Link related documentation:
```markdown
See [API Documentation](./API.md) for endpoint details.
See [Architecture](./ARCHITECTURE.md) for system design.
```

## Placeholders & Templates

### Use Clear Placeholders

**Good:**
```markdown
## Installation

\`\`\`bash
npm install {{PACKAGE_NAME}}
\`\`\`

## Configuration

\`\`\`javascript
{
  "apiKey": "{{YOUR_API_KEY}}",
  "environment": "{{ENVIRONMENT}}"
}
\`\`\`
```

**Bad:**
```markdown
Install the thing and configure it.
```

### Provide Examples

**Good:**
```markdown
## Environment Variables

Required:
- `DATABASE_URL` - PostgreSQL connection string
  Example: `postgresql://user:password@localhost:5432/mydb`

- `API_KEY` - Your API key from the dashboard
  Example: `sk_live_abc123def456`
```

## Code Examples

### Make Examples Complete

**Bad:**
```javascript
// Configure something
config.set({ key: value });
```

**Good:**
```javascript
const config = require('config-package');

// Configure the client
config.set({
  apiKey: process.env.API_KEY,
  environment: 'production',
  timeout: 5000
});

// Verify configuration
console.log(config.get('apiKey')); // → 'sk_live_...'
```

### Show Real Data

**Bad:**
```json
{
  "name": "foo",
  "email": "bar",
  "id": 123
}
```

**Good:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "id": "usr_abc123def456"
}
```

### Include Error Handling

**Bad:**
```javascript
const user = await getUser(id);
```

**Good:**
```javascript
try {
  const user = await getUser(id);
  console.log(user.name);
} catch (error) {
  if (error.code === 'NOT_FOUND') {
    console.error('User not found');
  } else {
    console.error('Error fetching user:', error.message);
  }
}
```

## Tables

Use tables for structured data:

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `name`    | string | Yes      | User's name |
| `email`   | string | Yes      | User's email |
| `age`     | number | No       | User's age |

## Versioning Documentation

### Document Version Requirements

```markdown
## Requirements

- Node.js >= 18.0.0
- PostgreSQL >= 14.0

Support for Node.js 16 was deprecated in v2.0.0.
```

### Indicate Version Differences

```markdown
## Installation

### v3.0.0+

\`\`\`bash
npm install package@^3.0.0
\`\`\`

### v2.x (Legacy)

\`\`\`bash
npm install package@^2.0.0
\`\`\`
```

## Error Documentation

### Document All Errors

```markdown
## Errors

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `VALIDATION_ERROR` | 400 | Invalid request data |
| `UNAUTHORIZED` | 401 | Authentication required |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `RATE_LIMIT` | 429 | Too many requests |

### Validation Error Example

\`\`\`json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": {
      "field": "email",
      "value": "invalid-email"
    }
  }
}
\`\`\`
```

## Images & Diagrams

### When to Use Diagrams

- Architecture overview
- Data flow
- Component relationships
- Process flows
- State machines

### Diagram Guidelines

- Use mermaid or ASCII diagrams for text-based docs
- Keep diagrams simple (max 10 elements)
- Provide text alternatives
- Update diagrams with code changes

**Mermaid Example:**
```markdown
\`\`\`mermaid
graph TD
    A[Client] --> B[API Gateway]
    B --> C[Service]
    C --> D[Database]
\`\`\`
```

**ASCII Example:**
```markdown
\`\`\`
┌─────────┐     ┌─────────┐
│ Client  │────>│  API    │
└─────────┘     └─────────┘
                      │
                      ↓
                ┌─────────┐
                │Database │
                └─────────┘
\`\`\`
```

## Maintainability

### Update Docs with Code

**Rule:** Every PR must include doc updates if behavior changes.

**Checklist:**
- [ ] Update README if installation/usage changed
- [ ] Update API docs if endpoints changed
- [ ] Update CHANGELOG
- [ ] Update code examples

### Review Documentation

**Self-Review Questions:**
- Will a newcomer understand this?
- Are examples complete and correct?
- Is everything up-to-date?
- Are placeholders replaced?
- Are links working?

### Automated Checks

```markdown
<!-- markdown-link-check -->
- [ ] All links valid
- [ ] Code examples compile
- [ ] API examples tested
```

## Accessibility

### Use Clear Language

- Aim for 8th-grade reading level
- Define acronyms on first use
- Avoid jargon
- Provide definitions for technical terms

### Structure for Screen Readers

- Use proper heading hierarchy (h1 → h2 → h3)
- Don't skip heading levels
- Use descriptive link text ("Read the architecture guide" not "Click here")

## Localization Considerations

### For Future Translation

- Avoid idioms and culture-specific references
- Use clear, simple English
- Provide context for ambiguous terms
- Use standard date/time formats (ISO 8601)
- Avoid hard-coded strings in examples

## Checklist

Before publishing documentation:

- [ ] All placeholders replaced
- [ ] All examples tested
- [ ] Links verified
- [ ] Spelling and grammar checked
- [ ] Headings properly nested
- [ ] Code blocks have language tags
- [ ] Error cases documented
- [ ] Prerequisites listed
- [ ] Version requirements noted
- [ ] Screenshots/diagrams current
- [ ] Installation instructions tested on clean environment

## Common Mistakes

1. **Outdated screenshots** - Re-capture with each UI change
2. **Broken links** - Verify all links work
3. **Missing prerequisites** - List all requirements
4. **Code that doesn't work** - Test all examples
5. **Ambiguous placeholders** - Use `{{CLEAR_NAME}}` not `{{X}}`
6. **No examples** - Always show code
7. **Too much jargon** - Explain technical terms
8. **No error documentation** - Document what can go wrong
9. **Incomplete installation** - Include all steps
10. **No troubleshooting** - Anticipate common problems

## Tools

### Documentation Generators
- **Swagger/OpenAPI** - API documentation
- **JSDoc/TSDoc** - Code documentation
- **TypeDoc** - TypeScript documentation
- **Sphinx** - Python documentation

### Diagram Tools
- **Mermaid** - Text-based diagrams
- **Draw.io** - Visual diagrams
- **PlantUML** - UML diagrams

### Linters
- **markdownlint** - Markdown linting
- **alex** - Catch insensitive language
- **write-good** - Improve writing style

## Further Reading

- [Write the Docs](https://www.writethedocs.org/)
- [Google Technical Writing](https://developers.google.com/tech-writing)
- [The Documentation System](https://documentation.divio.com/)
- [Docs for Developers](https://docsfordevelopers.com/)