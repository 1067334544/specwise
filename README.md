# Specwise

**Stop guessing. Start specifying.**

A Claude Code skill that transforms how AI handles complex tasks — by defining them clearly before executing.

```
Before:  Vague intent ──→ AI guesses ──→ "Not what I wanted" ──→ Redo

After:   Vague intent ──→ Spec ◀──→ Clarify ──→ Execute ──→ Verify against Spec
```

## The Problem

When you give AI a complex task, the real problem isn't intelligence — it's **implicit assumptions**.

You say "write me a market analysis report." The AI silently decides: the audience is probably executives, the format should be structured, about 2000 words, focus on competitors... None of these were discussed. If the AI guesses right, great. If not, you're redoing the whole thing.

**Specwise** fixes this by making AI's assumptions explicit before execution begins. Think of it as a **task contract** — both sides agree on what "done" looks like, then the AI delivers against that contract.

## How It Works

Four commands, one philosophy: **define before you do.**

### `/sw:spec` — The Main Command

The standard flow. Give it a task, and it will:

1. **Assess complexity** — Is this task simple enough to just do, or complex enough to need a spec?
2. **Generate a structured Spec** — Objective, expected output, constraints, definition of done
3. **Surface hidden assumptions** — Every inference marked with ⚠️, nothing silently filled in
4. **Clarify what matters** — Smart questions that help you think through what you actually need
5. **Confirm** — You review, modify, approve. This becomes the contract.

```
You:    /sw:spec Write a Q2 market analysis report for our SaaS product

Spec:   This is a complex task. I'll help define a clear Spec first.

        📋 Task Spec

        Core:
        - Objective: Analyze Q2 market trends for [product]...
        - Expected Output: Structured report, 3000-5000 words...
        - Constraints: Data-driven, no speculation without citation...
        - Definition of Done: Covers market size, competitors, trends...

        Stated Assumptions:
        ⚠️ Audience is executive leadership
        ⚠️ Focus on North American market
        ⚠️ Competitor set includes [X, Y, Z]

        Questions:
        ❓ Should the report include pricing analysis?
        ❓ Are there specific competitors to focus on?
```

### `/sw:explore` — Think First

For when you're not even sure what you want yet. A thinking partner that helps you go from fuzzy idea to clear requirements through conversation.

```
You:    /sw:explore I want to add some kind of membership system

AI:     Membership systems can serve different goals —
        retention? revenue? user segmentation?
        What matters most to you?

You:    Mainly retention

AI:     For retention, there are typically two approaches...
        [conversation continues, gradually converging]

AI:     Your thinking is getting clear. Want to turn this into a formal Spec?
```

### `/sw:draft` — Skip the Questions

For when you know exactly what you want. Generates a complete Spec instantly — you review and edit it yourself.

```
You:    /sw:draft Design a 3-tier SaaS pricing system with annual discount and free trial

Spec:   [Complete spec generated immediately, no questions asked]
        ⚠️ This Spec was not clarified. Please review carefully before confirming.
```

### `/sw:execute` — Deliver Against the Spec

Once a Spec is confirmed, execute it. The result comes with a **5-dimension verification report**:

```
📋 Verification Report

Goal Fulfillment:
✅ Core objective — Market analysis complete
✅ Competitor coverage — All 5 competitors analyzed

Constraint Compliance:
✅ Data citations — All claims sourced
⚠️ Word count — 15% over limit (justified: Competitor C needed extra depth)

Completeness:
- Missing: Data visualization (no raw data provided)
- Extra: None

Assumption Audit:
⚠️ "North American market" — Confirmed, still valid
⚠️ "Executive audience" — Confirmed

Risks:
⚠️ Market data as of 2025 Q3, may not reflect latest trends
```

## Installation

### Clone the repo

```bash
git clone https://github.com/user/specwise.git
```

### Copy skills to your project

Copy the `.claude/skills/` and `.claude/commands/` directories to your project:

```bash
cp -r specwise/.claude/skills/specwise-* your-project/.claude/skills/
cp -r specwise/.claude/commands/sw your-project/.claude/commands/
```

### Or install globally

Copy to your home directory for all projects:

```bash
cp -r specwise/.claude/skills/specwise-* ~/.claude/skills/
cp -r specwise/.claude/commands/sw ~/.claude/commands/
```

## The Spec Schema

Every Spec follows a 3-layer structure:

| Layer | Fields | When |
|-------|--------|------|
| **Core** (required) | Objective, Expected Output, Constraints, Definition of Done | Always |
| **Context** (most tasks) | Inputs, Background, References, **Stated Assumptions** | Most tasks |
| **Refinement** (as needed) | Priorities, Exclusions, Risks, Quality Preferences | Complex tasks |

The **Stated Assumptions** field is what makes this system unique. Instead of silently filling in gaps, the AI explicitly marks every inference with ⚠️ and asks you to confirm.

## Why This Works

| Direct Chat | Specwise |
|-------------|----------------|
| AI silently assumes | Every assumption is explicit ⚠️ |
| "Looks right" = done | 5-dimension verification = done |
| Redo when wrong | Clarify before executing |
| Risk hidden in confident prose | Risk disclosed as a managed dimension |
| Completion is a feeling | Completion is auditable |

**Specwise doesn't make AI smarter. It raises the floor on task quality** — by trading a small upfront investment in specification for significantly less rework, higher consistency, and verifiable results.

## Command Reference

| Command | Purpose | Best For |
|---------|---------|----------|
| `/sw:spec` | Full guided flow | Most tasks — the default choice |
| `/sw:explore` | Open-ended thinking | Vague ideas that need shaping |
| `/sw:draft` | Quick spec generation | Expert users who know what they want |
| `/sw:execute` | Execute + verify | After any Spec is confirmed |

## License

MIT
