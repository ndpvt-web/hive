# Crucible Universal Agent Roster

This roster covers all task families. Roles are not hardcoded — select the ones that
match the actual cognitive work required. Custom roles are encouraged.

## Table of Contents
1. [Universal Roles (always available)](#universal-roles)
2. [Knowledge Roles](#knowledge-roles)
3. [Construct Roles](#construct-roles)
4. [Transform Roles](#transform-roles)
5. [Decide Roles](#decide-roles)
6. [Technical Roles (when project has code/data)](#technical-roles)
7. [Role Selection Algorithm](#role-selection-algorithm)
8. [Wave Assignment Patterns](#wave-assignment-patterns)

---

## Universal Roles

These roles appear in almost every Crucible project regardless of task family.

### Lead Architect (always Wave 1, solo)
**Responsibility:** Designs the overall approach, generates the contract, defines deliverable
manifest, sets shared conventions (terminology, style, format).
**Model:** Opus 4.6
**Harness:** `general-purpose`
**Must run first**, alone. All other agents depend on the contract this agent generates.

### Integration Lead (always final wave, solo)
**Responsibility:** Combines all agent outputs into the final unified deliverable. Resolves
inconsistencies, ensures stylistic coherence, validates completeness.
**Model:** Opus 4.6
**Harness:** `general-purpose`
**Required when:** 3+ execution agents. Optional for simpler projects.

### Quality Critic
**Responsibility:** Reviews all outputs against quality criteria in the contract. Identifies
gaps, inconsistencies, errors. Does not fix — reports with specific, actionable findings.
**Model:** Opus 4.6
**Harness:** `general-purpose` (needs to read all outputs)
**Wave:** Second-to-last (after all content agents, before Integration Lead)

---

## Knowledge Roles

### Research Analyst
**Responsibility:** Discovers, evaluates, and synthesizes information from multiple sources.
Primary intelligence-gathering role. Produces annotated findings, not raw data dumps.
**Model:** Sonnet 4.6
**Harness:** `general-purpose` (web search + write findings)
**Wave:** Early (Wave 2 in most projects)

### Domain Expert (instantiate with specific domain)
**Responsibility:** Deep expertise in a specific field. Named per domain:
"Climate Science Expert", "Financial Markets Expert", "Regulatory Expert", etc.
The specialization should be in the agent's name AND prompt.
**Model:** Sonnet 4.6 (Opus if domain is highly technical and stakes are high)
**Harness:** `general-purpose`
**Wave:** Parallel with Research Analyst

### Fact Checker
**Responsibility:** Verifies claims, checks sources, flags unsubstantiated assertions.
High-volume verification work. Does not synthesize — reports pass/fail per claim.
**Model:** Haiku 4.5 (pattern-matching verification, not synthesis)
**Harness:** `general-purpose`
**Wave:** After content is generated, before Quality Critic

### Literature Reviewer
**Responsibility:** Surveys existing work in a field. Maps what is known, what is contested,
what is unknown. Produces a structured literature landscape.
**Model:** Sonnet 4.6
**Harness:** `general-purpose`
**Wave:** Early, parallel with Research Analyst

### Data Analyst
**Responsibility:** Quantitative and statistical analysis. Works with numbers, trends,
correlations. Produces charts, tables, and numeric insights.
**Model:** Sonnet 4.6 (GPT-4.1 if structured JSON output needed)
**Harness:** `general-purpose` (runs analysis scripts, writes output)
**Wave:** Parallel with Research Analyst if data is pre-available

---

## Construct Roles

### Lead Writer
**Responsibility:** Primary prose creation. Owns the main narrative. Writes from the
structural outline created by Lead Architect.
**Model:** Sonnet 4.6 (Opus for high-stakes publications)
**Harness:** `general-purpose`
**Wave:** After structure is set

### Section Writer (instantiate per section)
**Responsibility:** Owns one specific section of a longer document. Named per section:
"Executive Summary Writer", "Methods Section Writer", "Case Studies Writer".
**Model:** Sonnet 4.6
**Harness:** `general-purpose`
**Wave:** Parallel with other section writers (they're independent if contract is clear)

### Copy Editor
**Responsibility:** Grammar, style consistency, voice alignment, clarity improvements.
Does not change substance — only expression. Pattern-application work.
**Model:** Haiku 4.5
**Harness:** `general-purpose`
**Wave:** After all writing is complete

### Structural Designer
**Responsibility:** Designs the document architecture: sections, flow, hierarchy, narrative arc.
Produces an outline that all writers follow.
**Model:** Opus 4.6 (structure cascades to everything)
**Harness:** `general-purpose`
**Wave:** Wave 2 (after Lead Architect, before writers)

### Creative Director
**Responsibility:** Defines the creative vision, tone, and thematic approach. Sets the
"feel" that all other Construct agents must align with.
**Model:** Sonnet 4.6
**Harness:** `general-purpose`
**Wave:** Wave 2 (parallel with Structural Designer)

### Visual Narrator
**Responsibility:** Describes charts, diagrams, infographics, and visual elements.
Writes alt-text, captions, and data visualization specifications.
**Model:** Haiku 4.5 (structured description work)
**Harness:** `general-purpose`
**Wave:** After content agents, parallel with Copy Editor

---

## Transform Roles

### Data Processor
**Responsibility:** Cleans, normalizes, and transforms raw data. Handles missing values,
type conversions, schema alignment. High-volume mechanical work.
**Model:** Haiku 4.5
**Harness:** `general-purpose` (runs processing scripts)
**Wave:** Early

### Format Converter
**Responsibility:** Changes the representation format of content. Markdown → DOCX, CSV → JSON,
transcript → structured notes. Mechanical transformation.
**Model:** Haiku 4.5
**Harness:** `general-purpose`
**Wave:** Early or parallel

### Summarizer (instantiate per corpus)
**Responsibility:** Condenses a specific body of material. Named per corpus:
"Market Reports Summarizer", "Interview Transcripts Summarizer".
**Model:** Haiku 4.5 (for short-to-medium content), Sonnet 4.6 (for complex content)
**Harness:** `general-purpose`
**Wave:** Parallel when inputs are independent

### Classifier / Tagger
**Responsibility:** Applies taxonomies, labels, or categories to items.
Bulk categorical work.
**Model:** Haiku 4.5
**Harness:** `general-purpose`
**Wave:** Early; others may depend on classification output

### Extractor
**Responsibility:** Pulls specific entities, facts, or structured data from unstructured
sources. Named per target: "Entity Extractor", "Key Quote Extractor".
**Model:** Haiku 4.5 (GPT-4.1 if structured JSON output required)
**Harness:** `general-purpose`
**Wave:** Early

---

## Decide Roles

### Strategist
**Responsibility:** Synthesizes analysis into strategic recommendations. Applies frameworks
(SWOT, Porter's Five Forces, Jobs-to-be-Done, etc.). Highest-stakes Decide role.
**Model:** Opus 4.6
**Harness:** `general-purpose`
**Wave:** After research/analysis agents

### Risk Analyst
**Responsibility:** Identifies risks, failure modes, and mitigation paths. Adversarial
framing — looks for what could go wrong.
**Model:** Opus 4.6 (risks require deep reasoning; under-identified risks are costly)
**Harness:** `general-purpose`
**Wave:** Parallel with Strategist

### Options Generator
**Responsibility:** Produces a structured set of alternatives or options for evaluation.
Divergent thinking. Breadth over depth.
**Model:** Sonnet 4.6
**Harness:** `general-purpose`
**Wave:** Early in Decide projects (others evaluate these options)

### Evaluator / Scorer
**Responsibility:** Applies criteria to options and scores them. Comparative analysis.
Convergent thinking.
**Model:** Sonnet 4.6
**Harness:** `general-purpose`
**Wave:** After Options Generator

### Devil's Advocate
**Responsibility:** Challenges the leading recommendations. Argues the opposing case.
Stress-tests assumptions.
**Model:** Opus 4.6 (needs depth to find non-obvious counterarguments)
**Harness:** `general-purpose`
**Wave:** After Strategist and Evaluator

### Planner / Roadmap Builder
**Responsibility:** Translates decisions into sequenced action plans. Timelines, milestones,
dependencies.
**Model:** Sonnet 4.6
**Harness:** `general-purpose`
**Wave:** After decisions are made

---

## Technical Roles

Include these when the project has code, data engineering, or systems design components.
For full technical projects, see the Forge skill which has a more complete technical roster.

### Engineer / Developer
**Responsibility:** Implements code, scripts, pipelines, or technical components.
Domain specialized: "Python Data Engineer", "React Frontend Developer", etc.
**Model:** Sonnet 4.6
**Harness:** `general-purpose`

### Systems Architect
**Responsibility:** Designs technical systems, APIs, data models. High-stakes design work.
**Model:** Opus 4.6
**Harness:** `general-purpose`

### QA / Tester
**Responsibility:** Tests, verifies, validates technical outputs.
**Model:** Haiku 4.5 (pattern-based test generation)
**Harness:** `general-purpose`

---

## Role Selection Algorithm

```
START
  |
  v
ALWAYS ADD: Lead Architect (Wave 1)
  |
  v
EVALUATE TASK FAMILY:
  |
  ├─> Knowledge? → ADD: Research Analyst + [Domain Expert if specialized field]
  |
  ├─> Construct? → ADD: Structural Designer + Lead Writer (or Section Writers if long-form)
  |
  ├─> Transform? → ADD: appropriate Transform roles based on input/output types
  |
  ├─> Decide? → ADD: Options Generator + Evaluator + Strategist + Risk Analyst
  |
  └─> Hybrid? → ADD roles from each applicable family
  |
  v
EVALUATE QUALITY NEEDS:
  |
  ├─> High stakes / publication-grade? → ADD: Quality Critic + Fact Checker
  |
  ├─> Long-form (>5000 words)? → ADD: Copy Editor
  |
  └─> Decision output? → ADD: Devil's Advocate
  |
  v
IF agent_count >= 3:
  ADD: Integration Lead (final wave)
  |
  v
DONE: Return agent list with role + model + harness for each
```

---

## Wave Assignment Patterns

### Pattern 1: Pure Knowledge (3–5 agents)
```
Wave 1: Lead Architect
Wave 2: Research Analyst || Domain Expert || Literature Reviewer (parallel)
Wave 3: Data Analyst (if quantitative) || Fact Checker
Wave 4: Quality Critic
Wave 5: Integration Lead
```

### Pattern 2: Construct Document (4–7 agents)
```
Wave 1: Lead Architect
Wave 2: Structural Designer || Research Analyst (parallel)
Wave 3: Section Writers (all parallel)
Wave 4: Copy Editor || Fact Checker || Visual Narrator (parallel)
Wave 5: Quality Critic
Wave 6: Integration Lead
```

### Pattern 3: Decision / Strategy (5–8 agents)
```
Wave 1: Lead Architect
Wave 2: Research Analyst || Domain Expert (parallel)
Wave 3: Options Generator
Wave 4: Evaluator || Risk Analyst (parallel)
Wave 5: Strategist
Wave 6: Devil's Advocate || Planner (parallel)
Wave 7: Quality Critic
Wave 8: Integration Lead
```

### Pattern 4: Hybrid Complex (8–14 agents)
```
Wave 1: Lead Architect
Wave 2: Research + Domain experts (parallel)
Wave 3: Analysis agents (parallel)
Wave 4: Content creation agents (parallel)
Wave 5: Strategy/Decision agents (parallel)
Wave 6: Quality + Editing agents (parallel)
Wave 7: Integration Lead
```

### Pattern 5: Transform Pipeline (2–5 agents)
```
Wave 1: Lead Architect
Wave 2: Processor/Extractor/Classifier (parallel when inputs are independent)
Wave 3: Format Converter || Summarizer
Wave 4: Integration Lead (if 3+ agents)
```
