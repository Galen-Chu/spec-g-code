# 📋 Spec G-Code

> **The specification hub for the G-Code ecosystem.** This repository defines
> the functional requirements, interface contracts, and data schemas that
> govern all `*-g-code` projects.

## Purpose

The G-Code family consists of six specialized repositories spanning software
engineering, spiritual exploration, and human consciousness. `spec-g-code`
serves as the **single source of truth** for what each project does, how they
interconnect, and what standards they follow.

```
spec-g-code  ─── the spec hub (this repo)
    ├── spiritual-g-code      Dashboard + natal charts + AI insights
    ├── sync-g-code           Daily navigator + Gemini content generation
    ├── psychological-g-code  Chakra-Kabbalah correspondence system
    ├── physiological-g-code  DNA codon ↔ I Ching hexagram mapping
    ├── program-g-code        CI/CD pipeline toolkit
    └── skill-g-code          Claude Code skill definitions (master router)
```

## Development Plan

### Phase 1 — Ecosystem Overview（生態系統總覽）
- [ ] `specs/ecosystem.md` — How the six repos relate to each other
- [ ] `specs/naming-conventions.md` — Repo naming, module prefixes, API versioning

### Phase 2 — Core Specs（核心規格）
- [ ] `specs/spiritual-g-code.md` — Dashboard, natal chart calculation, transit engine
- [ ] `specs/sync-g-code.md` — Daily content generation pipeline, Gemini API contract
- [ ] `specs/psychological-g-code.md` — Data schemas (chakras, sephirot, correspondences)
- [ ] `specs/physiological-g-code.md` — Codon-hexagram mapping algorithms, API surface

### Phase 3 — Cross-Cutting Concerns（橫切規格）
- [ ] `specs/authentication.md` — Shared auth model across projects
- [ ] `specs/api-conventions.md` — REST API design, error formats, pagination
- [ ] `specs/bilingual-output.md` — English + 繁體中文 output requirements

### Phase 4 — Tooling Specs（工具規格）
- [ ] `specs/program-g-code.md` — CI/CD pipeline stages, notification hooks
- [ ] `specs/skill-g-code.md` — Claude Code skill routing, global constraints

## How to Use

1. **Adding a new spec**: Create `specs/{topic}.md` following the template below
2. **Referencing a spec**: Link directly from the implementation repo's README
3. **Versioning**: Specs use semantic versioning; breaking changes require a major bump

### Spec Template

```markdown
# Spec: {Feature Name}

## Status
Draft | Approved | Deprecated

## Scope
What this spec covers and what it does not.

## Functional Requirements
- FR-1: ...
- FR-2: ...

## Interface Contract
API endpoints, data schemas, or CLI commands.

## Dependencies
Which other specs this one depends on.

## References
Links to implementation repos.
```

## Global Constraints (herited from skill-g-code)

- **Bilingual Imperative**: All specs in English + Traditional Chinese (繁體中文)
- **CLI-First**: Specs must be implementable as headless, terminal-runnable systems
- **Rational Grounding**: Technical language over mystical jargon; "Neuro-Spiritual" framing

## Related Repos

| Repository | Purpose |
|-----------|---------|
| [spiritual-g-code](https://github.com/Galen-Chu/spiritual-g-code) | Django dashboard + astrology |
| [sync-g-code](https://github.com/Galen-Chu/sync-g-code) | Daily navigator + Gemini |
| [psychological-g-code](https://github.com/Galen-Chu/psychological-g-code) | Chakra-Kabbalah system |
| [physiological-g-code](https://github.com/Galen-Chu/physiological-g-code) | DNA ↔ I Ching platform |
| [program-g-code](https://github.com/Galen-Chu/program-g-code) | CI/CD toolkit |
| [skill-g-code](https://github.com/Galen-Chu/skill-g-code) | Claude Code skills |

## License

MIT — see [LICENSE](LICENSE).
