# FIL Coding Agent Edition · V1.0.0-beta
> "Governance over context."
> Adapted from FIL Framework V3.4.2 for long-running coding agent workflows.

---

## What This Is

A governance framework for coding agents on long-running projects.
Not another context dump. Not a prompt template.

**The core insight:** as a codebase grows, the problem isn't that the agent lacks context.
It's that all context looks equally important. FIL fixes that by making explicit
what governs decisions vs. what merely informs them.

---

## The Seven Files

| File | Role | Authority | Changes |
|------|------|-----------|---------|
| `CLAUDE.md` | Governance layer — architectural law | L1 within repository governance | Rarely |
| `AGENTS.md` | Operational procedures — session workflow | L2 | Occasionally |
| `DECISIONS.md` | Architectural decisions — permanent ADR record | L2 | On new decision |
| `DYNAMIC.md` | Operational memory — current state | L5 | Every session |
| `skills.md` | Domain knowledge — informs decisions | L4 | On update |
| `LOG_ERRORS.md` | Prevention knowledge — distilled lessons | Prevention layer | As errors occur |
| `BOOT.md` | Initialization wizard — setup only · generates all files | — | Rarely |

---

## How It Works — Full Flow

### 1. Setup (once)

Copy the 6 files to your repo root, then tell your agent:

```
Load BOOT.md and follow the FIL Boot sequence.
```

The agent interviews you one question at a time — about 15 questions — and generates all 5 project files fully populated. No `[placeholder]` to fill manually. It then proposes a first commit: `chore: initialize FIL Coding Agent V1.0.0-beta`. You review and confirm.

