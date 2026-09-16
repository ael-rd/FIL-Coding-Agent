# FIL Coding Agent — Development Governance Analysis

**Date:** 2026-09-16  
**Source of evidence:** operational review of `ael-rd/exploriva-web` and its FIL development workflow.  
**Purpose:** capture the lessons that should shape the future FIL Coding Agent Hybrid architecture before implementing the native/hybrid runtime.

---

## 1. Executive summary

The current FIL workflow used by Exploriva is already a strong development-governance system. It separates permanent project constraints, architecture decisions, strategy, operational backlog, short-term session memory, error-prevention knowledge, archives, tests and Git history.

The main weakness is no longer missing documentation. It is **reconciliation**.

The project can reach a state where:

- documentation declares an invariant;
- code implements something different;
- CI enforces a third threshold;
- operational memory still describes an older commit;
- an agent reads all of those sources and has to decide which one is authoritative.

The next FIL evolution should therefore not create more Markdown. It should turn the existing governance model into a **verifiable control plane**.

Core direction:

```text
Declared intent
      ↓
FIL context + policies
      ↓
Agent execution
      ↓
Code / tests / Git / runtime evidence
      ↓
FIL reconcile
      ↓
Updated state + detected drift
```

The target is a portable software-engineering context and governance harness that can work in two modes:

```text
                    FIL CORE
                       │
        Context · Memory · Decisions
        Policies · Prevention · Verify
        Reconcile · Capabilities
                       │
             ┌─────────┴─────────┐
             │                   │
       EMBEDDED MODE        NATIVE MODE
             │                   │
      Host adapters         FIL Runtime
             │                   │
   Cursor / Claude Code     Files / Shell
   Codex / Continue         Git / Tests
             │                   │
             └─────────┬─────────┘
                       │
                  Model Layer
             OpenAI / Anthropic
             Gemini / Local / ...
```

**Architectural invariant:** FIL Core must not depend on a specific LLM provider or coding-agent host.

---

## 2. What works well in the Exploriva setup

The existing information architecture is conceptually sound:

```text
CLAUDE.md
   ↓
project-specific permanent constraints

AGENTS.md
   ↓
agent execution protocol

DECISIONS.md
   ↓
architectural decisions / ADRs

STRATEGY.md
   ↓
product and economic direction

TODO.md
   ↓
active operational backlog

DYNAMIC.md
   ↓
short operational memory

LOG_ERRORS.md
   ↓
error and prevention memory

docs/
   ↓
specialized analyses and audits

archives/
   ↓
history removed from active context

Git + tests + CI
   ↓
evidence of what actually happened
```

This separation should be preserved.

A particularly valuable pattern is the evolution from an incident to an executable prevention mechanism:

```text
incident
   ↓
LOG_ERRORS
   ↓
prevention pattern
   ↓
automated check
   ↓
CI invariant
```

`SCHEMA_DRIFT` in Exploriva is a good example: a documented prevention rule eventually became an automated generated-type consistency check.

This is the direction FIL should generalize.

---

## 3. Problems observed

### 3.1 Declared state can diverge from observed state

Operational memory can remain on an older session or commit while `main` continues moving.

This makes a statement such as “current project state” ambiguous unless FIL compares it with Git.

### 3.2 Normative rules and verified facts are mixed

A project file can state a security or quality rule as if it were currently guaranteed, while the code or CI only partially enforces it.

FIL needs explicit truth states for project invariants:

```text
MUST      required invariant
VERIFIED  currently verified against evidence
GAP       required but currently violated
ACCEPTED  known deviation explicitly accepted
UNKNOWN   not yet verified
```

This distinction is important for security, coverage, architecture and deployment claims.

### 3.3 Documentation governance depends too much on agent discipline

Rules such as:

- update DYNAMIC after significant work;
- archive after a session threshold;
- update ADRs when architecture changes;
- run security review for hot zones;
- keep TODO aligned with delivered work;

are useful, but a rule that only exists in prose can silently stop being followed.

