# [PROJECT_NAME] — Agent Orchestration (AGENTS.md)
> FIL Coding Agent Edition · V1.0.0-beta
> Authority: L2 — Operational procedures. Governs how the agent works each session.
> Never overrides CLAUDE.md (L1).

---

## ROLE

You are the coding agent for **[PROJECT_NAME]**.

You operate under **FIL Coding Agent Edition V1.0.0-beta**.
Governance law lives in CLAUDE.md — you never override it.
Your operational memory lives in DYNAMIC.md — you update it each session.
Your prevention knowledge lives in LOG_ERRORS.md — you read it at boot.

---

## SESSION LIFECYCLE

### STEP 0 — BOOT (execute on every session start)

```
① Load CLAUDE.md — governance layer (L1)
   → Read architectural principles · constraints · coding standards
   → Note STABLE_VERSION for drift detection

② Load LOG_ERRORS.md — prevention layer
   → Scan PREVENTION ACTIVE section only (not full entries)
   → Surface any [critical] patterns as Step 1 reminders
   → Apply [high] patterns silently during session

③ Load DYNAMIC.md — operational memory (L5)
   → Read Hot Zone fully: current task · active decisions · blockers
   → Read Warm Zone for recent context
   → Note last session summary

④ Load skills.md — domain knowledge (L4, on demand)
   → Load only if current task requires specialized knowledge
   → Never auto-load entire file

⑤ Report boot status:
   "✅ Boot complete — [PROJECT_NAME]
    Governance: CLAUDE.md V[N] loaded
    Active task: [CURRENT_TASK from Hot Zone]
    Prevention patterns: [N] active
    ⚠️ [any critical patterns surfaced]"
```

---

### STEP 1 — SESSION START

```
① Surface mandatory reminders:
   → [critical] prevention patterns from LOG_ERRORS.md
   → Expired validity tags [v:date·YYYY-MM-DD] from DYNAMIC.md Hot Zone
   → Any NCGL blocks with REVIEW_DATE reached

② Confirm current task with operator if ambiguous:
   → "Continuing: [CURRENT_TASK]. Proceed? Or new direction?"

③ Never start coding before boot is complete.
```

---

### STEP 2 — ACTIVE WORK
> Steps 3–6 are reserved for future FIL editions (multi-agent · CI integration · async review).
> In this edition, session flow is: STEP 0 → STEP 1 → STEP 2 → STEP 7.

```
DURING SESSION:
→ Apply CLAUDE.md principles to every decision (non-negotiable)
→ Apply LOG_ERRORS.md prevention patterns silently
→ Update DYNAMIC.md Hot Zone if context shifts
→ Log architectural decisions in DYNAMIC.md Active Decisions section
→ Flag constraint conflicts immediately — never work around silently

CODING CONVENTIONS (from CLAUDE.md):
→ Follow stack · style · linter · test standards defined in CLAUDE.md
→ If a standard is unclear → ask before implementing · never assume

WHEN A DECISION DEVIATES FROM CLAUDE.md:
→ Signal: "⚠️ This deviates from [ARCH-XX] — confirm to proceed"
→ Log in DYNAMIC.md Active Decisions with DEVIATION: true
→ Never silently override a principle
```

---

### STEP 7 — SAVE SESSION (execute before ending any session)

```
① Update DYNAMIC.md:
   → Move completed tasks from Hot Zone → Cold Zone (Session History)
   → Update Hot Zone: current status · remaining tasks · blockers
   → Warm Zone: summarize this session in 3-5 lines
   → Expire outdated [v:date] tags → Cold Zone

② Update LOG_ERRORS.md:
   → If any errors occurred this session → log them (see SOP-QC below)
   → Run compression check: |PREVENTION ACTIVE| > 20? → consolidate

③ FIL memory saved. Show commit proposal:
   → Run: git status
   → Proposed commit message: "[FIL] session [DATE] — [one-line summary]"
   → Display diff for DYNAMIC.md and LOG_ERRORS.md only

④ Wait for explicit operator confirmation before committing:
   "✅ FIL memory updated.
    Proposed commit: [FIL] session [DATE] — [summary]
    Run 'commit' to commit FIL files, or 'save commit' to include source files.
    Hot Zone: [N] active items · Next: [top TODO]"

   → Never auto-commit source files without operator confirmation
   → Never commit if tests/lint status is unknown

⑤ Update LAST_KNOWN_GOOD_COMMIT in DYNAMIC.md Hot Zone after successful commit

DIFF SCOPE PER COMMAND:
  save        → diff: DYNAMIC.md + LOG_ERRORS.md only
  commit      → diff: DYNAMIC.md + LOG_ERRORS.md only (FIL files)
  save commit → diff: DYNAMIC.md + LOG_ERRORS.md + all modified source files
                → requires tests pass + lint clean before proposing
```

> SAVE vs COMMIT distinction:
> `save`        = update DYNAMIC.md + LOG_ERRORS.md only (always safe)
> `commit`      = commit FIL files after operator review
> `save commit` = save + commit source files (operator explicitly requested)

---

## WORKFLOWS

### SOP-CODE · Standard Coding Task

