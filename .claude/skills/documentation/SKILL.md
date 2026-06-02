---
description: Write clear, comprehensive documentation. Use when creating README, API docs, architecture docs, user guides, or documenting code.
disable-model-invocation: true
argument-hint: <file-or-directory>
---

## Core Principles

1. **Clarity Over Cleverness** - Write to be understood, not to impress
2. **Audience-First** - Write for your readers, not yourself
3. **Living Documents** - Keep docs updated with code changes
4. **Show, Don't Just Tell** - Use examples and code snippets
5. **Structure for Discovery** - Make information easy to find

## Documentation Types

### README.md
- Quick introduction
- Getting started guide
- Basic usage examples
- Installation instructions

### API Documentation
- Endpoints and methods
- Request/response formats
- Authentication
- Error codes
- Examples

### Architecture Docs
- System overview
- Component diagram
- Data flow
- Key decisions (ADRs)

### User Guides
- Getting started
- Common tasks
- Troubleshooting
- FAQ

## Writing Guidelines

### Be Concise
```markdown
# Bad
In order to install the package, you will need to run the installation command.

# Good
Install the package:
\`\`\`bash
npm install package-name
\`\`\`
```

### Front-Load Information
```markdown
# Bad
There are many configuration options. Before we discuss those, let's talk about installation.

# Good
Install in 30 seconds:
\`\`\`bash
npm install package-name
\`\`\`
```

### Use Examples
```markdown
# Bad
The function accepts three parameters: name (string), age (number), email (string).

# Good
\`\`\`typescript
createUser('John Doe', 30, 'john@example.com');
\`\`\`

Parameters:
- name (string): User's full name
- age (number): User's age
- email (string): User's email
```

### Use Active Voice
```markdown
# Bad
The configuration file should be edited by you.

# Good
Edit the configuration file.
```

### Use Tree Structures

Tree structures help visualize file hierarchies and project organization. Use them when they add value (not for every document).

**When to use:**
- Project structure overview
- Directory explanations
- Module organization
- Package/file relationships

**When to skip:**
- Flat structures (< 3 files)
- Simple concepts
- When prose is clearer

```markdown
# Good - Complex structure benefits from visualization
## Project Structure

\`\`\`
src/
├── components/           # UI components
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx
│   │   └── Button.styles.ts
│   └── Form/
├── services/             # Business logic
│   ├── api/
│   └── auth/
├── utils/                # Utility functions
└── types/                # TypeScript types
\`\`\`

# Bad - Over-documenting simple structure
## Files

\`\`\`
README.md
package.json
\`\`\`
```

### Use ASCII Diagrams

ASCII diagrams visualize flows, relationships, and architectures. Use them when they clarify complex concepts.

**When to use:**
- Architecture overviews
- Data flows
- Process sequences
- Component relationships
- State machines
- Sequence diagrams

**When to skip:**
- Simple linear processes
- When prose is clearer
- When a table works better

**Types of ASCII Diagrams:**

#### Flow Diagrams
```markdown
## Authentication Flow

\`\`\`
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Client  │────>│   API    │────>│ Database │
└──────────┘     └──────────┘     └──────────┘
      │                │                 │
      │                │                 │
      │                ↓                 │
      │         ┌──────────┐            │
      │         │   Auth   │            │
      │         │ Service  │            │
      │         └──────────┘            │
      │                                    │
      └────────────────────────────────────┘
\`\`\`
```

#### Sequence Diagrams
```markdown
## Request Flow

\`\`\`
Client          API            Database
  │               │                │
  ├─── GET /users┤                │
  │               ├─── query users┤
  │               │                │
  │               │◄─── users─────┤
  │◄─── users────┤                │
  │               │                │
\`\`\`
```

