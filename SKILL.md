---
name: hive
description: >
  General-purpose multi-agent orchestration for ANY task type — research, writing, strategy,
  analysis, creative work, coding, data, and mixed projects. Hive dynamically designs the
  agent swarm, selects the right model per agent (Haiku/Sonnet/Opus/GPT/Gemini/Grok via
  Aristotelian telos-matching), and chooses the right harness per agent (Claude Code
  general-purpose, Plan, Explore, or Codex CLI). Nothing is hardcoded — everything flows
  from task analysis. Use Hive proactively whenever a task would benefit from parallel
  specialist agents working toward a shared deliverable, or when the user says "build this",
  "research this thoroughly", "create a comprehensive X", "orchestrate agents", "multi-agent",
  "swarm", or asks for outputs that have multiple distinct components (report + code + slides,
  analysis + strategy + draft, etc.).
version: 1.0.0
author: HappyCapy Research
triggers:
  - hive
  - hive
  - multi-agent
  - orchestrate agents
  - agent swarm
  - build with agents
  - comprehensive research
  - deep analysis with agents
  - parallel agents
---

# Hive: General-Purpose Multi-Agent Orchestration

Hive is a domain-agnostic successor to Forge. Where Forge was designed for software
projects, Hive works for **any task** — research papers, marketing strategies, business
plans, data analyses, creative projects, and complex technical work. The agent swarm, model
assignments, and harness selections are all derived dynamically from task analysis.

**The core insight:** Multi-agent quality gains come not from having more agents, but from
matching each agent's role, model, and harness to the specific cognitive demands of its task.
A Researcher synthesizing 50 sources needs Opus. A Formatter applying a template needs Haiku.
Treating them identically wastes cost on one and sacrifices quality on the other.

## Resource Map (read these on demand, not upfront)

| File | When to read |
|------|-------------|
| `resources/task-taxonomy.md` | Phase 1: to classify task type and calculate coupling |
| `resources/agent-roster.md` | Phase 3: to select roles and wave assignments |
| `resources/model-harness-guide.md` | Phase 3: to assign model + harness per role |
| `resources/coordination-modes.md` | Phase 2: to select parallel/hybrid/sequential |
| `templates/agent-brief.md` | Phase 5: to compose each agent's prompt |
| `templates/hive-contract.md` | Phase 4: to generate the shared contract |

---

## The Seven Phases

### Phase 1: TASK ANALYSIS

Read `resources/task-taxonomy.md` now.

Decompose the user's request into:

1. **Task family** — which of the five families (Knowledge / Construct / Transform / Decide / Hybrid)
2. **Components** — the discrete deliverables or workstreams
3. **Complexity score** (1–25 scale; see taxonomy)
4. **Coupling score** — how tightly the components depend on each other (0.0–1.0)
5. **Quality tier** — what level of output quality does this task demand?

Output the analysis in this format:

```
CRUCIBLE ANALYSIS
=================
Task Family: [Knowledge|Construct|Transform|Decide|Hybrid]
Objective: [one-sentence statement]

Components:
1. [Component Name] — [What it produces]
2. ...

Complexity: [1-25] ([Simple|Moderate|Complex|Very Complex|Extreme])

Coupling Analysis:
  Shared references (analog of shared interfaces): X / Y total
  Circular dependencies: X / Y total
  Shared context ratio: 0.X
  COUPLING SCORE: 0.XX

Recommended Mode: [Parallel|Hybrid|Sequential]
Estimated Agents: [N]
```

---

### Phase 2: COORDINATION MODE SELECTION

Read `resources/coordination-modes.md` now.

Select one of three modes based on the coupling score:

| Coupling Score | Mode | When to Use |
|---------------|------|-------------|
| < 0.4 | **Contract-First Parallel** | Independent components, well-defined outputs |
| 0.4–0.7 | **Hybrid Wave** | Mixed dependencies, some parallel, some sequential |
| > 0.7 | **Progressive-Layer Sequential** | Tightly coupled, emergent design |

---

### Phase 3: AGENT DESIGN — THE CRUCIBLE CORE

This is where Hive differs from every other orchestration approach.

**Read `resources/agent-roster.md` and `resources/model-harness-guide.md` now.**

For each agent in the swarm, determine three things:

