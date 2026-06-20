---
name: plan-executor
description: Orchestrates execution of plans from using subagents in series. Always delegate tasks to subagents, invoke this skill when user asks to implement a plan verbs("implement", "implemente", "implante", "execute plan", "execute plano").
---

# Plan Executor - Instructions for AI Code Agent

**Use this skill when executing a technical plan from `.plans/`.**

## Core Rules

- **ALWAYS use pi-subagents**: Delegate every task to a subagent. The main agent **NEVER** writes code during plan execution.
- **SERIES ONLY**: Never parallelize. One agent at a time. `run_in_background: false`.
- **Autonomous subagent**: Each subagent implements, tests, lints, updates the plan, and commits — all by itself.
- **Clean context**: The parent only orchestrates (selects task, delegates, verifies result).

## Main Agent — Loop

```
1. Read → Load the plan from .plans/PLANO-<FEATURE>.md
2. Select → Find the FIRST unchecked task `- [ ]` whose dependencies are all `[x]`
3. Delegate → Agent({ prompt: ..., subagent_type: "general-purpose", run_in_background: false })
4. Wait → Wait for return (foreground blocks automatically)
5. Verify → Tests passed? Lint clean? Plan updated? Commit made?
   - Se o subagente NÃO marcou a tarefa, atualize `- [ ]` → `- [x]` manualmente
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
4. Update plan → mark THIS task as `[x]` using the `edit` tool — faça isso AGORA, antes de qualquer commit.
5. Commit → `git add . && git commit -m "feat: Task X.Y — description"`

⚠️ IMPORTANTE: Você deve marcar sua tarefa como `[x]` no plano `.plans/` antes de fazer o commit. Cada subagente é responsável por atualizar o checklist do plano imediatamente após completar sua tarefa — nunca deixe isso para o main agent ou para o final.

## Return: files created/modified, test/lint status, plan and commit confirmation.
```

### Delegation rules

- ❌ Never multiple `Agent` calls in the same message
- ❌ Never `run_in_background: true` or `steer_subagent`
- ❌ Never write code directly during execution
- ✅ One task at a time, wait for return before the next

## Failures

- **Test fails** → re-delegate to subagent with fix instructions. Never advance.
- **Lint fails** → re-delegate fix to subagent. Never advance.
- **Vague task** → revise the plan, re-delegate with corrected instructions.

## Completion Checklist

Before declaring the plan complete:
- [ ] All tasks marked `[x]` in the plan
- [ ] `git log` shows commits for all tasks
- [ ] Zero lint errors, zero failing tests
