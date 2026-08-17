# Spec: Physiological G-Code

> Django + DRF platform bridging DNA/RNA codons and I Ching hexagrams,
> with pattern analysis, comparative tools, and community features.

---

## Status

Approved (v1.0.0)

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

## Acceptance Criteria

### Codon Translation (FR-1–4)

```gherkin
Given codon "ATG" with mapping_scheme "scheme_1"
When translate_codon is called
Then the result is a valid hexagram number between 1 and 8
And the result is deterministic (same input → same output)

Given sequence "ATGCGATAA" (9 bases = 3 codons)
When analyze_sequence is called
Then hexagram_sequence has length 3
And amino_acid_sequence is "M*R" (Met-Arg-Stop)
And gc_content is approximately 33.3%
```

### Pattern Analysis (FR-5)

```gherkin
Given a sequence with a repeating motif "ATGCGA"
When motif_discovery is called
Then the motif appears in the results with a position list
And the statistical significance (p-value) is below 0.05
```

### Community — API Key Security (FR-14)

```gherkin
Given a user creates an API key
When the response is returned
Then the plaintext key appears exactly once (in the creation response)
And the database stores only the SHA-256 hash
And a subsequent GET /api/api-keys/ shows only the prefix
```

---

## Mock API Examples

### Analyze Sequence

```http
POST /api/analysis/analyze_sequence/
Authorization: Bearer <token>

{
  "sequence": "ATGCGATAA",
  "name": "Sample Gene",
  "sequence_type": "DNA",
  "mapping_scheme": "scheme_1"
}
```

```json
{
  "hexagram_sequence": [1, 3, 5],
  "amino_acid_sequence": "M*R",
  "gc_content": 33.33,
  "codon_frequency": {"ATG": 1, "CGA": 1, "TAA": 1},
  "dominant_hexagram": 1,
  "diversity_score": 0.78
}
```

### Community — Create Discussion

```http
POST /api/discussions/
Authorization: Bearer <token>

{
  "title": "Understanding codon degeneracy",
  "content": "Why do multiple codons map to the same amino acid?",
  "discussion_type": "question",
  "tags": ["genetics", "codons"]
}
```

```json
{
  "id": 42,
  "author": "alice",
  "title": "Understanding codon degeneracy",
  "slug": "understanding-codon-degeneracy",
  "vote_score": 0,
  "comment_count": 0
}
```

### Error — Invalid Sequence

```json
{
  "error": "Invalid DNA sequence: contains non-nucleotide characters.",
  "code": "validation_error",
  "details": {"sequence": "Only A, T, G, C, U allowed"}
}
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
