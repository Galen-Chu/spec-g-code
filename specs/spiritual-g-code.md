# Spec: Spiritual G-Code

> Django platform for astrology dashboards, natal charts, and AI-powered
> spiritual insights.

---

## Status

Draft

---

## Scope

Defines the functional requirements, API surface, and data models for
the spiritual dashboard platform.

---

## Functional Requirements

### Natal Chart Engine
- FR-1: Calculate planetary positions (Sun through Pluto) at birth using PyEphem
- FR-2: Compute zodiac signs, degrees, ascendant, and dominant elements
- FR-3: Auto-create natal chart on user registration from birth data
- FR-4: Support birth date, time, location, and timezone inputs

### Transit Calculation
- FR-5: Daily transit calculation via cron (GitHub Actions 00:00 UTC)
- FR-6: Aspect analysis between current transits and natal chart
- FR-7: G-Code intensity score (1–100) derived from aspect harmony

### AI Interpretation
- FR-8: Gemini API integration for personalized daily guidance
- FR-9: Bilingual output (EN + 繁體中文) for all interpretations
- FR-10: Fallback to mock calculator when Gemini API unavailable

### Dashboard
- FR-11: Interactive Chart.js visualizations (frequency, trends, radar)
- FR-12: D3.js natal wheel (zodiac circle with planet positions)
- FR-13: Solar system transit visualization (three.js)
- FR-14: Chart annotations (user notes on data points)
- FR-15: Date-range comparison (side-by-side analysis)

---

## Interface Contract

### REST API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register/` | Register + auto-create natal chart |
| POST | `/api/auth/login/` | JWT auth |
| GET | `/api/natal/calculate/` | Calculate natal chart from birth data |
| GET | `/api/natal/wheel/` | Natal wheel data |
| GET | `/api/dashboard/overview/` | Dashboard summary |
| GET | `/api/dashboard/charts/` | Chart data |
| GET | `/api/solar-system/transits/` | Solar system transit data |
| GET | `/api/health/` | Health check |

### Key Data Models

```
GCodeUser(AbstractUser)
  ├── birth_date, birth_time, birth_location, timezone
  └── OneToOne → UserProfile (reputation, badges)

NatalChart
  ├── user FK, chart_data JSON, sun_sign, moon_sign
  ├── ascendant, dominant_elements JSON, key_aspects JSON

DailyTransit
  ├── user FK, transit_date, transit_data JSON
  ├── g_code_score (1-100), intensity_level
  └── themes, interpretation, affirmation, practical_guidance

GeneratedContent
  ├── user FK, content_type, platform, content
  └── status, metadata JSON
```

---

## Dependencies

- `specs/api-conventions.md` — REST API design patterns
- `specs/bilingual-output.md` — Output language requirements
- `specs/authentication.md` — Auth model

---

## References

- [spiritual-g-code](https://github.com/Galen-Chu/spiritual-g-code) — Implementation
- [PyEphem](https://rhodesmill.org/pyephem/) — Astronomy library
