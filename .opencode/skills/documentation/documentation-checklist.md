# Documentation Checklist

Comprehensive checklist for creating and maintaining documentation.

## Before Writing

### Understand Audience
- [ ] Who will read this?
- [ ] What do they already know?
- [ ] What do they need to know?
- [ ] What will they use this for?

### Identify Purpose
- [ ] What question does this answer?
- [ ] What problem does this solve?
- [ ] What task does this help with?
- [ ] What decision does this inform?

### Gather Information
- [ ] All necessary information collected
- [ ] Subject matter experts consulted
- [ ] Code/system reviewed for accuracy
- [ ] Examples identified

## Structure

### Organization
- [ ] Logical flow of information
- [ ] Easy to scan (headings, lists)
- [ ] Table of contents (for long docs)
- [ ] Clear section headings

### Navigation
- [ ] Links to related documentation
- [ ] Links to external resources
- [ ] Cross-references where needed
- [ ] Index or search optimization

## Content

### Accuracy
- [ ] Technical details correct
- [ ] Code examples work
- [ ] Commands tested
- [ ] Links working

### Clarity
- [ ] Simple language used
- [ ] Jargon explained or avoided
- [ ] Active voice preferred
- [ ] Concise paragraphs (3-4 sentences)

### Completeness
- [ ] All steps included
- [ ] Prerequisites listed
- [ ] Error cases documented
- [ ] Troubleshooting included

### Consistency
- [ ] Terminology consistent
- [ ] Formatting consistent
- [ ] Tone consistent
- [ ] Style follows guidelines

## Code Examples

### Quality
- [ ] Examples are complete
- [ ] Examples are realistic
- [ ] Examples are tested
- [ ] Examples are explained

### Presentation
- [ ] Syntax highlighted
- [ ] Language specified
- [ ] Line numbers (if helpful)
- [ ] Comments included where needed

## Formatting

### Headings
- [ ] Hierarchy correct (H1 > H2 > H3)
- [ ] No skipped levels
- [ ] Descriptive headings
- [ ] Consistent capitalization

### Lists
- [ ] Used for sequences (ordered)
- [ ] Used for options (unordered)
- [ ] Parallel structure
- [ ] Consistent punctuation

### Tables
- [ ] Used for structured data
- [ ] Headers clear
- [ ] Alignment appropriate
- [ ] Not overly complex

### Links
- [ ] Descriptive link text
- [ ] Links to correct targets
- [ ] External vs internal clear
- [ ] No broken links

## Specific Document Types

### README.md
- [ ] Project name and description
- [ ] Installation instructions
- [ ] Quick start guide
- [ ] Basic usage examples
- [ ] License
- [ ] Contributing link
- [ ] Contact/support info

### API Documentation
- [ ] Authentication method
- [ ] Base URL
- [ ] Request format
- [ ] Response format
- [ ] All endpoints documented
- [ ] Error codes documented
- [ ] Rate limiting info
- [ ] Examples in multiple languages
- [ ] SDK/client libraries

### Architecture Docs
- [ ] High-level overview
- [ ] Component diagram
- [ ] Data flow
- [ ] Key decisions explained
- [ ] Trade-offs documented
- [ ] Future considerations

### User Guide
- [ ] Prerequisites
- [ ] Getting started
- [ ] Common tasks
- [ ] Advanced usage
- [ ] Troubleshooting
- [ ] FAQ

### ADR (Architecture Decision Record)
- [ ] Title and number
- [ ] Status (proposed/accepted/etc.)
- [ ] Context/Problem statement
- [ ] Decision
- [ ] Consequences
- [ ] Alternatives considered
- [ ] Date and authors

## Maintenance

### Updates
- [ ] Updated with code changes
- [ ] Examples still work
- [ ] Links still valid
- [ ] Screenshots current
- [ ] Version numbers updated

### Review Cycle
- [ ] Regular review scheduled
- [ ] Outdated docs flagged
- [ ] Feedback incorporated
- [ ] Deprecation notices added

## Accessibility

### Readability
- [ ] 8th-grade reading level (or appropriate for audience)
- [ ] Acronyms defined on first use
- [ ] Technical terms explained
- [ ] Short sentences

### Screen Readers
- [ ] Proper heading hierarchy
- [ ] Descriptive link text
- [ ] Alt text for images
- [ ] Tables have headers

### Internationalization
- [ ] Avoid idioms
- [ ] Clear, simple English
- [ ] Standard date formats (ISO 8601)
- [ ] Avoid culture-specific references

## Publishing

### Pre-Publication
- [ ] Reviewed by subject matter expert
- [ ] Reviewed by technical writer (if available)
- [ ] Tested by someone unfamiliar with topic
- [ ] All checklist items verified

### Post-Publication
- [ ] Announced to users
- [ ] Added to documentation index
- [ ] Linked from relevant places
- [ ] Feedback mechanism available

## Anti-Patterns to Avoid

- [ ] Outdated screenshots
- [ ] Outdated code examples
- [ ] Broken links
- [ ] Missing prerequisites
- [ ] Ambiguous placeholders (`{{X}}` instead of `{{API_KEY}}`)
- [ ] No examples
- [ ] Too much jargon
- [ ] No error documentation
- [ ] Incomplete installation instructions
- [ ] No troubleshooting section
- [ ] Copy-pasted from other sources without verification
- [ ] Screenshots of code instead of actual code blocks
- [ ] Walls of text without structure
- [ ] Missing version information
- [ ] No date or last-updated info