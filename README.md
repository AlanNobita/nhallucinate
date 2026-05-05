# nhallucinate

An OpenCode/Claude Code skill that prevents AI hallucination with project-specific memory.

## What It Does

- **Remembers** what worked and what didn't across sessions
- **Tracks** failed approaches (manual)
- **Grounds** the AI in your project

Each project gets its own memory folder.

## Problem It Solves

- AI forgets what was tried 30 minutes ago
- Repeats same broken solutions
- Context window exhaustion

## Core Features

1. **Memory Persistence** - External files survive context
2. **Manual Tracking** - Check lessons-learned.md yourself
3. **Simple** - No auto-parsing needed

## Quick Start

```bash
# Copy skill to your skills folder
cp SKILL.md ~/.agents/skills/nhallucinate/SKILL.md

# Initialize project memory
mkdir -p .agent/nhallucinate
touch .agent/nhallucinate/memory.md
touch .agent/nhallucinate/project-context.md
touch .agent/nhallucinate/lessons-learned.md
touch .agent/nhallucinate/current-task.md
```

## Folder Structure

```
project/
└── .agent/
    └── nhallucinate/
        ├── memory.md
        ├── project-context.md
        ├── lessons-learned.md
        └── current-task.md
```

## Note

Use **graphify** skill to understand file structure. nhallucinate remembers context.