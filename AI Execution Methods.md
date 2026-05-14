# AI Orchestration and Automation Framework

## Purpose
Define how to choose and combine Prompts, Instructions, Information, Skills, Tools/Plugins, Hooks, Agents, Workflows, and Automations based on task structure, reuse, autonomy, governance, and operational risk.

---

## Core Principle

> Information grounds  
> Instructions constrain  
> Skills execute  
> Tools act  
> Hooks guard  
> Agents orchestrate  
> Workflows structure  
> Automations scale  
> Prompts explore

---

## System Layers

### 1. Prompt — Ad Hoc Intelligence Layer
Use when:
- task is exploratory, creative, ambiguous, or one-off
- output does not require deterministic reuse
- human remains in control of review and execution
- no persistent workflow/state is needed
- the goal is ideation, framing, transformation, or interpretation

Examples:
- write blog copy
- brainstorm features
- draft schema ideas
- suggest UI improvements
- summarize notes into themes
- generate alternative names or messaging

Characteristics:
- stateless
- flexible
- human-directed
- low setup cost
- low operational overhead

Rule:
> Use a Prompt when the task is open-ended and not worth operationalizing yet.

---

### 2. Instructions — Policy and Constraint Layer
Use when:
- the system needs stable behavioral guidance
- outputs must follow business rules, tone, policy, or compliance requirements
- task routing should be constrained before execution begins
- reusable operating guidance is needed across prompts, skills, agents, automations, or workflows
- multiple components must behave consistently

Examples:
- response style guide for customer support
- approval rules for financial actions
- safety rules for tool usage
- domain-specific formatting requirements
- naming conventions for generated assets
- escalation rules for production incidents

Characteristics:
- declarative
- reusable
- governance-oriented
- cross-cutting
- policy-bearing

Rule:
> Use Instructions to constrain how execution happens, not to perform the execution itself.

---

### 3. Information — Knowledge and Context Layer
Use when:
- execution depends on facts, records, logs, docs, or state
- grounding is required before reasoning or action
- historical or real-time context changes outcomes
- model output quality depends on retrieval fidelity
- prior runs or user memory change the correct answer

Subtypes:
- Reference Information: docs, specs, policies
- Operational Information: DB data, APIs, telemetry, logs
- Memory/State Information: previous runs, user settings, workflow state
- Discovery Information: search, vector retrieval, embeddings
- Real-Time Information: streaming feeds, current metrics, events

Examples:
- fetch player history before projections
- load course metadata before simulation
- retrieve prior bets before recommendations
- read system docs before code generation
- check CRM state before sending outreach
- load unresolved incidents before triage

Characteristics:
- read-heavy
- grounding layer
- truth source
- context provider
- freshness-sensitive

Rule:
> If the task depends on data or context, retrieve Information first.

---

### 4. Skill — Reusable Capability Layer
Use when:
- inputs/outputs are clear
- logic is repeatable
- execution should be fast and consistent
- capability will be reused across tasks or systems
- behavior can be tested as a bounded function

Examples:
- markdown to HTML
- normalize weather feed
- compute STORM score
- convert betting odds
- insert row with UUID
- classify lead quality
- validate payload schema

Characteristics:
- deterministic
- modular
- testable
- reusable
- composable

Rule:
> Use a Skill when the task can be defined as a repeatable function.

---

### 5. Plugins / Tools — External Action Layer
Use when:
- the system must call external services, APIs, databases, filesystems, or SaaS products
- execution requires capabilities the model does not natively possess
- real-world side effects or fresh data retrieval are required
- a skill or agent must operate beyond the model boundary

Examples:
- query weather API
- write to Supabase
- send Slack alert
- create GitHub issue
- read from vector store
- trigger webhook
- update CRM record

Characteristics:
- capability-extending
- side-effect-capable
- permission-scoped
- observable
- dependency-bound

