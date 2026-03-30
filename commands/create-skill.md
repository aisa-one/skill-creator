---
description: "Scaffold a new Claude Code skill with SKILL.md template and resource directories"
argument-hint: "<skill-name>"
allowed-tools: [Read, Write, Bash, Glob]
---

Create a new skill directory with proper structure and SKILL.md template.

## Instructions

1. Run the init script: `python3 "${CLAUDE_PLUGIN_ROOT}/skills/skill-creator/scripts/init_skill.py" $ARGUMENTS`
2. If the script is not available, manually create the directory structure:
   ```
   skills/<skill-name>/
   ├── SKILL.md
   ├── scripts/
   ├── references/
   └── templates/
   ```
3. Generate a SKILL.md with proper YAML frontmatter (`name`, `description`, `version`) and TODO placeholders.
4. Report the created path and suggest next steps.
