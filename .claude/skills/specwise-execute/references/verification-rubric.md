# Verification Rubric Reference

This file defines the 5-dimension verification framework for the Specwise system. The execute command must perform self-checks against this framework after task completion.

## Why Verification Is Essential

Without a verification layer, the Spec degrades into a "wish list" — it records what was wanted but doesn't track whether it was achieved. Verification makes "completion" an auditable, discussable, traceable judgment rather than a vague feeling.

## Five Dimensions

### Dimension 1: Goal Fulfillment

**Method**: Check each objective in the Spec's Core Layer against the result, item by item.

**Criteria**:
- ✅ **Fully met**: Result completely covers the objective with no gaps
- ⚠️ **Partially met**: Result covers part of it, but gaps or deviations exist
  - Must explain: What was missed? Why?
- ❌ **Not met**: Result failed to cover the objective
  - Must explain: Why? Insufficient information, technical limitations, or other reasons?

### Dimension 2: Constraint Compliance

**Method**: Check each constraint in the Spec's Core Layer, item by item.

**Criteria**:
- ✅ **Compliant**: Constraint fully observed
- ❌ **Violated**: Constraint violated
  - Must explain: What was violated? To what degree? Why?
  - Constraint violations are serious and must be highlighted in the report

**Note**: Constraints are a "non-negotiable" dimension. Unlike objectives, constraints don't have "partially met" — they're either compliant or violated.

### Dimension 3: Completeness

**Check two directions**:

**Omission check**: Content the Spec required but is missing from the result.
- Cross-reference the Spec's "Expected Output" and "Definition of Done" item by item
- List all omissions

**Out-of-scope check**: Content in the result that the Spec didn't request.
- Out-of-scope content isn't necessarily a problem, but must be flagged
- Let the user know which parts are additions beyond the Spec's requirements

### Dimension 4: Assumption Audit

**Method**: Review the "Stated Assumptions" from the Spec's Context Layer.

**Questions to answer**:
1. Is each declared assumption still valid given what was learned during execution?
   - Valid → mark as "Verified" or "Still assumed"
   - Invalid → mark as "Invalidated" and explain the impact
2. Were any new assumptions made during execution?
   - If new inferences were necessary → mark as "🆕 New assumption"

### Dimension 5: Risk Disclosure

**Content to disclose**:
- Which parts of the result have lower confidence?
- Which conclusions depend on uncertain information sources?
- Are there timeliness risks (data may be outdated)?
- Are there methodology risks (analytical method may not apply)?
- Are there completeness risks (information may be incomplete)?

Each risk item should note impact level: **High** / **Medium** / **Low**.

## Output Format

```markdown
## 📋 Verification Report

### Goal Fulfillment
- ✅ [Goal 1]: [fulfillment description]
- ⚠️ [Goal 2]: Partially met — [reason]
- ❌ [Goal 3]: Not met — [reason]

### Constraint Compliance
- ✅ [Constraint 1]: Compliant
- ❌ [Constraint 2]: Violated — [violation details and reason]

### Completeness
- **Omissions**: [list, or "None"]
- **Out of scope**: [list, or "None"]

### Assumption Audit
- ⚠️ Assumption 1: [Verified / Still assumed / Invalidated]
- ⚠️ Assumption 2: [status]
- 🆕 New assumptions: [assumptions made during execution]

### Risks & Uncertainties
- ⚠️ [Risk item] (Impact: High/Medium/Low): [description]

---
**Summary**: [One sentence summary]
```

## The Core Idea of "Explainable Completion"

Not outputting "85% complete" as a meaningless number. Instead, letting the user see precisely:
- What was accomplished
- What wasn't accomplished
- Why it wasn't accomplished
- Whether the accomplished parts carry hidden risks

This is the true value of Spec-driven verification — **it makes "completion" an auditable judgment rather than a vague feeling.**
