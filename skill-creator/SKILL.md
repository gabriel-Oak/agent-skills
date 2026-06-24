---
name: skill-creator
description: "Creates new pi-agent skills following established conventions. Trigger words: criar skill, criar habilidade, criar nova skill, criar nova habilidade, create skill, create new skill, new skill."
---

# Skill Creator Skill

## When to Use

Activate this skill whenever the user asks to create a new pi-agent skill, new skill, or new agent skill/habilidade.

## Skill Anatomy

Every skill follows a strict file format. The skill file lives at:

```
/home/gabs/.pi/agent/skills/<skill-name>/SKILL.md
```

### 1. YAML Frontmatter

Every skill starts with YAML frontmatter between `---` delimiters:

```yaml
---
name: <slug-name>
description: <Short, direct description of what the skill does. Trigger words: <word1>, <word2>, ...>
---
```

**Rules for frontmatter:**
- `name`: lowercase, hyphen-separated slug (e.g., `nextjs-setup`, `gh`, `plan-executor`)
- `description`: one short sentence describing the skill's purpose, followed by `Trigger words:` and a comma-separated list of trigger words in both Portuguese and English
- Keep the description **short and direct** — no more than one line
- Trigger words should cover common phrasings in Portuguese and English

**Examples of good descriptions:**

```yaml
# Planning skill
description: Triggers whenever creating plans. Use for feature planning, task breakdown, and orchestrating multi-step implementations using subagents. Trigger words (plan, plano, planeje).

# GitHub CLI skill
description: Patterns for invoking the GitHub CLI (gh) from agents. Covers structured output, pagination, repo targeting, search vs list, gh api fallback.

# Next.js setup skill
description: Sets up a complete Next.js project with TypeScript, Vitest, Playwright, Tailwind CSS, Husky, commitlint, and GitHub Actions CI. Trigger words: criar projeto next, criar projeto nextjs, novo projeto next, nextjs starter, next project setup.
```

### 2. Title

After the frontmatter, start with an H1 title:

```markdown
# <Skill Name> Skill
```

Or with an emoji prefix if the skill has a strong identity:

```markdown
# 📋 Planning Skill
# ⚠️ Git Guidelines - Reference for AI Agent
```

### 3. Section Structure

Choose your sections based on the skill's purpose:

#### For Reference Skills (like `gh`)

These skills document how to use a tool or library. Structure:

```markdown
## <Topic 1>
<Detailed content, code examples, edge cases>

## <Topic 2>
<More sections as needed>
```

No fixed "When to Use" or "Completion Criteria" needed — the skill is a reference.

#### For Process/Setup Skills (like `nextjs-setup`, `planning`)

These skills guide the agent through a process. Structure:

```markdown
## When to Use
<When the agent should activate this skill>

## <Core Concept / Stack / Principles>
<Foundational information the agent needs>

## <Procedure / Steps / Structure>
<Step-by-step instructions or templates>

## Completion Criteria
<Checklist of what must be true before the skill's task is done>

## Important Notes
<Caveats, rules, gotchas — use "Never" and "Always" for emphasis>
```

#### For Guideline Skills (like `git-guidelines`)

These skills enforce rules and constraints. Structure:

```markdown
## ⛔ Regras Absolutas (Never do)
<Absolute rules that must never be violated>

## ⚠️ Regras Condicionais (Ask before)
<Rules that require user consent>

## ✅ Boas Práticas (Good practices)
<Recommended behaviors>

## 🔄 Fluxo Recomendado
<Step-by-step workflow>
```

#### For Execution/Orchestration Skills (like `plan-executor`)

These skills tell the agent how to orchestrate other agents. Structure:

```markdown
## Core Rules
<Rules the main agent must follow>

## <Main Agent Loop / Workflow>
<Step-by-step orchestration pattern>

### <Subsection>
<Detailed instructions for specific scenarios>

## Failures
<How to handle errors>

## Completion Checklist
<Checklist to verify before declaring done>
```

### 4. Code Blocks

Use code blocks for:
- **Shell commands**: `bash`
- **Config files**: match the file type (`ts`, `js`, `json`, `yaml`, `sh`)
- **Code examples**: match the language (`ts`, `js`, `tsx`)
- **Template content**: use the appropriate language

```bash
# Shell commands
npm install -D <package>

npx <command> <args>
```

```ts
// TypeScript config
import { defineConfig } from '<package>';
```

```json
// JSON config
{
  "key": "value"
}
```

```yaml
# YAML config
key: value
```

### 5. Completion Criteria

Use a checkbox list for completion criteria. Every process/setup skill should have one:

```markdown
## Completion Criteria

The project is **ready for development** only when ALL of the following are true:

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3
```

Use strong language for emphasis:
- **Never** skip any step
- **Always** verify before declaring done
- **SEMPRE** (Portuguese emphasis) for critical rules

### 6. Important Notes / Rules

Use bullet points with strong verbs:

```markdown
## Important Notes

- **Never** skip verification steps
- **Never** disable or bypass hooks
- **Always** check the latest version before creating
- **Always** use the project root for config files
```

For rules that must never be violated, use:
- `### ⛔ <Rule Name>` with sub-bullets explaining what not to do
- `**Sob hipótese alguma**` (Portuguese: under no circumstances) for absolute rules
- `**NUNCA**` for emphasis in Portuguese sections

### 7. Emojis

