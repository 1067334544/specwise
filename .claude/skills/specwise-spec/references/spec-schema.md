# Spec Schema Reference

This file defines the 3-layer structure of a universal Spec. All Specwise commands (spec / draft / explore→spec) should follow this structure when generating Specs.

## Core Layer — Required for Any Task

No matter how simple the task, the four Core fields must be answered. Simple tasks can be covered in one sentence; complex tasks need expansion.

### Objective
**Question answered**: What to do + why?
- State the task's purpose in one sentence
- If there are multiple objectives, rank by importance
- Objectives should be verifiable — you can judge "done or not done"

### Expected Output
**Question answered**: What exactly to deliver?
- Deliverable form (report, code, plan, document...)
- Deliverable structure (how many sections, what format)
- Deliverable granularity (summary-level or detailed, how long, how deep)

### Constraints
**Question answered**: What can't be done / what must be obeyed?
- Rules that absolutely cannot be violated
- Standards or norms that must be followed
- Boundaries (time, resources, scope)

### Definition of Done
**Question answered**: What state counts as "complete"?
- What's the minimum acceptance bar
- Which items are must-haves vs nice-to-haves
- Completion criteria should be objective and verifiable

## Context Layer — Most Tasks Need This

### Inputs
Raw data, materials, documents to process. If there's no explicit input, note "No specific input materials."

### Background
- Why did this task come about? What prior decisions were made?
- Who's the audience? What's the use case?
- Any historical context to understand?

### References
- Style references, similar cases, existing standards
- Examples or templates provided by the user
- Industry conventions or best practices

### Stated Assumptions
**This is one of the most critical fields in the Spec system.**

Rules:
- For dimensions with insufficient information, the model **must never silently fill in**
- Must explicitly list inferences as "Stated Assumptions"
- Mark each assumption with ⚠️
- Let the user confirm or modify

Example:
```
Stated Assumptions:
⚠️ Assuming the target audience is the internal product team
⚠️ Assuming report format is structured analysis (not narrative)
⚠️ Assuming length of approximately 2000-3000 words
⚠️ Assuming no raw data appendix is needed
```

## Refinement Layer — Expand as Needed

### Priorities
Trade-off rules when requirements conflict. Example: "If time and quality conflict, prioritize core feature quality."

### Exclusions
Explicitly declared out-of-scope items. Clearly stating "what we won't do" is just as important as vaguely stating "what we will do."

### Risks
Known risks, areas of information uncertainty. Example: "Market data may not reflect the latest trends."

### Quality Preferences
Preferences along these dimensions:
- Depth vs breadth
- Rigorous vs creative
- Detailed vs concise
- Conservative vs bold

## Spec Presentation Format

```markdown
## 📋 Task Spec

### Core
- **Objective**: [...]
- **Expected Output**: [...]
- **Constraints**: [...]
- **Definition of Done**: [...]

### Context
- **Inputs**: [...]
- **Background**: [...]
- **References**: [...]
- **Stated Assumptions**:
  - ⚠️ [Assumption 1]
  - ⚠️ [Assumption 2]

### Refinement (if applicable)
- **Priorities**: [...]
- **Exclusions**: [...]
- **Risks**: [...]
- **Quality Preferences**: [...]
```

## Complexity vs Layer Depth

| Complexity | Core Layer | Context Layer | Refinement Layer |
|-----------|------------|--------------|-----------------|
| Simple | One-sentence confirmation | Usually not needed | Not needed |
| Medium | Fully filled | Key fields | As needed |
| Complex | Fully filled | All fields filled | All fields filled |

## File Format — Persisted Specs

When a Spec is confirmed, it is saved to `specs/{slug}.spec.md` with YAML frontmatter:

```markdown
---
date: 2026-04-14
status: confirmed
source: /sw:spec
---

## 📋 Task Spec

### Core
- **Objective**: [...]
- **Expected Output**: [...]
- **Constraints**: [...]
- **Definition of Done**: [...]

### Context
- **Inputs**: [...]
- **Background**: [...]
- **References**: [...]
- **Stated Assumptions**:
  - ⚠️ [Assumption 1]
  - ⚠️ [Assumption 2]

### Refinement (if applicable)
- **Priorities**: [...]
- **Exclusions**: [...]
- **Risks**: [...]
- **Quality Preferences**: [...]
```

### Frontmatter Fields

| Field | Values | Description |
|-------|--------|-------------|
| `date` | `YYYY-MM-DD` | Date the Spec was confirmed |
| `status` | `confirmed` → `executed` | Updated by `/sw:execute` after execution |
| `source` | `/sw:spec` or `/sw:draft` | Which command generated the Spec |
| `executed_date` | `YYYY-MM-DD` | Added by `/sw:execute` after execution |

### Naming Convention

- Location: `specs/` directory in project root
- Filename: `{slug}.spec.md` where `{slug}` is a short, lowercase, hyphenated slug derived from the Spec's Objective
- Examples: `add-user-auth.spec.md`, `q2-market-analysis.spec.md`, `redesign-checkout-flow.spec.md`
