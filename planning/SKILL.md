---
name: planning
description: Triggers whenever creating plans. Use for feature planning, task breakdown, and orchestrating multi-step implementations using subagents. Trigger words (plan, plano, planeje).
---

# 📋 Planning Skill

## 🚀 Trigger
Activate this skill whenever:
- You need to create a technical plan for a feature or fix.
- You are executing a plan defined in `.plans/`.
- You need to break down a complex request into smaller, implementable steps.

## 📂 Storage
All plans must be saved in the `.plans/` directory.
- Naming convention: `DD-MM-YYYY-PLAN-<FEATURE>.md`.

## 🧠 Core Principles

1. **Break into small tasks (Atomic Tasks)**: Break large features into small, autonomous tasks that a subagent can execute in isolation (e.g., "Create DB migration", "Update API route", "Fix component").
2. **Visual clarity**: Always include a visual diagram (Mermaid) explaining the architecture or flow.
3. **Traceability**: Maintain a clear `Todo List` with checkboxes to track status.
4. **Isolation**: Tasks must be independent enough to be worked on in parallel or sequence without unnecessary blocking.
5. **Stress-test before execute**: Never ship a plan without first grill-me'ing it with the user.

## 📐 Plan Structure

Every plan in `.plans/` must follow this structure:

1. **Title and Objective**: What are we building?
2. **Visual Diagram**: Mermaid diagram of the architecture/flow.
3. **Todo List**: List of atomic tasks with checkboxes (e.g., `- [ ] Task 1`).
4. **Task Details**: Specific instructions for each task (files to create/modify, logic to implement, acceptance criteria, and immediate unit test requirements for components/hooks).
5. **Execution Order**: Recommended sequence (e.g., `1.1 -> 1.2 -> 2.1`).
6. **Visual Debug**: Final validation step with screenshots (see 📸 Visual Debug Phase below).

## 🔥 Grill Phase (During Planning)

While designing the plan (before saving it), **activate the grill-me skill** to stress-test the plan with the user. Read its SKILL.md for instructions. Refine the plan based on the grill before writing to `.plans/`.

## 📝 Task Format in the Plan

For each task in the plan, include:

```markdown
#### **Task X.Y** — Brief description

**File(s)**: `path/to/file.ts` (new or modified)

**What to do**:
1. Step 1...
2. Step 2...

**Acceptance criteria**:
- Criterion 1...
- Criterion 2...
```

## 📸 Visual Debug Phase

**ALWAYS** add a final task in the plan for visual validation:

```
#### **Task N.N** — Visual debug validation

**What to do**:
1. Run the `visual-debug` skill to capture screenshots
2. Validate layouts, responsiveness, and component states
3. Save screenshots to `tmp/` directory
4. Update Todo List with visual validation status

**Command**:
```
/skill:visual-debug
```

**Acceptance criteria**:
- [ ] Screenshot captured and saved to `tmp/`
- [ ] Layout validated (desktop + mobile)
- [ ] User can view and approve visually
```

## ⚠️ Execution Rules

- **Always** run unit tests after each task.
- **Always** run linting before committing.
- **Always** update the `Todo List` in the plan file after completing a task.
- **Always** include a visual debug task at the end of every plan.
- **Never** skip tasks even if they seem obvious.
- **Ensure** that the subagent commits the changes before returning to the main loop.
- **Always** execute `/skill:visual-debug` after all implementation tasks are complete.