Rule:
> Use Plugins/Tools when execution needs outside-world access or system actions.

---

### 6. Hooks — Event Interception Layer
Use when:
- logic must run before or after a step
- validation, enrichment, logging, guardrails, or cleanup should trigger automatically
- cross-cutting lifecycle behaviors are needed around prompts, skills, workflows, automations, or tool calls
- the execution path should be augmented without changing primary ownership

Examples:
- pre-execution policy check
- post-run audit logger
- before-save schema validation
- on-failure notifier
- after-response memory write
- before-tool permission gate

Characteristics:
- event-driven
- lifecycle-bound
- composable
- lightweight orchestration
- cross-cutting

Rule:
> Use Hooks for automatic lifecycle logic attached to events, not for primary task ownership.

---

### 7. Agent — Autonomous Decision Layer
Use when:
- the task has multiple dependent steps
- decisions or branching are required
- tool/API selection may vary
- retries, planning, or state tracking are needed
- partial or substantial autonomy is acceptable
- the system must adapt based on outcomes

Examples:
- ingest -> clean -> validate -> store pipeline with fallback decisions
- arbitrage monitor scan -> compare -> alert
- task orchestrator expand -> assign -> log -> report
- weather adjustment pipeline with replanning on missing data
- support triage agent that inspects logs and recommends action

Characteristics:
- stateful
- tool-using
- multi-step
- semi-autonomous or autonomous
- adaptive

Rule:
> Use an Agent when the system must plan, choose, adapt, or recover across steps.

---

### 8. Workflow — Structured Process Layer
Use when:
- a recurring process has a known sequence of steps
- multiple tasks, approvals, tools, skills, and agents must be coordinated consistently
- observability, retries, handoffs, and status tracking are required at the process level
- execution spans time, systems, or teams
- business operations require reliable state transitions

Examples:
- content publish workflow
- lead intake -> enrich -> score -> assign process
- ETL with approval checkpoint
- bug triage and release readiness flow
- onboarding request process with approvals and provisioning

Characteristics:
- process-oriented
- stateful
- monitorable
- repeatable
- governance-friendly

Rule:
> Use a Workflow when the business process itself needs to be modeled, tracked, and repeated reliably.

---

### 9. Automation — Operationalized System Layer
Use when:
- a workflow, agent, or skill chain should run automatically on triggers or schedules
- the system should operate continuously with minimal human intervention
- recurring operational work should be delegated to infrastructure
- execution must happen at scale across many events, records, or users
- business value comes from reliable unattended execution

Examples:
- nightly data enrichment automation
- inbound lead routing automation
- schedule-based content generation and publishing
- continuous monitoring and alert automation
- ticket intake classification and assignment automation
- autonomous reporting pipeline

Characteristics:
- trigger-based or scheduled
- persistent operational ownership
- scalable
- monitorable
- reliability-sensitive

Rule:
> Use Automation when the system should run repeatedly without manual initiation.

---

## Automation Types

### Event-Driven Automation
Use when:
- execution should start from a system event
- responsiveness matters
- actions depend on incoming state changes

Examples:
- new form submission triggers enrichment
- payment failure triggers recovery workflow
- repository issue opened triggers classification

### Scheduled Automation
Use when:
- tasks should run on fixed intervals
- freshness and cadence matter more than immediate reaction

Examples:
- hourly sync job
- nightly report generation
- weekly cleanup and archival pass

### Human-in-the-Loop Automation
Use when:
- automation is desired but approval or review is required at specific checkpoints
- decisions are high-risk, costly, or regulated

Examples:
- draft outbound campaign then require approval
- suggest remediation steps then wait for operator confirmation
- prepare code change plan before deployment approval

### Autonomous Automation
Use when:
- the domain is controlled enough for unattended execution
- rules, skills, fallbacks, and observability are mature

Examples:
- auto-tag and route support tickets
- auto-reconcile records with confidence thresholds
- auto-scale routine operational responses

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

