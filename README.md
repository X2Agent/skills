# Skills

This repository contains Agent Skills for use with Claude and compatible AI agents.

## Installation

### Via OpenSkills (Claude Code, Cursor, Windsurf, Aider, and more)

Install the skills from this repository using [openskills](https://github.com/numman-ali/openskills):

```bash
# Install into the current project
npx openskills install X2Agent/skills

# Register installed skills in AGENTS.md so your agent can see them
npx openskills sync
```

To load a specific skill at runtime (agents run this automatically):

```bash
npx openskills read code-review
```

### Via Claude Code (native)

Claude Code discovers skills automatically from `.claude/skills/`. You can also install manually:

```bash
mkdir -p .claude/skills
cp -r code-review .claude/skills/
```

---

## Available Skills

### [code-review](./code-review/)

代码规范审查技能（Code Standards Review Skill）

Performs code standards and style convention checks for **C#**, **Python**, **Golang**, and **JavaScript/TypeScript** projects, including popular frameworks:

- **C#**: Microsoft C# Coding Conventions, ASP.NET Core, Entity Framework
- **Python**: PEP 8, PEP 257, Django, FastAPI, Flask
- **Golang**: Effective Go, Go Code Review Comments, Gin, Echo, GORM
- **JavaScript/TypeScript**: Airbnb Style Guide, React, Vue.js, Angular, Node.js/Express

> **Note**: This skill performs coding standards checks only. It does **not** perform code logic review, performance analysis, or security audits.

## Skill Format

Each skill is a folder containing a `SKILL.md` file with YAML frontmatter and markdown instructions:

```markdown
---
name: skill-name
description: Description of what this skill does and when to use it
---

# Skill Instructions
```