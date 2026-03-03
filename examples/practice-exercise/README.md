# Hello World - Ralph Practice Project

This is a simple practice project to learn the Ralph workflow.

## What You'll Build

A simple Node.js script that greets users in different ways.

## Prerequisites

- Node.js installed (you have v22)
- Git initialized
- Claude Code available

## How to Practice

### Step 1: Copy this example to a new folder

```bash
cp -r examples/hello-world ~/Desktop/ralph-practice
cd ~/Desktop/ralph-practice
```

### Step 2: Initialize git

```bash
git init
git add .
git commit -m "Initial commit"
```

### Step 3: Run Ralph

```bash
./scripts/ralph/ralph.sh 5
```

### Step 4: Watch Ralph work

Ralph will:
1. Create the greeting functions
2. Add personalization
3. Make it executable
4. All while you watch (or sleep!)

## User Stories

This example has 3 simple stories:

1. **US-001**: Create basic greeting function
2. **US-002**: Add personalized greeting
3. **US-003**: Make script executable from command line

## After Ralph Completes

You'll have a working greeter:

```bash
node src/greet.js
# Output: Hello, World!

node src/greet.js Ivan
# Output: Hello, Ivan! Welcome!
```

## Learning Points

This simple example teaches you:
- How prd.json defines tasks
- How progress.txt accumulates learnings
- How Ralph commits after each story
- How the loop stops when done
