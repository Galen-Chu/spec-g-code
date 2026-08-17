# Spec: REST API Conventions

> Design patterns for all G-Code REST APIs.

---

## Status

Draft

---

## Scope

Defines endpoint naming, response formats, error handling, pagination,
and status code conventions.

---

## Functional Requirements

### Endpoint Design
- FR-1: Resources are plural, kebab-case (`/api/codon-requests/`)
- FR-2: Custom actions use verb endpoints (`/api/files/rename/`)
- FR-3: Nested resources under parent (`/api/discussions/{id}/comments/`)
- FR-4: Config/metadata under `/api/config/`

### Response Format
- FR-5: Success responses include `count`, `results` or `data` key
- FR-6: List responses paginated (default 50, max 200)
- FR-7: Pagination metadata: `page`, `page_size`, `total`, `has_next`, `next_page`

### Error Handling
- FR-8: Errors return FHIR `OperationOutcome`-style objects where applicable:
  ```json
  {
    "error": "human-readable message",
    "code": "machine_code",
    "details": { ... }
  }
  ```
- FR-9: Validation errors return field-level details:
  ```json
  {
    "field_name": ["error message 1", "error message 2"]
  }
  ```

### Status Codes

| Code | Usage |
|------|-------|
| 200 | Success (GET, PATCH, PUT) |
| 201 | Created (POST) |
| 204 | No content (DELETE) |
| 400 | Bad request / validation error |
| 401 | Not authenticated |
| 403 | Authenticated but forbidden |
| 404 | Not found |
| 409 | Conflict (duplicate, race condition) |
| 429 | Rate limited |
| 500 | Server error |

### Rate Limiting
- FR-10: Default throttle: 60/min anonymous, 300/min authenticated
- FR-11: Rate limit headers included in 429 responses

### Streaming/Download
- FR-12: File downloads use `Content-Disposition: attachment; filename="..."`
- FR-13: Preview mode uses `?inline=1` (no attachment header)
- FR-14: Large responses use `StreamingHttpResponse` with chunked transfer

---

## Interface Contract

### Standard List Response

```json
{
  "count": 50,
  "total": 120,
  "page": 1,
  "page_size": 50,
  "has_next": true,
  "next_page": 2,
  "results": [ ... ]
}
```

### Standard Error Response

```json
{
  "error": "Detailed error message",
  "code": "validation_error",
  "details": {
    "field": "issue description"
  }
}
```

---

## Dependencies

- `specs/authentication.md` — Auth for protected endpoints

---

## References

- [DRF](https://www.django-rest-framework.org/)
- [JSON:API](https://jsonapi.org/) — Response format inspiration
