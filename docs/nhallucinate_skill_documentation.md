# nhallucinate Skill - Complete Documentation

## Overview

**nhallucinate** is an AI coding assistant skill designed to prevent hallucination with project-specific memory, pattern detection, and signature verification.

**Core Principle**: Every project has its own memory. What the AI learns in Project A stays in Project A.

---

## Problem It Solves

### Hallucination in AI Coding

When working on long coding sessions or complex projects, AI assistants often:

1. **Forget previous work** - Can't remember what was tried 30 minutes ago
2. **Repeat failed approaches** - Try the same broken solution again
3. **Invent code** - Create functions or APIs that don't exist
4. **Lose context** - Get confused when switching tasks

### Example of the Problem

```
User: Fix the login bug
AI: *tries solution A* - FAILS
User: (30 min later) Find the bug
AI: *tries solution A again* - FAILS
```

nhallucinate solves this with 4 core features.

---

## Core Features

### 1. Memory Persistence
External file storage survives context window limits. Memory persists across sessions.

### 2. Pattern Detection
Detects when AI is stuck in repetitive error loops. Automatically suggests alternatives.

### 3. Signature Verification
Verifies function/method signatures exist before generating code. Rejects invented imports.

### 4. Context Management
Intelligently manages context with compression when window fills.

---

## How It Works

### Project-Based Memory

Each project gets its own memory folder:

```
/path/to/project/
├── .agent/
│   └── nhallucinate/
│       ├── memory.md              # Milestones, recent changes
│       ├── project-context.md       # Tech stack, file structure
│       ├── lessons-learned.md      # Mistakes to avoid
│       ├── current-task.md         # Active task state
│       ├── error-patterns.json     # Detected error patterns
│       ├── function-signatures.json # Extracted signatures
│       └── history/                # Session snapshots
├── src/
└── ...
```

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

## Milestones
- [x] Set up React + Supabase
- [x] User login system
```

### 2. project-context.md

**Purpose**: Tech stack, file structure, key files

**Content Example**:
```markdown
# Project Context

## Tech Stack
- Frontend: React 18 + TypeScript
- Backend: Supabase

## Key Files
- /src/lib/auth.ts
- /src/components/ui/
```

### 3. lessons-learned.md

**Purpose**: What didn't work in this project

**Content Example**:
```markdown
# Lessons Learned

## Don't Do This
- Don't use localStorage for tokens
- Don't put auth in components

## Past Problems
- 2026-05-05: Login redirect loop
```

### 4. current-task.md

**Purpose**: Active task, approach, what's completed

**Content Example**:
```markdown
# Current Task

## Active Task
Fix payment processing bug

## Approach
1. Check error logs
2. Verify Stripe webhook

## Completed
- [x] Reproduced bug
```

### 5. error-patterns.json

**Purpose**: Track repeated error patterns

**Content Example**:
```json
{
  "patterns": [
    {
      "error": "login redirect loop",
      "approach": "reset password flow",
      "attempts": 3,
      "outcome": "failed"
    }
  ]
}
```

### 6. function-signatures.json

**Purpose**: Project function signatures

**Content Example**:
```json
{
  "signatures": {
    "@/lib/auth": {
      "useAuth": "() → AuthState",
      "signIn": "(email, password) → Promise<void>"
    }
  }
}
```

---

## Enhanced Workflow

```
1. DETECT PROJECT
   └─> Identify project directory

2. READ MEMORY
   └─> Read .agent/nhallucinate/ files

3. PATTERN DETECTION
   └─> Check error-patterns.json for repeats
   └─> If same approach >2 times → flag it

4. SIGNATURE VERIFICATION
   └─> Parse AST to extract signatures
   └─> Compare generated code against known
   └─> Reject invented imports

5. BEFORE WORKING
   └─> Check lessons-learned.md first

6. DURING WORK
   └─> Document changes with timestamps

7. ON FAILURE
   └─> After 3 failures → pattern detected
   └─> Suggest alternative

8. SESSION END
   └─> Save to memory.md
   └─> Write to history/
