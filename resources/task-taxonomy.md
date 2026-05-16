# Crucible Task Taxonomy

## The Five Task Families

Every user request maps to one of five families. Identify the primary family first,
then check for hybrid overlap.

---

### 1. KNOWLEDGE

**Essence:** Finding, evaluating, and synthesizing information.

**Indicators:** "research", "analyze", "investigate", "find out", "what is", "how does",
"compare", "evaluate", "audit", "review the literature", "deep dive"

**Typical components:**
- Source discovery and collection
- Critical evaluation of sources
- Synthesis across sources
- Findings documentation
- Recommendations from evidence

**Coupling profile:** Usually medium-high (0.5–0.7). Different research threads must converge
on shared conclusions. A finding in one domain often changes framing in another.

**Quality tiers:**
- Quick scan (complexity 1–5): 1–2 agents, surface-level
- Standard research (6–12): 3–5 agents, moderate depth
- Deep analysis (13–20): 6–10 agents, exhaustive coverage
- Scholarly/professional (21–25): 10+ agents, citation-grade rigor

---

### 2. CONSTRUCT

**Essence:** Creating an artifact — writing, design, narrative, plan, proposal.

**Indicators:** "write", "create", "build", "draft", "design", "produce", "generate",
"compose", "develop a", "make a"

**Typical components:**
- Research/context gathering
- Structural design
- Primary content creation
- Editing and polish
- Supporting materials (appendices, references, visuals)

**Coupling profile:** Medium (0.4–0.6). Sections must be stylistically consistent and
logically coherent. The structure agent's output constrains all writers.

**Quality tiers:**
- Draft (1–5): 1–2 agents
- Professional document (6–12): 3–6 agents
- Comprehensive deliverable (13–20): 5–10 agents
- Publication-grade (21–25): 8–15 agents

---

### 3. TRANSFORM

**Essence:** Converting, extracting, reformatting, or processing existing material.

**Indicators:** "convert", "extract", "summarize", "reformat", "translate", "classify",
"tag", "process", "clean", "parse", "restructure"

**Typical components:**
- Input parsing/understanding
- Transformation logic application
- Output formatting
- Validation of transform fidelity

**Coupling profile:** Usually low (0.2–0.4). Transform steps are often pipeline-sequential
but loosely coupled. Parallelism is high when inputs are independent.

**Quality tiers:**
- Single-pass transform (1–5): 1–2 agents
- Multi-step pipeline (6–12): 3–5 agents
- Complex extraction (13–20): 4–8 agents

---

### 4. DECIDE

**Essence:** Producing recommendations, strategies, decisions, or evaluations.

**Indicators:** "should we", "what's the best", "recommend", "strategy for", "plan for",
"evaluate options", "make a decision", "prioritize", "roadmap", "go-to-market"

**Typical components:**
- Problem framing
- Options generation
- Options evaluation (criteria-based)
- Risk analysis
- Recommendation with rationale
- Implementation plan

**Coupling profile:** High (0.6–0.8). The problem framing constrains option generation;
option evaluation feeds recommendation; risk analysis modifies recommendation. Tightly
coupled by nature.

**Quality tiers:**
- Quick recommendation (1–5): 1–2 agents
- Structured decision (6–12): 3–5 agents
- Strategic analysis (13–20): 5–9 agents
- Board/executive-grade (21–25): 8–14 agents

---

### 5. HYBRID

**Essence:** Any task combining two or more of the above families.

**Indicators:** "research and then write", "analyze and recommend", "build a strategy and
execute it", combinations of the above indicator phrases

**Examples:**
- Business plan: Knowledge + Construct + Decide
- Competitive analysis + marketing strategy: Knowledge + Decide + Construct
- Data pipeline + report: Transform + Construct + Knowledge

**Coupling profile:** Variable — use the highest coupling score of any sub-family.

---

## Complexity Scoring (1–25)

Use this scale for ALL task families:

| Score | Label | Indicators |
|-------|-------|-----------|
| 1–5 | Simple | Single deliverable, clear scope, one domain |
| 6–10 | Moderate | 2–4 components, some interdependencies |
| 11–15 | Complex | 5–8 components, multiple domains, state management |
| 16–20 | Very Complex | 9–15 components, cross-domain synthesis, cascading dependencies |
| 21–25 | Extreme | 15+ components, novel domain combinations, emergent complexity |

**Complexity factors (add points):**
- Each additional domain/subject area: +1
- Requires original analysis (not just summarizing): +2
- Multiple output formats (e.g., report + slides + code): +2
- Tight quality requirements (publication-grade): +2
- Real-time or current data required: +1
- Stakeholder-facing (client/exec deliverable): +1
- Long-form output > 10,000 words: +2

---

## Coupling Score Calculation

Adapted from Forge for non-code tasks:

```
coupling_score = (shared_references / total_references) × 0.4
              + (convergence_dependencies / total_dependencies) × 0.3
              + (shared_context_ratio) × 0.3
```

Where:
- **shared_references**: Definitions, facts, terminology used by 2+ agents
- **total_references**: All references/sources in the project
- **convergence_dependencies**: Components whose output changes based on another component
- **total_dependencies**: Total inter-component dependencies
- **shared_context_ratio**: Proportion of agents that must share a common understanding

### Coupling Quick-Estimate

If you don't want to calculate precisely:

| Task Shape | Approximate Coupling |
|-----------|---------------------|
| Independent parallel workstreams | 0.1–0.3 |
| Shared style + independent content | 0.3–0.5 |
| Each section references previous | 0.5–0.7 |
| All components deeply intertwined | 0.7–0.9 |

---

## Task Analysis Output Template

```
CRUCIBLE TASK ANALYSIS
======================

Task Family: [Knowledge | Construct | Transform | Decide | Hybrid]
Sub-families (if hybrid): [list]

Objective: [One sentence statement of what we're producing]

Components:
1. [Name] — [What it produces, who consumes it]
2. [Name] — [What it produces, who consumes it]
...

Complexity Score: [1–25]
  - Base: [N]
  - Adjustments: [list factors]
  - Final: [N] ([Simple|Moderate|Complex|Very Complex|Extreme])

Coupling Analysis:
  Shared references: [X] / [Y] total
  Convergence dependencies: [X] / [Y] total
  Shared context ratio: [0.X]
  COUPLING SCORE: [0.XX]

Recommended Mode: [Parallel | Hybrid | Sequential]
Estimated Agents: [N]
Quality Tier: [Draft | Professional | Comprehensive | Publication-grade]
```
