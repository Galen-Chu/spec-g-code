# Spec: Sanctuary G-Code (Procedural World Generator)

> A deterministic procedural world generator: seed + preset in, a complete
> reproducible simulated environment out — today as a runnable Unbodysoma
> scenario TOML. "Every seed deserves a world to live in."

---

## Status

Approved (v0.3.0 — Phase 1-3 implemented)

---

## Scope

Defines the determinism contract (per-system RNG streams, pure-function
fields, golden byte-pinning), the composable generation-system registry and
archetype model, the statistical-acceptance protocol, and the adapter
capability declarations. Does NOT define rendering (Phase 4 preview
adapter), infinite/streaming worlds, or real-time generation.

---

## Functional Requirements

### Determinism Contract
- FR-1: A self-hosted PRNG (mulberry32, ported bit-for-bit from
  abyssal-garden) — never Python's `random`; the same algorithm must produce
  identical sequences in JS and Python (cross-language lineage test pins the
  first constants)
- FR-2: Per-system streams: system `i` draws from stream seed `base + i·7919`;
  system ids are a lifetime-stable contract, never renumbered (terrain=1,
  food=2, water=3, shelter=4, hazard=5, patrol=6, seasons=7)
- FR-3: Pure generation: `generate(seed, preset) -> World` has no side
  effects; density fields are pure functions of position; the same (seed,
  preset) yields byte-identical output across platforms and Python versions
- FR-4: Deterministic float formatting (`_f()`: 6 decimals, trailing zeros
  trimmed), LF-only files, and a golden specimen (`meadow × seed 1`)
  byte-pinning the whole chain

### Composable Systems (archetypes)
- FR-5: The pipeline is a registry of systems (`System{name, stream_id,
  apply, needs}`) over a mutable `Generation` accumulation; execution is the
  canonical topological order restricted to the archetype's selection
- FR-6: An archetype is a (system composition + parameters) pair — expansion
  = register a new system; contraction = drop a system from the composition
  (no zero-parameter detours); reorganization must not move a single draw
- FR-7: Registered systems (Phase 1-3): food, water, shelter,
  hazard-permanent, hazard-patrol (strategy-dispatched: fixed / sweep /
  circadian / regional), terrain (elevation field; passability is a HARD
  constraint on plant placement, never relaxed by the placement ladder),
  seasons (global sinusoidal regrowth cycle whose multipliers mean exactly
  1.0 over one cycle)
- FR-8: Composition validation refuses unknown systems, empty compositions,
  and missing dependencies, loudly, at generate time

### Statistical Acceptance
- FR-9: Single worlds pass invariants (counts, bounds, gaps, clearance);
  classes of worlds pass statistical fingerprints — four metrics with
  executable theory centres × documented band factors, never tuned until
  green (food→water / shelter→water / shelter→food medians, hazard coverage)
- FR-10: Population mode (`--count N`): world `i`'s seed is `master + i` —
  each world reproduces standalone; the `population.toml` manifest is the
  group provenance (per-file SHA-256, no timestamps)

### Adapter Contract
- FR-11: The TOML adapter emits exactly Unbodysoma's accepted key set
  (ADR-0008); every adapter declares its capability (the systems it can
  express) — vocabulary beyond capability raises loudly, never silently
  omits (terrain and seasons raise until Unbodysoma's key set catches up,
  ADR-0010)
- FR-12: The JSON side (archetype index + specimen cards) is deterministic;
  worlds are never stored — a specimen card is (seed, archetype@version,
  fingerprint)

### Global Constraints (inherited)
- FR-13: Bilingual Imperative (EN + 繁體中文); CLI-First; Rational Grounding
  (every constant and rule has a documented source or reason in DESIGN.md);
  Reproducibility Required

---

## Interface Contract

### CLI

```
sanctuary --list-presets
sanctuary --seed 1 --preset meadow --out world.toml
sanctuary --seed 1 --preset meadow --count 3 --out runs/worlds    # population mode
sanctuary --preset meadow --fingerprint                           # 100-seed statistics
sanctuary --index                                                 # archetype library JSON
```

### Preset Contract

Every preset pins: dimensions, resource counts, gap/clearance constants,
field scales, patrol strategy, the system composition (`systems`), and a
`version` (bumped whenever any number or generation semantic changes;
old worlds record the revision that made them).

---

## Dependencies

- Core: Python stdlib only (`tomllib` on the consumer side)
- CI: pytest (golden + invariant + statistical bands), ruff check, ruff
  format, mypy strict, cross-platform matrix (ubuntu/macos/windows × 3.11+)

## References

- [Repository](https://github.com/Galen-Chu/sanctuary-g-code)
- [DESIGN.md](https://github.com/Galen-Chu/sanctuary-g-code/blob/main/docs/DESIGN.md)
- Lineage: [Noniv/abyssal-garden](https://github.com/Noniv/abyssal-garden) (MIT)
- First consumer: [Unbodysoma](https://github.com/Galen-Chu/Unbodysoma) — its
  [WORLDGEN.md](https://github.com/Galen-Chu/Unbodysoma/blob/main/docs/WORLDGEN.md)
  is this project's design origin
