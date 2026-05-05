---
name: nhallucinate
description: Use when writing code, fixing bugs, or making changes to any project - prevents hallucination by grounding in project-specific memory with pattern tracking
---

# nhallucinate Skill

An AI anti-hallucination skill using project-specific memory with manual pattern tracking.

## Problem It Solves

- AI forgets what was tried 30 minutes ago
- Repeats same broken solutions
- Context window exhaustion in long sessions

## Core Features

### 1. Memory Persistence
External file storage survives context window limits. Memory persists across sessions.

### 2. Pattern Tracking
Manually track failed approaches. Check before trying solutions.

### 3. Context Management
Simple approach - just save often to external files.

---

## Folder Structure

Each project gets its own memory at `.agent/nhallucinate/`:

```
/path/to/project/
├── .agent/
│   └── nhallucinate/
│       ├── memory.md              # Milestones, recent changes
│       ├── project-context.md       # Tech stack, file structure
│       ├── lessons-learned.md     # Mistakes to avoid
│       ├── current-task.md         # Active task state
│       └── history/              # Session snapshots
├── src/
└── ...
```

## When to Use

- When writing new code in a project
- When fixing bugs or debugging
- When making changes to existing files
- When switching between projects

## When NOT to Use

- Pure questions (no code changes)
- Repository exploration without edits
- One-off commands

---

## Memory Files

| File | Purpose |
|------|---------|
| memory.md | Project state, milestones |
| project-context.md | Tech stack, key files |
| lessons-learned.md | What didn't work |
| current-task.md | Current task, progress |

---

## Workflow

### Step 1: Read Memory

Read `.agent/nhallucinate/`:
1. memory.md - current state
2. current-task.md - active task
3. project-context.md - tech stack
4. lessons-learned.md - what to avoid

### Step 2: Check Before Working

- Look at lessons-learned.md first
- Check if similar approach failed before

### Step 3: During Work

- Document changes with timestamps
- Note what didn't work

### Step 4: On Failure

After attempts fail:
1. Write to memory.md
2. Add to lessons-learned.md
3. Suggest alternative

### Step 5: Session End

- Save to memory.md
- Write snapshot to history/

---

## Pattern Tracking

### Manual Process

```
Before trying: Check lessons-learned.md
After failure: Add to lessons-learned.md
Next time:    Read before trying
```

### What to Track

```markdown
## Failed Approaches
- 2026-05-05: Reset password flow - caused redirect loop

## Solutions That Worked
- Check auth state first
```

---

## Context Management

When context fills:
- Save to memory files immediately
- Keep summaries
- Read back when needed

---

## Commands

### /nhallucinate init

```bash
mkdir -p .agent/nhallucinate/history
touch .agent/nhallucinate/memory.md
touch .agent/nhallucinate/project-context.md
touch .agent/nhallucinate/lessons-learned.md
touch .agent/nhallucinate/current-task.md
```

### /nhallucinate status

```bash
echo "=== $(pwd) ==="
cat .agent/nhallucinate/memory.md
cat .agent/nhallucinate/current-task.md
```

---

## Quick Reference

| Command | Description |
|---------|-------------|
| `/nhallucinate init` | Initialize folder |
| `/nhallucinate status` | Show status |
| `/nhallucinate reset` | Clear memory |

---

## Best Practices

1. **Check lessons-learned.md first** - Before trying any solution
2. **Document failures** - Add to lessons-learned.md immediately
3. **Save often** - Update memory after milestones
4. **Use timestamps** - Always include dates

---

## Summary

| Component | Problem Solved |
|-----------|---------------|
| Memory Persistence | Forgets context |
| Pattern Tracking | Repeats errors |
| Manual Verification | Check files yourself |

**Use graphify** to understand file structure. nhallucinate remembers context.

**Core Principle**: What AI learns in Project A stays in Project A.