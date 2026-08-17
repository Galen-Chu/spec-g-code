# Spec: Psychological G-Code

> Chakra-Kabbalah correspondence system with structured data, JSON schema
> validation, and an interactive assessment engine.

---

## Status

Draft

---

## Scope

Defines the data schemas (chakras, sephirot, correspondences), the
assessment system, and the visualization components.

---

## Functional Requirements

### Data Layer
- FR-1: 7 chakras with Sanskrit names, locations, elements, colors, seed syllables, balance/imbalance indicators
- FR-2: 10 sephirot with Hebrew names, pillars (Mercy/Severity/Balance), archangels, tarot correspondences, and 22 connecting paths
- FR-3: 3 mapping systems: Theosophical/Golden Dawn, Alice Bailey/Arcane School, Alternative/Expanded
- FR-4: Synthetic integrations: Middle Pillar Alignment, Pillar Balancing with Chakra Pairs, Psychological Integration Model
- FR-5: JSON Schema validation (`data-schema.json`) with `npm run validate`

### Assessment Engine
- FR-6: 56 self-reflection questions (8 per level × 7 levels)
- FR-7: Scoring algorithm for balance assessment per chakra/sephira
- FR-8: Pillar dominance detection (Mercy vs Severity vs Balance)
- FR-9: Personalized practice recommendations based on scores
- FR-10: XP/streak/level system with localStorage persistence

### Visualization
- FR-11: SVG-based interactive diagram (chakra column + Tree of Life side-by-side)
- FR-12: Click-to-learn with information panels
- FR-13: Correspondence overlays (matching chakra ↔ sephira)
- FR-14: Works standalone in browser or as ES module import

---

## Interface Contract

### Data Schema (JSON Schema)

```json
{
  "definitions": {
    "chakra": {
      "required": ["id", "name", "number", "location"]
    },
    "sephira": {
      "required": ["id", "name", "number", "position"]
    },
    "correspondence": {
      "properties": {
        "chakra_id": { "anyOf": ["string", "array of strings"] },
        "sephira_id": { "anyOf": ["string", "array of strings"] }
      }
    }
  }
}
```

### Assessment Engine API

```javascript
import AssessmentEngine from './src/assessment-engine.js';
const engine = new AssessmentEngine();
const report = engine.generateReport(assessmentData, userResponses, pillarResponses);
```

### Visualization API

```javascript
import ChakraKabbalahVisualization from './src/visualization/interactive-diagram.js';
const viz = new ChakraKabbalahVisualization('container', {
  showChakras: true, showSephirot: true, interactive: true
});
viz.init();
```

---

## Dependencies

- No runtime dependencies (pure client-side HTML/CSS/JS)
- `ajv` (devDependency for schema validation)

---

## References

- [psychological-g-code](https://github.com/Galen-Chu/psychological-g-code) — Implementation
- [JSON Schema](https://json-schema.org/) — Validation standard
