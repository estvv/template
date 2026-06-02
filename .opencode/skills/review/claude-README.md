# Claude Code Usage

This skill can be used with Claude Code with the following command:

```bash
# Review changes in current branch vs main
claude --skill review --argument "main"

# Review specific file
claude --skill review --argument "path/to/file.ts"

# Review staged changes
claude --skill review --argument "--staged"
```

**Dynamic execution:** Claude Code will automatically execute `git diff main` and inject the output into the skill context.

## Skill Invocation

Claude Code uses the skill system differently than OpenCode:

### OpenCode
```typescript
// Agent sees skill in available_skills
// Agent calls skill tool to load content
skill({ name: "review" })
```

### Claude Code
```bash
# User invokes skill explicitly
claude --skill review --argument "main"

# Or agent detects skill is relevant and uses it
# Content is loaded with dynamic git diff injected
```

## Compatibility Notes

1. **OpenCode**: Uses `name` in frontmatter
2. **Claude Code**: Uses `description` to find skills (name optional)
3. **Both**: Progressive disclosure works the same
4. **Claude-specific**: Can execute commands with `!`command``
5. **OpenCode-specific**: Can use `compatibility: [opencode]` field

## Making Skills Work in Both

### Universal frontmatter:
```yaml
---
name: skill-name                    # OpenCode uses this
description: Skill description      # Both use this
license: MIT                        # Optional, both support
compatibility: [opencode, claude]   # Metadata for both
argument-hint: <branch-or-file>     # Claude-specific
---
```

### Universal content:
```markdown
## For OpenCode
Use the checklist in skill-checklist.md

## For Claude Code
<!-- !`git diff $ARGUMENTS` -->
Review the above changes using the checklist in review-checklist.md
```