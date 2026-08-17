# Spec: Program G-Code (CI/CD Toolkit)

> Language-agnostic CI/CD pipeline toolkit with lint, test, build,
> deploy, and rollback automation.

---

## Status

Draft

---

## Scope

Defines the CI/CD pipeline stages, script interfaces, notification
hooks, and configuration model.

---

## Functional Requirements

### CI Pipeline
- FR-1: Auto-detect project type (Node.js, Python, Go, Java, Docker)
- FR-2: Run linters based on detected tools (ESLint, flake8, golangci-lint, etc.)
- FR-3: Run test runners with configurable parallel execution
- FR-4: Generate coverage reports with configurable thresholds

### CD Pipeline
- FR-5: Build artifacts (language-specific or Docker)
- FR-6: Deploy to configurable environments (dev, staging, prod)
- FR-7: Health checks post-deployment (HTTP endpoint or TCP port)
- FR-8: Automatic rollback on deployment failure
- FR-9: Manual rollback to specific version

### Pre-flight Checks
- FR-10: Git working directory status (clean/dirty)
- FR-11: Branch sync status vs main
- FR-12: CI pipeline status (via GitHub CLI)
- FR-13: Documentation validation (README present, CHANGELOG current)

### Notifications
- FR-14: Slack webhook notifications
- FR-15: Email notifications (SMTP)
- FR-16: Generic webhook notifications

---

## Interface Contract

### Script Entry Points

| Script | Purpose | Key Options |
|--------|---------|-------------|
| `scripts/ci/lint.sh` | Run linters | `--fix` (auto-fix) |
| `scripts/ci/test.sh` | Run tests | `--coverage`, `--parallel` |
| `scripts/cd/build.sh` | Build artifacts | `--docker`, `--version` |
| `scripts/cd/deploy.sh` | Deploy | `--pre-flight`, `--force` |
| `scripts/cd/rollback.sh` | Rollback | `--version`, `--list` |
| `scripts/utils/health-check.sh` | Check service | `--tcp`, `--retry` |
| `scripts/utils/pre-flight.sh` | Orchestrated checks | `--strict`, `--quick` |
| `scripts/utils/update-changelog.sh` | CHANGELOG automation | `--auto`, `--type` |

### Configuration

```ini
# config/ci-cd.conf
[general]
project_type=auto
log_level=info
dry_run=false

[ci]
lint_enabled=true
test_enabled=true
coverage_threshold=80

[cd]
environments=dev,staging,prod
health_check_timeout=300
auto_rollback=true

[notifications]
slack_webhook=
enabled_events=deploy_success,deploy_failure
```

### Environment Overrides

All config values overridable via environment variables:
`LOG_LEVEL=debug`, `DRY_RUN=true`, `BUILD_VERSION=1.0.0`

---

## Dependencies

None (infrastructure layer)

---

## References

- [program-g-code](https://github.com/Galen-Chu/program-g-code) — Implementation
- [GitHub Actions](https://docs.github.com/actions) — CI platform
