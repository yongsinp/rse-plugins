---
name: research-workflow-management
description: "Use when the user asks for a structured technical research approach, a spike/proof of concept, feasibility investigation, or research-driven implementation planning. Guides a six-phase workflow (Research, Plan, Iterate Plan, Experiment, Implement, Validate) that generates concrete artifacts: research questions and findings, phased implementation plans with success criteria, experiment reports, implementation logs, and validation reports."
metadata:
  assets:
    - assets/research-template.md
    - assets/plan-template.md
    - assets/experiment-template.md
    - assets/implement-template.md
    - assets/handoff-template.md
---

# Research Workflow Management

Use this skill for complex software work that needs evidence before implementation.

## Use When

- The user asks for a structured investigation or technical research workflow.
- The task sounds like a spike, proof of concept, feasibility study, or architecture investigation.
- The user needs a plan with explicit success criteria before coding.
- The user asks to validate implementation against a prior plan.

## What This Skill Produces

- `research-<slug>.md`: existing patterns, architecture findings, risks, open questions.
- `plan-<slug>.md`: phased implementation plan, automated/manual success criteria, out-of-scope list.
- `experiment-<slug>.md`: approach comparisons with measured observations and recommendation.
- `implement-<slug>.md`: execution log, phase status, deviations from plan.
- validation report: criterion-by-criterion pass/fail with evidence and follow-up actions.

All workflow artifacts are written to `.agents/` in the project root.

## Six-Phase Workflow

1. **Research (`/research`)**
   - Action: generate research questions, inspect code, document current behavior and constraints.
   - Output: `.agents/research-<slug>.md`.

2. **Plan (`/plan`)**
   - Action: create phased implementation plan with file-level scope and measurable criteria.
   - Output: `.agents/plan-<slug>.md`.

3. **Iterate Plan (`/iterate-plan`)**
   - Action: update an existing plan based on feedback/new constraints while preserving consistency.
   - Output: edited plan document.

4. **Experiment (`/experiment`, optional)**
   - Action: run 2-3 alternatives and record evidence (not just theory).
   - Output: `.agents/experiment-<slug>.md`.

5. **Implement (`/implement`)**
   - Action: execute plan phase-by-phase; record progress and deviations.
   - Output: `.agents/implement-<slug>.md` plus code changes.

6. **Validate (`/validate`)**
   - Action: verify each plan criterion with automated checks and explicit manual checks.
   - Output: validation report tied to plan criteria.

## Command Examples

Example slash command usage and expected artifacts:

```text
/research auth-system
-> .agents/research-auth-system.md
   Sections: scope, findings with file refs, risks, open questions

/plan auth-system
-> .agents/plan-auth-system.md
   Sections: phases, success criteria (automated/manual), out-of-scope

/experiment jwt-vs-session
-> .agents/experiment-jwt-vs-session.md
   Sections: hypothesis, setup, observations, recommendation

/implement .agents/plan-auth-system.md
-> .agents/implement-auth-system.md
   Sections: per-phase progress, deviations, checks run

/validate .agents/plan-auth-system.md
-> validation report with pass/fail per criterion + evidence
```

## Validation Checkpoints and Feedback Loop

Use these checkpoints in sequence:

1. **After Research:** confirm scope is complete and key unknowns are explicit.
2. **After Plan:** confirm every phase has success criteria and verification steps.
3. **After Experiment (if used):** choose one approach and update plan accordingly.
4. **During Implement:** after each phase, run listed automated checks; if fail, fix or update plan with rationale.
5. **Final Validate:** report pass/fail for each criterion and list remaining manual checks.

If validation fails, do not continue silently: return to **Iterate Plan** or **Implement** and re-run validation.

## Data and Naming Conventions

- `research-<slug>.md`
- `plan-<slug>.md`
- `experiment-<slug>.md`
- `implement-<slug>.md`

Slug format: lowercase, hyphenated from the command topic.

Cross-reference prior docs in each artifact's `## References` section.

## Template Assets

Templates are in `${CLAUDE_PLUGIN_ROOT}/skills/research-workflow-management/assets/`:

- `research-template.md`
- `plan-template.md`
- `experiment-template.md`
- `implement-template.md`
- `handoff-template.md`

Use templates to keep phase outputs concise, comparable, and auditable.

## Quick Selection Guide

- Need to understand existing code first -> **Research**
- Need executable implementation strategy -> **Plan**
- Need to revise approved plan -> **Iterate Plan**
- Need evidence between competing approaches -> **Experiment**
- Ready to execute approved plan -> **Implement**
- Need acceptance decision against criteria -> **Validate**
