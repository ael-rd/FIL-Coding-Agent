# [PROJECT_NAME] — Dynamic Context · V1.0.0-beta
> FIL Coding Agent Edition · Operational memory layer.
> Updated every session. Committed after operator review.
> Authority: L5 — data only. Instructions live in AGENTS.md (L2).

---

## NCGL ACTIVE BLOCKS 🔷
> NCGL = Natural Constrained Governance Language
> Structured governance blocks for critical multi-session items.
> Scanned at every boot (Step 0B). Validated by the probabilistic interpreter.
>
> USE when 2+ of these conditions are true:
>   ✓ PRIORITY high or critical
>   ✓ EXPECTED_BEHAVIOR non-trivial (multi-condition or multi-session)
>   ✓ VALIDITY = specific date or structured frequency
>   ✓ Multi-session governance required
>
> NEVER use in Warm Zone or Cold Zone.
> Full syntax reference: CLAUDE.md → NCGL section · AGENTS.md → NCGL INTERPRETER

*(empty — add NCGL blocks here when needed)*

```
Quick reference — most common blocks:

TASK block:           ALERT block:          DECISION block:
  TASK: [title]         ALERT: [title]        DECISION: [title]
  STATUS: active        SEVERITY: high        TRUTH: user-confirmed
  TRUTH: verified       STATUS: active        VALIDITY: permanent
  VALIDITY: date·X      TRUTH: verified       REVERSIBLE: yes|no
  PRIORITY: high        VALIDITY: date·X      RATIONALE: ...
  SCOPE: hot            CAUSE: ...            IMPACT: ...
  CONTEXT: ...          ACTION: ...
                        FALLBACK: ...
```

---

## HOT ZONE 🔴
> Scanned every session. Max 100 lines.
> Contains only what must remain in active attention.

### CURRENT STATUS
```
Last session            : [YYYY-MM-DD]
Sprint / phase          : [e.g. MVP · v1.2 · bugfix sprint]
Agent focus             : [1-2 line description of what we're working on right now]
Git branch              : [current branch]
Last commit             : [hash · message]
Last known good commit  : [hash] ← updated after every successful FIL commit · used by 'recovery'
```

### TODO
```
Priority actions — tag with [v:date·YYYY-MM-DD] if deadline-sensitive:

[ ] [Task 1 — e.g. Implement auth middleware · [v:date·2026-07-25]]
[ ] [Task 2 — e.g. Fix pagination bug in /api/items]
[ ] [Task 3 — e.g. Write tests for UserRepository]
```

### BLOCKERS 🚧
```
*(empty — add blockers here · remove when resolved)*

Format:
⛔ [BLOCKER]: [description] · [since: DATE] · [waiting on: X]
```

### REMINDERS 🔔
```
*(empty — add active reminders · with [v:date·X] tags)*

Format:
🔔 [REMINDER] · [v:date·YYYY-MM-DD] or [v:session]
```

### ACTIVE DECISIONS 🧭
> Architectural or technical decisions made this sprint.
> Decisions that modify CLAUDE.md principles must be logged here first.

*(empty — add decisions here)*

```
Format:
DECISION  : [what was decided]
RATIONALE : [why]
IMPACT    : [files/modules affected]
REVERSIBLE: yes | no
DEVIATION : true | false  ← true if this deviates from CLAUDE.md
DATE      : [YYYY-MM-DD]
REVIEW_DATE: [YYYY-MM-DD or N/A]
```

### ACTIVE ALERTS ⚠️
```
*(empty — add active incidents · remove when resolved)*

Format:
⚠️ [ALERT]: [description] · [since: DATE] · [status: monitoring | blocked | mitigated]
```

---

## WARM ZONE 🟡
> Loaded on demand. Recent context from last 30 days.
> Not in active attention — available when relevant.

### Previous Session Summary
*(empty at initialization)*

```
[YYYY-MM-DD] — [2-4 line summary of what was done · what changed · what was decided]
```

### Recent Completed Tasks
*(empty)*

### Pending Reviews
*(empty — PRs · decisions · code reviews awaiting input)*

---

## COLD ZONE 🔵
> Permanent archive. Never deleted. Load explicitly only.
> Historical record of the project's operational memory.

### Session History
*(empty at initialization)*

```
Format:
[YYYY-MM-DD] · commit [hash] · [1-line summary]
```

### Archived Decisions
*(empty — decisions moved here once implemented and stable)*

### Expired Data
*(empty — [v:date] items that have passed their validity date)*

---

## LOADING PROTOCOL

```
Every session:
① CLAUDE.md — always first (governance · L1)
② DECISIONS.md — architectural decisions (L2)
③ LOG_ERRORS.md — PREVENTION ACTIVE section (prevention layer)
④ This file — Hot Zone fully · Warm Zone on demand (operational · L5)
   → Step 0B: scan NCGL ACTIVE BLOCKS · run probabilistic interpreter
⑤ skills.md — only if current task requires it (domain knowledge · L4)

Hot Zone → Warm Zone: completed tasks · expired data (at Step 7)
Warm Zone → Cold Zone: items older than 30 days (at Step 7)
Nothing is ever deleted — only moved down.
NCGL blocks: expired → inline summary in Warm Zone · deprecated → Cold Zone
```

---

*[PROJECT_NAME] Dynamic Context · FIL Coding Agent Edition V1.0.0-beta*
*Updated every session. Committed after operator review.*
