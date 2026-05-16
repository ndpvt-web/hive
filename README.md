# Hive — Multi-Agent Orchestration for Claude Code

**Hive** is a Claude Code skill that turns any complex task into a coordinated swarm of specialist AI agents. Give it a goal; it designs the team, assigns the right model to each role, and delivers a unified result.

Works for code, research, writing, strategy, analysis, creative projects — anything with more than one moving part.

---

## Why Hive

Most multi-agent setups assign the same model to every agent and hope for the best.

Hive is built on a different principle: **the right model for the right cognitive job**. A researcher synthesizing 50 sources needs Opus. A formatter applying a template needs Haiku. Treating them identically wastes cost on one and breaks quality on the other.

This is Aristotelian telos-matching — every agent's model is chosen by what the role demands, not by habit.

---

## How It Works

Hive runs seven phases automatically:

| Phase | What Happens |
|-------|-------------|
| **1. Task Analysis** | Decomposes the goal into components, scores complexity and coupling |
| **2. Mode Selection** | Picks parallel, hybrid wave, or sequential based on coupling score |
| **3. Agent Design** | Assigns role + model + harness (Claude vs Codex CLI) to each agent |
| **4. Contract Generation** | Opus writes a shared contract — the single source of truth for all agents |
| **5. Execution** | Agents run with full contract context; Hybrid Wave agents can message each other |
| **6. Validation** | Critic agent checks completeness, coherence, and contract compliance |
| **7. Delivery** | Outputs merged in dependency order, targeted fixes applied, final package delivered |

---

## Coordination Modes

Chosen automatically based on how tightly the components depend on each other:

```
Coupling < 0.4   →  Contract-First Parallel
                    All agents run simultaneously. No A2A needed.
                    Speedup: 4–8x vs sequential.

Coupling 0.4–0.7 →  Hybrid Wave
                    Agents grouped in waves. Within a wave: parallel.
                    Between waves: sequential (frozen outputs passed forward).
                    A2A messaging enabled for real-time peer coordination.
                    Speedup: 2–5x vs sequential.

Coupling > 0.7   →  Progressive-Layer Sequential
                    Each layer builds directly on the previous layer's frozen output.
                    Used when design cannot be fully specified upfront.
```

---

## Agent-to-Agent (A2A) Communication

In Hybrid Wave mode, agents can message each other directly — without routing through the orchestrator.

Three permitted triggers:

1. **Interface deviation** — output format must change from what the contract specified
2. **Partial output ready** — a section is final and a blocked peer can start consuming it
3. **Peer-resolvable blocker** — a specific gap that only a peer agent can fill

Everything else is resolved through the contract. A2A is the exception channel, not the default one.

The orchestrator (Claude Code) receives all messages automatically.

---

## Model Selection

| Cognitive Demand | Model |
|-----------------|-------|
| Deep synthesis, strategy, high-stakes decisions | `claude-opus-4-6` |
| Production writing, analysis, implementation | `claude-sonnet-4-6` |
| Bulk generation, formatting, pattern application | `claude-haiku-4-5` |
| Structured JSON / schema-heavy output | `openai/gpt-4.1` |
| Long documents (200k+ tokens) | `google/gemini-3.1-pro-preview` |
| Real-time / social context | `x-ai/grok-3` |

Non-Anthropic models run via Codex CLI with an AI Gateway passthrough.

---

## File Structure

```
hive/
├── SKILL.md                         # Main orchestration logic (7 phases)
├── resources/
│   ├── task-taxonomy.md             # 5 task families, complexity scoring, coupling formula
│   ├── agent-roster.md              # Universal role catalog with telos descriptions
│   ├── model-harness-guide.md       # Model selection rules + harness assignment
│   └── coordination-modes.md        # Parallel / Hybrid Wave / Sequential specs + A2A rules
└── templates/
    ├── agent-brief.md               # Per-agent prompt template with A2A section
    └── crucible-contract.md         # Shared contract template (Section 8: A2A protocol)
```

Resources are loaded on demand — only the files relevant to the current phase are read.

---

## When to Use Hive

Use Hive when the task has **3 or more components** that must **interoperate** or when it's **too large for one agent's context window**.

```
Good fits:
  - "Research the EV market and write a 20-page strategy report"
  - "Build a full-stack app with tests and documentation"
  - "Create a pitch deck + financial model + executive summary"
  - "Analyze this dataset and produce visualizations + narrative"

Not a good fit:
  - Single-step tasks (just use Agent directly)
  - Quick fixes or one-file changes
  - Exploratory research with no concrete deliverable
```

---

## Installation

Copy to your Claude Code skills directory:

```bash
# Via ClawHub
/install hive

# Manual
cp -r hive/ ~/.claude/skills/hive/
```

Then trigger with: `/hive`, `multi-agent`, `agent swarm`, or any prompt that asks for a comprehensive multi-part output.

---

## Anti-Patterns

| Anti-Pattern | Fix |
|-------------|-----|
| Same model for all agents | Telos-match every role individually |
| Partial contract per agent | Every agent receives the full contract |
| Sequential execution when coupling < 0.4 | Use parallel — 4–8x speed gain |
| A2A messaging in Parallel mode | A2A is Hybrid Wave only |
| Anonymous agents in Hybrid Wave | Use `TeamCreate` + `name=` for A2A to work |
| Messaging peers for status updates | Only message for the 3 permitted triggers |

---

## The Principle Behind the Design

> *"The excellence of a thing is the activity by which it fulfills its function well."*
> — Aristotle, Nicomachean Ethics I.7

A model's excellence is not its benchmark score — it's how well it performs the specific task it was designed for. The virtuous orchestrator doesn't use the most powerful model; they use the most appropriate one. Hive's entire architecture is built on this principle.

---

Built by [HappyCapy Research](https://happycapy.ai)
