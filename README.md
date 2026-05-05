# nhallucinate

An OpenCode/Claude Code skill that prevents AI hallucination with project-specific memory, pattern detection, and signature verification.

## What It Does

- **Remembers** what worked and what didn't across coding sessions
- **Prevents** repeated failed approaches (pattern detection)
- **Verifies** function signatures before generating code
- **Grounds** the AI in your project's unique context

Each project gets its own memory folder - what the AI learns in Project A stays in Project A.

## Problem It Solves

AI coding assistants often:
- Forget what was tried 30 minutes ago
- Repeat the same broken solutions
- Invent functions/APIs that don't exist
- Lose context mid-session (context window limits)

nhallucinate fixes this with:

1. **Memory Persistence** - External file storage survives context window
2. **Pattern Detection** - Detects repetitive error loops
3. **Signature Verification** - Validates functions exist in project
4. **Context Management** - Intelligent compression when context fills

## Problem It Solves

AI coding assistants often:
- Forget what was tried 30 minutes ago
- Repeat the same broken solutions
- Lose context mid-session

nhallucinate fixes this with persistent project memory.

## Quick Start

### 1. Copy the Skill

```bash
# Copy SKILL.md to your skills folder
cp SKILL.md ~/.agents/skills/nhallucinate/SKILL.md
```

### 2. Initialize Project Memory

In your project directory:
```bash
mkdir -p .agent/nhallucinate
touch .agent/nhallucinate/memory.md
touch .agent/nhallucinate/project-context.md
touch .agent/nhallucinate/lessons-learned.md
touch .agent/nhallucinate/current-task.md
```

### 3. Use the Skill

When working on code tasks, invoke the nhallucinate skill. It will read memory from `.agent/nhallucinate/` before starting work.

## Folder Structure

```
project/
└── .agent/
    └── nhallucinate/
        ├── memory.md           # Milestones, recent changes
        ├── project-context.md  # Tech stack, key files
        ├── lessons-learned.md   # What NOT to do
        └── current-task.md      # Active task state
```

## Memory Files

| File | Purpose |
|------|---------|
| memory.md | Project state, milestones, recent changes |
| project-context.md | Tech stack, file structure, key files |
| lessons-learned.md | What failed before (avoid!) |
| current-task.md | Current task, progress |

## Commands

- `/nhallucinate init` - Initialize memory folder
- `/nhallucinate status` - Show project status
- `/nhallucinate reset` - Clear and regenerate memory

## Example

See `docs/nhallucinate_skill_documentation.md` for complete documentation with examples.

## Credits

Created by Alan | Feel free to fork and adapt

---

If this helps you, star the repo!