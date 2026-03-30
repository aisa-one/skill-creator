---
name: skill-creator
description: |
  Use when the user asks to "create a skill", "scaffold a skill", "create a plugin", "improve a skill",
  "validate a skill", or needs guidance on SKILL.md authoring, plugin directory layout, progressive disclosure,
  workflow design, or bundling scripts/references/templates into a Claude Code plugin.
  MUST read this skill first before editing skill files directly.
version: 1.0.0
---

# Skill Creator

Guide for creating effective Claude Code skills and plugins.

## About Skills

Skills are modular packages that extend Claude Code with specialized knowledge, workflows, and tools. They transform Claude from a general-purpose agent into a domain specialist equipped with procedural knowledge no model fully possesses.

### What Skills Provide

1. **Specialized workflows** — multi-step procedures for specific domains
2. **Tool integrations** — instructions for working with specific file formats or APIs
3. **Domain expertise** — company-specific knowledge, schemas, business logic
4. **Bundled resources** — scripts, references, and assets for complex tasks

## Core Principles

### Concise is Key

The context window is a shared resource. Skills compete with system prompt, conversation history, other skills' metadata, and the user's request.

**Default assumption: Claude is already very smart.** Only add context Claude doesn't already have. Challenge each piece: "Does Claude really need this?" and "Does this justify its token cost?"

Prefer concise examples over verbose explanations.

### Degrees of Freedom

Match specificity to task fragility:

- **High freedom** (text instructions): multiple valid approaches, context-dependent decisions
- **Medium freedom** (pseudocode/parameterized scripts): preferred pattern exists, some variation acceptable
- **Low freedom** (specific scripts, few parameters): fragile operations, consistency critical

### Anatomy of a Skill

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter (name + description, required)
│   └── Markdown instructions (required)
└── Bundled Resources (optional)
    ├── scripts/      — Executable code (Python/Bash)
    ├── references/   — Documentation loaded on demand
    └── templates/    — Output assets (logos, fonts, boilerplate)
```

#### SKILL.md Frontmatter

```yaml
---
name: skill-name
description: |
  What this skill does AND when to use it. This is the primary trigger mechanism —
  the body only loads after the description matches.
version: 1.0.0
---
```

The `description` field is critical: it determines when Claude activates the skill. Be clear and comprehensive about what the skill does AND when it should be used.

#### Bundled Resources

- **`scripts/`** — Token-efficient: run without loading into context
- **`references/`** — Loaded on demand. Keeps SKILL.md lean
- **`templates/`** — Not loaded into context. Used in output

**Rules:** No duplication between SKILL.md and references. No README.md or CHANGELOG.md — skills are for agents, not humans.

### Progressive Disclosure

Three-level loading:

1. **Metadata** — always in context (~100 words)
2. **SKILL.md body** — when skill triggers (keep under 500 lines)
3. **Bundled resources** — as needed

Key principle: core workflow in SKILL.md; variant-specific details in reference files.

For detailed patterns, see `${CLAUDE_PLUGIN_ROOT}/skills/skill-creator/references/progressive-disclosure-patterns.md`.

## Skill Creation Process

### Step 1: Understand the Skill

Gather concrete examples of how the skill will be used. Ask:
- "What functionality should this skill support?"
- "Can you give examples of how it would be used?"

### Step 2: Plan Reusable Contents

| Resource Type | When to Use | Example |
|---------------|-------------|---------|
| `scripts/` | Code rewritten repeatedly | `rotate_pdf.py` |
| `templates/` | Same boilerplate each time | HTML starter template |
| `references/` | Documentation needed repeatedly | Database schemas |

### Step 3: Initialize

Run the scaffold script:

```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/skill-creator/scripts/init_skill.py" <skill-name>
```

This creates a skill directory with SKILL.md template, example `scripts/`, `references/`, and `templates/` directories.

### Step 4: Edit

When editing, remember the skill is for another Claude instance. Include non-obvious procedural knowledge.

**Design pattern references** (load as needed):
- Multi-step processes: `${CLAUDE_PLUGIN_ROOT}/skills/skill-creator/references/workflows.md`
- Output formats: `${CLAUDE_PLUGIN_ROOT}/skills/skill-creator/references/output-patterns.md`
- Progressive disclosure: `${CLAUDE_PLUGIN_ROOT}/skills/skill-creator/references/progressive-disclosure-patterns.md`

**Writing guidelines:** Use imperative/infinitive form. Frontmatter `description` must include what AND when.

### Step 5: Validate & Deliver

```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/skill-creator/scripts/quick_validate.py" <skill-name>
```

Fix any validation errors. Then deliver the SKILL.md path to the user.

### Step 6: Iterate

1. Use the skill on real tasks
2. Notice struggles or inefficiencies
3. Update SKILL.md or bundled resources
4. Test again

## Plugin Packaging (Claude Code)

To distribute a skill as a Claude Code plugin:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json          # name (required), version, description, author, keywords
├── skills/
│   └── my-skill/
│       ├── SKILL.md
│       ├── scripts/
│       └── references/
├── commands/                # Optional slash commands (.md files)
├── agents/                  # Optional agent definitions (.md files)
├── hooks/                   # Optional event handlers
├── .mcp.json                # Optional MCP server config
└── README.md
```

Minimal `plugin.json`:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What this plugin does"
}
```

Use `${CLAUDE_PLUGIN_ROOT}` for all intra-plugin path references.
