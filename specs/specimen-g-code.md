# Spec: Specimen G-Code (Preparation & Assay Pipeline)

> A deterministic preparation-and-assay pipeline: raw data in, a registered,
> leakage-safe, bit-reproducible **specimen** out — plus comparable
> model-training protocols run on it. "Data you can testify about."

---

## Status

Approved (v0.4.0 — Phase 0-4a implemented)

---

## Scope

Defines the five-stage pipeline (acquisition → preparation → staining →
catalog → assay), the four runtime invariants, the manifest format, the
recipe/step/selector registries, and the domain extension points. Does NOT
define visualization, streaming, or general experiment tracking (MLflow's job).

---

## Functional Requirements

### The Specimen Model
- FR-1: A specimen is an immutable directory whose truth is `manifest.toml`
  (name, version, seed, recipe identity, inputs, outputs, contracts, splits)
- FR-2: Bit-reproducibility (I4): (raw hash + recipe + seed) → byte-identical
  outputs; the manifest pins the recipe module's `source_sha256`, and `verify`
  names code drift instead of emitting an anonymous "differs"
- FR-3: The `created` / `environment` fields are opt-in annotations — never
  reproduction inputs (cross-machine byte-reproduction)

### The Four Invariants (runtime gates, refuse-not-warn)
- FR-4: I1 Time safety — splits follow the declared order; transformers fit
  on the train segment only; every labeling is re-checked at the assay door
- FR-5: I2 Target purity — the target never appears among the features; every
  non-key feature declares `availability_lag_days`; undeclared availability is
  a refusal, not an assumption
- FR-6: I3 Grain separation — different grains live in different tables; the
  only sanctioned cross-grain operator is the registered as-of join
  (`staining.asof_join`, lag ≥ 90 days for quarterly→daily); rows before
  availability are dropped, never filled
- FR-7: I3 structural half — every output/contract pair agrees on one
  well-formed grain, checked at seal time and at the assay door

### The Staining Step Library (Phase 3)
- FR-8: Derived columns come from **named, registered steps with declared
  parameters** (`asof_join` / `diff` / `ratio` / `pct_change`); anonymous
  lambdas are forbidden; every invocation is recorded in the manifest steps
  in canonical form (`step_call`)
- FR-9: `ratio` refuses zero denominators loudly; `pct_change` drops rows
  without the lookback window — nothing is guessed, nothing is filled

### Assay Protocols (Phase 2-3)
- FR-10: Protocols are a registry (`PROTOCOLS`); `time-split-baseline` runs
  constant/logistic/decision_tree/knn with ROC-AUC / PR-AUC against the
  sealed splits; `[[assays]]` records are append-only and never overwritten
- FR-11: Feature selection is a registry (`SELECTORS`: correlation-filter /
  mutual-information-top-k / rfe-top-k); every selector **fits on the train
  segment only** (I1); `[[assays]]` records name the selector, its params and
  the exact features the models saw

### Domain Extension Points (SOP §6)
- FR-12: Domain differences live only in four blades — recipe packs
  (`specimen/recipes/<domain>/` + `REGISTRY`), contract vocabulary
  (`ColumnSpec`), assay protocols, acquisition adapters — never in the core
- FR-13: The second domain must not force core abstractions; a core change
  (manifest format, invariant definitions) requires an ADR first
  (ADR-0001: atemporal specimens split by `seed-ordered` boundaries over the
  int `seed` key — I1/I3 degenerate to seed order, gates still run)

### Determinism & Dependencies
- FR-14: Zero-dependency core (manifest/contract/catalog/invariants/assay
  protocol/staining); recipes opt into `[etl]` (pandas), `[ml]`
  (scikit-learn) blades
- FR-15: CLI-first: `prepare` / `verify` / `catalog` / `assay`; `verify`
  reports manifest/integrity/contract/reproduce and exits 0/1

---

## Interface Contract

### Registry Shape

```python
REGISTRY: dict[str, RecipeSpec]        # name → module, source_sha256, steps, inputs, contracts, splits, run
STEPS: dict[str, StepFn]               # staining steps; require_registered(name) else refuse
PROTOCOLS: dict[str, ProtocolFn]       # assay protocols
SELECTORS: dict[str, SelectorFn]       # feature-selection protocols
```

### Verify Report

```
manifest:   structure + I3          (PASS/FAIL)
integrity:  I4 seal side            (PASS/FAIL)
contract:   schema conformance      (PASS/FAIL)
reproduce:  I4 reproduction side    (PASS/FAIL — drift is named)
```

---

## Dependencies

- Core: Python stdlib only
- `[etl]`: pandas (recipes that need it)
- `[ml]`: scikit-learn (assay protocols, selection blade)
- CI gates: pytest, ruff check, ruff format, mypy strict

## References

- [Repository](https://github.com/Galen-Chu/specimen-g-code)
- [DESIGN.md](https://github.com/Galen-Chu/specimen-g-code/blob/main/docs/DESIGN.md)
- [SOP.md](https://github.com/Galen-Chu/specimen-g-code/blob/main/docs/SOP.md)
- [ADR-0001](https://github.com/Galen-Chu/specimen-g-code/blob/main/docs/adr/ADR-0001-atemporal-specimens.md)
