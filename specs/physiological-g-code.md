# Spec: Physiological G-Code

> Django + DRF platform bridging DNA/RNA codons and I Ching hexagrams,
> with pattern analysis, comparative tools, and community features.

---

## Status

Draft

---

## Scope

Defines the codon-hexagram mapping engine, analysis services, REST API,
and community layer.

---

## Functional Requirements

### Codon-Hexagram Engine
- FR-1: 64 DNA codons mapped to 64 hexagrams via 4 binary schemes (purine/pyrimidine, AT/GC, hydrogen bond, molecular weight)
- FR-2: Each scheme maps a base to one bit (3-bit codon → 8 possible hexagram groups)
- FR-3: Translate full DNA/RNA sequences to hexagram sequences
- FR-4: Amino acid translation (standard genetic code)

### Analysis Services
- FR-5: Pattern analysis — position analysis, sliding window, motif discovery, conservation, entropy, autocorrelation
- FR-6: Comparative analysis — side-by-side, statistical tests (chi-square, Fisher's exact, KS), similarity metrics, conserved regions
- FR-7: Visualization data — frequency charts, transition networks, heatmaps, 3D relations
- FR-8: Export — CSV, JSON, FASTA, PDF reports

### AI Integration
- FR-9: Gemini-powered hexagram interpretations (biological + philosophical)
- FR-10: Patient-facing plain-language Q&A and clinician-facing summaries

### Community
- FR-11: User profiles with reputation tracking
- FR-12: Discussions with threaded comments and voting
- FR-13: Notifications (comment → author)
- FR-14: API keys (SHA-256 hashed, plaintext shown once, revoke/rotate)
- FR-15: Webhooks with secret regeneration

---

## Interface Contract

### REST API

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register/` | Register + profile + JWT |
| POST | `/api/auth/login/` | JWT login |
| GET | `/api/codons/` | 64 codons |
| GET | `/api/hexagrams/` | 64 hexagrams |
| POST | `/api/analysis/analyze_sequence/` | Full sequence analysis |
| POST | `/api/patterns/position_analysis/` | Pattern detection |
| POST | `/api/comparative/side_by_side/` | Compare sequences |
| POST | `/api/export/{format}/` | CSV/JSON/FASTA/PDF |
| GET | `/api/profiles/me/` | Own profile |
| CRUD | `/api/discussions/` | Community discussions |

### Key Data Models

```
Codon: triplet, amino_acid, is_start, is_stop, binary
Hexagram: number, binary, chinese_name, english_name
CodonSequence: user, sequence, hexagram_sequence, gc_content
CodonHexagramMapping: scheme, codon FK, hexagram FK
```

---

## Dependencies

- `specs/api-conventions.md` — REST API design
- `specs/authentication.md` — JWT auth model
- `specs/bilingual-output.md` — Output requirements

---

## References

- [physiological-g-code](https://github.com/Galen-Chu/physiological-g-code) — Implementation
- [DRF](https://www.django-rest-framework.org/) — REST framework
