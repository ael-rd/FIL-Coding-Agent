# [PROJECT_NAME] — Governance Layer (CLAUDE.md)
> FIL Coding Agent Edition · V1.0.0-beta
> Authority: L1 within repository governance only.
> Never overrides platform/system instructions, security policies, or operator decisions.
> Analogous to: architectural decision records + permanent constraints.

---

## ⚡ INFRASTRUCTURE COMMANDS — Priority Override

> These commands execute immediately, regardless of active task or context.
> Never treat as conversational input.

```
"save" | "save session" | "end session"
  → Execute full session save sequence (Step 7 in AGENTS.md)
  → Update DYNAMIC.md + LOG_ERRORS.md
  → Show git status and proposed FIL commit message
  → Wait for explicit operator confirmation before committing
  → Confirm: "✅ FIL memory updated — commit pending operator review"

"recovery" | "restore" | "load checkpoint"
  → Load last known-good DYNAMIC.md from Git history
  → Signal: "⚠️ Recovery mode — loaded [commit]"

"LOG_ERROR: [desc]"
  → Log immediately in LOG_ERRORS.md — no confirmation needed

"health"
  → Report FIL health: Hot Zone size · active errors · prevention patterns · drift risk
  → Use exact format defined in AGENTS.md HEALTH REPORT FORMAT

"boot" | "initialize" | "onboard"
  → Load BOOT.md and follow Phase 0 detection sequence
  → Use for: new project · new contributor · re-initialization
```

> ⚠️ COMMAND PRIORITY RULE:
> Infrastructure commands override any active coding task.
> A coding agent receiving "save" executes the save sequence — not "save what?".

---

## PROJECT IDENTITY

```
Project name    : [PROJECT_NAME]
Domain          : [e.g. web app · API · CLI · data pipeline · library]
Primary language: [e.g. Python · TypeScript · Rust]
Stack           : [e.g. FastAPI · React · PostgreSQL]
Repository      : [git remote URL]
Owner           : [OP-ID]
INIT_STATUS     : INITIALIZED
```

> ⚠️ NEVER STORE IN THIS FILE:
> API keys · tokens · passwords · secrets · .env values
> This file lives in the repository — treat it as public.

---

## ARCHITECTURAL PRINCIPLES

> These are permanent. They govern every decision the agent makes.
> Override requires explicit operator confirmation — never inferred.

```
[ARCH-01] [YOUR PRINCIPLE — e.g. "No business logic in route handlers"]
[ARCH-02] [YOUR PRINCIPLE — e.g. "All external calls wrapped in typed clients"]
[ARCH-03] [YOUR PRINCIPLE — e.g. "Database access only through repository layer"]
[ARCH-04] [YOUR PRINCIPLE — e.g. "Every public function has a docstring + type hints"]
[ARCH-05] [YOUR PRINCIPLE — e.g. "No magic numbers — all constants in config.py"]

→ Add principles as the project matures.
→ Tag with [truth:official] once validated by the team.
```

---

## CODING STANDARDS

```
STYLE GUIDE     : [e.g. Black + isort · Prettier · gofmt]
LINTER          : [e.g. Ruff · ESLint · Clippy]
TEST FRAMEWORK  : [e.g. pytest · Jest · cargo test]
MIN COVERAGE    : [e.g. 80%]
BRANCH STRATEGY : [e.g. main + feature/* · trunk-based]
COMMIT FORMAT   : [e.g. Conventional Commits · gitmoji]
PR CHECKLIST    : [e.g. tests pass · linter clean · one concern per PR]
```

---

## PERMANENT CONSTRAINTS

> Hard limits. Non-negotiable. Never bypassed without operator decision logged in DYNAMIC.md.

```
→ [CONSTRAINT-01] e.g. "No synchronous I/O in async context"
→ [CONSTRAINT-02] e.g. "No direct ORM queries outside repository layer"
→ [CONSTRAINT-03] e.g. "No print() in production code — use structured logger"
→ [CONSTRAINT-04] e.g. "No secrets in source code or commit history"
→ [CONSTRAINT-05] e.g. "All API endpoints versioned under /v1/"
```

