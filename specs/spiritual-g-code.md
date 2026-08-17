# Spec: Spiritual G-Code

> Django platform for astrology dashboards, natal charts, and AI-powered
> spiritual insights.

---

## Status

Approved (v1.0.0)

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

## Acceptance Criteria

### Natal Chart (FR-1–4)

```gherkin
Given a user with birth_date=1990-06-15, birth_time=14:30, birth_location=Taipei
When POST /api/natal/calculate/ is called
Then the response contains chart_data with 10 planetary positions
And sun_sign, moon_sign, ascendant are valid zodiac names
And dominant_elements sums to a positive integer

Given a user without birth_time
When a natal chart is calculated
Then birth_time defaults to 00:00 and the chart is still generated
```

### Transit Score (FR-5–7)

```gherkin
Given a user with a natal chart
When the daily transit cron runs
Then g_code_score is an integer between 1 and 100
And intensity_level is one of: low, medium, high
And at least one theme tag is present
```

---

## Mock API Examples

### Calculate Natal Chart

```http
POST /api/natal/calculate/
Authorization: Bearer <token>

{
  "birth_date": "1990-06-15",
  "birth_time": "14:30",
  "birth_location": "Taipei, Taiwan",
  "timezone": "Asia/Taipei"
}
```

```json
{
  "chart_data": {
    "sun": {"sign": "Gemini", "degree": 24.3, "longitude": 84.3},
    "moon": {"sign": "Cancer", "degree": 8.2, "longitude": 98.2},
    "ascendant": "Leo",
    "dominant_elements": {"fire": 2, "earth": 3, "air": 4, "water": 1}
  },
  "sun_sign": "Gemini",
  "moon_sign": "Cancer",
  "key_aspects": [
    {"bodies": ["sun", "moon"], "type": "square", "orb": 2.1}
  ]
}
```

### Error — Invalid Birth Date

```json
{
  "error": "Invalid birth_date format. Use YYYY-MM-DD.",
  "code": "validation_error",
  "details": {"birth_date": "Date must be in the past"}
}
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
