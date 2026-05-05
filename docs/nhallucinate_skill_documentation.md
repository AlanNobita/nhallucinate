# nhallucinate Skill - Complete Documentation

## Overview

**nhallucinate** is an AI coding assistant skill designed to prevent hallucination by grounding the AI in project-specific memory. It ensures the AI remembers what was done in previous sessions, what worked, what didn't, and the project's unique structure.

**Core Principle**: Every project has its own memory. What the AI learns in Project A stays in Project A.

---

## Problem It Solves

### Hallucination in AI Coding

When working on long coding sessions or complex projects, AI assistants often:

1. **Forget previous work** - Can't remember what was tried 30 minutes ago
2. **Repeat failed approaches** - Try the same broken solution again
3. **Invent code** - Create functions or APIs that don't exist
4. **Lose context** - Get confused when switching between tasks

### Example of the Problem

```
User: Fix the login bug
AI: *tries solution A* - FAILS
User: (30 min later) Find the bug
AI: *tries solution A again* - FAILS
AI: *tries solution B* - FAILS
AI: *tries solution A again* - FAILS
```

The AI forgot it already tried solution A. **nhallucinate** solves this.

---

## How It Works

### Project-Based Memory

Each project gets its own memory folder:

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

**Key point**: The memory is stored in the PROJECT, not in a global location. This makes it portable (it travels with git) and project-specific.

---

## Memory Files Explained

### 1. memory.md

**Purpose**: Current project state, milestones, and recent changes

**Content Example**:
```markdown
# Project Memory - my-app

## Last Session (2026-05-05)
- Added user authentication via Supabase
- Migrated from PostgreSQL to Supabase
- Fixed login redirect bug

## Milestones
- [x] Set up React + Supabase
- [x] User login system
- [x] Dashboard with charts
- [ ] Payment integration

## Recent Changes
- 2026-05-05: Added auth context
- 2026-05-04: Created login form
```

### 2. project-context.md

**Purpose**: Tech stack, file structure, key files

**Content Example**:
```markdown
# Project Context - my-app

## Tech Stack
- Frontend: React 18 + TypeScript
- Backend: Supabase (PostgreSQL)
- Auth: Supabase Auth
- Styling: Tailwind CSS
- State: Zustand

## Key Files
- /src/app/auth/* - Authentication
- /src/lib/supabase.ts - Supabase client
- /src/components/ui/* - UI components

## Important Patterns
- All API calls go through /src/lib/
- Auth state in useAuth hook
- Components use shadcn/ui
```

### 3. lessons-learned.md

**Purpose**: What didn't work in this project (IMPORTANT)

**Content Example**:
```markdown
# Lessons Learned - my-app

## Don't Do This
- Don't use localStorage for tokens (use Supabase session)
- Don't put auth logic in components (use context)
- Don't use MongoDB (stick to Supabase)

## Past Problems
- 2026-05-05: Login redirect loop - needed to check auth state first
- 2026-05-04: Database timeout - added connection pooling
- 2026-05-03: API rate limit - added caching

## Solutions That Worked
- Auth: useSupabase hook + auth context
- Caching: React Query with staleTime: 5000
- Forms: react-hook-form + zod validation
```

### 4. current-task.md

**Purpose**: Active task, approach, what's completed

**Content Example**:
```markdown
# Current Task - my-app

## Active Task
Fix payment processing bug

## Approach
1. Check error logs in dashboard
2. Verify Stripe webhook
3. Test with test cards

## Completed
- [x] Reproduced bug with test card

## Remaining
- [ ] Find root cause
- [ ] Implement fix
- [ ] Test fix
```

---

## Workflow

### Step-by-Step Process

```
1. DETECT PROJECT
   └─> Identify current project directory

2. READ MEMORY
   └─> Read .agent/nhallucinate/ files:
       ├─ memory.md
       ├─ current-task.md
       ├─ project-context.md
       └─ lessons-learned.md

3. BEFORE WORKING
   └─> Check lessons-learned.md first!
   └─> Review current-task.md

4. DURING WORK
   └─> Document changes with timestamps
   └─> Update current-task.md

5. ON FAILURE
   └─> After 3 failed attempts:
       ├─ Revert changes
       ├─ Write to memory.md
       ├─ Add to lessons-learned.md
       └─ Notify user

6. SESSION END
   └─> Save to memory.md
   └─> Write snapshot to history/
```

---

## Real Use Cases

### Use Case 1: Long Bug Fix

**Scenario**: User is fixing a complex bug that takes multiple sessions

