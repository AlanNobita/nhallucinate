---
name: nhallucinate
description: Use when writing code, fixing bugs, or making changes to any project - prevents hallucination by grounding in project-specific memory
---

# nhallucinate Skill

## Folder Structure

Each project gets its own memory at `.agent/nhallucinate/`:

```
/path/to/project/
├── .agent/
│   └── nhallucinate/
│       ├── memory.md            # Milestones, recent changes
│       ├── project-context.md   # Tech stack, file structure
│       ├── lessons-learned.md  # Mistakes to avoid
│       ├── current-task.md     # Active task state
│       └── history/            # Session snapshots
├── src/
└── ...
```

- When writing new code in a project
- When fixing bugs or debugging
- When making changes to existing files
- When switching between projects

**When NOT to use:**
- Pure questions (no code changes)
- Repository exploration without edits
- One-off commands that don't modify code

## Folder Structure

Each project gets its own memory at `.agent/nhallucinate/`:

```
/path/to/project/
├── .agent/
│   └── nhallucinate/
│       ├── memory.md            # Milestones, recent changes
│       ├── project-context.md   # Tech stack, file structure
│       ├── lessons-learned.md  # Mistakes to avoid
│       ├── current-task.md     # Active task state
│       └── history/            # Session snapshots
├── src/
└── ...
```

## Memory Files

| File | Purpose |
|------|---------|
| memory.md | Current state, milestones, recent changes |
| project-context.md | Tech stack, file structure, key files |
| lessons-learned.md | What didn't work in this project |
| current-task.md | Active task, approach, completed |

## Workflow

### Step 1: Detect Project

Identify project directory.

### Step 2: Read Memory

Read PROJECT's `.agent/nhallucinate/`:
1. memory.md - current state
2. current-task.md - active task
3. project-context.md - tech stack
4. lessons-learned.md - what to avoid

### Step 3: Before Working

Check for past solutions - look in lessons-learned.md first.

### Step 4: During Work

Create folder if needed:
```bash
mkdir -p .agent/nhallucinate/history
```
Document changes with timestamps.

### Step 5: On Failure

After 3 attempts:
1. Revert working changes
2. Write problem to memory.md
3. Add to lessons-learned.md
4. Notify user

### Step 6: Session End

Save milestones and changes to memory.md. Write snapshot to history/.

## Quick Reference

| Command | Description |
|---------|-------------|
| `/nhallucinate init` | Initialize folder in current project |
| `/nhallucinate reset` | Clear memory, regenerate |
| `/nhallucinate status` | Show project status |

## Commands

### /nhallucinate init

```bash
mkdir -p .agent/nhallucinate/history
touch .agent/nhallucinate/memory.md
touch .agent/nhallucinate/project-context.md
touch .agent/nhallucinate/lessons-learned.md
touch .agent/nhallucinate/current-task.md
```

### /nhallucinate reset

```bash
cp -r .agent/nhallucinate .agent/nhallucinate.bak
rm -rf .agent/nhallucinate
# Then run /nhallucinate init
```

### /nhallucinate status

```bash
echo "=== $(pwd) ==="
echo "--- memory.md ---"
cat .agent/nhallucinate/memory.md
echo "--- current-task.md ---"
cat .agent/nhallucinate/current-task.md
```

## Common Mistakes

| Mistake | Fix |
|--------|-----|
| Writing to global skill folder | Always write to PROJECT's .agent/ |
| Not checking past work | Look in lessons-learned.md first |
| Documenting too much | Keep summaries, link details |
| Forgetting to update memory | Update after each milestone |

## Optimizations

- **Time limit**: <30 seconds to read memory
- **Age limit**: Ignore changes >30 days old
- **Focus**: Structure, dependencies, recent only
- **Flag uncertainty**: Explicitly note when unsure