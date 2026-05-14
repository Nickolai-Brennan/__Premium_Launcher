# AI Orchestration Framework

## Purpose
Define how to choose between Prompt, Skill, Agent, and Information layers based on task structure, reuse, autonomy, and operational risk.

---

## Core Principle

> Information grounds  
> Skills execute  
> Agents orchestrate  
> Prompts explore

---

## Execution Layers

### 1. Prompt — Ad Hoc Intelligence Layer
Use when:
- task is exploratory, creative, or one-off
- output does not require deterministic reuse
- human remains in control
- no persistent workflow/state is needed

Examples:
- write blog copy
- brainstorm features
- draft schema ideas
- suggest UI improvements

Characteristics:
- stateless
- flexible
- human-directed
- low setup cost

Rule:
> Use a Prompt when the task is open-ended and not worth operationalizing yet.

---

### 2. Skill — Reusable Capability Layer
Use when:
- inputs/outputs are clear
- logic is repeatable
- execution should be fast and consistent
- capability will be reused across tasks or systems

Examples:
- markdown to HTML
- normalize weather feed
- compute STORM score
- convert betting odds
- insert row with UUID

Characteristics:
- deterministic
- modular
- testable
- reusable

Rule:
> Use a Skill when the task can be defined as a repeatable function.

---

### 3. Agent — Autonomous Workflow Layer
Use when:
- the task has multiple dependent steps
- decisions or branching are required
- tool/API selection may vary
- retries, planning, or state tracking are needed
- partial autonomy is acceptable

Examples:
- ingest -> clean -> validate -> store pipeline
- arbitrage monitor scan -> compare -> alert
- task orchestrator expand -> assign -> log -> report
- weather adjustment pipeline with replanning on missing data

Characteristics:
- stateful
- tool-using
- multi-step
- semi-autonomous

Rule:
> Use an Agent when the system must plan, choose, adapt, or recover across steps.

---

### 4. Information — Knowledge and Context Layer
Use when:
- execution depends on facts, records, logs, docs, or state
- grounding is required before reasoning or action
- historical or real-time context changes outcomes

Subtypes:
- Reference Information: docs, specs, policies
- Operational Information: DB data, APIs, telemetry, logs
- Memory/State Information: previous runs, user settings, workflow state
- Discovery Information: search, vector retrieval, embeddings

Examples:
- fetch player history before projections
- load course metadata before simulation
- retrieve prior bets before recommendations
- read system docs before code generation

Characteristics:
- read-heavy
- grounding layer
- truth source
- context provider

Rule:
> If the task depends on data or context, retrieve Information first.

---

## Routing Decision Tree

For every task, answer in order:

1. Is external context or system state required?
   - Yes -> Information first

2. Is there already a reusable function/module/tool for this?
   - Yes -> use Skill

3. Is the workflow fixed and deterministic?
   - Yes -> chain Skills
   - No -> continue

4. Does the task require planning, branching, retries, or state across steps?
   - Yes -> Agent

5. Is the task primarily exploratory, creative, or human-guided?
   - Yes -> Prompt

6. Is the action high-risk, costly, sensitive, or irreversible?
   - Yes -> require approval gate before execution

---

## Standard Execution Order

1. Clarify objective
2. Retrieve Information if needed
3. Reuse existing Skill if available
4. Decide whether Skill chaining is enough
5. Escalate to Agent only if orchestration is required
6. Use Prompt for ad hoc generation or interpretation
7. Log the routing decision and result

---

## Combined Usage Patterns

### Pattern A — Simple Creative Task
Prompt -> Output

Example:
- article draft
- UI idea
- headline variants

### Pattern B — Grounded Calculation
Information -> Skill -> Output

Example:
- query weather data -> compute adjustment -> return score

### Pattern C — Deterministic Pipeline
Information -> Skill -> Skill -> Storage

Example:
- fetch feed -> normalize -> validate -> save

### Pattern D — Adaptive Workflow
Information -> Agent -> Skills -> Storage -> Report

Example:
- ingest changing feeds, retry on failure, choose fallback source, log result

### Pattern E — Full Product Loop
Information <-> Agent <-> Skills <-> Database <-> Prompt/UI

Example:
- user asks question -> system retrieves data -> agent coordinates logic -> skills compute -> prompt formats response

---

## Escalation Rules

- Prompt used 3+ times for same task -> convert to Skill
- Skill chain with branching/retries/state -> wrap in Agent
- Agent repeatedly solving same workflow -> formalize as service/microservice
- Missing context causing poor output -> strengthen Information layer
- Human approval required repeatedly at same step -> formalize approval checkpoint

---

## Agent vs Skill Boundary

Use Skill chain when:
- sequence is fixed
- no dynamic planning is required
- no memory/state is needed
- failure handling is simple

Use Agent when:
- sequence can change
- tool choice is dynamic
- retries/replanning are needed
- state must persist across steps
- completion depends on adaptation

---

## Operational Governance

For every task, also classify:

- Risk: low / medium / high
- Cost sensitivity: low / medium / high
- Latency sensitivity: low / medium / high
- Data sensitivity: public / internal / restricted
- Approval required: yes / no
- Observability level: basic / standard / full

Rules:
- High-risk actions require approval
- High-cost tasks require budget-aware routing
- Sensitive data requires restricted tool access
- Critical workflows require logs, traces, and auditability

---

## Failure and Fallback Rules

If Information is unavailable:
- use fallback source
- or return blocked status with missing dependency

If Skill fails:
- retry if safe
- otherwise escalate to Agent or human review

If Agent fails:
- return current state, failed step, and recommended recovery action

If Prompt output is low quality:
- do not keep re-prompting indefinitely
- convert repeated structure into a Skill template

---

## Output Contract

For every routed task, return:

- Selected Layer
- Why it was selected
- Information required
- Skills/tools required
- Approval requirement
- Expected output format
- Fallback path
- Suggested next evolution

---

## Example Decision

Task: Generate PGA weather-adjusted projections

Decision:
- Needs tournament history, player stats, and weather -> Information
- Needs weather adjustment formula -> Skill
- Needs sequencing across retrieval, calculation, and persistence -> Agent

Result:
> Use an Agent orchestrating Information + Skills

---

## Minimal Decision Template

Selected Layer:
Reason:
Information Needed:
Skill(s) Needed:
Autonomy Level:
Approval Required:
Output Format:
Fallback:
Next Evolution:

---

## System Principle

> Prompts explore  
> Information grounds  
> Skills execute  
> Agents orchestrate