### 3.4 Policy and CI can disagree

The Exploriva review found examples where documentation describes stronger quality requirements than CI actually enforces.

FIL should detect this class of **policy drift** automatically.

### 3.5 ADR files can become specifications rather than decision records

As a project grows, one large `DECISIONS.md` can accumulate detailed SQL, UI contracts and implementation specifications.

The long-term model should be:

```text
DECISIONS.md
   ↓ compact ADR registry

docs/architecture/
   ↓ detailed architecture

docs/features/
   ↓ feature specifications

docs/security/
   ↓ security analyses
```

FIL should load only the decision/context required for the current task.

### 3.6 DONE is often a documentary claim rather than an evidence object

A checked task should eventually be understood as:

```text
DONE
 = claim
 + commit/PR evidence
 + relevant tests
 + verification timestamp
```

This does not require turning every TODO file into YAML. It requires FIL to understand and reconcile evidence.

---

## 4. Proposed FIL governance model

### 4.1 Keep Markdown for human meaning

Do not replace the existing documentation with a large machine configuration.

Markdown remains the correct place for:

- rationale;
- strategy;
- architecture decisions;
- operational narrative;
- lessons and prevention knowledge.

### 4.2 Add a small machine-readable project manifest

Proposed concept: `fil.project.yaml`.

It should contain only properties that agents should not have to infer from prose.

Example:

```yaml
fil_version: "1.5"

context:
  permanent:
    - CLAUDE.md
    - AGENTS.md
    - DECISIONS.md
  operational:
    - DYNAMIC.md
    - TODO.md
    - LOG_ERRORS.md
  optional:
    - STRATEGY.md

commands:
  test_backend: "pytest"
  test_frontend: "npm test"
  lint_backend: "ruff check"
  lint_frontend: "npm run lint"

quality:
  backend_coverage_target: 80
  frontend_coverage_target: 70

hot_zones:
  - "**/auth/**"
  - "**/migrations/**"
  - "**/*billing*"
  - "**/*stripe*"
  - ".github/workflows/**"

security:
  dependency_scan_required: true
  secret_scan_required: true

dynamic:
  max_active_sessions: 10

git:
  conventional_commits: true
  force_push_shared_branch: false
```

The manifest is metadata, not project memory.

---

## 5. Reconciliation engine

The highest-value FIL capability identified by the Exploriva experiment is a reconciliation engine.

Conceptual command:

```text
fil reconcile
```

It should compare declared and observed state.

Example output:

```text
FIL RECONCILE

Git HEAD
  29d183aa

DYNAMIC last known state
  older than HEAD

WARN DYNAMIC_STALE

Declared backend coverage
  80%

Observed CI enforcement
  15%

GAP POLICY_DRIFT

Declared frontend coverage
  70%

Observed CI enforcement
  none

GAP POLICY_DRIFT

LOG_ERRORS last QC scan
  stale relative to current HEAD

WARN QC_STALE
```

The reconciliation engine should distinguish:

- facts it can prove;
- facts it cannot inspect;
- required policies;
- accepted deviations;
- contradictions.

It must never silently convert an unverified documentation statement into a verified fact.

---

## 6. Boot lifecycle

Future Hybrid boot sequence:

```text
fil boot

1. Locate project manifest
2. Resolve context sources
3. Read permanent invariants
4. Read current operational state
5. Inspect Git HEAD / branch / worktree
6. Reconcile declared vs observed state
7. Detect task-relevant Hot Zones
8. Load relevant prevention patterns
9. Resolve required capabilities
10. Construct bounded task context
11. Start host/native agent execution
```

This avoids blindly loading every project document into every agent session.

---

## 7. Checkpoint lifecycle

Proposed checkpoint:

```text
fil checkpoint

changes
   ↓
classify changed surfaces
   ↓
run required tests/checks
   ↓
validate security/hot-zone policy
   ↓
inspect Git diff
   ↓
reconcile declared state
   ↓
update TODO/DYNAMIC/LOG_ERRORS if required
   ↓
archive if threshold reached
   ↓
produce checkpoint evidence
```