```

---

## Pattern Detection

### How It Works

```
Attempt 1: Try solution A → Fails → Record pattern
Attempt 2: Try solution A → Fails → Detect pattern!
Attempt 3: Try solution A → Blocked → Suggest alternative
```

### Detection Triggers

- Same approach tried >2 times
- Same error message repeated
- >3 attempts within 10 minutes

### After Detection

Add to lessons-learned.md:
```markdown
## Don't Do This
- Reset password flow (causes redirect loop)

## Solutions That Worked
- Check auth state before redirect
```

---

## Signature Verification

### How It Works

Before generating code like `import { useAuth } from '@/lib/auth'`:

1. Parse AST of actual project files
2. Extract exported functions/methods
3. Compare against generated code
4. Reject if signature doesn't exist

### Example

```
AI generates: import { usePermissions } from '@/lib/auth'
Check signatures.json: usePermissions NOT FOUND
Reject: "usePermissions does not exist. Available: useAuth, signIn"
```

### Verification Process

- Function declarations parsed from source
- Imported/exported functions tracked
- Generated calls compared against known
- Clear error if invented

---

## Context Management

### When Context Fills

At 70% context usage:

1. **Prioritize**:
   - Recent actions (highest)
   - Project patterns
   - General rules (lowest)

2. **Compress**:
   - Remove verbose details
   - Keep summaries

3. **Serialize**:
   - Write to external files
   - Free context space

### Memory Optimization

- **Time limit**: <30 seconds to read memory
- **Age limit**: Ignore changes >30 days old
- **Focus**: Recent changes, dependencies
- **Flag uncertainty**: Note when unsure

---

## Real Use Cases

### Use Case 1: Long Bug Fix

**Scenario**: Fixing a bug across multiple sessions

```
Session 1:
- Tries reset password flow - FAILS
- Documents in error-patterns.json
- Session ends

Session 2:
- Pattern detected from previous attempts
- Tries different approach - SUCCESS
```

### Use Case 2: Avoiding Failed Approaches

**Scenario**: AI tries something that failed before

```
AI: Going to use localStorage for tokens...
AI: *checks lessons-learned.md*
AI: Found: "Don't use localStorage"
AI: Uses Supabase session instead
```

### Use Case 3: Signature Mismatch

**Scenario**: AI generates wrong import

```
AI: import { usePermissions } from '@/lib/auth'
AI: *checks function-signatures.json*
AI: usePermissions NOT FOUND
AI: "Available: useAuth, signIn, signOut"
```

### Use Case 4: Context Exhaustion

**Scenario**: Long session, context filling

```
Context at 70%:
- Serialize to memory files
- Compress to summaries
- Clear unused context
- Continue working
```

---

## Commands

### /nhallucinate init

```bash
mkdir -p .agent/nhallucinate/history
touch .agent/nhallucinate/memory.md
touch .agent/nhallucinate/project-context.md
touch .agent/nhallucinate/lessons-learned.md
touch .agent/nhallucinate/current-task.md
touch .agent/nhallucinate/error-patterns.json
touch .agent/nhallucinate/function-signatures.json
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
cat .agent/nhallucinate/memory.md
cat .agent/nhallucinate/error-patterns.json
```

### /nhallucinate extract-signatures

```bash
# Parse project AST
# Write to function-signatures.json
```

---

## Best Practices

1. **Update memory after each milestone** - Don't wait until session end

2. **Check lessons-learned.md first** - Before trying any solution

3. **Track error patterns** - Document failures immediately

4. **Extract signatures** - When joining a project

5. **Use timestamps** - Always include dates

6. **Flag uncertainty** - Explicitly note when unsure

---

## Common Mistakes

| Mistake | Fix |
|--------|-----|
| Writing to global folder | Always use PROJECT .agent/ |
| Not checking patterns | Check error-patterns.json |
| Not verifying signatures | Extract first |
| Forgetting to update | Update each milestone |

---

## Summary

| Component | Problem Solved | Mechanism |
|-----------|---------------|-----------|
| Memory Persistence | Forgets context | External files |
| Pattern Detection | Repeats errors | Match patterns |
| Signature Verification | Invents functions | AST parsing |
| Context Management | Context overflow | Compression |

**Remember**: What AI learns in Project A stays in Project A.

---

## Links

- **GitHub Repo**: https://github.com/AlanNobita/nhallucinate
- **Skill File**: SKILL.md
- **Template**: .agent/nhallucinate/