→ See [**Setup**](#setup) below for full details (Option A guided · Option B manual).

---

### 2. Every working session

At startup, the agent automatically executes Step 0:

```
① Loads CLAUDE.md — reads your architectural principles, constraints, coding standards
② Loads DECISIONS.md — applies permanent architectural decisions alongside CLAUDE.md
③ Scans LOG_ERRORS.md PREVENTION ACTIVE — silently applies past error patterns
④ Loads DYNAMIC.md Hot Zone — resumes exactly where you left off
   (current task · blockers · active decisions)
⑤ Reports:
   ✅ Boot complete — [PROJECT] · 3 prevention patterns · 5 architectural decisions · Hot Zone: 2 active items
```

During work, if you propose something that violates a CLAUDE.md principle, the agent signals:

```
⚠️ This deviates from [ARCH-02] — confirm to proceed
```

Never silently applied.

---

### 3. End of session

Type `save`:

```
→ Agent updates DYNAMIC.md + LOG_ERRORS.md
→ Shows diff of both files
→ Proposes: "[FIL] session YYYY-MM-DD — [summary]"
→ Waits for your confirmation
```

Type `commit` to commit the FIL files. Source files are never auto-committed.

---

### 4. Next session

The agent reloads the updated files — it knows exactly where you are, which errors to avoid, which decisions were made. No context repetition.

---

**In short:**
`BOOT.md` = guided setup · `CLAUDE.md` = permanent law · `DECISIONS.md` = architectural ADRs · `DYNAMIC.md` = memory between sessions · `LOG_ERRORS.md` = recurring error prevention · Git = persistence.

---

## Security Warning

FIL files are committed to Git by design — they are your project's operational memory.

```
NEVER store in FIL files:
→ API keys · tokens · passwords · secrets
→ .env values · private credentials
→ Personal customer data · GDPR-sensitive content
→ Any value that belongs in .env or a secrets manager
```

Run a secret scan before every commit:
```bash
# Example with git-secrets
git secrets --scan

# Example with trufflehog
trufflehog git file://. --since-commit HEAD
```

If you accidentally commit a secret: rotate it immediately. Git history is permanent.

---

## Setup

### Option A — Guided setup via BOOT.md (recommended, ~10 minutes)

Load `BOOT.md` into your coding agent and say:
```
Load BOOT.md and follow the FIL Boot sequence.
```

The agent interviews you and generates all 5 files fully populated — no placeholders to fill manually.
BOOT.md handles: project identity · architectural principles · coding standards · domain knowledge · error categories · first Git checkpoint.

### Option B — Manual setup (~15 minutes)

**Step 1 — Add files to your repo root**

```bash
cp CLAUDE.md your-project/CLAUDE.md
cp AGENTS.md your-project/AGENTS.md
cp DYNAMIC.md your-project/DYNAMIC.md
cp skills.md your-project/skills.md
cp LOG_ERRORS.md your-project/LOG_ERRORS.md
```

**Step 2 — Fill CLAUDE.md (your governance layer)**

Open `CLAUDE.md` and fill in:
- `PROJECT IDENTITY` — name · language · stack · repo
- `ARCHITECTURAL PRINCIPLES` — 3-5 principles that govern every decision
- `CODING STANDARDS` — linter · formatter · test framework · coverage target
- `PERMANENT CONSTRAINTS` — hard limits that cannot be bypassed silently

This is the most important step. The more specific your principles, the better the agent behaves.

**Step 3 — Fill DYNAMIC.md Hot Zone**

Open `DYNAMIC.md` and fill in:
- `CURRENT STATUS` — what you're working on right now
- `TODO` — top 3-5 priority tasks

**Step 4 — Do not gitignore FIL files**

```bash
# FIL files are committed — do NOT gitignore them
# They are your project's operational memory
# Exception: never add secrets to FIL files (see Security Warning above)
```

**Step 5 — First session**

Tell your agent:
```
Load CLAUDE.md, AGENTS.md, DYNAMIC.md, and LOG_ERRORS.md.
Follow the session lifecycle defined in AGENTS.md.
```

The agent boots, scans prevention patterns, and reports status.

---

## The Difference from Just Adding Context

| Approach | What happens when project grows |
|----------|----------------------------------|
| Add more context | All information competes for attention equally |
| FIL | Explicit hierarchy — governance overrides state overrides reference |

**Without FIL:** the agent treats a deprecated workaround and an architectural principle as equally valid.

**With FIL:** `CLAUDE.md` is law. `DYNAMIC.md` is state. `LOG_ERRORS.md` is prevention.
The agent always knows which to apply.

---

## Persistence via Git

Unlike the original FIL (which uses Google Drive), this edition uses Git as the persistence layer.

```
DYNAMIC.md and LOG_ERRORS.md evolve alongside the source code.
Full history · auditable · recoverable.

Session commit format:
[FIL] session YYYY-MM-DD — [one-line summary]
```

### Save vs Commit

FIL separates memory update from code commit — by design.

```
save        → updates DYNAMIC.md + LOG_ERRORS.md only · always safe · no confirmation needed
commit      → commits FIL files after operator reviews the diff
save commit → saves + commits source files · explicit operator request only
              never auto-committed · requires tests + lint status known
```

This protects against committing broken code, accidental secrets, or out-of-scope changes.

### Recovery Semantics

A valid FIL checkpoint is a commit where:
- DYNAMIC.md was updated (session summary present in Warm Zone)
- LOG_ERRORS.md was updated or explicitly unchanged
- Commit message starts with `[FIL] session`

```bash
# Find last FIL checkpoint
git log --oneline --grep="\[FIL\] session"

# Restore FIL memory from a checkpoint
git show <commit>:DYNAMIC.md
git show <commit>:LOG_ERRORS.md

# Optional: tag stable checkpoints
git tag fil-good-YYYYMMDD
```

`recovery` hotkey in session loads the most recent `[FIL] session` commit automatically.

---

## What This Is Not

```
NOT a replacement for:  code review · CI/CD · test suite · documentation
NOT an autonomous agent: FIL governs · humans decide
NOT a magic memory system: it structures what you choose to persist

IS a:
→ Governance layer for AI-assisted coding
→ Operational continuity protocol between sessions
→ Prevention system that stops you repeating the same mistakes
```

---

## Customization

**CLAUDE.md** is the most project-specific file.
Spend time on your architectural principles — they're what make the agent useful.

**skills.md** grows as the project grows.
Add a section for each major domain area · tag every claim with `[truth:type]`.

**LOG_ERRORS.md** fills itself.
Use `LOG_ERROR: [desc]` in any session. The agent logs it automatically.

**AGENTS.md** can be extended with project-specific SOPs.
Add a `SOP-[NAME]` section for any recurring workflow.

---

## Compatibility

Tested with: Claude Code · GPT-4o · Gemini 2.0.
Any agent capable of following structured multi-step instructions can run FIL.
Behavior may vary between models — test your `AGENTS.md` session lifecycle
on your primary model before relying on it in production.

---

## Example Session

```
# First boot
"Load CLAUDE.md, AGENTS.md, DYNAMIC.md, and LOG_ERRORS.md.
 Follow the session lifecycle defined in AGENTS.md."

→ Agent reports: "✅ Boot complete — [PROJECT] · 0 prevention patterns · Hot Zone: empty"

# Coding task
"Implement JWT auth middleware"
→ Agent checks CLAUDE.md for relevant ARCH principles
→ Agent checks LOG_ERRORS.md for past auth-related errors
→ Agent implements · asks before deviating from any principle

# Error occurs
"That middleware is blocking all requests, not just unauthenticated ones"
→ Agent corrects immediately
→ Agent: "Should I log this for future prevention? (yes/no)"
→ "yes" → LOG_ERRORS.md updated

# End of session
"save"
→ Agent updates DYNAMIC.md + LOG_ERRORS.md
→ Agent shows: "✅ FIL memory updated.
               Proposed commit: [FIL] session 2026-07-20 — implemented JWT auth middleware
               Run 'commit' to commit."
"commit"
→ Operator confirms · agent commits FIL files
```

---

## File Inventory

```
BOOT.md         ← start here · interview wizard · generates all files
CLAUDE.md       ← governance law · fill manually or via BOOT.md
AGENTS.md       ← session workflow · do not modify often
DECISIONS.md    ← architectural decisions · ADR · permanent · never archived
DYNAMIC.md      ← update every session · committed after operator review
skills.md       ← domain knowledge · grows with project
LOG_ERRORS.md   ← prevention memory · auto-filled by agent
README.md       ← this file
LICENSE         ← BSD 3-Clause
```

---

## License

BSD 3-Clause. See LICENSE file.

---

*FIL Coding Agent Edition V1.0.0-beta*
*Adapted from FIL Framework V3.4.2*
*"Governance over context."*
