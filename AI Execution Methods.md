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

## Execution Extensions

### 5. Instructions — Policy and Constraint Layer
Use when:
- the system needs stable behavioral guidance
- outputs must follow business rules, tone, policy, or compliance requirements
- task routing should be constrained before execution begins
- reusable operating guidance is needed across prompts, skills, agents, or workflows

Examples:
- response style guide for customer support
- approval rules for financial actions
- safety rules for tool usage
- domain-specific formatting requirements

Characteristics:
- declarative
- reusable
- governance-oriented
- cross-cutting

Rule:
> Use Instructions to constrain how execution happens, not to perform the execution itself.

---

### 6. Plugins / Tools — External Action Layer
Use when:
- the system must call external services, APIs, databases, filesystems, or SaaS products
- execution requires capabilities the model does not natively possess
- real-world side effects or fresh data retrieval are required

Examples:
- query weather API
- write to Supabase
- send Slack alert
- create GitHub issue
- read from vector store

Characteristics:
- capability-extending
- side-effect-capable
- permission-scoped
- observable

Rule:
> Use Plugins/Tools when execution needs outside-world access or system actions.

---

### 7. Hooks — Event Interception Layer
Use when:
- logic must run before or after a step
- validation, enrichment, logging, guardrails, or cleanup should trigger automatically
- cross-cutting lifecycle behaviors are needed around agents, skills, workflows, or tool calls

Examples:
- pre-execution policy check
- post-run audit logger
- before-save schema validation
- on-failure notifier
- after-response memory write

Characteristics:
- event-driven
- lifecycle-bound
- composable
- lightweight orchestration

Rule:
> Use Hooks for automatic lifecycle logic attached to events, not for primary task ownership.

---

### 8. Workflows — Structured Process Layer
Use when:
- a recurring process has a known sequence of steps
- multiple tasks, approvals, tools, skills, and agents must be coordinated consistently
- observability, retries, handoffs, and status tracking are required at the process level
- execution spans time, systems, or teams

Examples:
- content publish workflow
- lead intake -> enrich -> score -> assign process
- ETL with approval checkpoint
- bug triage and release readiness flow

Characteristics:
- process-oriented
- stateful
- monitorable
- repeatable

Rule:
> Use a Workflow when the business process itself needs to be modeled, tracked, and repeated reliably.

---

## Routing Decision Tree

For every task, answer in order:

1. Is external context or system state required?
   - Yes -> Information first

2. Are there policy, formatting, compliance, or behavioral constraints?
   - Yes -> load Instructions

3. Is there already a reusable function/module/tool for this?
   - Yes -> use Skill
   - If outside-world access is required -> attach Plugin/Tool

4. Is the workflow fixed and deterministic?
   - Yes -> chain Skills or Workflow steps
   - No -> continue

5. Does the task require planning, branching, retries, or state across steps?
   - Yes -> Agent

6. Does execution need automatic pre/post validation, logging, or guardrails?
   - Yes -> add Hooks

7. Is the task primarily exploratory, creative, or human-guided?
   - Yes -> Prompt

8. Is the action high-risk, costly, sensitive, or irreversible?
   - Yes -> require approval gate before execution

---

## Standard Execution Order

1. Clarify objective
2. Retrieve Information if needed
3. Apply Instructions and constraints
4. Reuse existing Skill if available
5. Attach required Plugins/Tools
6. Decide whether Skill chaining or Workflow structure is enough
7. Escalate to Agent only if orchestration/adaptation is required
8. Add Hooks for validation, logging, and recovery boundaries
9. Use Prompt for ad hoc generation or interpretation
10. Log the routing decision and result

---

## Combined Usage Patterns

### Pattern A — Simple Creative Task
Instructions -> Prompt -> Output

Example:
- article draft with brand voice
- UI idea with formatting rules
- headline variants with tone constraints

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

### Pattern F — Governed Automation
Instructions -> Information -> Workflow -> Skills/Plugins -> Hooks -> Output

Example:
- intake request -> validate permissions -> fetch records -> run approved actions -> log audit trail

### Pattern G — Tool-Augmented Agent
Instructions -> Information -> Agent -> Plugins/Tools -> Hooks -> Report

Example:
- analyze support issue -> inspect logs -> open incident -> notify Slack -> write run summary

---

## Escalation Rules