**Session 1**:
```
AI: Working on login bug...
AI: *tries reset password flow*
AI: *documents in current-task.md*
AI: Session ends, saves to memory.md
```

**Session 2** (next day):
```
AI: Reads memory.md - "Session 1 tried reset password flow"
AI: Reads lessons-learned.md
AI: *tries different approach - session token*
AI: Success! Updates memory.md
```

### Use Case 2: Switching Between Projects

**Scenario**: User works on Project A, then switches to Project B

**Project A**:
```
AI: Working on API development
AI: Uses project-context.md for tech stack
AI: Writes to .agent/nhallucinate/ (Project A)
```

**Project B** (different project):
```
AI: Different project detected
AI: Reads Project B's memory
AI: Different tech stack - Next.js not React
AI: Writes to .agent/nhallucinate/ (Project B)
```

### Use Case 3: Avoiding Failed Approaches

**Scenario**: AI tries something that failed before

```
AI: Going to use localStorage for token storage...
AI: *checks lessons-learned.md*
AI: Found: "Don't use localStorage for tokens"
AI: Uses Supabase session instead
```

### Use Case 4: Feature Development

**Scenario**: Building new feature with understanding of existing code

```
AI: Reading project-context.md...
AI: Found: "All API calls go through /src/lib/"
AI: Adds new API function to /src/lib/api.ts
AI: Updates project-context.md
```

---

## Commands

### /nhallucinate init

Initialize the memory folder in the current project:

```bash
mkdir -p .agent/nhallucinate/history
touch .agent/nhallucinate/memory.md
touch .agent/nhallucinate/project-context.md
touch .agent/nhallucinate/lessons-learned.md
touch .agent/nhallucinate/current-task.md
```

### /nhallucinate reset

Clear memory and regenerate (keeps backup):

```bash
cp -r .agent/nhallucinate .agent/nhallucinate.bak
rm -rf .agent/nhallucinate
# Then run /nhallucinate init
```

### /nhallucinate status

Show current project status:

```bash
echo "=== $(pwd) ==="
echo "--- memory.md ---"
cat .agent/nhallucinate/memory.md
echo "--- current-task.md ---"
cat .agent/nhallucinate/current-task.md
```

---

## Best Practices

### 1. Update Memory After Each Milestone

Don't wait until end of session. After each significant change:

```markdown
## Session Log
- 2026-05-05 14:30: Added user authentication
- 2026-05-05 15:00: Fixed auth redirect
```

### 2. Check lessons-learned.md First

Before trying any solution, check what failed before:

```markdown
## Past Problems
- DON'T: Use localStorage for tokens
- DON'T: Put auth in components
```

### 3. Keep Summaries, Link Details

Don't copy everything. Summarize + link to original:

```markdown
## Recent Changes
- Refactored API - see history/session-2026-05-05.md
```

### 4. Use Timestamps

Always include dates for context:

```markdown
- 2026-05-05: Added Supabase auth
- 2026-05-04: Fixed query timeout
```

### 5. Document Failures Quickly

When something fails, note it immediately:

```markdown
## Failed Approaches
- Tried JWT tokens - expired too quickly
- Solution: Use Supabase session refresh
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Writing to global skill folder | Always write to PROJECT's .agent/nhallucinate/ |
| Not checking past work | Look in lessons-learned.md first |
| Documenting too much | Keep summaries, link details |
| Forgetting to update memory | Update after each milestone |
| Skipping initialization | Run /nhallucinate init first time |

---

## Optimization Guidelines

- **Time limit**: <30 seconds to read memory
- **Age limit**: Ignore changes older than 30 days
- **Focus**: Recent changes, dependencies, structure
- **Flag uncertainty**: Explicitly note when unsure

---

## File Structure Summary

```
PROJECT/
├── .agent/
│   └── nhallucinate/
│       ├── memory.md           ← Project state & milestones
│       ├── project-context.md  ← Tech stack & files
│       ├── lessons-learned.md   ← What NOT to do
│       ├── current-task.md     ← Active task
│       └── history/           ← Session snapshots
├── src/
└── package.json
```

---

## Integration with Other Skills

- **cavecrew**: Use when pattern detection finds repeated errors
- **debugging**: Feed detected patterns for systematic debugging
- **brainstorming**: Consider before implementing major changes

---

## Summary

| Aspect | Details |
|--------|---------|
| **Type** | Project-based (not global) |
| **Location** | `PROJECT_ROOT/.agent/nhallucinate/` |
| **Files** | 4 main + history folder |
| **Key Benefit** | Prevents repeated failures |
| **Memory** | Persists across sessions |

**Remember**: What AI learns in Project A stays in Project A. Each project has its own memory.