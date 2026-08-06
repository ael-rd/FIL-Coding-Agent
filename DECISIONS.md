# [PROJECT_NAME] — Architectural Decisions (DECISIONS.md)
> FIL Coding Agent Edition · V1.0.0-beta
> Authority: L2 — Permanent architectural decisions. Loaded every session alongside CLAUDE.md.
> Never overrides CLAUDE.md (L1). Governs every decision not explicitly covered by CLAUDE.md.

---

## WHAT BELONGS HERE

```
✅ IN DECISIONS.md:
→ Technology choices (language · framework · database · cloud provider)
→ Architectural patterns (repository pattern · event sourcing · CQRS · etc.)
→ Integration choices (which third-party service and why)
→ Any decision that will govern future sessions permanently
→ Rejected alternatives (important for onboarding and future re-evaluation)

❌ NOT IN DECISIONS.md:
→ Session workarounds → DYNAMIC.md Active Decisions
→ Coding standards → CLAUDE.md
→ Architectural principles → CLAUDE.md
→ Domain knowledge → skills.md
```

---

## ADR FORMAT

```
Each ADR uses this exact format:

### ADR-[NNN] · [SHORT TITLE]

STATUS    : ACTIVE | DEPRECATED | SUPERSEDED BY ADR-[NNN]
DATE      : YYYY-MM-DD
DECIDED BY: [OP-ID or "team"]

DECISION:
[One paragraph. What was decided. Precise and unambiguous.]

RATIONALE:
[Why this decision was made. Forces, constraints, goals that drove it.]

ALTERNATIVES CONSIDERED:
→ [Alternative 1] — rejected because [reason]
→ [Alternative 2] — rejected because [reason]

CONSEQUENCES:
Positive:
→ [Positive consequence 1]
→ [Positive consequence 2]
Negative:
→ [Negative consequence · accepted tradeoff]

REVIEW_DATE: [YYYY-MM-DD · or 'none' if truly permanent]
[truth:user-confirmed · DATE]
```

---

## ACTIVE DECISIONS

> Decisions currently in force. Agent applies these every session.

*(Add your first ADRs here via BOOT.md interview or `adr new` hotkey)*

---

## DEPRECATED DECISIONS

> Decisions that are no longer in force. Never delete — kept for historical context.
> Tag with STATUS: DEPRECATED · add SUPERSEDED BY if replaced.

*(empty at initialization)*

---

## DECISION LOG

> Index of all decisions ever recorded.
> Format: [ADR-NNN] · [DATE] · [TITLE] · [STATUS]

*(empty at initialization)*

---

## USAGE RULES

```
AGENT RULES:
→ Load DECISIONS.md at every boot (Step ② of AGENTS.md boot sequence)
→ Apply ACTIVE decisions as constraints alongside CLAUDE.md principles
→ Before implementing a technology or pattern: check if a decision governs it
→ If a task would violate an ACTIVE decision:
   → Signal: "⚠️ This conflicts with ADR-[NNN] ([TITLE]) — confirm to proceed"
   → Wait for operator confirmation before continuing
   → On confirmation: log deviation in DYNAMIC.md Active Decisions

OPERATOR RULES:
→ Record new permanent decisions with `adr new` or SOP-ADR in AGENTS.md
→ Never modify historical ADR content — add SUPERSEDED BY instead
→ Review ADRs with REVIEW_DATE reached — deprecate or renew
→ Deprecated decisions: change STATUS only · never delete content
```

---

## CHANGELOG

```
V1.0.0-beta ([DATE])
→ Initial creation — DECISIONS.md · FIL Coding Agent Edition V1.0.0-beta
→ ADR format defined · usage rules established
```

---

*DECISIONS.md · FIL Coding Agent Edition V1.0.0-beta*
*"Record why, not just what."*