#### Component Diagrams
```markdown
## System Architecture

\`\`\`
┌─────────────────────────────────────────┐
│              Load Balancer               │
└────────────────┬────────────────────────┘
                 │
        ┌────────┴────────┐
        │                  │
   ┌────▼────┐       ┌────▼────┐
   │  API 1  │       │  API 2  │
   └────┬────┘       └────┬────┘
        │                  │
        └────────┬─────────┘
                 │
         ┌───────▼────────┐
         │    Database    │
         └────────────────┘
\`\`\`
```

#### State Machine
```markdown
## Order State Machine

\`\`\`
      ┌──────────┐
      │  Created │
      └─────┬────┘
            │ confirm
      ┌─────▼────┐
      │ Pending  │
      └─────┬────┘
            │ process
      ┌─────▼────┐
      │ Processed│◄─┐
      └─────┬────┘  │
            │       │ retry
            │ error │
            ▼       │
      ┌──────────┐  │
      │  Failed  │──┘
      └──────────┘
\`\`\`
```

#### Decision Tree
```markdown
## Error Handling Strategy

\`\`\`
            ┌─────────────┐
            │ Error Type? │
            └──────┬──────┘
                   │
         ┌─────────┼─────────┐
         │         │         │
         ▼         ▼         ▼
     ┌───────┐ ┌───────┐ ┌───────┐
     │ Auth  │ │ Data  │ │Network│
     └───┬───┘ └───┬───┘ └───┬───┘
         │         │         │
         ▼         ▼         ▼
      Retry     Return    Retry
      with       400      with
      refresh             backoff
\`\`\`
```

**ASCII Drawing Tips:**
1. Use box-drawing characters: `┌ ┐ └ ┘ ├ ┤ ┬ ┴ ┼ ─ │`
2. Use arrows: `─ ► ▶ → ← ▲ ▼ │`
3. Align boxes and labels properly
4. Keep it simple - if it needs > 20 lines, consider a diagram tool
5. Use fixed-width font in markdown code blocks

**When to Use Each Type:**
| Diagram Type | Best For |
|--------------|----------|
| Flow diagram | System flows, data flows, decision trees |
| Sequence diagram | Request/response, API interactions |
| Component diagram | Architecture, system components |
| Tree structure | File hierarchies, package organization |
| State machine | States, transitions, workflows |

## Documentation Structure

```
docs/
  README.md           # Overview
  ARCHITECTURE.md     # System architecture
  API.md              # API documentation
  DATABASE.md         # Database schema
  DEPLOYMENT.md       # Deployment guide
  DEVELOPMENT.md      # Development setup
  TESTING.md          # Testing guide
  SECURITY.md         # Security practices
  adr/                # Architecture Decision Records
    0001-use-typescript.md
```

## README Template

\`\`\`markdown
# Project Name

Brief description.

## Quick Start

\`\`\`bash
npm install package-name
npm start
\`\`\`

## Installation

\`\`\`bash
npm install package-name
\`\`\`

## Usage

Basic example.

## Features

- Feature 1
- Feature 2

## Documentation

- [API](./docs/API.md)
- [Architecture](./docs/ARCHITECTURE.md)

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md)

## License

MIT
\`\`\`

## Code Documentation

### Functions
\`\`\`typescript
/**
 * Calculate the total price including tax.
 * 
 * @param price - Base price
 * @param taxRate - Tax rate (0.1 = 10%)
 * @returns Total price including tax
 * 
 * @example
 * calculateTotal(100, 0.1); // 110
 */
function calculateTotal(price: number, taxRate: number): number {
  return price * (1 + taxRate);
}
\`\`\`

## Checklist

Before publishing:

- [ ] All placeholders replaced
- [ ] Examples tested
- [ ] Links verified
- [ ] Spelling/grammar checked
- [ ] Headings properly nested
- [ ] Code blocks have language tags
- [ ] Prerequisites listed

See `documentation-checklist.md` for comprehensive checklist.

## Related Skills

- Use **adr** skill for architecture decision records
- Use the existing documentation/ folder structure