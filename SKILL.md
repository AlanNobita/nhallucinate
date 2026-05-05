---
name: nhallucinate
description: Use when writing code, fixing bugs, or making changes to any project - prevents hallucination by grounding in project-specific memory with pattern detection and signature verification
---

# nhallucinate Skill

An AI anti-hallucination skill using project-specific memory with automated pattern detection and signature verification.

## Problem It Solves

- AI forgets what was tried 30 minutes ago
- Repeats same broken solutions repeatedly
- Invents functions/APIs that don't exist
- Context window exhaustion in long sessions

## Core Features

### 1. Memory Persistence
Stores memory in external files to survive context window limits. Memory persists across sessions.

### 2. Pattern Detection
Detects when AI is stuck in repetitive error loops. Automatically suggests alternatives.

### 3. Signature Verification
Verifies function/method signatures exist before generating code. Rejects invented imports.

### 4. Context Management
Intelligently manages context with compression and prioritization.

---

## Folder Structure

Each project gets its own memory at `.agent/nhallucinate/`:

```
/path/to/project/
├── .agent/
│   └── nhallucinate/
│       ├── memory.md              # Milestones, recent changes
│       ├── project-context.md     # Tech stack, file structure
│       ├── lessons-learned.md     # Mistakes to avoid
│       ├── current-task.md        # Active task state
│       ├── error-patterns.json     # Detected error patterns
│       ├── function-signatures.json # Extracted signatures
│       └── history/               # Session snapshots
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
- One-off commands that don't modify code

---

## Memory Files

| File | Purpose |
|------|---------|
| memory.md | Project state, milestones, recent changes |
| project-context.md | Tech stack, file structure, key files |
| lessons-learned.md | What didn't work in this project |
| current-task.md | Current task, approach, completed |
| error-patterns.json | Detected repetitive error patterns |
| function-signatures.json | Extracted function/API signatures |

---

## Enhanced Workflow

### Step 1: Detect Project

Identify project directory. Load project-specific memory.

### Step 2: Read Memory

Read PROJECT's `.agent/nhallucinate/`:
1. memory.md - current state
2. current-task.md - active task
3. project-context.md - tech stack
4. lessons-learned.md - what to avoid

### Step 3: Pattern Detection

Before attempting fixes:
- Check error-patterns.json for repeated failures
- If same approach tried >2 times → flag as pattern
- Suggest alternative approach from lessons-learned.md

### Step 4: Signature Verification

Before generating code:
- Parse AST to extract function signatures from project
- Compare generated code against known signatures
- Reject invented imports/functions

### Step 5: Before Working

Check for past solutions - look in lessons-learned.md first.

### Step 6: During Work

- Create folder if needed
- Document changes with timestamps
- Track attempts in error-patterns.json

### Step 7: On Failure

After 3 attempts:
1. Detect pattern match (same approach)
2. Revert working changes
3. Write problem to memory.md
4. Add to lessons-learned.md
5. Suggest alternative from patterns

### Step 8: Session End

- Save milestones to memory.md
- Write snapshot to history/
- Update error-patterns.json
- Serialize persistent memory to files

---

## Pattern Detection Details

### How It Works

```
Attempt 1: Try solution A → Fails → Record in error-patterns.json
Attempt 2: Try solution A → Fails → Detect pattern → Alert!
Attempt 3: Try solution A → Blocked → Suggest alternative
```

### Error Pattern JSON

```json
{
  "patterns": [
    {
      "error": "login redirect loop",
      "approach": "reset password flow",
      "attempts": 3,
      "firstAttempt": "2026-05-05T10:00:00Z",
      "outcome": "failed"
    }
  ]
}
```

### Lessons Learned Integration

After pattern detected, add to lessons-learned.md:
```markdown
## Don't Do This
- Reset password flow (causes redirect loop)
- Use localStorage for tokens

## Solutions That Worked
- Check auth state before redirect
- Use Supabase session
```

---

## Signature Verification Details

### How It Works

Before generating code like `import { useAuth } from '@/lib/auth'`:
1. Parse AST of actual project files
2. Extract exported functions/methods
3. Compare against generated code
4. Reject if signature doesn't exist

### Function Signatures JSON

```json
{
  "signatures": {
    "@/lib/auth": {
      "useAuth": "() → AuthState",
      "signIn": "(email, password) → Promise<void>",
      "signOut": "() → void"
    }
  }
}
```

### Verification Process

```
AI generates: import { usePermissions } from '@/lib/auth'
Check signatures.json: usePermissions NOT FOUND
Reject: "usePermissions does not exist. Available: useAuth, signIn, signOut"
```

---

## Context Management Details

### When Context Fills

Monitor context usage. At 70% threshold:
1. Prioritize: recent actions → project patterns → general rules
2. Compress less important context
3. Serialize to external files
4. Free context space

### Memory Optimization

- **Time limit**: <30 seconds to read memory
- **Age limit**: Ignore changes >30 days old
- **Focus**: Recent changes, dependencies, structure
- **Flag uncertainty**: Explicitly note when unsure

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
echo "--- memory.md ---"
cat .agent/nhallucinate/memory.md
echo "--- error-patterns.json ---"
cat .agent/nhallucinate/error-patterns.json
```

### /nhallucinate extract-signatures

Parse project files to extract function signatures:
```bash
# Use AST parser to extract signatures
# Write to function-signatures.json
```

---

## Quick Reference

| Command | Description |
|---------|-------------|
| `/nhallucinate init` | Initialize folder in current project |
| `/nhallucinate reset` | Clear memory, regenerate |
| `/nhallucinate status` | Show project status |
| `/nhallucinate extract-signatures` | Extract function signatures |

| Feature | Trigger |
|---------|---------|
| Pattern detection | Same approach >2 failures |
| Signature verification | Before generating imports |
| Context management | At 70% context usage |

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Writing to global skill folder | Always write to PROJECT's .agent/ |
| Not checking past work | Look in lessons-learned.md first |
| Not tracking patterns | Update error-patterns.json on failure |
| Not verifying signatures | Extract signatures before coding |

---

## Best Practices

1. **Update memory after each milestone** - Don't wait until session end
2. **Check lessons-learned.md first** - Before trying any solution
3. **Track error patterns** - Document failures immediately
4. **Extract signatures** - When joining a project
5. **Use timestamps** - Always include dates

---

## Summary

| Component | Problem Solved | Mechanism |
|-----------|---------------|-----------|
| Memory Persistence | Forgets context | External file storage |
| Pattern Detection | Repeats errors | Similarity matching |
| Signature Verification | Invents functions | AST parsing |
| Context Management | Context overflow | Intelligent compression |

**Core Principle**: What AI learns in Project A stays in Project A. Each project has its own memory.