4. Must logic run automatically before/after a step?
   - Yes -> add Hooks

5. Is the workflow fixed and deterministic?
   - Yes -> chain Skills or Workflow steps
   - No -> continue

6. Does the task require planning, branching, retries, or state across steps?
   - Yes -> Agent

7. Should the process run on triggers, schedules, or unattended operations?
   - Yes -> Automation

8. Is the task primarily exploratory, creative, or human-guided?
   - Yes -> Prompt

9. Is the action high-risk, costly, sensitive, or irreversible?
   - Yes -> require approval gate before execution

---

## Standard Execution Order

1. Clarify objective
2. Classify risk, cost, latency, and sensitivity
3. Retrieve Information if needed
4. Apply Instructions and constraints
5. Reuse existing Skill if available
6. Attach required Plugins/Tools
7. Add Hooks for validation, logging, and policy enforcement
8. Decide whether Skill chaining, Workflow structure, or Agent orchestration is needed
9. Decide whether Automation should trigger or schedule the process
10. Use Prompt for ad hoc generation, interpretation, or creative output
11. Log the routing decision, result, and execution trace

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

### Pattern D — Adaptive Agent Run
Information -> Agent -> Skills -> Tools -> Report

Example:
- ingest changing feeds, retry on failure, choose fallback source, log result

### Pattern E — Governed Workflow
Instructions -> Information -> Workflow -> Skills/Tools -> Hooks -> Output

Example:
- intake request -> validate permissions -> fetch records -> run approved actions -> log audit trail

### Pattern F — Tool-Augmented Agent
Instructions -> Information -> Agent -> Plugins/Tools -> Hooks -> Report

Example:
- analyze support issue -> inspect logs -> open incident -> notify Slack -> write run summary

### Pattern G — Fully Automated Operation
Trigger/Schedule -> Instructions -> Information -> Workflow/Agent -> Skills -> Tools -> Hooks -> Storage/Report

Example:
- new lead arrives -> enrich -> score -> assign -> notify -> log metrics automatically

### Pattern H — Human-in-the-Loop Automation
Trigger -> Instructions -> Information -> Agent/Workflow -> Approval Gate -> Tools -> Hooks -> Output

Example:
- draft contract summary -> request approval -> send to customer -> store audit record

---

## Escalation Rules

- Prompt used 3+ times for same task -> convert to Skill
- Repeated prompt needing stable tone/policy -> extract Instructions
- Skill requiring external side effects -> formalize Tool contract
- Skill chain with branching/retries/state -> wrap in Agent
- Repeated fixed multi-step process -> formalize as Workflow
- Workflow repeatedly triggered manually -> convert to Automation
- Agent repeatedly solving same workflow -> formalize as service/microservice or automation runtime
- Missing context causing poor output -> strengthen Information layer
- Frequent lifecycle validation/logging needs -> add Hooks
- Repeated external system calls -> formalize Plugin/Tool contracts
- Human approval required repeatedly at same step -> formalize approval checkpoint
- Autonomous run failures at scale -> reduce autonomy and reintroduce human-in-the-loop controls

---

## Prompt vs Skill vs Agent vs Workflow vs Automation Boundary

Use Prompt when:
- the problem is exploratory
- output can vary widely
- human judgment is central
- reuse is low

Use Skill when:
- logic is bounded and repeatable
- sequence is simple
- determinism matters
- failure handling is simple

Use Agent when:
- sequence can change
- tool choice is dynamic
- retries/replanning are needed
- state must persist across steps
- completion depends on adaptation

Use Workflow when:
- sequence is fixed but operationally important
- approvals, retries, SLAs, or handoffs must be tracked
- observability is needed across the full process
- multiple actors/systems participate

Use Automation when:
- the process should run without manual initiation
- triggering/scheduling is part of system design
- scale, reliability, and unattended execution matter
- operations need continuous execution rather than one-off runs

