# Architecture-to-Execution Handoff Gate (AEHG) v1

## Purpose

Design a fail-closed handoff process between Archy (system architect) and Ollie (operations agent) so implementation stays scoped, verifiable, and maintainable.

This gate ensures every execution request is:

- unambiguous
- scope-bounded
- verifiable
- reversible

---

## 1) Goal

Ensure every change from Archy → Ollie is:

- unambiguous
- scope-bounded
- verifiable
- reversible

The gate is **fail-closed**: if required info is missing or scope expands, execution stops and returns for clarification.

---

## 2) Lifecycle (State Machine)

`Drafted → Validated → Accepted → Executing → Verification Submitted → Closed`

Safety exit states:

- `Blocked`
- `Rejected`

### Transitions

1. **Drafted (Archy)**
   - Task spec created.

2. **Validated (Gate checks)**
   - Schema and policy checks pass.

3. **Accepted (Ollie)**
   - Ollie explicitly confirms target files and constraints.

4. **Executing (Ollie)**
   - Work performed only within approved scope.

5. **Verification Submitted (Ollie)**
   - Evidence bundle posted.

6. **Closed (Archy or policy)**
   - Verification approved.

### Fail-safe transitions

- Any schema/policy issue → `Rejected`
- Any scope mismatch/new files needed → `Blocked` (requires Archy amendment)

---

## 3) Mandatory Handoff Contract (Single Source of Truth)

Every task must include these fields:

- **Task ID**
- **Objective**
- **Context**
- **In-Scope Files** (explicit allowlist)
- **Out-of-Scope Areas** (explicit denylist)
- **Constraints**
- **Risks/Assumptions**
- **Verification Plan**
  - functional checks
  - regression checks
- **Rollback Plan**
- **Acceptance Criteria** (binary pass/fail bullets)

This extends the base task format with guardrail-critical fields (`Task ID`, `Out-of-Scope`, `Rollback`, `Acceptance Criteria`).

---

## 4) Gate Rules (Policy Engine)

### Rule A — Schema Completeness

If any required field is missing: **Reject**.

### Rule B — Scope Lock

Ollie may only modify files in `In-Scope Files`.
If additional files are required: **Block** and request amended spec.

### Rule C — No Silent Behavior Change

Any behavior change not listed in Objective/Acceptance Criteria: **Block**.

### Rule D — Verification Evidence Required

No closure without required evidence bundle.

### Rule E — Rollback Required for Risky Changes

If config/schema/data is touched, rollback steps must exist before execution.

---

## 5) Required Evidence Bundle (Ollie)

At completion, Ollie must provide:

- **Actual changed file list**
- **Diff summary by file**
- **Commands/tests run + outputs**
- **Acceptance criteria checklist** (each marked pass/fail)
- **Known limitations**
- **Rollback confirmation** (how to undo)

No evidence bundle = no closure.

---

## 6) Operational Artifacts (Suggested Structure)

Within the primary workspace:

`docs/architecture-handoff/`

- `tasks/<TASK_ID>.md` — Archy task specification
- `runs/<TASK_ID>-execution.md` — Ollie execution log + evidence
- `policies/handoff-gate.md` — gate rules
- `templates/task-spec.md` — authoring template

---

## 7) Minimal Templates

### `task-spec.md` (Archy)

- Task ID:
- Objective:
- Context:
- In-Scope Files:
- Out-of-Scope Areas:
- Constraints:
- Risks/Assumptions:
- Verification Plan:
- Rollback Plan:
- Acceptance Criteria:

### `execution-report.md` (Ollie)

- Task ID:
- Scope confirmation (before work):
- Files modified:
- Commands/tests run:
- Results:
- Acceptance criteria status:
- Rollback note:
- Blockers/deviations:

---

## 8) Governance Boundaries

- **Archy owns**: spec quality, scope definition, acceptance criteria.
- **Ollie owns**: compliant execution and evidence.
- **Non-trivial changes must not bypass gate.**
- Emergency hotfixes are allowed only with post-hoc retro task record.

---

## 9) Adoption Plan

1. Week 1: enforce gate for all code/config changes.
2. Week 2: require evidence bundle for closure.
3. Week 3: add basic completeness validation script.
4. Week 4: review blocked/rejected tasks and refine policy.

---

## Status

Version: **v1**
Owner: **Archy (System Architect)**
Scope: **Design baseline for immediate use and iteration**
