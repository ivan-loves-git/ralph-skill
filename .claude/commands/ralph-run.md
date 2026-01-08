# Ralph Run

Execute the Ralph autonomous coding loop.

## Instructions

You are starting a Ralph session. Follow these steps:

### 1. Verify Ralph is initialized

Check that `scripts/ralph/` exists with:
- ralph.sh
- prompt.md
- prd.json
- progress.txt

If not found, tell the user to run `/ralph-init` first.

### 2. Check prd.json has stories

Read `scripts/ralph/prd.json` and verify:
- At least one user story exists
- At least one story has `passes: false`
- Branch name is specified

If all stories already pass, inform the user Ralph is complete.

### 3. Confirm before running

Show the user:
- Number of stories remaining
- Branch that will be used
- Ask for confirmation before starting

### 4. Start the loop

Run the Ralph script:

```bash
./scripts/ralph/ralph.sh [iterations]
```

Default is 10 iterations. User can specify more if needed.

### 5. Important notes

Remind the user:
- Ralph uses `--dangerously-skip-permissions` flag
- Each iteration is autonomous
- Progress is saved in git commits and progress.txt
- They can stop anytime with Ctrl+C
- Use `/ralph-status` to check progress

## Arguments

- `$ARGUMENTS` - Optional: number of max iterations (default: 10)

## Example

User runs `/ralph-run 25` to start Ralph with max 25 iterations.
