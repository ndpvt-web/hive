# Crucible Model & Harness Guide
## Aristotelian Telos-Matching for Agent Assignment

The core principle: **assign the minimum-sufficient model that can fulfill the role's
cognitive demands.** Over-assigning wastes cost. Under-assigning breaks quality.
Match the model's telos to the role's telos.

---

## The Telos-Matching Decision Process

For each agent role, ask three questions in order:

1. **What cognitive demands does this role have?**
   (Depth of reasoning, synthesis, pattern vs. invention, stakes if wrong)

2. **What is the minimum model whose telos covers these demands?**
   (Start from Haiku, escalate only when necessary)

3. **What harness does this role need?**
   (Does it need file I/O? Run code? Or just think?)

---

## Model Selection: Full Decision Table

### Available Models (via AI Gateway)

| Model ID | Telos | Best For | Sophia Score |
|----------|-------|----------|-------------|
| `anthropic/claude-haiku-4.5` | Speed & scale | Bulk generation, pattern application, formatting | 2/5 |
| `anthropic/claude-sonnet-4.6` | General production | Writing, analysis, coding, balanced tasks | 4/5 |
| `anthropic/claude-opus-4-6` | Deep synthesis | Strategy, architecture, high-stakes, synthesis | 5/5 |
| `google/gemini-3.1-flash-preview` | Fast multimodal | Image+text, fast visual tasks | 2.5/5 |
| `google/gemini-3.1-pro-preview` | Long-context multimodal | >200k token docs, video+audio+text | 4.5/5 |
| `openai/gpt-4.1` | Structured output | JSON schemas, function calling, structured formats | 4/5 |
| `openai/gpt-5.5` | Frontier STEM | Math, formal proofs, competitive algorithms | 4.5/5 |
| `x-ai/grok-3` | Real-time / social | Current events, social media, trend analysis | 4/5 |
| `minimax` (via Codex) | Multilingual creative | Chinese/Japanese/Korean, long-form creative | 3/5 |

---

## Role → Model Mapping

### Always Opus 4.6

These roles require maximum depth. The cost is justified because errors cascade to all
downstream agents or because the stakes of a wrong answer are very high.

| Role | Why Opus |
|------|---------|
| Lead Architect | Designs the contract — single point of failure for the swarm |
| Integration Lead | Must synthesize ALL other agents' outputs coherently |
| Strategist | Complex decisions with cascading trade-offs |
| Risk Analyst | Misses here become real risks — needs full adversarial reasoning depth |
| Devil's Advocate | Must find non-obvious counterarguments, not surface-level challenges |
| Systems Architect | Technical architecture cascades like Lead Architect |
| Quality Critic | Must identify subtle incoherence across the full swarm output |
| Domain Expert (high-stakes) | When the domain is security, medical, legal, or financial |

**Escalate any role to Opus when:**
- Errors in this role's output would break all downstream agents
- The task is security-critical, legally significant, or financially high-stakes
- The role requires synthesizing 5+ other agents' outputs simultaneously

---

### Default: Sonnet 4.6

The production workhorse. Covers ~70% of roles in a typical Crucible project.

| Role | Why Sonnet |
|------|-----------|
| Research Analyst | Synthesis and evaluation, not just retrieval |
| Domain Expert (standard) | Expertise without extreme stakes |
| Lead Writer | Quality prose requires more than pattern matching |
| Section Writers | Solid production writing |
| Structural Designer | Organizing a document structure |
| Data Analyst | Statistical reasoning and insight |
| Options Generator | Creative options require some reasoning depth |
| Evaluator / Scorer | Comparative analysis |
| Literature Reviewer | Survey and synthesis |
| Planner / Roadmap Builder | Sequencing and dependencies |
| Creative Director | Creative framing and vision |
| Engineers / Developers | Production code work |

---

### Escalate to Haiku 4.5

These roles are high-volume, pattern-application, or mechanical transformation.
They do not need deep reasoning — they need speed and throughput.

| Role | Why Haiku |
|------|----------|
| Copy Editor | Grammar and style pattern application, not reasoning |
| Fact Checker | Verification is pattern matching (does this claim match sources?) |
| Format Converter | Mechanical transformation |
| Summarizer | Condensing content, not analyzing it |
| Classifier / Tagger | Categorical assignment |
| Data Processor | Cleaning and normalizing, not reasoning |
| Extractor | Pulling entities from text |
| Visual Narrator | Describing visuals in structured format |
| QA / Tester (for code) | Test pattern generation |
| Documentation Writer | Structured writing, not synthesis |

**Rule:** If the role's output could be described as "apply template X to input Y",
use Haiku. If it requires "figure out what X should be", use Sonnet or Opus.

---

### Use Non-Anthropic Models Via Codex CLI

These models have specific teloses where they outperform Claude family:

| Scenario | Model | Codex CLI Command |
|----------|-------|------------------|
| Structured JSON contract output | `openai/gpt-4.1` | `codex -c model_provider=ai_gateway -m openai/gpt-4.1 exec "..."` |
| Document > 200k tokens | `google/gemini-3.1-pro-preview` | `codex -c model_provider=ai_gateway -m google/gemini-3.1-pro-preview exec "..."` |
| Image + text analysis (fast) | `google/gemini-3.1-flash-preview` | `codex -c model_provider=ai_gateway -m google/gemini-3.1-flash-preview exec "..."` |
| Current events / social media | `x-ai/grok-3` | `codex -c model_provider=ai_gateway -m x-ai/grok-3 exec "..."` |
| STEM / formal proofs | `openai/gpt-5.5` | `codex -c model_provider=ai_gateway -m openai/gpt-5.5 exec "..."` |
| East Asian languages | minimax | `codex -c model_provider=ai_gateway -m minimax exec "..."` |

