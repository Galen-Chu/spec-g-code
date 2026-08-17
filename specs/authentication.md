# Spec: Authentication Model

> Shared authentication and authorization patterns across G-Code projects.

---

## Status

Approved (v1.0.0)

---

## Scope

Defines the auth mechanisms (session, JWT, API key), token storage
requirements, and permission models used across projects.

---

## Functional Requirements

### User Registration
- FR-1: Registration endpoint creates User + UserProfile atomically
- FR-2: Password validation (Django validators: similarity, length, common, numeric)
- FR-3: Registration returns JWT pair immediately (no separate login call needed)

### Login
- FR-4: JWT token pair (access + refresh) via `POST /api/auth/login/`
- FR-5: Access token: 1 hour; Refresh token: 7 days (configurable)
- FR-6: Refresh token rotation enabled

### Session Auth (Web UI)
- FR-7: Session authentication for browser-based UI
- FR-8: CSRF protection on all form submissions
- FR-9: OAuth `state` CSRF nonce for cloud-drive connections

### API Key Auth (Developer Access)
- FR-10: API keys stored as SHA-256 hash; plaintext shown only once on creation
- FR-11: Key format: `gcode_` + 48 hex chars
- FR-12: Support revoke (deactivate) and rotate (new key, same record)

### Token Security
- FR-13: OAuth tokens encrypted at rest (Fernet via `EncryptedTextField`)
- FR-14: Token refresh handled transparently by service layer
- FR-15: 401 on expired/invalid token; 403 on insufficient permissions

---

## Interface Contract

### DRF Configuration

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
}
```

### Permission Matrix

| Role | List | Create | Own Record | Others' Records |
|------|------|--------|------------|-----------------|
| Anonymous | Read-only* | — | — | — |
| Authenticated | Read | Create | Update/Delete | Read |
| Owner | Read | Create | Update/Delete | — |
| Admin | Full | Full | Full | Full |

*Only for public resources (codons, hexagrams, chakras)

---

## Dependencies

None (foundational spec)

---

## References

- [Simple JWT](https://django-rest-framework-simplejwt.readthedocs.io/)
- [Fernet encryption](https://cryptography.io/en/latest/fernet/)