#### 3a. Role
Select from the universal role catalog in `agent-roster.md`. Roles are not
hardcoded — match them to the actual cognitive work required. A "Backend Engineer"
is wrong for a marketing strategy; a "Strategist" + "Market Researcher" is right.
Custom roles are allowed and encouraged when standard roles don't fit.

#### 3b. Model
Apply the Aristotelian telos-matching principle from `model-harness-guide.md`:
- Assign the **minimum-sufficient model** that can fulfill the role's cognitive demands.
- Over-assigning wastes cost. Under-assigning breaks quality.
- The model should match the ROLE'S telos, not the user's preference.

Quick reference (full table in model-harness-guide.md):

| Cognitive Demand | Model |
|-----------------|-------|
| Deep synthesis, strategy, high-stakes | `claude-opus-4-6` |
| Production work, writing, analysis | `claude-sonnet-4-6` |
| Bulk generation, pattern application | `claude-haiku-4-5` |
| Structured JSON / schema output | `openai/gpt-4.1` |
| Long documents > 200k tokens | `google/gemini-3.1-pro-preview` |
| Real-time / social context | `x-ai/grok-3` |

#### 3c. Harness
Select the execution environment for each agent:

| Harness | When to Use |
|---------|-------------|
| `general-purpose` | Agent needs to read/write files, run code, use tools |
| `Plan` subagent | Read-only analysis, planning, no file side-effects |
| `Explore` subagent | Discovery scan, file/codebase exploration |
| Codex CLI | Non-Anthropic model needed, or shell-pipeline dispatch |

**Rule:** All execution agents (producing deliverables) use `general-purpose`.
Phase 1 analysis uses `Plan`. Discovery uses `Explore`. Non-Anthropic models use Codex CLI.

---

### Phase 4: CONTRACT GENERATION

Generate the shared contract using Opus 4.6. The contract is the single source of truth
for all agents. A weak contract cascades into swarm failure.

Use `templates/hive-contract.md` as the template. The contract must specify:
1. **Deliverable manifest** — every output, its format, its owner agent
2. **Shared references** — facts, terminology, style conventions all agents must align on
3. **Interface definitions** — how outputs connect (e.g., how the Analysis feeds the Strategy)
4. **Section boundaries** — exactly what each agent owns and must not touch
5. **Quality criteria** — what "done" means for each deliverable

**Contract generation should use Opus 4.6** — either inline if the current session is Opus,
or via Codex CLI if not:

```bash
codex -c model_provider=ai_gateway -m anthropic/claude-opus-4-6 \
      --skip-git-repo-check \
      exec "Generate Hive contract for: [task description and component list]"
```

---

### Phase 5: EXECUTION

Execute agents according to the mode from Phase 2. Use the agent brief template at
`templates/agent-brief.md` for every agent prompt. Fill in:
- `{{AGENT_NAME}}`, `{{AGENT_ROLE}}`, `{{MODEL_CLASS}}`
- `{{MODEL_RATIONALE}}` — why this model was chosen for this role
- `{{HARNESS_TYPE}}` — so agents know their execution context
- `{{CONTRACT}}` — the full shared contract
- `{{ASSIGNMENT}}` — this agent's specific deliverable(s)
- `{{DEPENDENCIES}}` — what this agent consumes from others
- `{{EXPORTS}}` — what this agent produces for others

**All execution agents receive the FULL contract, not just their section.**
Agents given partial context produce 38% more coordination errors (Forge research finding).

#### Spawning Pattern

The pattern depends on the coordination mode selected in Phase 2:

**Contract-First Parallel (coupling < 0.4) — anonymous agents, no A2A:**
```python
# All agents in ONE message — no TeamCreate needed
Agent(subagent_type="general-purpose", model="opus",   prompt=agent_1_brief)
Agent(subagent_type="general-purpose", model="sonnet", prompt=agent_2_brief)
Agent(subagent_type="general-purpose", model="haiku",  prompt=agent_3_brief)

# For non-Anthropic models — Codex CLI
codex -c model_provider=ai_gateway -m openai/gpt-4.1 exec "..."
codex -c model_provider=ai_gateway -m x-ai/grok-3 exec "..."
```