- Prompt used 3+ times for same task -> convert to Skill
- Repeated prompt needing stable tone/policy -> extract Instructions
- Skill chain with branching/retries/state -> wrap in Agent
- Repeated fixed multi-step process -> formalize as Workflow
- Agent repeatedly solving same workflow -> formalize as service/microservice
- Missing context causing poor output -> strengthen Information layer
- Frequent lifecycle validation/logging needs -> add Hooks
- Repeated external system calls -> formalize Plugin/Tool contracts
- Human approval required repeatedly at same step -> formalize approval checkpoint

---

## Agent vs Skill vs Workflow Boundary

Use Skill chain when:
- sequence is fixed
- no dynamic planning is required
- no memory/state is needed
- failure handling is simple

Use Workflow when:
- sequence is fixed but operationally important
- approvals, retries, SLAs, or handoffs must be tracked
- observability is needed across the full process
- multiple actors/systems participate

Use Agent when:
- sequence can change
- tool choice is dynamic
- retries/replanning are needed
- state must persist across steps
- completion depends on adaptation

---

## Hooks Design Rules

Pre-hooks may:
- validate inputs
- enforce permissions
- enrich context
- block unsafe execution

Post-hooks may:
- log outputs
- persist memory/state
- trigger notifications
- emit metrics/traces
- launch cleanup or follow-up tasks

Constraints:
- hooks should be idempotent when possible
- hooks should not silently change critical outputs without logging
- hooks must fail closed on security checks
- long-running hooks should be offloaded asynchronously when safe

---

## Plugin / Tool Governance

For each plugin/tool, define:
- purpose
- required permissions
- input/output schema
- retry policy
- timeout
- side effects
- audit/logging requirements
- fallback behavior

Rules:
- write-capable tools require stronger approval than read-only tools
- tools touching money, credentials, or production systems require explicit policy gates
- tool outputs should be validated before downstream execution
- all side effects should be attributable and logged

---

## Workflow Design Rules

Every workflow should define:
- trigger
- ordered steps
- decision points
- rollback or compensation path
- approval gates
- ownership/handoffs
- state transitions
- observability checkpoints
- success/failure terminal states

Rule:
> A Workflow is the operational container; Skills, Agents, Hooks, and Plugins execute inside it.

---

## Operational Governance

For every task, also classify:

- Risk: low / medium / high
- Cost sensitivity: low / medium / high
- Latency sensitivity: low / medium / high
- Data sensitivity: public / internal / restricted
- Approval required: yes / no
- Observability level: basic / standard / full
- Tool access level: none / read / write / privileged
- Workflow criticality: optional / business-important / mission-critical

Rules:
- High-risk actions require approval
- High-cost tasks require budget-aware routing
- Sensitive data requires restricted tool access
- Critical workflows require logs, traces, and auditability
- Privileged tool usage must be explicitly scoped and reviewable

---

## Failure and Fallback Rules

If Information is unavailable:
- use fallback source
- or return blocked status with missing dependency

If Instructions are unclear or conflicting:
- prefer the higher-priority policy
- otherwise pause for clarification

If Skill fails:
- retry if safe
- otherwise escalate to Agent or human review

If Plugin/Tool fails:
- retry per policy
- switch to fallback provider if available
- return side-effect status explicitly

If Hook fails:
- block execution for security/compliance pre-hooks
- log and continue only for non-critical post-hooks if policy allows

If Workflow fails:
- return failed step, current state, rollback status, and next action

If Agent fails:
- return current state, failed step, and recommended recovery action

If Prompt output is low quality:
- do not keep re-prompting indefinitely
- convert repeated structure into a Skill template

---

## Output Contract

For every routed task, return:

- Selected Layer(s)
- Why each was selected
- Instructions required
- Information required
- Skills/tools/plugins required
- Hooks required
- Workflow required or not
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
- Needs API fetches and database writes -> Plugins/Tools
- Needs audit logging after run -> Hooks

Result:
> Use an Agent orchestrating Information + Skills + Plugins/Tools with post-run Hooks

---

## Minimal Decision Template

Selected Layer(s):
Reason:
Instructions Needed:
Information Needed:
Skill(s) Needed:
Plugin(s)/Tool(s) Needed:
Hook(s) Needed:
Workflow Needed:
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
> Workflows structure  
> Hooks guard  
> Plugins act  
> Instructions constrain
