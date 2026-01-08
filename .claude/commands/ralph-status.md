# Ralph Status

Check the current status of a Ralph session.

## Instructions

Provide a comprehensive status report of the Ralph session.

### 1. Check prd.json

Read `scripts/ralph/prd.json` and report:
- Total number of stories
- Stories completed (passes: true)
- Stories remaining (passes: false)
- Current priority story (next to be worked on)

Display as a table:

| ID | Title | Priority | Status |
|----|-------|----------|--------|
| US-001 | Story title | 1 | Done |
| US-002 | Another story | 2 | Pending |

### 2. Check progress.txt

Read `scripts/ralph/progress.txt` and summarize:
- Codebase patterns discovered
- Recent story completions
- Key learnings

### 3. Check git status

Show:
- Current branch
- Recent commits (last 5)
- Any uncommitted changes

### 4. Provide recommendations

Based on status:
- If all done: Congratulate and suggest next steps (PR, merge)
- If in progress: Show what's next
- If stuck: Suggest reviewing the failing story's criteria

## Output Format

```
RALPH STATUS REPORT
==================

Stories: 3/10 complete (30%)
Branch: ralph/feature-name

| ID     | Title              | Priority | Status  |
|--------|--------------------|---------:|---------|
| US-001 | First story        |        1 | Done    |
| US-002 | Second story       |        2 | Done    |
| US-003 | Third story        |        3 | Done    |
| US-004 | Fourth story       |        4 | Pending |
...

Patterns Learned:
- Pattern 1
- Pattern 2

Recent Commits:
- feat: US-003 - Third story
- feat: US-002 - Second story
- feat: US-001 - First story

Next: US-004 - Fourth story
```
