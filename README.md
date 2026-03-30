# Skill Creator

Create, validate, and iterate on Claude Code skills and plugins — with best-practice guidance baked in.

## What it does

Guides you through the full skill creation lifecycle:

1. **Understand** — gather concrete usage examples
2. **Plan** — identify reusable scripts, references, templates
3. **Scaffold** — generate directory structure and SKILL.md template
4. **Edit** — write instructions using proven design patterns
5. **Validate** — check structure and frontmatter requirements
6. **Iterate** — improve based on real usage

Includes reference guides for progressive disclosure patterns, workflow design, and output formatting.

## Installation

### 通过市场安装（推荐）
```bash
# 添加市场源（仅需一次）
/plugin marketplace add https://github.com/aisa-one/skill-creator
# 安装插件
/plugin install skill-creator
```

## Components

```
skill-creator/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   └── create-skill.md             # /create-skill slash command
├── skills/
│   └── skill-creator/
│       ├── SKILL.md                 # Auto-activated creation guide
│       ├── references/
│       │   ├── workflows.md         # Multi-step process patterns
│       │   ├── output-patterns.md   # Template and example patterns
│       │   └── progressive-disclosure-patterns.md
│       └── scripts/
│           ├── init_skill.py        # Scaffold generator
│           └── quick_validate.py    # Structure validator
└── README.md
```

## Usage

**Slash command:**

```
/create-skill my-new-skill
```

**Automatic activation** — Claude detects skill creation intent:

- "Create a skill for database migrations"
- "Help me build a plugin for our deployment workflow"
- "Validate my skill structure"
- "Improve this SKILL.md"

## License

MIT
