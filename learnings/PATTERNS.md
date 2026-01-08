# Ralph Patterns & Learnings

This file accumulates patterns and learnings from Ralph sessions across all projects.

## Story Writing Patterns

### Size Guidelines
- If a story takes more than 1 iteration, it's too big
- If you can say "and" in the title, split it
- Each story should be 5-15 minutes of work

### Good Acceptance Criteria
```
- Specific file exists
- Function returns expected value
- Command produces expected output
- typecheck/tests pass
```

### Bad Acceptance Criteria
```
- "Works correctly" (too vague)
- "Looks good" (subjective)
- "Is performant" (not measurable)
```

## Common Gotchas

### Node.js
- Use `process.argv[2]` for first CLI argument (0 is node, 1 is script)
- Always handle undefined arguments with defaults

### Git
- Ralph creates branch if it doesn't exist
- Commits only happen when validation passes
- Check `git log` to see Ralph's work

### File Structure
- Keep `scripts/ralph/` at project root
- All four files are required (ralph.sh, prompt.md, prd.json, progress.txt)

## Debugging Ralph

### If Ralph gets stuck
1. Check progress.txt for error logs
2. Check prd.json - is the story too big?
3. Check acceptance criteria - are they specific enough?

### If Ralph skips a story
- The story might be blocked by a previous one
- Check priority ordering
- Read notes field for context

## Your Learnings

<!-- Add your own learnings below as you use Ralph -->

---
