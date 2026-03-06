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

### [spec-driven-development](./spec-driven-development/)

规范驱动开发技能（Spec-Driven Development Skill）

Guides developers from vibe-coding to structured Agentic Coding: write a spec first, get it reviewed, then implement with TDD.

- **Requirement Dialogue**: clarifies goals, users, boundaries, and success criteria before writing anything
- **Spec Document**: produces a structured spec with interface contracts, Given/When/Then behaviour items, error handling, and Out of Scope
- **Hard Gate**: no implementation code until the spec is confirmed by the user
- **TDD Loop**: for each spec item, strictly RED → GREEN → REFACTOR → COMMIT
- **Acceptance Verification**: every spec item has a passing automated test before the feature is considered done

> **Note**: Use this skill at the start of any new feature, module, or significant change — spec first, code second.

## Skill Format

Each skill is a folder containing a `SKILL.md` file with YAML frontmatter and markdown instructions:

```markdown
---
name: skill-name
description: Description of what this skill does and when to use it
---

# Skill Instructions
```