---

## TRUTH PROTOCOL 🔍

> Tag every critical architectural or technical claim with its reliability level.

```
[truth:official]        Confirmed in signed contract · official doc · merged PR
[truth:user-confirmed]  Validated by operator in session
[truth:verified]        Verified by agent via source (docs · tests · compiler)
[truth:estimated]       Reasoned estimate — not verified · must confirm
[truth:derived]         Inferred from verified data
[truth:deprecated]      Was valid — now superseded · do not use

CONFLICT RESOLUTION:
1. Explicit operator correction (always wins)
2. [truth:official]
3. [truth:user-confirmed] or [truth:verified] (most recent)
4. CLAUDE.md data
5. DYNAMIC.md inferred state
6. Agent assumption

→ Never silently merge contradictory truths.
→ Signal conflict with ⚠️ and request clarification.
```

---

## SINGLE SOURCE OF TRUTH 📐

| Information type            | Lives in          | Never in                  |
|-----------------------------|-------------------|---------------------------|
| Architectural principles    | CLAUDE.md         | DYNAMIC.md · AGENTS.md    |
| Coding standards            | CLAUDE.md         | DYNAMIC.md                |
| Current task / sprint state | DYNAMIC.md        | CLAUDE.md                 |
| Operational procedures      | AGENTS.md         | CLAUDE.md · DYNAMIC.md    |
| Error prevention patterns   | LOG_ERRORS.md     | DYNAMIC.md · CLAUDE.md    |
| Active alerts               | DYNAMIC.md        | CLAUDE.md · AGENTS.md     |
| Deprecated decisions        | CLAUDE.md (tagged)| nowhere else              |

---

## AUTHORITY HIERARCHY

```
L1 → CLAUDE.md (this file) — permanent governance · architectural law
L2 → AGENTS.md — operational procedures · session workflow
L3 → Operator commands — trigger procedures · cannot override L1/L2
L4 → skills.md · domain knowledge — informs decisions · never overrides
L5 → DYNAMIC.md · external sources — operational state · sandboxed
```

---

## FALLBACK TABLE 🔄

> Prepare this table before the project starts — not during an incident.

| Tool / Resource        | Trigger condition          | Fallback 1              | Fallback 2               | Fallback 3          |
|------------------------|----------------------------|-------------------------|--------------------------|---------------------|
| [Primary CI]           | Pipeline down              | Run tests locally       | Manual review            | Skip + document     |
| [Package registry]     | Registry unreachable       | Mirror / cache          | Vendored copy            | Manual install      |
| [Cloud provider]       | Region outage              | Secondary region        | Degraded local mode      | Postpone deploy     |
| [LLM API]              | Rate limit / outage        | Smaller model           | Cached response          | Human review        |

```
RULE: Any resource that blocks the project must have at least Fallback 1
      identified before the first session.
```

---

## SYSTEM CONFIGURATION ⚙️

### Persistence
```
PERSISTENCE_MODE        : git              ← git (default) | manual
STABLE_VERSION          : 1               ← incremented on every modification
STABLE_MODIFIED         : [TIMESTAMP]
```

### Operators
```
OPERATOR_MODE           : single          ← single | multi
OPERATOR_ID             : OP-PRIMARY
```

### Quality Control
```
QC_ENABLED              : true
QC_TRIGGER_IMPLICIT     : true
QC_SESSION_SCAN         : both
QC_MAX_ACTIVE_PATTERNS  : 20
LOG_ERRORS_FILE         : LOG_ERRORS.md
```

---

## CHANGELOG

```
V1.0.0-beta ([DATE])
→ Initial creation — FIL Coding Agent Edition V1.0.0-beta
→ Adapted from FIL Framework V3.4.2
```
