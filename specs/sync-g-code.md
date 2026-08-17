# Spec: Sync G-Code

> Automated daily navigator that generates bilingual spiritual content
> via Gemini AI and writes to Google Docs.

---

## Status

Approved (v1.0.0)

---

## Scope

Defines the content generation pipeline, Gemini API contract, Google Docs
integration, and automation schedule.

---

## Functional Requirements

### Content Generation
- FR-1: Generate daily spiritual content at 00:00 UTC (08:00 Taiwan) via GitHub Actions cron
- FR-2: Each generation produces an awakening quote (1 line) and navigation guidance (~100 words)
- FR-3: All output in English + Traditional Chinese (繁體中文)
- FR-4: Tone: rational, technical, grounding — no vague mysticism
- FR-5: Content contextualized by date and seasonal energy patterns

### Gemini Integration
- FR-6: Use Gemini 1.5 Pro model (`gemini-1.5-pro`)
- FR-7: Structured prompt with date, persona, and output format constraints
- FR-8: Graceful error handling for API timeouts and quota limits

### Google Docs (Flight Log)
- FR-9: Write generated content to a designated Google Doc
- FR-10: Authenticate via Service Account (`credentials.json`)
- FR-11: Append entries chronologically with date headers

### Automation
- FR-12: GitHub Actions workflow at `.github/workflows/daily_navigator.yml`
- FR-13: Manual trigger available (`workflow_dispatch`)
- FR-14: Required secrets: `GEMINI_API_KEY`, `GOOGLE_DOCS_ID`, `GOOGLE_CREDENTIALS`

---

## Interface Contract

### Input

```python
# main.py entry point
GEMINI_API_KEY = os.environ["GEMINI_API_KEY"]
GOOGLE_DOCS_ID = os.environ.get("GOOGLE_DOCS_ID")

# Prompt template (bilingual)
prompt = f"""
Today is {date}. You are a Spiritual G-Code navigator...
Output: 1 awakening quote (EN + 繁中), ~100 word guidance (EN + 繁中).
"""
```

### Output

```
Awakening Quote: [English] / [繁體中文]
Navigation Guidance: [~100 words English] / [~100 words 繁體中文]
```

### Pipeline

```
GitHub Actions cron (00:00 UTC)
  → Checkout code
  → Install deps (google-generativeai, requests)
  → Run main.py
  → Gemini API call → content
  → Google Docs API → append to Flight Log
```

---

## Acceptance Criteria

### Content Generation (FR-1–5)

```gherkin
Given the GitHub Actions cron triggers at 00:00 UTC
When main.py executes
Then Gemini returns content containing both English and 繁體中文
And the awakening quote is a single line in each language
And the guidance is approximately 100 words per language
And the tone is rational (no "crystal energy" or vague mysticism)
```

### Google Docs Integration (FR-9–11)

```gherkin
Given a valid Service Account with Editor access to the target Doc
When content is generated
Then the entry is appended to the Doc with a date header
And the previous entries remain unchanged
```

### Error Handling (FR-8)

```gherkin
Given the Gemini API is unreachable or quota-exceeded
When main.py attempts to generate content
Then the script logs the error and exits with a non-zero code
And no partial content is written to Google Docs
```

---

## Mock API Examples

### Gemini Request

```json
{
  "model": "gemini-1.5-pro",
  "prompt": "Today is 2026-08-17. You are a Spiritual G-Code navigator...\nOutput: 1 awakening quote (EN + 繁中), ~100 word guidance (EN + 繁中)."
}
```

### Expected Output

```json
{
  "quote_en": "Every refactoring of the self begins with a single commit.",
  "quote_zh": "每一次自我的重構，都始於一次內心的提交。",
  "guidance_en": "Today's transit suggests... (~100 words)",
  "guidance_zh": "今日流日建議...（約100字）"
}
```

### Error — Gemini Timeout

```json
{
  "error": "Gemini API timeout after 30s",
  "code": "ai_timeout",
  "retry": true
}
```

---

## Dependencies

- `specs/bilingual-output.md` — Output format requirements
- Google AI Studio API key
- Google Cloud Service Account with Docs API access

---

## References

- [sync-g-code](https://github.com/Galen-Chu/sync-g-code) — Implementation
- [Gemini API](https://ai.google.dev/) — Google AI documentation