Use emojis **consistently** within a skill:

| Emoji | Meaning |
|-------|---------|
| 🚀 | Trigger / activation |
| 📋 | Planning / checklist |
| 📂 | Storage / file location |
| 🧠 | Core principles / reasoning |
| 📐 | Structure / format |
| 📝 | Writing / templates |
| ⚠️ | Warnings / important rules |
| ✅ | Good practices / requirements |
| 🔄 | Workflow / flow |
| 🔒 | Security / safety |
| 🚫 | Prohibited actions |
| 🔍 | Investigation / search |

Don't mix emoji-heavy and emoji-free styles within the same skill. Pick one approach.

### 8. Language

- **Description field**: trigger words in both Portuguese and English
- **Content**: English is preferred (like `gh`, `plan-executor`, `nextjs-setup`)
- **Guideline skills** can be in Portuguese if they're meant for a PT-speaking user (like `git-guidelines`)
- **Never** mix languages within the same section — pick one language per skill

### 9. File Paths

When referencing files:
- Use backtick-quoted paths: `path/to/file.ts`
- For project-relative paths: `src/test/setup.ts`, `.github/workflows/ci.yml`
- For skill directory references: use relative paths resolved to the skill directory

### 10. Tables

Use tables for quick reference information:

```markdown
| Tool | Version/Config |
|------|----------------|
| **Next.js** | Latest stable version |
| **TypeScript** | Enabled (default with `--typescript`) |
```

## Creation Workflow

When asked to create a new skill, follow this workflow:

### Step 1 — Understand the Skill's Purpose

Determine what kind of skill this is:
- **Reference** — documents how to use a tool/library (like `gh`)
- **Process/Setup** — guides through a multi-step process (like `nextjs-setup`)
- **Guideline** — enforces rules and constraints (like `git-guidelines`)
- **Execution/Orchestration** — orchestrates other agents (like `plan-executor`)
- **Planning** — helps plan and structure work (like `planning`)

### Step 2 — Choose the Name

- Use a lowercase, hyphen-separated slug
- Keep it concise and descriptive
- Examples: `docker-setup`, `api-testing`, `code-review`, `database-migration`

### Step 3 — Write the Description

Craft a one-line description that:
1. States what the skill does
2. Lists trigger words in Portuguese and English

Format: `description: <What>. Trigger words: <pt1>, <pt2>, <en1>, <en2>.`

### Step 4 — Design the Structure

Pick the appropriate section structure based on the skill type (see section structure rules above).

### Step 5 — Write the Content

Fill in each section with:
- Clear, actionable instructions
- Code blocks with correct language tags
- Completion criteria with checkboxes
- Important notes with "Never"/"Always" emphasis

### Step 6 — Review

Before writing the file, verify:
- [ ] Frontmatter is valid YAML
- [ ] Description is one line, includes trigger words
- [ ] Sections follow the chosen structure pattern
- [ ] Code blocks have correct language tags
- [ ] Completion criteria are actionable and verifiable
- [ ] No language mixing within sections
- [ ] Emojis are consistent (if used)
- [ ] File path references use backticks

### Step 7 — Write the File

Create the file at:

```
/home/gabs/.pi/agent/skills/<skill-name>/SKILL.md
```

Use the `write` tool to create the file. The directory is created automatically.

## Reference Examples

### Minimal Reference Skill (like `gh`)

```yaml
---
name: tool-name
description: Description of what this skill covers. Trigger words: <words>.
---
# Title

## Section 1
<Content>

## Section 2
<Content>
```

### Minimal Process Skill (like `nextjs-setup`)

```yaml
---
name: process-name
description: Description of what this skill does. Trigger words: <words>.
---
# Title

## When to Use
<When to activate>

## <Core Concept>
<Foundational info>

## <Procedure>
<Steps with code blocks>

## Completion Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Important Notes
- **Never** skip X
- **Always** do Y
```

### Minimal Guideline Skill (like `git-guidelines`)

```yaml
---
name: guideline-name
description: Description. Trigger words: <words>.
---
# Title

## ⛔ Regras Absolutas
<Absolute rules>

## ⚠️ Regras Condicionais
<Rules requiring consent>

## ✅ Boas Práticas
<Recommended practices>
```

### Minimal Execution Skill (like `plan-executor`)

```yaml
---
name: execution-name
description: Description. Trigger words: <words>.
---
# Title

**Use this skill when...**

## Core Rules
- **ALWAYS** do X
- **NEVER** do Y

## <Workflow>
<Steps>

## Completion Checklist
- [ ] Checklist item
```

## Common Mistakes to Avoid

1. **Long descriptions** — Keep the description to one line. The trigger words come after `Trigger words:`.
2. **Inconsistent emoji usage** — Don't mix emoji-heavy and emoji-free sections.
3. **Mixed languages** — Pick English or Portuguese for the entire skill, don't mix within sections.
4. **Missing completion criteria** — Process/setup skills must have a `Completion Criteria` section.
5. **Wrong code block language** — Match the language tag to the file type (`ts` for TypeScript, `yaml` for YAML, etc.).
6. **No trigger words** — Every skill must have trigger words in both Portuguese and English.
7. **Verbatim frontmatter** — The frontmatter must use `---` delimiters, not comments or other markers.
8. **Over-structuring reference skills** — Reference skills (like `gh`) don't need "When to Use" or "Completion Criteria" — they're organized by topic.