---

## Harness Selection: Full Decision Table

### The Four Harnesses

#### `general-purpose` — The Default Execution Harness

**Use when:** The agent needs to produce files, run code, read existing files, or use tools.

All execution agents (the ones actually producing deliverables) use `general-purpose`.
This includes ALL roles from the agent roster.

```python
# Claude Code Agent tool
Agent(
    subagent_type="general-purpose",
    model="opus",    # or "sonnet" or "haiku"
    prompt="..."
)
```

**Every execution agent should have `subagent_type="general-purpose"`.** No exceptions.

---

#### `Plan` subagent — Read-Only Planning

**Use when:** The task is pure analysis/planning with NO file side-effects needed.

The `Plan` subagent CANNOT write files. Using it for execution agents breaks the project.
Use it only for:
- Phase 1 task analysis (before agents are designed)
- Pre-wave planning (which agents go in which wave)
- Contract quality review (read-only sanity check before freezing)

```python
Agent(
    subagent_type="Plan",
    model="sonnet",
    prompt="Analyze this task and produce a Crucible analysis..."
)
```

---

#### `Explore` subagent — Discovery and Scanning

**Use when:** An agent needs to read many files/sources to discover patterns, without
producing new files.

Use for:
- Security/quality audit initial passes (read all outputs, report findings)
- Codebase or document discovery (what exists before building on it)
- Pre-validation scanning (verify file structure before formal validation)

```python
Agent(
    subagent_type="Explore",
    model="sonnet",
    prompt="Scan all outputs in this directory and report completeness..."
)
```

---

#### Codex CLI — Non-Anthropic Model Dispatch

**Use when:**
- You need a model outside the Anthropic family (GPT, Gemini, Grok)
- You want shell-native, scriptable model dispatch
- You want to chain model calls in a pipeline

```bash
# Dispatch to any model via AI Gateway
codex -c model_provider=ai_gateway -m <model-id> --skip-git-repo-check exec "prompt"

# Examples:
codex -c model_provider=ai_gateway -m openai/gpt-4.1 exec "Extract JSON schema from..."
codex -c model_provider=ai_gateway -m google/gemini-3.1-pro-preview exec "Analyze this 500-page report..."
codex -c model_provider=ai_gateway -m x-ai/grok-3 exec "Write social media copy for..."
```

---

## Hybrid Harness Strategy

Some agents benefit from a two-phase approach:

### Pattern: Explore-then-Write

1. First, spawn `Explore` subagent to discover/scan all relevant material
2. Feed Explore output to a `general-purpose` agent to produce the deliverable

**Good for:** Quality Critic (scan all outputs first, then write review),
Security Analyst (scan codebase first, then write threat model)

### Pattern: Codex-then-Claude-Code

1. Codex CLI calls a specialized model (e.g., Gemini for a 500k-token document)
2. Gemini's analysis output is saved to a file
3. Claude Code `general-purpose` agent uses that file to produce the final deliverable

**Good for:** Tasks where the input exceeds Claude's context window but the output
should be written by Claude (which has superior writing quality)

---

## Quick Reference: The Assignment Matrix

| Cognitive Demand | Harness | Model |
|-----------------|---------|-------|
| Swarm-wide design, contract gen | `general-purpose` | **Opus** |
| Strategic synthesis, high-stakes decisions | `general-purpose` | **Opus** |
| Risk analysis, adversarial review | `general-purpose` | **Opus** |
| Final integration of all outputs | `general-purpose` | **Opus** |
| Research, analysis, production writing | `general-purpose` | **Sonnet** |
| Planning, structuring, evaluation | `general-purpose` | **Sonnet** |
| Domain expertise (standard) | `general-purpose` | **Sonnet** |
| Bulk generation, pattern application | `general-purpose` | **Haiku** |
| Editing, formatting, classification | `general-purpose` | **Haiku** |
| Pre-execution task analysis | `Plan` | Sonnet |
| Discovery/scan passes | `Explore` | Sonnet |
| Non-Anthropic model tasks | Codex CLI | GPT/Gemini/Grok |
| >200k token input | Codex CLI | Gemini Pro |
| Real-time/social content | Codex CLI | Grok-3 |
| Structured JSON output | Codex CLI | GPT-4.1 |

---

## The Aristotelian Verification Test

Before finalizing any model assignment, apply this test:

1. **Telos match:** Does this model's designed purpose match this role's purpose?
2. **Necessity:** Would a cheaper model fail at this role? (If no, step down)
3. **Sufficiency:** Would a more expensive model do it better in a way that matters? (If yes, step up)
4. **Stakes:** If this agent produces wrong output, how bad is it? (High stakes = escalate)
5. **Volume:** Is this a high-volume, repetitive task? (High volume = Haiku if possible)

A model assignment is virtuous when all five tests are satisfied.
