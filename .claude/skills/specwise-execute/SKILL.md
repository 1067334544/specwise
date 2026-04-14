---
name: specwise-execute
description: |
  Execute a task against a confirmed Spec and perform 5-dimension verification. The execution engine of the Specwise system.
  Trigger when the user says "execute against the spec", "start executing", "execute", "go based on the spec", or asks to begin work after confirming a Spec.
  A confirmed Spec must exist to execute. If there's no confirmed Spec in the conversation, prompt the user to generate and confirm one first using /sw:spec or /sw:draft.
---

# Execute: Execute Against Spec with Verification

## What This Command Is

Execute is the execution engine of the Specwise system. It takes a confirmed Spec, generates an execution plan, performs the task, then conducts a 5-dimension self-check against the Spec, outputting results and a verification report.

Core principle: **Execution is strictly bound by the Spec.** What the Spec says must be done; what the Spec doesn't say cannot be added; when information is insufficient, don't silently assume — pause instead.

## Prerequisites

A **confirmed Spec** must exist before execution (from `/sw:spec`, `/sw:draft`, or `/sw:explore` → `/sw:spec` flow).

If there's no confirmed Spec in the current conversation, inform the user:
> "No confirmed Spec found. Please use `/sw:spec` or `/sw:draft` to generate and confirm a Spec first, then execute."

## Execution Workflow

### Step 1: Generate Execution Plan

Based on the Spec's content, break the task into concrete execution steps. Show the user:

```
## Execution Plan

Based on the confirmed Spec, I'll execute in these steps:

1. [Step 1 description]
2. [Step 2 description]
3. [Step 3 description]
...

Beginning execution.
```

Every step in the plan should be traceable to a specific requirement in the Spec.

### Step 2: Execute Step by Step

Complete the task according to the execution plan.

#### Execution Interrupt Mechanism

This is a critical design decision: **If a problem with the Spec is discovered during execution, you must pause.**

Situations that trigger a pause:
- A Stated Assumption turns out to be invalid
- Two requirements in the Spec contradict each other
- A decision point not covered by the Spec but necessary for execution is encountered
- Execution results suggest the Definition of Done may be unreasonable

Behavior when pausing:

```
## ⚠️ Execution Paused

While executing step [N], the following issue was discovered:

**Issue**: [specific description]

**Scope of Impact**: [which Spec requirements this affects]

**Suggested Spec Modifications**:
- [specific modification suggestions]

Please confirm Spec modifications to continue execution, or provide other instructions.
```

**What you must NOT do**: Don't make your own decision after discovering a problem, silently work around it, then cover it up in the final result. You must explicitly pause and let the user decide.

### Step 3: 5-Dimension Verification Self-Check

After task execution is complete, conduct a 5-dimension self-check against the Spec:

#### Dimension 1: Goal Fulfillment

Check each objective declared in the Spec against the result, item by item.

Criteria:
- ✅ **Fully met**: Result completely covers the objective
- ⚠️ **Partially met**: Result covers part of it, but has gaps or deviations (must explain why)
- ❌ **Not met**: Result failed to cover the objective (must explain why)

#### Dimension 2: Constraint Compliance

Check each constraint declared in the Spec, item by item.

Criteria:
- ✅ **Compliant**: Constraint fully observed
- ❌ **Violated**: Constraint violated (must explain the violation and reason)

Constraint violations are serious. If any exist, they must be highlighted in the verification report.

#### Dimension 3: Completeness

Check two directions:
- **Omissions**: Content the Spec required but is missing from the result
- **Out of scope**: Content in the result that the Spec didn't request

#### Dimension 4: Assumption Audit

Review the "Stated Assumptions" from the Spec:
- Is each assumption still valid given what was learned during execution?
- Were any new assumptions made during execution that weren't declared in the Spec?

#### Dimension 5: Risk Disclosure

Disclose risk factors in the result:
- Which parts have lower confidence?
- Which conclusions depend on uncertain information sources?
- What risks could affect the validity of the results?

### Step 4: Output Verification Report

Final output format:

```
## Execution Result

[Complete task execution result]

---

## 📋 Verification Report

### Goal Fulfillment
- ✅ [Goal 1]: [fulfillment status]
- ⚠️ [Goal 2]: [partially met, reason...]
- ❌ [Goal 3]: [not met, reason...]

### Constraint Compliance
- ✅ [Constraint 1]: Compliant
- ✅ [Constraint 2]: Compliant

### Completeness
- Omissions: [list, or "None"]
- Out of scope: [list, or "None"]

### Assumption Audit
- ⚠️ Assumption 1: [status — still valid / invalidated / pending verification]
- ⚠️ Assumption 2: [status]
- 🆕 New assumptions: [assumptions made during execution, if any]

### Risks & Uncertainties
- ⚠️ [Risk 1]: [impact level and description]
- ⚠️ [Risk 2]: [impact level and description]

---

**Summary**: [One sentence summarizing completion status, e.g., "All core objectives achieved, 1 constraint partially met, 2 risks flagged for attention."]
```

This is "explainable completion" — not a vague percentage, but letting the user see precisely what was done, what wasn't, and why.

## Guardrails

- Execute strictly against the Spec; do not add content the Spec didn't request
- Must pause when problems are discovered; never silently work around them
- The verification report must be honest — better to expose problems than conceal them
- Assumptions must be explicitly declared; never silently fill in
- Risks must be disclosed; uncertainty is a managed dimension, not a hidden weakness
