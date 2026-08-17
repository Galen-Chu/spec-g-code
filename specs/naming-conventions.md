# Spec: Naming Conventions

> Repository naming, module prefixes, API versioning, and emoji conventions
> across the G-Code ecosystem.

---

## Status

Approved (v1.0.0)

---

## Scope

Defines how repos, modules, branches, commits, API endpoints, and
documentation are named across all `*-g-code` projects.

---

## Repository Naming

Pattern: `{domain}-g-code`

| Domain | Repo | Emoji |
|--------|------|-------|
| spec | spec-g-code | 📋 |
| skill | skill-g-code | 🛸 |
| program | program-g-code | ⚙️ |
| sync | sync-g-code | 🛰️ |
| spiritual | spiritual-g-code | 🔮 |
| psychological | psychological-g-code | 🕉️ |
| physiological | physiological-g-code | 🧬 |

Rules:
- All lowercase, hyphen-separated
- Suffix always `-g-code`
- Emoji appears in README title and Related Repos tables

---

## Branch Naming

```
main                          — production branch
feature/{version}-{topic}    — feature branches (e.g. feature/1.3.0-phase4-api)
fix/{description}             — bugfix branches (e.g. fix/ci-requirements)
docs/{description}            — documentation-only changes
```

---

## Commit Message Convention

Format: `type: subject` or `type(scope): subject`

| Type | Usage |
|------|-------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `test` | Adding or correcting tests |
| `ci` | CI/CD configuration changes |
| `chore` | Maintenance (deps, configs) |

Examples:
```
feat: add Phase 4 community API surface
fix: resolve all critical blockers preventing the project from running
docs: align README with actual project state
ci: make Docker Build non-blocking
```

Body: explain **why**, not what. Wrap at ~72 characters. End with
`Co-Authored-By:` trailer when AI-assisted.

---

## API Endpoint Naming

Pattern: `/api/{resource}/{action}/`

| Element | Convention | Example |
|---------|-----------|---------|
| Resource | Plural, kebab-case | `/api/codon-requests/` |
| Action | Verb, kebab-case | `/api/files/rename/` |
| Nested | Parent then child | `/api/discussions/{id}/comments/` |
| Config | Under `/api/config/` | `/api/config/fhir-servers/` |

Versioning: `/api/v1/{resource}/` when breaking changes are needed.
Current repos use unversioned paths (v1 implied).

---

## Environment Variable Naming

Pattern: `{SCOPE}_{VARIABLE}` (uppercase, underscore-separated)

| Prefix | Scope | Example |
|--------|-------|---------|
| `GEMINI_` | Gemini AI | `GEMINI_API_KEY` |
| `GOOGLE_` | Google Cloud | `GOOGLE_DOCS_ID` |
| `AWS_` | AWS S3 | `AWS_ACCESS_KEY` |
| `DJANGO_` | Django core | `DJANGO_SECRET_KEY` |
| `FHIR_` | FHIR settings | `FHIR_ENV` |
| `MS_` | Microsoft OAuth | `MS_CLIENT_ID` |

---

## Documentation Naming

| File | Purpose | Location |
|------|---------|----------|
| `README.md` | Project overview, quick start | Repo root |
| `LICENSE` | MIT license text | Repo root |
| `CHANGELOG.md` | Keep a Changelog format | Repo root |
| `CONTRIBUTING.md` | Contribution guidelines | Repo root |
| `IMPLEMENTATION.md` | Setup and deployment guide | Repo root (optional) |
| `SKILL.md` | Claude Code skill definition | Module directory |
| `docs/*.md` | Deep-dive documentation | `docs/` directory |

Rules:
- Historical/planning docs belong under `docs/`, not root
- Per-module READMEs use `README_{ModuleName}.md` format
- No spaces in filenames; use hyphens

---

## Emoji Conventions

Every `##` section heading in README files should have a thematic emoji:

| Section Type | Common Emoji |
|-------------|--------------|
| Features | ✨ |
| Quick Start | 🚀 |
| Architecture | 🏗️ |
| Configuration | ⚙️ |
| Usage | 📖 |
| Testing | 🧪 |
| Contributing | 🤝 |
| License | 📄 |
| Related/Similar | 🔗 |
| Roadmap | 🗺️ |
| Philosophy/Vision | 🧭 |

Separator: `---` between every `##` section.

---

## References

- [Keep a Changelog](https://keepachangelog.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
