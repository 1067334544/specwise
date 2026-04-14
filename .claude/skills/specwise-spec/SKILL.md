---
name: specwise-spec
description: |
  Universal task specification tool (main command). Establishes an explicit specification process between user intent and task execution — define clearly what needs to be done before doing it.
  Trigger when the user describes a task to be completed, wants to generate a task spec, mentions "spec" or "specification", or asks to define requirements clearly before execution.
  Even if the user doesn't explicitly mention "spec", suggest using this skill if the task looks complex enough (multi-step, multi-constraint, vague intent).
  Do NOT trigger when the user is just chatting, asking simple questions, or explicitly says "just do it".
---

# Spec: Task Specification Main Command

## What This Skill Is

Spec is a **Task Contract Layer** — it doesn't make the model "better at answering." Instead, before execution begins, it helps the user and model reach explicit consensus on "what exactly is the task."

Why does this matter? In direct conversation, users express vague intent while the model silently fills in all unstated dimensions (format, depth, audience, focus...). These implicit assumptions are invisible to the user and untraceable in the result. If the guess is right, great. If wrong, it's rework.

Spec's approach: **surface these implicit assumptions**, making them visible, modifiable, and confirmable by both sides. Once confirmed, the Spec becomes the basis for execution and verification.

## Full Workflow

After receiving the user's task description, follow these steps:

### Step 1: Complexity Assessment

Internally assess the task's complexity across four dimensions:

- **Ambiguity**: How many key dimensions are missing from the user's input? (objective, format, audience, constraints...)
- **Constraint density**: How many conditions must be obeyed?
- **Output complexity**: How many independent parts does the expected deliverable have?
- **Error cost**: How expensive is rework if the result goes off track?

After assessment, **inform the user**:

**Complex task** (most cases):
> "This is a fairly complex task. I'll help you define a clear Spec first, ensuring we agree on objectives, scope, and constraints before execution."

**Simple task**:
Use the AskUserQuestion tool to ask:
> "This task is straightforward and may not need a full Spec process. Would you like me to execute directly, or still generate a Spec first?"
- Option 1: Execute directly
- Option 2: Still generate a Spec

If the user chooses direct execution, skip the Spec process and complete the task normally.

The user can always override the system's assessment — if the user says "I think this needs a Spec" or "skip the Spec," defer to their judgment.

### Step 2: Generate Initial Spec

Generate the initial specification using the 3-layer Spec Schema:

#### Core Layer (required for any task)

| Field | Meaning |
|-------|---------|
| **Objective** | What to do + why. State the task's purpose in one sentence |
| **Expected Output** | What exactly to deliver: form, structure, granularity |
| **Constraints** | What absolutely cannot be done / what must be obeyed / where the boundaries are |
| **Definition of Done** | What state counts as "complete," what's the minimum acceptance bar |

#### Context Layer (most tasks need this)

| Field | Meaning |
|-------|---------|
| **Inputs** | Raw data, materials, documents to process |
| **Background** | Why the task exists, prior decisions, audience, use case |
| **References** | Style references, similar cases, existing standards |
| **Stated Assumptions** | Inferences the model made where information was insufficient |

**About "Stated Assumptions" — this is one of the most critical fields.** In normal conversation, the model's assumptions are implicit and invisible to the user. Spec's core value is making these assumptions explicit.

Rules:
- For dimensions with insufficient information, **never silently fill in**
- Must explicitly list as "Stated Assumptions" with ⚠️ markers
- Let the user confirm or modify each assumption

#### Refinement Layer (expand as needed)

| Field | Meaning |
|-------|---------|
| **Priorities** | Trade-off rules when multiple requirements conflict |
| **Exclusions** | Explicitly declared out-of-scope items |
| **Risks** | Known risks, areas of information uncertainty |
| **Quality Preferences** | Depth vs breadth, rigorous vs creative, detailed vs concise |

Simple tasks only need the Core Layer (even a single sentence suffices). Complex tasks need Context and Refinement layers expanded. Decide the depth based on the complexity assessment.

### Step 3: Identify Information Gaps and Clarify

After generating the initial Spec, check for information gaps. If any exist, enter the clarification loop.

#### Core Clarification Principles

**Principle 1: Impact-driven, not completeness-driven.**
Only ask questions where "getting it wrong would seriously derail the result." Don't chase trivial details for the sake of completeness.

**Principle 2: Batch, don't drip-feed.**
Analyze all discovered gaps, then package them by priority into one round. Don't ask one question and wait — that annoys users.

**Principle 3: Provide defaults, not open questions.**
Offer your best inference as a default, letting users "confirm or modify" rather than fill in from scratch. This dramatically reduces cognitive burden.

**Principle 4: Distinguish "blocking gaps" from "optimization gaps."**
- Blocking: Can't execute without this info → must ask first
- Optimization: Can execute but result won't be ideal → assume and mark as ⚠️

**Principle 5: No hard round limit; use convergence judgment instead.**
Convergence criteria: All four Core fields have clear content + no remaining blocking gaps + remaining uncertainty converted to Stated Assumptions.

**Principle 6: User can skip at any time.**
If the user says "stop asking questions, just start," convert all gaps to assumption items with risk levels and proceed.

#### Strategies for Helping Vague Users Converge

Some users are vague not because they're lazy, but because they genuinely haven't figured it out yet. Don't just "wait for answers" — actively help narrow the decision space:

**Strategy A: Option Guidance.** Don't ask open questions; offer 2-3 typical approaches to choose from.
> "Membership systems typically follow three models: ① spend-based points ② subscription tiers ③ behavior-based growth. Which direction appeals to you?"

**Strategy B: Scenario Concretization.** Use specific scenarios to help users discover their preferences.
> "If a user registers but never makes a purchase, should they still enjoy membership benefits?"

**Strategy C: Minimum Viable Spec.** If the user truly can't be more specific, propose a minimal-scope Spec, excluding undecided parts.
> "Based on what you've said so far, I'll define this minimal scope. The parts you haven't decided on won't be included yet — they can be expanded later."

### Step 4: Present Final Spec and Request Confirmation

When convergence judgment determines the Spec is sufficient, present the complete final Spec to the user.

Presentation format:

```
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

---
Please confirm this Spec, or suggest modifications.
After confirmation, use `/sw:execute` to execute based on this Spec.
```

### The Contractual Nature of the Spec

**This is a critical design decision: Spec confirmation = contract in effect.**

- Before confirmation: The user has unlimited modification rights. The system has a duty to help the user think clearly.
- After confirmation: The system delivers against the Spec. Results matching the Spec = qualified delivery.
- If results match the Spec but the user is still unsatisfied → that's a Spec-definition problem, not an execution problem.

Precisely because of this, the pre-confirmation clarification process cannot be a formality — it must genuinely help the user surface issues they haven't thought of.

## Relationship with Other Commands

This Skill is one of four commands in the Specwise system:

- **`/sw:spec`** (this command) — Main flow: automated assessment → clarification → generation
- **`/sw:explore`** — Exploration mode, for when the user knows their idea is still vague. Explore's context seamlessly flows into spec
- **`/sw:draft`** — Quick draft, skips clarification and outputs Spec directly, user modifies themselves
- **`/sw:execute`** — Executes against a confirmed Spec with verification

Three paths (spec / explore→spec / draft) all converge to the same endpoint: a confirmed Spec. Then execute takes over.
