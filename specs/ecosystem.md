# Spec: G-Code Ecosystem Architecture

> How the six repositories relate to each other, what data flows between
> them, and which layer each occupies.

---

## Status

Approved (v1.0.0)

---

## Scope

Defines the high-level architecture of the G-Code ecosystem: repository
roles, dependency topology, data flow, and the boundary between this
spec hub and the implementation repos.

Does **not** define individual project features — see the per-project
specs in Phase 2.

---

## Layered Architecture

```
Layer 4 — SKILL ORCHESTRATION
  skill-g-code          Claude Code skill router; global constraints
                        (bilingual, CLI-first, rational grounding)

Layer 3 — DOMAIN MODULES (peers, no inter-dependency)
  spiritual-g-code      Astrology, natal charts, transits, AI insights
  psychological-g-code  Chakra-Kabbalah correspondence, self-assessment
  physiological-g-code  DNA codon ↔ I Ching hexagram, analysis engine

Layer 2 — DATA PIPELINE
  sync-g-code           Daily content generation via Gemini; Flight Log
                        writes to Google Docs; cron-driven

Layer 1 — INFRASTRUCTURE
  program-g-code        CI/CD toolkit (lint, test, build, deploy, rollback)

Layer 0 — SPECIFICATION
  spec-g-code           This repo — single source of truth
```

---

## Dependency Topology

| From | Depends on | Type |
|------|-----------|------|
| skill-g-code | All domain repos | Routing references (read-only) |
| sync-g-code | spiritual-g-code (brand context) | Conceptual, not code-level |
| Domain repos | program-g-code (optional) | CI/CD pipeline adoption |
| All repos | spec-g-code | Spec compliance |
| spec-g-code | None | Standalone |

Key property: **domain modules are peers** (spiritual, psychological,
physiological). No domain module imports from another. Inter-module
awareness happens only through:

1. skill-g-code routing (`[Prefix]` tags)
2. spec-g-code contracts (shared API conventions, auth model)

---

## Data Flow

### Daily Cycle (sync-g-code)

```
GitHub Actions (cron 00:00 UTC)
  → main.py calls Gemini 1.5 Pro
  → generates bilingual content (quote + guidance)
  → writes to Google Docs (Flight Log)
```

No cross-repo API calls in the current architecture. Each repo is
self-contained; sync-g-code generates content independently.

### Skill Routing Cycle (skill-g-code)

```
User sends `[Spiritual] calculate natal chart`
  → Claude Code reads skill-g-code/SKILL.md
  → Routes to spiritual-g-code/SKILL.md
  → Agent inherits domain-specific constraints
  → Executes task using that module's tools/references
```

---

## Repository Roles

| Role | Repository | What it provides |
|------|-----------|-----------------|
| Spec hub | spec-g-code | Functional requirements, interface contracts |
| Skill router | skill-g-code | Claude Code routing, global constraints |
| Infrastructure | program-g-code | CI/CD scripts, pipeline templates |
| Data pipeline | sync-g-code | Gemini content generation, Google Docs sync |
| Domain: spiritual | spiritual-g-code | Django dashboard, astrology engine |
| Domain: psychological | psychological-g-code | Chakra-Kabbalah data, assessment |
| Domain: physiological | physiological-g-code | DNA-I Ching platform, analysis |

---

## Constraints

1. **No cross-domain imports** — domain repos must not import each other's code
2. **Spec-first** — breaking changes require a spec update before implementation
3. **Bilingual output** — every user-facing output in EN + 繁體中文 (see `bilingual-output.md`)
4. **CLI-first** — all tools must run headless in terminal/cron environments

---

## References

- [skill-g-code](https://github.com/Galen-Chu/skill-g-code) — master SKILL.md
- [program-g-code](https://github.com/Galen-Chu/program-g-code) — CI/CD toolkit
- [sync-g-code](https://github.com/Galen-Chu/sync-g-code) — daily navigator
- [spiritual-g-code](https://github.com/Galen-Chu/spiritual-g-code) — Django platform
- [psychological-g-code](https://github.com/Galen-Chu/psychological-g-code) — correspondence system
- [physiological-g-code](https://github.com/Galen-Chu/physiological-g-code) — genetics platform
