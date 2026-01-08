# Ralph Skill Repository

A blueprint for autonomous AI coding using the Ralph approach. This repository serves as:

1. **Learning playground** - Practice Ralph methodology hands-on
2. **Claude Code skill** - Reusable commands to start Ralph-based projects
3. **Template library** - Ready-to-use files for new projects

## What is Ralph?

Ralph is an autonomous AI coding loop that ships features while you sleep. Created by Geoffrey Huntley, it runs your AI agent (Claude Code) repeatedly until all tasks are complete.

**Key concept**: Each iteration is a fresh context window. Memory persists only through:
- Git commits
- `progress.txt` (learnings)
- `prd.json` (task status)

## Quick Start

### 1. Initialize Ralph in any project

```bash
cd your-project
/ralph-init
```

This creates the `scripts/ralph/` folder with all necessary files.

### 2. Define your user stories

Edit `scripts/ralph/prd.json` to add your tasks:

```json
{
  "branchName": "ralph/my-feature",
  "userStories": [
    {
      "id": "US-001",
      "title": "Your first task",
      "acceptanceCriteria": ["Specific", "Measurable", "Criteria"],
      "priority": 1,
      "passes": false
    }
  ]
}
```

### 3. Run Ralph

```bash
/ralph-run
```

Or manually:
```bash
./scripts/ralph/ralph.sh 10
```

### 4. Check progress

```bash
/ralph-status
```

## Repository Structure

```
ai--ralph-skill/
├── README.md                    # This file
├── RALPH_GUIDE.md              # Deep-dive documentation
├── .claude/commands/           # Claude Code skills
│   ├── ralph-init.md           # Initialize Ralph in a project
│   ├── ralph-run.md            # Execute the Ralph loop
│   └── ralph-status.md         # Check Ralph progress
├── templates/                  # Ready-to-use files
│   ├── ralph.sh                # The loop script
│   ├── prompt.md               # Agent instructions
│   ├── prd-template.json       # User stories template
│   └── progress-template.txt   # Progress log template
├── examples/                   # Practice examples
│   └── hello-world/            # Simple first project
└── learnings/
    └── PATTERNS.md             # Accumulated learnings
```

## Critical Success Factors

### 1. Small Stories
Each story must fit in one context window.

```
Bad:  "Build entire auth system"
Good: "Add login form"
      "Add email validation"
      "Add auth server action"
```

### 2. Explicit Acceptance Criteria

```
Bad:  "Users can log in"
Good: - Email/password fields exist
      - Validates email format
      - Shows error on invalid input
      - typecheck passes
```

### 3. Fast Feedback Loops
Ralph needs quick validation:
- `npm run typecheck`
- `npm test`

### 4. Learnings Compound
By story 10, Ralph knows patterns from stories 1-9.

## Learn More

See [RALPH_GUIDE.md](./RALPH_GUIDE.md) for complete documentation.

## Credits

- Original concept by [@GeoffreyHuntley](https://twitter.com/GeoffreyHuntley)
- Guide by [@ryancarson](https://twitter.com/ryancarson)
