# Ralph Init

Initialize the Ralph autonomous coding structure in the current project.

## Instructions

You are setting up Ralph for autonomous AI coding. Follow these steps:

### 1. Create the Ralph directory structure

Create `scripts/ralph/` folder in the current project root.

### 2. Copy template files

Copy these files from the ralph-skill templates to `scripts/ralph/`:

**ralph.sh** (make executable with chmod +x):
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

**prompt.md**:
```markdown
# Ralph Agent Instructions

## Your Task

1. Read `scripts/ralph/prd.json` for user stories
2. Read `scripts/ralph/progress.txt` (check Codebase Patterns section first)
3. Verify you're on the correct branch (create if needed)
4. Pick the highest priority story where `passes: false`
5. Implement that ONE story only
6. Run typecheck and/or tests as specified in acceptance criteria
7. If passing, commit with message: `feat: [ID] - [Title]`
8. Update prd.json: set `passes: true` for completed story
9. Append learnings to progress.txt following the format

## Progress Format

APPEND to the end of progress.txt:

## [Date] - [Story ID]
- What was implemented
- Files changed
- **Learnings:**
  - Patterns discovered
  - Gotchas encountered
---

## Codebase Patterns

If you discover reusable patterns, ADD them to the Codebase Patterns section at the TOP of progress.txt.

## Stop Condition

If ALL stories have `passes: true`, output exactly:
<promise>COMPLETE</promise>

Otherwise, end your response normally. The loop will restart with fresh context.
```

**prd.json**:
```json
{
  "branchName": "ralph/feature-name",
  "userStories": [
    {
      "id": "US-001",
      "title": "First user story",
      "acceptanceCriteria": [
        "Specific measurable criterion",
        "Another criterion",
        "typecheck passes"
      ],
      "priority": 1,
      "passes": false,
      "notes": ""
    }
  ]
}
```

**progress.txt**:
```markdown
# Ralph Progress Log
Started: [TODAY'S DATE]

## Codebase Patterns
- [Add patterns discovered during development here]

## Key Files
- [List important files for context]

---
```

### 3. Initialize git branch

Check if the project has git initialized. If not, inform the user.

### 4. Provide next steps

Tell the user:
1. Edit `scripts/ralph/prd.json` to define their user stories
2. Run `/ralph-run` or `./scripts/ralph/ralph.sh` to start
3. Monitor with `/ralph-status`

## Output

Confirm what was created and provide clear next steps.
