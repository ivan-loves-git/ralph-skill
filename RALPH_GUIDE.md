# Ralph Complete Guide

This is the comprehensive guide to the Ralph autonomous coding methodology.

## Table of Contents

1. [Philosophy](#philosophy)
2. [How It Works](#how-it-works)
3. [File Structure](#file-structure)
4. [Writing Good User Stories](#writing-good-user-stories)
5. [The Loop Script](#the-loop-script)
6. [The Prompt](#the-prompt)
7. [Progress Tracking](#progress-tracking)
8. [Common Gotchas](#common-gotchas)
9. [When NOT to Use Ralph](#when-not-to-use-ralph)
10. [Monitoring](#monitoring)

---

## Philosophy

Ralph is built on a simple principle: **AI agents work best with small, clear tasks and fast feedback.**

Traditional AI coding sessions suffer from:
- Context window bloat (too much history)
- Unclear goals (what should I build?)
- No validation (did it actually work?)
- Lost learnings (same mistakes repeated)

Ralph solves these by:
- Fresh context each iteration (small threads)
- Explicit acceptance criteria (clear goals)
- Typecheck + tests after each story (fast feedback)
- Progress file accumulates learnings (compound knowledge)

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│                     RALPH LOOP                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│   1. Read prd.json (find next story)                   │
│              ↓                                          │
│   2. Read progress.txt (check learnings)               │
│              ↓                                          │
│   3. Implement ONE story                               │
│              ↓                                          │
│   4. Run typecheck + tests                             │
│              ↓                                          │
│   5. If passing → commit                               │
│              ↓                                          │
│   6. Mark story as done in prd.json                    │
│              ↓                                          │
│   7. Append learnings to progress.txt                  │
│              ↓                                          │
│   8. REPEAT until all stories pass                     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Memory persists ONLY through:**
- Git commits (what was built)
- progress.txt (what was learned)
- prd.json (what's done/remaining)

## File Structure

Every Ralph-enabled project has:

```
your-project/
├── scripts/ralph/
│   ├── ralph.sh          # The loop script
│   ├── prompt.md         # Instructions for each iteration
│   ├── prd.json          # User stories (your task list)
│   └── progress.txt      # Accumulated learnings
└── ... your project files
```

### ralph.sh

The bash script that runs the loop:

```bash
#!/bin/bash
set -e

MAX_ITERATIONS=${1:-10}
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

echo "Starting Ralph loop (max $MAX_ITERATIONS iterations)"

for i in $(seq 1 $MAX_ITERATIONS); do
  echo "═══════════════════════════════════════"
  echo "Iteration $i of $MAX_ITERATIONS"
  echo "═══════════════════════════════════════"

  OUTPUT=$(cat "$SCRIPT_DIR/prompt.md" | claude --dangerously-skip-permissions 2>&1 | tee /dev/stderr) || true

  if echo "$OUTPUT" | grep -q "<promise>COMPLETE</promise>"; then
    echo "All stories complete!"
    exit 0
  fi

  sleep 2
done

echo "Max iterations reached. Check progress.txt for status."
exit 1
```

### prompt.md

Instructions given to the AI each iteration:

```markdown
# Ralph Agent Instructions

## Your Task

1. Read `scripts/ralph/prd.json` for user stories
2. Read `scripts/ralph/progress.txt` (check Codebase Patterns first)
3. Verify you're on the correct branch
4. Pick highest priority story where `passes: false`
5. Implement that ONE story
6. Run typecheck and tests
7. Commit with message: `feat: [ID] - [Title]`
8. Update prd.json: set `passes: true`
9. Append learnings to progress.txt

## Stop Condition

If ALL stories have `passes: true`, output:
<promise>COMPLETE</promise>

Otherwise, end normally (loop will restart).
```

### prd.json

Your task list in JSON format:

```json
{
  "branchName": "ralph/feature-name",
  "userStories": [
    {
      "id": "US-001",
      "title": "Short description",
      "acceptanceCriteria": [
        "Specific criterion 1",
        "Specific criterion 2",
        "typecheck passes"
      ],
      "priority": 1,
      "passes": false,
      "notes": ""
    }
  ]
}
```

### progress.txt

Accumulates learnings across iterations:

```markdown
# Ralph Progress Log
Started: 2025-01-08

## Codebase Patterns
- [Patterns discovered during work go here]
- [These persist across iterations]

## Key Files
- [Important files the agent should know about]

---

## 2025-01-08 - US-001
- What was implemented
- Files changed
- **Learnings:**
  - Pattern discovered
  - Gotcha encountered
---
```

## Writing Good User Stories

### The Golden Rules

1. **One thing per story** - If you use "and", split it
2. **Fits in one context** - Can be done in ~10-15 minutes
3. **Explicit criteria** - No ambiguity about "done"
4. **Include validation** - "typecheck passes" or "tests pass"

### Bad vs Good Examples

```
BAD:  "Build the authentication system"
GOOD: "US-001: Add login form with email/password fields"
      "US-002: Add email format validation"
      "US-003: Add password strength indicator"
      "US-004: Connect form to auth API"
      "US-005: Add error handling for failed login"
```

```
BAD:  "Make the UI look better"
GOOD: "US-010: Add consistent spacing (8px grid)"
      "US-011: Apply color palette to buttons"
      "US-012: Add hover states to clickable elements"
```

### Acceptance Criteria Template

```json
{
  "acceptanceCriteria": [
    "WHAT: [specific thing that exists]",
    "BEHAVIOR: [what it does when interacted with]",
    "VALIDATION: typecheck passes",
    "VALIDATION: tests pass (if applicable)"
  ]
}
```

## The Loop Script

### Running Ralph

```bash
# Run with default 10 iterations
./scripts/ralph/ralph.sh

# Run with custom iteration count
./scripts/ralph/ralph.sh 25

# Run in background (ships while you sleep)
nohup ./scripts/ralph/ralph.sh 50 > ralph.log 2>&1 &
```

### What happens each iteration

1. Script reads `prompt.md`
2. Pipes it to `claude --dangerously-skip-permissions`
3. Claude executes the instructions
4. If output contains `<promise>COMPLETE</promise>`, stops
5. Otherwise, waits 2 seconds and loops

### The `--dangerously-skip-permissions` flag

This flag lets Claude:
- Read/write files without asking
- Run commands without confirmation
- Make git commits autonomously

**Use responsibly** - only on projects where you trust the AI's actions.

## Progress Tracking

### Codebase Patterns Section

At the TOP of progress.txt, maintain reusable patterns:

```markdown
## Codebase Patterns
- Migrations: Always use IF NOT EXISTS
- React refs: Use useRef<NodeJS.Timeout | null>(null)
- API routes: Export named functions, not default
- Tests: Run dev server before e2e tests
```

These patterns are read FIRST each iteration, so Ralph learns from past mistakes.

### Story Logs

After each story, Ralph appends:

```markdown
## 2025-01-08 - US-003
- Implemented password strength indicator
- Files changed: src/components/PasswordInput.tsx
- **Learnings:**
  - Used zxcvbn library for strength calculation
  - Component expects `onChange` prop, not `onInput`
---
```

## Common Gotchas

### 1. Interactive Prompts

Some commands ask for input. Pre-answer them:

```bash
# Bad - will hang waiting for input
npm run db:generate

# Good - pipes empty responses
echo -e "\n\n\n" | npm run db:generate
```

### 2. Idempotent Migrations

Database changes should be safe to run multiple times:

```sql
-- Bad
ALTER TABLE users ADD COLUMN email TEXT;

-- Good
ALTER TABLE users ADD COLUMN IF NOT EXISTS email TEXT;
```

### 3. Schema Changes Cascade

When you change a database schema or type definition, related files break:
- Server actions
- API routes
- UI components

Tell Ralph it's OK to fix these:

```json
{
  "notes": "May need to update related server actions if schema changes"
}
```

### 4. Branch Management

Ralph creates/uses the branch specified in prd.json. Make sure:
- You're not on a protected branch
- The branch name doesn't exist if starting fresh
- You've committed any work-in-progress first

## When NOT to Use Ralph

Ralph is NOT suitable for:

| Situation | Why |
|-----------|-----|
| Exploratory work | No clear acceptance criteria |
| Major refactors | Too big for one context window |
| Security-critical code | Needs human review |
| Learning new concepts | You want to understand, not just ship |
| Ambiguous requirements | "Make it better" won't work |

## Monitoring

### Check story status

```bash
cat scripts/ralph/prd.json | jq '.userStories[] | {id, title, passes}'
```

### View recent commits

```bash
git log --oneline -10
```

### Read learnings

```bash
cat scripts/ralph/progress.txt
```

### Watch in real-time

```bash
tail -f ralph.log
```

---

## Summary

Ralph works because it embraces constraints:
- Small tasks (fit in context)
- Clear criteria (know when done)
- Fast feedback (typecheck/tests)
- Persistent memory (git + files)

Start small. One feature. A few stories. See it work. Then scale up.
