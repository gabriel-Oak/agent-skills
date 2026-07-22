---
name: plan-executor
description: Orchestrates execution of plans by delegating tasks to subagents. Always delegate tasks to subagents, invoke this skill when user asks to implement a plan verbs("implement", "implemente", "implante", "execute plan", "execute plano").
---

# Plan Executor - Instructions for AI Code Agent

**Use this skill when executing a technical plan from `.plans/`.**

## Core Rules

- **ALWAYS use pi-subagents**: Delegate every task to a subagent. The main agent **NEVER** writes code during plan execution.
- **Autonomous subagent**: Each subagent works independently — implement, test, lint, update the plan, and commit.

- **Clean context**: The parent only orchestrates (selects task, delegates, verifies result).

## Main Agent — Loop

```
1. Read → Load the plan from .plans/DD-MM-YYYY-PLAN-<FEATURE>.md
2. Select → Find the NEXT unchecked task `- [ ]` whose dependencies are all `[x]` (or batch multiple)
3. Delegate → Agent({ prompt: ..., subagent_type: "general-purpose", run_in_background: false })
4. Wait → Wait for return (foreground blocks automatically)
5. Verify → Tests passed? Lint clean? Plan updated? Commit made?
   - If subagent didn't mark the task, update `- [ ]` → `- [x]` manually
6. Repeat → Go back to step 2 until all tasks are `[x]`
```

### When delegating (subagent prompt)

Copy the full task instructions from the plan and include:

```markdown
## Context: Next.js 15 + React 19 + TS. Path alias @/* → ./src/*. Dark theme, no external UI libs.

## Task: [copy full instructions from the plan]

## Required flow:
1. Implement code (follow existing design system, reuse when possible)
2. Tests → `npm test`. Create test file if new component/hook. Fix if failed.
3. Lint → `npm run lint -- --fix`. Fix all errors.
4. Update plan → mark THIS task as `[x]` using the `edit` tool — do this NOW, before any commit.
5. Commit → `git add . && git commit -m "feat: Task X.Y — description"`

⚠️ IMPORTANT: You must mark your task as `[x]` in the `.plans/` plan before committing. Each subagent is responsible for updating the plan checklist immediately after completing its task.

## Return: files created/modified, test/lint status, plan and commit confirmation.
```

### Delegation rules

- ❌ Never write code directly during execution
- ✅ Wait for each subagent to return before delegating the next task

## Failures

- **Test fails** → re-delegate to subagent with fix instructions. Never advance.
- **Lint fails** → re-delegate fix to subagent. Never advance.
- **Vague task** → revise the plan, re-delegate with corrected instructions.

## Completion Checklist

Before declaring the plan complete:
- [ ] All tasks marked `[x]` in the plan
- [ ] `git log` shows commits for all tasks
- [ ] Zero lint errors, zero failing tests