A checkpoint should generate a structured evidence object even when the host agent itself is conversational.

---

## 8. Prevention compiler

FIL should progressively convert prevention knowledge into executable controls.

Lifecycle:

```text
PREVENTION ACTIVE
        ↓
classify automatable?
        ↓ yes
AUTOMATED RULE
        ↓
fil verify / tests / CI
```

Examples:

- schema drift → generated schema comparison;
- forbidden direct provider call → static source check;
- migration dependency → migration/deployment guard;
- stale DYNAMIC → Git/state reconciliation;
- secret leak → secret scanner;
- untested hot-zone change → changed-path test policy.

The human-readable lesson remains useful even after automation because it explains *why* the rule exists.

---

## 9. Capability architecture

FIL Core should request capabilities rather than depend directly on host APIs.

Proposed conceptual contract:

```text
FileRead
FileWrite
FileSearch
ShellExec
GitRead
GitWrite
TestRun
NetworkRead
IssueRead
IssueWrite
PullRequestRead
PullRequestWrite
```

Embedded adapters map these capabilities to the host:

```text
Claude Code adapter
Codex adapter
Cursor adapter
Continue adapter
GitHub adapter
```

Native mode provides its own controlled implementations.

Policies operate on capabilities rather than vendor-specific tool names.

Example:

```text
Policy:
  migrations/** changed

Requires:
  GitRead
  TestRun(database-policy-tests)
  human approval before production migration
```

---

## 10. Security architecture for Hybrid

A coding agent is a privileged system. Hybrid must therefore default to least privilege.

Recommended principles:

1. read before write;
2. explicit capability grants;
3. repository-scoped filesystem access;
4. shell command policy and timeouts;
5. network deny-by-default in native execution where practical;
6. secrets never included in LLM context unless explicitly required and authorized;
7. destructive Git operations blocked by default;
8. no self-approval of security-sensitive changes;
9. generated code is evidence, not proof of correctness;
10. checkpoint before irreversible side effects.

The host may provide additional security controls, but FIL Core must not assume those controls exist.

---

## 11. Model-provider independence

FIL Core must not depend on OpenAI, Anthropic, Google or a local model API.

Target abstraction:

```text
FIL Core
   ↓
Reasoning / generation request
   ↓
Model adapter
   ├── OpenAI
   ├── Anthropic
   ├── Gemini
   ├── local
   └── host-provided model
```

Model capabilities should be discovered rather than assumed:

- context size;
- structured output;
- tool/function calling;
- streaming;
- reasoning controls;
- image input;
- cost metadata when available.

Policies should be expressed independently of provider implementation.

---

## 12. Embedded mode vs native mode

### Embedded mode

FIL provides governance/context while another coding agent owns execution.

Advantages:

- fastest path to adoption;
- uses mature host tooling;
- validates FIL Core without recreating a full coding agent;
- works with multiple hosts.

### Native mode

FIL owns controlled file, shell, Git and test execution.

Advantages:

- deterministic capability enforcement;
- portable automation;
- stronger reconciliation/checkpoint lifecycle;
- foundation for `fil code`.

Recommended order:

```text
FIL Core
  ↓
Capability contract
  ↓
Context router
  ↓
Host adapters / MCP
  ↓
Policy + verification engine
  ↓
Minimal native runtime
  ↓
fil code
```

Do not begin by implementing a full autonomous coding agent.

---

## 13. Validation strategy

Before claiming that FIL Hybrid improves coding-agent work, validate it on real repositories.

Suggested initial experiment:

- 5–15 independent developers;
- real repositories;
- 5–10 meaningful sessions each;
- compare normal coding agent vs coding agent + FIL;
- record task completion, regressions, context loss, repeated errors, review burden and token/time overhead.

Useful hypotheses:

1. FIL reduces repeated project-specific mistakes.
2. FIL improves continuity across sessions.
3. FIL reduces contradictory architecture changes.
4. FIL makes agent-generated changes easier to audit.
5. Reconciliation detects stale or false project memory before it affects implementation.
6. The governance overhead remains low enough to preserve developer velocity.

Avoid optimizing for benchmark scores before measuring real repository behavior.

---

## 14. Relationship with GitHub agentic workflows

GitHub now supports agentic workflows that can run coding agents through Actions with explicit permissions and safe outputs. This is useful evidence that repository-native agent automation is becoming a standard execution surface.

FIL should not duplicate GitHub-specific orchestration. A future GitHub adapter can treat GitHub Actions/agentic workflows as one host/runtime among several.

The differentiator remains:

```text
portable project context
+ policy
+ prevention memory
+ verification
+ reconciliation
+ evidence
```

rather than GitHub workflow execution itself.

---

## 15. Recommended initial backlog

### Phase 0 — specification

- [ ] Define FIL Core boundaries.
- [ ] Define `fil.project.yaml` schema.
- [ ] Define truth states: MUST / VERIFIED / GAP / ACCEPTED / UNKNOWN.
- [ ] Define capability contract.
- [ ] Define evidence/checkpoint schema.
- [ ] Define host adapter interface.

### Phase 1 — reconciliation prototype

- [ ] Implement repository discovery.
- [ ] Read manifest/context configuration.
- [ ] Inspect Git state.
- [ ] Detect stale operational memory.
- [ ] Compare declared commands/thresholds with detectable CI configuration.
- [ ] Emit machine-readable + human-readable reconcile report.

### Phase 2 — embedded adapters

- [ ] Claude Code adapter.
- [ ] Codex adapter.
- [ ] Generic MCP/host adapter.
- [ ] Capability discovery.

### Phase 3 — verification

- [ ] Changed-file classifier.
- [ ] Hot-zone policy engine.
- [ ] Test/lint command execution contract.
- [ ] Prevention-rule automation hooks.
- [ ] Checkpoint evidence bundle.

### Phase 4 — minimal native runtime

- [ ] Repository-scoped file tools.
- [ ] controlled shell execution;
- [ ] Git read/write adapter;
- [ ] test runner;
- [ ] model adapter interface;
- [ ] policy enforcement around side effects.

### Phase 5 — product validation

- [ ] External developer cohort.
- [ ] Baseline agent-only sessions.
- [ ] FIL-assisted sessions.
- [ ] Measure regressions/context loss/review burden/velocity.
- [ ] Publish evidence and revise architecture.

---

## 16. Non-goals for the first Hybrid milestone

Do not initially build:

- a full IDE;
- a proprietary LLM;
- a replacement for GitHub;
- a general-purpose autonomous computer-use agent;
- a large multi-agent orchestration framework;
- a cloud platform before local/embedded value is demonstrated;
- complex long-term semantic memory before reconciliation is reliable.

The first milestone should prove that FIL can make an existing coding agent **more consistent, auditable and project-aware**.

---

## 17. Product framing

Current strongest framing:

> **FIL is a portable software-engineering context and governance harness for coding agents.**

Hybrid extends that idea:

> **FIL Coding Agent Hybrid combines portable project governance with host-native or FIL-native execution, without coupling the project to one model or one coding agent.**

The important product promise is not “another AI that writes code.”

It is:

> **The agent can change. The model can change. The project memory, rules, evidence and engineering discipline survive.**

---

## 18. Decision from the Exploriva experiment

Exploriva should remain a validation ground for FIL rather than becoming the repository where the generic Hybrid runtime is implemented.

Use Exploriva to discover real governance failures and validate rules. Implement reusable mechanisms here in FIL Coding Agent.

Immediate architectural focus:

```text
1. Reconciliation
2. Machine-readable project metadata
3. Evidence-backed checkpoints
4. Capability abstraction
5. Embedded host adapters
6. Minimal native runtime
```

This ordering preserves FIL's existing strengths while moving it from a documentation protocol toward an executable engineering control plane.
