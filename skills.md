# [PROJECT_NAME] — Domain Knowledge (skills.md)
> FIL Coding Agent Edition · V1.0.0-beta
> Authority: L4 — informs decisions · never overrides governance (L1/L2).
> Load on demand only — not auto-loaded every session.
> Tag every technical claim with [truth:type] and [v:refresh] if it can change over time.

---

## LOADING RULE

```
Load this file ONLY when the current task requires it.
Never load the entire file — load the relevant section.
Do not keep in context across sessions unless actively used.
```

---

## STACK KNOWLEDGE

### [Primary Language] — [e.g. Python 3.12]

```
VERSION         : [e.g. 3.12.x] [truth:verified · 2026-07-20]
PACKAGE MANAGER : [e.g. uv · pip · poetry]
VIRTUAL ENV     : [e.g. .venv in project root]

Key conventions for this project:
→ [Convention 1] [truth:official]
→ [Convention 2] [truth:user-confirmed]
→ [Convention 3] [truth:estimated · confirm before relying on this]

Documentation: [URL] [v:refresh · check on major version bump]
```

### [Framework] — [e.g. FastAPI 0.111]

```
VERSION         : [e.g. 0.111.x] [truth:verified · 2026-07-20] [v:refresh]
ROUTER PATTERN  : [e.g. APIRouter per domain module]
DEPENDENCY INJ  : [e.g. Depends() for all shared services]
MIDDLEWARE      : [e.g. CORS · Auth · Logging]

Known gotchas for this project:
→ [Gotcha 1] [truth:user-confirmed]
→ [Gotcha 2] [truth:verified]

Documentation: [URL]
```

### [Database] — [e.g. PostgreSQL 16]

```
VERSION         : [e.g. 16.x] [truth:official] [v:refresh · on infra changes]
ORM             : [e.g. SQLAlchemy 2.x · async mode]
MIGRATION TOOL  : [e.g. Alembic]
CONNECTION POOL : [e.g. asyncpg · max 10 connections]

Naming conventions:
→ Tables: snake_case plural  [truth:official]
→ Columns: snake_case        [truth:official]
→ Indexes: idx_[table]_[col] [truth:official]
```

---

## DOMAIN KNOWLEDGE

> Project-specific knowledge that the agent needs to reason correctly.
> Generated and maintained here — not in CLAUDE.md or DYNAMIC.md.

### [Domain Area 1 — e.g. Authentication]

```
APPROACH        : [e.g. JWT · OAuth2 · session-based]
TOKEN_LIFETIME  : [e.g. access 15min · refresh 7d] [truth:official]
REFRESH_STRATEGY: [e.g. sliding window · rotate on use]

Key rules:
→ [Rule 1] [truth:official]
→ [Rule 2] [truth:user-confirmed]

References: [URL · doc · RFC] [v:refresh · on security updates]
```

### [Domain Area 2 — e.g. API Contracts]

```
VERSIONING      : [e.g. /v1/ prefix · breaking changes = new version]
PAGINATION      : [e.g. cursor-based · page+limit]
ERROR FORMAT    : [e.g. {error: code, message: str, details: list}]
AUTH HEADER     : [e.g. Authorization: Bearer <token>]

[truth:official · from API spec v[N]]
[v:refresh · check on contract updates]
```

### [Domain Area 3 — e.g. Testing Strategy]

```
UNIT TESTS      : [e.g. pytest · mock external calls]
INTEGRATION     : [e.g. testcontainers · real DB · ephemeral]
E2E             : [e.g. Playwright · against staging]
COVERAGE TARGET : [e.g. 80% minimum · 90% for auth module]
NAMING PATTERN  : [e.g. test_[unit]_[scenario]_[expected]]

[truth:user-confirmed · agreed in project kickoff]
```

---

## THIRD-PARTY INTEGRATIONS

> Document integrations that require specific knowledge to use correctly.

### [Service Name — e.g. Stripe]

```
VERSION / API   : [e.g. API v2 · SDK 8.x] [truth:verified · 2026-07-20] [v:refresh]
ENV VARIABLES   : [e.g. STRIPE_SECRET_KEY · STRIPE_WEBHOOK_SECRET]
WEBHOOK EVENTS  : [e.g. payment_intent.succeeded · charge.failed]
IDEMPOTENCY     : [e.g. always pass Idempotency-Key header on mutations]

Key patterns for this project:
→ [Pattern 1] [truth:user-confirmed]
→ [Pattern 2] [truth:verified]

Documentation: [URL]
```

---

## REGULATIONS & CONSTRAINTS

> Legal, compliance, or domain-specific rules that govern implementation.

```
[REG-01] [e.g. GDPR: user data must be deletable on request] [truth:official] [v:refresh · annually]
[REG-02] [e.g. PCI-DSS: never log card numbers] [truth:official]
[REG-03] [e.g. SOC2: all data encrypted at rest] [truth:official]
[REG-04] [project-specific regulatory constraint]

→ Tag with [truth:official] only if sourced from a signed contract or official doc.
→ Tag with [v:refresh] for any regulation that may change.
→ Add source URL for every regulatory claim.
```

---

## CACHE_TIER CLASSIFICATION

> Controls how frequently domain knowledge is revalidated.

```
live   → changes every session or daily (e.g. API rate limits · live prices)
slow   → changes monthly (e.g. third-party API versions · library versions)
stable → changes rarely (e.g. core domain rules · language conventions)

Assign CACHE_TIER when adding new sections above.
Step 2 (active work) applies [v:refresh] checks based on tier.
```

---

## CHANGELOG

```
V1.0.0-beta ([DATE])
→ Initial domain knowledge creation — FIL Coding Agent Edition V1.0.0-beta
→ Sections: [list sections added]
```

---

*skills.md · FIL Coding Agent Edition V1.0.0-beta*
*Load on demand · never auto-load · tag every claim.*
