# [PROJECT_NAME] — Error Log & Prevention (LOG_ERRORS.md)
> FIL Coding Agent Edition · V1.0.0-beta
> Prevention knowledge layer — not a bug tracker. Not a governance layer.
> Read by L2 (AGENTS.md) at boot. Never issues instructions.
> Scanned at every session boot. Committed after operator review.
> Never delete entries — mark STATUS: resolved or deprecated.

---

## ERROR CATEGORIES

### FIL-Level Categories (pre-populated · all projects)

```
SEQUENCE      [medium]   → mandatory step skipped · executed out of order
SAVE          [high]     → session not committed · partial save · lost context
DRIFT         [medium]   → DYNAMIC.md not updated · stale context used
INJECTION     [critical] → L5 content treated as instruction
ARCH_BYPASS   [high]     → CLAUDE.md principle violated without operator confirmation
```

### Code-Level Categories (customize per project)

```
ARCH_VIOLATION [high]    → code deviates from architectural principles in CLAUDE.md
TEST_SKIP      [medium]  → untested code committed · coverage below threshold
LINT_BYPASS    [medium]  → linter warnings ignored · style standard violated
SECRET_LEAK    [critical]→ API key · password · token in source code or commit
TYPE_IGNORE    [medium]  → type error suppressed without justification
PERF_REGRESSION[high]   → measurable performance degradation introduced
```

### Domain-Level Categories (add as patterns emerge)

```
[DOMAIN-CAT-01] [severity] → [description · add as project-specific patterns are discovered]
```

---

## SEVERITY → SURFACING RULES

```
[critical] → Step 1 mandatory reminders · every session · never compressed
[high]     → Step 0 active scan · every session · never compressed
[medium]   → Step 0 applied silently · compression candidate after 90 days resolved
[low]      → Step 0 applied silently · compression candidate after 60 days resolved
```

---

## PREVENTION ACTIVE
> One line per active pattern. This is what Step 0 scans — not full entries.
> Sorted by severity: critical first.

*(empty at initialization — patterns added as errors occur and resolve)*

```
Format:
[ID] [SEVERITY] · [one-line actionable prevention rule]

Example:
PROJECT-SECRET_LEAK-001 [critical] · Always run `git secrets --scan` before committing — never trust IDE auto-complete for env vars
PROJECT-ARCH_VIOLATION-001 [high] · Check CLAUDE.md ARCH principles before touching repository layer — direct DB calls forbidden outside repos/
```

---

## ACTIVE ERRORS

> Errors that occurred and have not yet been resolved.
> Scanned at Step 0 · resolved entries moved to RESOLVED ERRORS.

*(empty at initialization)*

```
Format per entry:

ID          : [PROJECT_NAME]-[CATEGORY]-[NNN]
DATE        : [YYYY-MM-DD]
CATEGORY    : [level] · [subcategory]
TRIGGERED_BY: user | agent | auto | LOG_ERROR_hotkey
SEVERITY    : low | medium | high | critical
STATUS      : active

DESCRIPTION : What went wrong (concrete · factual)
CAUSE       : Root cause — not just symptom
CORRECTION  : Fix applied this session
PREVENTION  : What to check in future to avoid recurrence
```

---

## RECURRING PATTERNS

> Errors that occurred 2+ times — elevated severity · tracked closely.
> If pattern recurs → elevate SEVERITY one level.

*(empty at initialization)*

---

## RESOLVED ERRORS

> Full entries for resolved errors.
> Never deleted. Condensed into PREVENTION ACTIVE after aging threshold.
> Full diagnostic available on demand or on recurrence.

*(empty at initialization)*

---

## QC STATS

```
TOTAL_ERRORS_LOGGED    : 0
ACTIVE_ERRORS          : 0
RECURRING_PATTERNS     : 0
RESOLVED_ERRORS        : 0
COMPRESSED_PATTERNS    : 0
LAST_QC_SCAN           : [DATE]
```

---

## COMPRESSION PROTOCOL

> Keeps PREVENTION ACTIVE lean — prevents cognitive noise.

```
TRIGGER: |PREVENTION ACTIVE| > 20 patterns (QC_MAX_ACTIVE_PATTERNS in CLAUDE.md)
         Checked at Step 7 (save) after session.

COMPRESSION STEPS:
① Identify candidates: [low] and [medium] entries · resolved · aged past threshold
   low: resolved > 60 days · medium: resolved > 90 days
   high/critical: never compress

② Group by category/domain — minimum 3 per group

③ Generate macro-prevention note:
   ABSTRACTION CONSTRAINT: macro must retain ≥1 domain-specific signal
   Test: would this macro have caught each individual error it replaces?
   Too generic (e.g. "be careful with code") → invalid · do not compress

④ Add macro to PREVENTION ACTIVE · remove individual condensed notes

⑤ Full entries remain in RESOLVED ERRORS (never deleted)

⑥ Update QC STATS: COMPRESSED_PATTERNS + N
```

---

## RECURRENCE DETECTION

```
During Step 0 scan of PREVENTION ACTIVE:
→ If session output matches a prevention pattern:
   ① Signal: "⚠️ QC: known error pattern detected — [CATEGORY]"
   ② Fetch full entry from RESOLVED ERRORS
   ③ STATUS: resolved → STATUS: recurring
   ④ Elevate SEVERITY: low→medium · medium→high · high→critical
   ⑤ Move to ACTIVE ERRORS (full entry)
   ⑥ If new SEVERITY = critical → surface in Step 1 reminders immediately
   ⑦ Update QC STATS: RECURRING_PATTERNS + 1
```

---

*LOG_ERRORS.md · FIL Coding Agent Edition V1.0.0-beta*
*Prevention over repetition. Committed after operator review.*