**Hybrid Wave (coupling 0.4–0.7) — named teammates, A2A enabled:**
```python
# Step 1: Create team ONCE before any agents are spawned
TeamCreate(team_name="hive-{project-slug}-{timestamp}")

# Step 2: Spawn wave agents as named teammates (ONE message per wave)
Agent(subagent_type="general-purpose", model="sonnet",
      name="researcher", team_name="hive-{project-slug}-{timestamp}",
      prompt=researcher_brief)   # brief includes A2A_ENABLED=true, TEAM_NAME, PEER_AGENTS
Agent(subagent_type="general-purpose", model="sonnet",
      name="analyst", team_name="hive-{project-slug}-{timestamp}",
      prompt=analyst_brief)

# Agents can now: SendMessage(to="researcher", message="...", summary="...")
# Orchestrator receives all messages automatically

# Step 3: After all waves complete — shut down and clean up
SendMessage(to="*", message={"type": "shutdown_request", "reason": "Hive swarm complete"})
TeamDelete()
```

**Progressive-Layer Sequential (coupling > 0.7) — anonymous agents, no A2A:**
```python
# One agent per layer; pass frozen previous layer output in prompt
output_layer_1 = Agent(subagent_type="general-purpose", model="opus",
    prompt=f"CONTRACT: {contract}. Build Layer 1: {layer_1_spec}")
# validate + fix (max 3 tries)
output_layer_2 = Agent(subagent_type="general-purpose", model="sonnet",
    prompt=f"CONTRACT: {contract}. FROZEN LAYER 1: {output_layer_1}. Build Layer 2: {layer_2_spec}")
```

Spawn all agents within the same wave in **one message** (not sequentially). Wait for
wave completion before starting the next wave.

---

### Phase 6: VALIDATION

Validation criteria depend on task type. After all agents complete:

1. **Completeness check** — every deliverable in the manifest exists
2. **Interface check** — outputs that feed other outputs are compatible
3. **Quality check** — spot-check against quality criteria in contract
4. **Coherence check** — does the whole swarm output read as unified work?

For coherence validation on complex outputs, spawn a Critic agent:

```python
Agent(
    subagent_type="general-purpose",
    model="opus",
    prompt=f"""You are the Quality Critic for this Hive swarm.

CONTRACT: {contract}
ALL OUTPUTS: {merged_outputs}

Review for: completeness, coherence, contract compliance, quality.
Report: what passed, what failed, what needs fixing. Be specific."""
)
```

---

### Phase 7: DELIVERY

1. **Collect** all agent outputs
2. **Merge** in dependency order (outputs that feed others go first)
3. **Fix** targeted validation failures (spawn one Fixer with specific errors, not full regen)
4. **Package** into final deliverable(s)
5. **Attribute** via AGENTS.md (agent name, role, model, harness, deliverable)

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It Fails | Correct Approach |
|-------------|-------------|-----------------|
| Same model for all agents | Wastes Opus on formatting, underpowers synthesis | Telos-match every role |
| `general-purpose` for analysis phases | Burns context on read-only work | Use `Plan` for Phase 1 |
| Partial contract per agent | 38% more coordination errors | Every agent gets full contract |
| Sequential when coupling < 0.4 | 4–8x slower for no quality gain | Parallel when safe |
| Regenerating entire output on validation fail | 4x slower than targeted fix | Spawn Fixer with specific errors |
| Hardcoding tech roles for non-tech tasks | Wrong agents, wrong telos | Use universal roster + custom roles |
| A2A messaging in Parallel mode | Adds overhead with zero benefit; agents are independent | A2A only in Hybrid Wave mode |
| Using anonymous `Agent` tool for Hybrid Wave | Agents can't send/receive messages | Use `TeamCreate` + `name=` for Hybrid Wave |
| Agents messaging for status/confirmations | Floods orchestrator, slows execution | Only message for the 3 permitted triggers |
| Not calling `TeamDelete()` after swarm | Leaves dangling team resources | Always `TeamDelete()` after final wave |

---

## The Aristotelian Principle at the Core of Hive

> *"The excellence (arete) of a thing is the activity by which it fulfills its function well."*
> — Aristotle, Nicomachean Ethics I.7

Applied: a model's excellence is not its benchmark score but how well it performs
the specific task for which it was designed. The virtuous orchestrator does not use
the most powerful model — they use the most appropriate one. Hive's entire
model-selection system is built on this principle.

---

## When NOT to Use Hive

- Single-agent tasks with no interdependencies (use Task/Agent directly)
- Tasks completable in one step by one agent
- Quick fixes or one-file changes
- Exploratory research without a concrete deliverable

Hive adds value when there are **3+ components** that must **interoperate** or
**when the task is too large for one agent's context window**.
