# Spec: Bilingual Output Requirements

> All user-facing output must be in English and Traditional Chinese.

---

## Status

Draft

---

## Scope

Defines the bilingual output mandate for all G-Code projects — READMEs,
API responses, AI-generated content, and code comments.

---

## Functional Requirements

### Output Format
- FR-1: Every user-facing string produced in **both** English and Traditional Chinese (繁體中文)
- FR-2: **No Pinyin** — use Traditional Chinese characters, not romanization
- FR-3: English first, Chinese second (consistent ordering)

### README Format
- FR-4: Section headings: `## 🚀 Quick Start · 快速開始` (bilingual with `·` separator)
- FR-5: Paragraphs: English paragraph followed by Chinese paragraph
- FR-6: Bullets: inline pairs — `**Feature name** 功能名稱 — description 說明`
- FR-7: Tables: description cells use `English · 中文` format
- FR-8: Code blocks remain language-neutral (comments may be bilingual)

### AI-Generated Content
- FR-9: Gemini prompts must explicitly request bilingual output
- FR-10: Output format: awakening quote (EN + 繁中), guidance (~100 words EN + ~100 words 繁中)
- FR-11: Tone in both languages: rational, technical, grounding

### API Responses
- FR-12: Error messages may include bilingual text
- FR-13: Field labels and display names support both languages
- FR-14: Not a hard requirement for machine-readable fields (IDs, URLs, timestamps)

### Code Comments
- FR-15: Public-facing docstrings in both languages (preferred)
- FR-16: Internal comments in either language (developer preference)

---

## Terminology Glossary

| English | 繁體中文 | Notes |
|---------|---------|-------|
| natal chart | 本命盤 | Astrology term |
| transit | 流日 | Current planetary movement |
| hexagram | 卦 | I Ching term |
| codon | 密碼子 | Genetics term |
| chakra | 脈輪 | Energy center |
| sephira | 源質 | Kabbalah term |
| Flight Log | 飛行日誌 | Sync G-Code's daily log |
| Aetheric Pilot | 先行者 | Target audience (HSP) |
| G-Code | G-Code | Untranslated (brand name) |

---

## Implementation Notes

### README Bilingual Pattern

```markdown
## 🚀 Quick Start · 快速開始

Install dependencies and run the server.
安裝依賴並啟動伺服器。

```bash
pip install -r requirements.txt
python manage.py runserver
```
```

### Gemini Prompt Bilingual Pattern

```python
prompt = f"""
Please respond in both English and Traditional Chinese (繁體中文).
請以���文和繁體中文回覆。
Output format:
1. Awakening Quote / 覺醒金句: [EN] / [繁中]
2. Navigation Guidance / 導航建議: [~100 words EN] / [~100字 繁中]
"""
```

---

## Dependencies

None (foundational spec)

---

## References

- [skill-g-code SKILL.md](https://github.com/Galen-Chu/skill-g-code) — Source of the bilingual mandate