---

## Hooks Design Rules

Pre-hooks may:
- validate inputs
- enforce permissions
- enrich context
- block unsafe execution
- check rate limits and quotas

Post-hooks may:
- log outputs
- persist memory/state
- trigger notifications
- emit metrics/traces
- launch cleanup or follow-up tasks
- record audit entries

Constraints:
- hooks should be idempotent when possible
- hooks should not silently change critical outputs without logging
- hooks must fail closed on security checks
- long-running hooks should be offloaded asynchronously when safe
- hook side effects should be observable

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
- ownership
- data handling rules

Rules:
- write-capable tools require stronger approval than read-only tools
- tools touching money, credentials, or production systems require explicit policy gates
- tool outputs should be validated before downstream execution
- all side effects should be attributable and logged
- privileged tools should be narrowly scoped and reviewable

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
- timeout and retry boundaries

Rule:
> A Workflow is the operational container; Skills, Agents, Hooks, and Plugins execute inside it.

---

## Automation Design Rules

Every automation should define:
- trigger or schedule
- scope of autonomy
- workflow or agent used
- involved skills and tools
- approval checkpoints
- failure handling policy
- alerting and observability rules
- stop conditions
- escalation path
- throughput/concurrency limits
- rollback or suppression behavior

Rules:
- automations should have explicit ownership
- all automations should be observable
- risky automations should default to human-in-the-loop
- autonomous write actions require stronger controls than read-only automations
- schedules, triggers, and side effects must be documented
- production automations need kill switches or pause controls

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
- Autonomy level: manual / assisted / semi-autonomous / autonomous
- Recovery mode: retry / fallback / human review / rollback

Rules:
- High-risk actions require approval
- High-cost tasks require budget-aware routing
- Sensitive data requires restricted tool access
- Critical workflows require logs, traces, and auditability
- Privileged tool usage must be explicitly scoped and reviewable
- Higher autonomy requires stronger observability and tighter policy controls

---

## Failure and Fallback Rules

If Information is unavailable:
- use fallback source
- use cached state if policy allows
- or return blocked status with missing dependency

If Instructions are unclear or conflicting:
- prefer the higher-priority policy
- otherwise pause for clarification

If Skill fails:
- retry if safe
- otherwise escalate to Agent, Workflow exception path, or human review

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

If Automation fails:
- halt or degrade according to policy
- emit alert
- preserve execution state
- route to human/operator if threshold exceeded

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
- Skills required
- Plugins/Tools required
- Hooks required
- Agent required or not
- Workflow required or not
- Automation required or not
- Autonomy level
- Approval requirement
- Expected output format
- Fallback path
- Suggested next evolution

---

## Example Decision

Task: Generate PGA weather-adjusted projections automatically every morning

Decision:
- Needs tournament history, player stats, and weather -> Information
- Needs weather adjustment formula -> Skill
- Needs sequencing across retrieval, calculation, validation, and persistence -> Agent or Workflow depending on fixedness
- Needs API fetches and database writes -> Plugins/Tools
- Needs audit logging after run -> Hooks
- Needs daily unattended execution -> Automation

Result:
> Use an Automation that triggers a Workflow or Agent orchestrating Information + Skills + Plugins/Tools with post-run Hooks and approval controls if outputs affect money or production actions.

---

## Minimal Decision Template

Selected Layer(s):
Reason:
Instructions Needed:
Information Needed:
Skill(s) Needed:
Plugin(s)/Tool(s) Needed:
Hook(s) Needed:
Agent Needed:
Workflow Needed:
Automation Needed:
Autonomy Level:
Approval Required:
Output Format:
Fallback:
Next Evolution:

---

## System Principle

> Prompts explore  
> Information grounds  
> Instructions constrain  
> Skills execute  
> Tools act  
> Hooks guard  
> Agents orchestrate  
> Workflows structure  
> Automations scale
