# Spec: Skill G-Code (Claude Code Skills)

> Claude Code skill definitions for the G-Code monorepo — master router
> and five domain modules.

---

## Status

Approved (v1.0.0)

---

## Scope

Defines the skill routing mechanism, global constraints, and per-module
SKILL.md structure.

---

## Functional Requirements

### Master Router
- FR-1: Root `SKILL.md` routes `[Prefix]`-tagged tasks to domain modules
- FR-2: Supported prefixes: `[Spiritual]`, `[Program]`, `[Psychological]`, `[Physiological]`, `[Synchronize]`
- FR-3: Each prefix maps to `{module}-g-code/SKILL.md`

### Global Constraints
- FR-4: Bilingual Imperative — all output EN + 繁體中文, no Pinyin
- FR-5: CLI-First — prioritize headless, terminal-runnable solutions
- FR-6: Rational Grounding — technical language, not mystical jargon

### Module Structure
- FR-7: Each module directory contains `SKILL.md`, `scripts/`, `references/`, `assets/`
- FR-8: SKILL.md uses YAML frontmatter (name, version, description, sub_modules)
- FR-9: Domain-specific constraints inherited from the module's SKILL.md

### Tech Metaphors
- FR-10: Consciousness = Operating System
- FR-11: Spiritual growth = Refactoring / Patch Notes
- FR-12: Daily guidance = Navigation System

---

## Interface Contract

### SKILL.md Frontmatter

```yaml
---
name: skill-g-code
version: 1.3.0
description: Master control router for the Skill G-Code Monorepo
sub_modules: [spiritual, program, psychological, physiological, sync]
---
```

### Routing Table

| Prefix | Module Directory |
|--------|-----------------|
| `[Spiritual]` | `spiritual-g-code/` |
| `[Program]` | `program-g-code/` |
| `[Psychological]` | `psychological-g-code/` |
| `[Physiological]` | `physiological-g-code/` |
| `[Synchronize]` | `sync-g-code/` |

### Installation

```bash
cp -r skill-g-code/ ~/.claude/commands/skill-g-code/
```

---

## Dependencies

- `specs/naming-conventions.md` — Naming and emoji standards

---

## References

- [skill-g-code](https://github.com/Galen-Chu/skill-g-code) — Implementation
- [Claude Code](https://claude.ai/code) — Agent platform