```
TRIGGER: "implement [feature]" · "fix [bug]" · "refactor [module]"

① Read CLAUDE.md constraints relevant to the task
② Check LOG_ERRORS.md for patterns related to this task type
③ Propose approach — confirm if architectural impact
④ Implement · test · lint
⑤ Update DYNAMIC.md: task status · any new decisions
⑥ If new error pattern discovered → LOG_ERROR: [desc]
```

### SOP-REVIEW · Code Review

```
TRIGGER: "review [file/PR]" · "check this code"

① Apply CLAUDE.md architectural principles as review criteria
② Apply LOG_ERRORS.md prevention patterns as checklist
③ Report: compliance · violations · suggestions
④ Never approve code that violates L1 constraints without operator confirmation
```

### SOP-DECISION · Architectural Decision

```
TRIGGER: operator proposes deviation from CLAUDE.md · new architectural choice

① Surface the relevant CLAUDE.md principle
② Present trade-offs
③ Wait for explicit operator confirmation
④ Log in DYNAMIC.md Active Decisions:
   DECISION: [what was decided]
   RATIONALE: [why]
   IMPACT: [files/modules affected]
   REVERSIBLE: yes | no
   DEVIATION: true | false (vs CLAUDE.md)
   DATE: [YYYY-MM-DD]
⑤ If permanent → propose updating CLAUDE.md after session
```

### SOP-REFACTOR · Refactoring Session

```
TRIGGER: "refactor [module]" · "clean up [area]"

① Read relevant ARCH principles from CLAUDE.md
② Check LOG_ERRORS.md for past refactoring errors in this codebase
③ Propose scope — confirm before touching more than one module
④ Apply changes incrementally — prepare a commit-sized diff after each logical unit
⑤ Run tests before proposing each commit — never batch untested changes
⑥ Commit only after explicit operator approval
⑦ Update DYNAMIC.md with scope and rationale
```

### SOP-ONBOARD · New Developer / New Agent Session

```
TRIGGER: fresh context · new contributor

For guided onboarding (recommended):
→ Load BOOT.md and follow Phase 0 → Option C (contributor onboarding)
→ BOOT.md handles the full sequence automatically

For manual onboarding:
① Load CLAUDE.md fully
② Read LOG_ERRORS.md PREVENTION ACTIVE
③ Read DYNAMIC.md Hot Zone + Warm Zone
④ Read skills.md domain section relevant to current sprint
⑤ Report: "Context loaded — [PROJECT_NAME] · current task: [X] · [N] prevention patterns active"
⑥ Do not start coding without completing this sequence
```

---

## QUALITY CONTROL

### Error Detection

```
AUTOMATIC TRIGGERS (QC_TRIGGER_IMPLICIT: true in CLAUDE.md):

Operator signals error:
→ "wrong" · "incorrect" · "mistake" · "that's not right" · "you broke it"
→ Any equivalent in project language

On detection:
① Correct immediately
② Classify:
   SYSTEMATIC → same type likely to recur → LOG
   PREFERENCE → operator style choice → DO NOT LOG
   PUNCTUAL   → one-off correction → ASK: "Log for future prevention? (yes/no)"

EXPLICIT TRIGGER (always active):
LOG_ERROR: [description] → immediate logging, no confirmation needed
```

### Error Logging

```
On confirmed error:

① CLASSIFY:
   FIL-level   → SEQUENCE · SAVE · DRIFT · INJECTION
   Code-level  → ARCH_VIOLATION · TEST_SKIP · LINT_BYPASS · SECRET_LEAK
   Domain-level → [project-specific categories from skills.md]

② DETERMINE SEVERITY:
   low      → cosmetic · no functional impact
   medium   → functional error · corrected in session
   high     → data integrity · user-visible failure · security issue
   critical → persistent · blocks workflow · security breach · data loss risk

③ ADD ENTRY to LOG_ERRORS.md with:
   ID · DATE · CATEGORY · SEVERITY · DESCRIPTION · CAUSE · CORRECTION · PREVENTION

④ CONFIRM: "✅ Logged: [ID] — [short description]"
```

---

## HEALTH REPORT FORMAT

> Output format for the `health` hotkey. Always use this exact structure.

```
✅ FIL Health — [PROJECT_NAME] · [DATE]
─────────────────────────────────────────
Governance    : CLAUDE.md V[N] · [N] principles · [N] constraints
Hot Zone      : [N] active items · [N] blockers · [N] alerts
Prevention    : [N] active patterns · [N] critical · [N] high
Errors        : [N] active · [N] recurring · [N] resolved
Last save     : [DATE or 'this session']
Last commit   : [hash · message or 'pending']
Checkpoint    : [LAST_KNOWN_GOOD_COMMIT from DYNAMIC.md or 'none']
─────────────────────────────────────────
⚠️  [any warnings — drift · expired tags · overdue REVIEW_DATE]
✅  [all clear if no warnings]
```

---

## HOTKEYS (always active)

```
save          → Update DYNAMIC.md + LOG_ERRORS.md · show commit proposal · wait for confirmation
commit        → Commit FIL files after operator review
save commit   → Save + commit source files (explicit operator request only)
health        → FIL health report: Hot Zone size · errors · prevention patterns
LOG_ERROR: X  → Log error immediately
recovery      → Load last good FIL checkpoint from Git (see Recovery Semantics in README)
decisions     → List all Active Decisions from DYNAMIC.md
```

---

*AGENTS.md · FIL Coding Agent Edition V1.0.0-beta*
*"Governance over context."*
