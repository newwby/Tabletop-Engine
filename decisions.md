# Architectural and Design Decisions

This document tracks confirmed design decisions for the Tabletop-Engine project.

**Last Updated**: 06/04/2026  
**Source**: .reflections/ directory (current-status documents only)

---

## Project Goals

**Decision**: Build an effective and reproducible multi-agent system for supporting tabletop DMs.

### MVP Scope
* DM preparation as a build process; primer deliverables and wiki structure to support sessions
* Explicit multi-agent orchestration with iterative review
* TTRPG-specific grounding for rules/state/spoiler control
* Long-term campaign memory (evolving versioned canon through bible, world state, consistent NPCs)
* Established workflow; approved canon maintained on main & session changes pushed to branch first

---

## Architecture Decisions

### Core Architecture Pattern
**Decision**: Build as a modular monolith.

**Rationale**: Balances simplicity with clear separation of concerns without the operational overhead of distributed services.

### Orchestration Strategy
**Decision**: Use hierarchical planner-worker model with specialized subagents.

**Rationale**: Structured delegation improves control, traceability, and prevents context pollution. Avoids the ambiguity, conflicting outputs, and weak auditability of flat multi-agent systems.

**Implementation**: Utilize GitHub Copilot subagents for structured delegation.

### Workflow Pattern
**Decision**: Structure tasks as staged pipeline transformations.

**Rationale**: Build pipelines create session deliverables; supports preparation workflows.

### Event-Driven Extensions
**Decision**: Defer GitHub Actions and hooks integration for post-MVP.

**Rationale**: Not suitable for MVP complexity; revisit after core orchestration is stable.

---

## Design Principles

### Human-in-the-Loop
**Decision**: Require human checkpoints for all canonical changes.

**Rationale**: Preserves system understanding and control; prevents unreliable outputs from becoming canonical state.

### Single Responsibility
**Decision**: Each agent has one clear, bounded responsibility.

**Rationale**: Prevents overlap and ambiguity between agents.

### Separation of Concerns
**Decision**: Distinct agents handle generation, validation, and session support.

**Rationale**: Enables clear boundaries and testable components.

### Design by Contract
**Decision**: Define clear inputs, outputs, and failure modes per agent.

**Rationale**: Supports reliability and testability.

### YAGNI
**Decision**: Implement features only as needs demand them.

**Rationale**: Avoids premature complexity in autonomous systems.

### DRY
**Decision**: Develop reusable prompts, templates, and validation schemas.

**Rationale**: Reduce duplication while avoiding premature abstraction.

---

## Canonical Truth Management

### Source of Truth
**Decision**: Maintain strict canonical truth with validation gates.

**Implementation Requirements**:
- State machines to formalize lifecycle transitions (Drafted → Validated → Approved → Canonized)
- Context management to determine agent access and prevent information leakage
- Memory segmentation: durable canon, working notes, drafts, temporary session state
- Versioning to track changes and avoid destructive overwrites
- Schema design for structured entities (NPCs, locations, quests)
- Idempotency to ensure safe retries

**Rationale**: Agent failure must not cascade across workflows; source of truth must remain absolute.

### Observability
**Decision**: Implement logging, tracing, and audit trails.

**Implementation**:
- Error handling distinguishes between model/tool failure, invalid output, and canon conflicts
- Unit testing isolates tool choice, file updates, prompts
- Evaluation harnesses for comparing system iterations

**Rationale**: Required for debugging and maintaining trust in autonomous systems.

---

## Agent System Design

### Agent File Principles
**Decision**: Agent files are modular and task-specific.

**Implementation**:
- Shared project context in `.github/copilot-instructions.md`
- Agent-specific context only in individual agent files
- Agents read root `.md` files and `/docs/` for project context
- Agents operate only within defined responsibility
- Agents ask if requirements are unclear
- Agents decline if another agent is more appropriate

**Rationale**: Enables reuse in both Copilot and GitHub Actions without noise or unintended behavior.

### Agent Orchestration Mechanism
**Decision**: Use VS Code runSubagent orchestration (Orchestra pattern) over handoffs or manual agent selection.

**Rationale**: 
- Fully isolated context windows per subagent prevent context pollution across stages
- Intermediate reasoning is discarded, only final results return to orchestrator
- Automated pipeline execution with quality gates
- Best measure for achieving transform pipeline with clearly defined roles
- Aligns with project goal to determine effective and reproducible multi-agent system

**Trade-offs**: 
- Higher setup complexity compared to handoffs
- Context isolation means detailed information from previous steps must be explicitly passed
- Requires conductor agent plus narrow task agents

**Alternatives Considered**:
- **Handoffs**: Human-paced, context-accumulating, simpler setup but accumulates context pollution. Better for 2-3 stage flows with deliberate human checkpoints.
- **Singular Specialist Agents**: Manual selection per task. Simplest to use, best for well-scoped one-shot tasks but no orchestration benefits.

#### Orchestrator implementation
**Decision**: One orchestrator agent invoked directly by humans.

**Responsibilities**:
1. Classify request and decompose into subtasks
2. Select specialist agents or MCP tools and invoke workers
3. Merge outputs and validate against project rules

**Note**: Does not produce content directly; manages delegation and integration only.

#### Worker agent implementation
**Decision**: Narrow specialist agents with clear bounded authority, non-overlapping mandates.

**Defined Agents**:
- **Continuity Agent**: Checks canon, world state, unresolved threads, entity consistency
- **Prep Agent**: Produces session deliverables
- **Encounter Agent**: Creates arcs/quests, encounters, locations, events
- **Lore Agent**: Creates world geography & history, cultures, factions, NPCs
- **Documentation Agent**: Reviews reflection/campaign docs, readme, devlogs, audit logs
- **Historian Agent**: Maintains change history record of campaign updates

#### Initial layer rollout
**Decision**: Introduce agents in controlled layers instead of enabling all proposed roles at once.

**Introduction Order**:
1. Orchestrator + Continuity Agent + Historian Agent
2. Prep/Session Compiler Agent
3. Encounter Agent + Lore Agent
4. Documentation Agent hardening passes between each rollout stage

**Rationale**:
- Reduces startup complexity and orchestration noise
- Establishes control, validation, and logging before content scale-out
- Keeps MVP focused on reliable preparation outputs

#### Anti-overlap enforcement
**Decision**: Add explicit anti-overlap controls for agent responsibilities.

**Implementation Requirements**:
- Single-owner task matrix per agent capability area
- Decline-and-route behavior for out-of-scope requests
- Structured handoff contracts (inputs/outputs/acceptance criteria)
- Orchestrator conflict gate for overlapping or conflicting outputs

**Rationale**: Prevents duplicate authority, contradictory outputs, and deadlocks.

#### Scope creep and overengineering controls
**Decision**: Enforce explicit controls before introducing new agents or workflow layers.

**Implementation Requirements**:
- New agent proposals require "why now" decision entry tied to MVP goals
- One-layer-at-a-time rollout with validation gate between layers
- Prefer prompt/contract refinement before adding new agents
- Defer non-MVP complexity unless justified by validated MVP gaps

**Rationale**: Preserves delivery focus and avoids premature system complexity.

#### IP conflict avoidance in agent responsibilities
**Decision**: Agent responsibilities must include explicit IP conflict safeguards.

**Implementation Requirements**:
- Do not reproduce third-party copyrighted/trademarked setting content unless authorized by user input or clear licensing
- Prefer original material or clearly licensed/open references
- Escalate uncertain provenance to human review before persistence
- Capture provenance notes when external material influences outputs

**Rationale**: Reduces legal/compliance risk and protects repository integrity.

#### Progress accounting and decision logging expectations
**Decision**: Require progress-accounting and decision logging outputs in orchestration cycles.

**Implementation Requirements**:
- Historian records approved updates and progress deltas for future accounting
- Exploratory work must end with: decision, deferred decision with owner/date, or explicit blocker
- Avoid repeated exploration loops without logged outcome
- Summarize outcomes in root-level decision tracking artifacts

**Rationale**: Improves auditability, prevents endless exploration, and supports measurable project progress.

#### MCP Tooling
**Decision**: Utilize MCP servers for campaign data, rule references, storage, retrieval, and generation.

**Rationale**: Enables all agents to access same capabilities without duplicating rules expertise.


---

## MVP Deliverables

**Decision**: Deliver the following for MVP:

* Narrow task agents managed through orchestration layer with defined constraints and roles
* Formal branching policy (enabling rollback/versioning), enforced by repo instructions and PR workflows
* Global agent instructions, agent usage guidance, and directory-specific agent instructions
* Atomic files to support wiki, fast lookup, and reduced context overload
* Atomic schemas per file type (NPC/quest/enemy/encounter/location)
* Reusable prompts for content generation
* Structured validation schemas for agents
* Issue/PR templates and GitHub quickstart guide
* Session primer generation (TL;DR, scene flow, relevant file links, fallback logic)
* Session log output schema and standardized post-session intake schema
* Observability of campaign changes (historical record)
* Devlog write-ups from reflection notes

---

## Scope Boundaries

### Out of Scope for MVP
**Decision**: The following are explicitly deferred:

* Live session workflow reliability (latency, graceful degradation)
* Scalability optimizations (canon drift, wiki expansion at scale)
* Developer experience tooling beyond basics
* Advanced governance and control mechanisms

### Post-MVP Considerations
**Decision**: Evaluate after MVP validation:

* Campaign IDE with deep document state integration
* Deterministic orchestration for live sessions
* Multimodal asset pipeline (maps, tokens, audio)
* Evolving NPC agents with persistent memory
* Narrative layer standardization across rulesets

---

## Known Issues from Existing Work

### Multi-Agent GM System Risks
**Decision**: Address three identified failure modes from existing multi-agent GM research:

1. **Misunderstanding rules**: Mitigated by MCP tools for rules-grounding
2. **Deviating outside existing story/world**: Mitigated by canonical truth validation gates and Continuity Agent
3. **Giving spoilers to players**: Mitigated by context management and information access controls

**Source**: TRPG Game Mastering Using LLM-Based Multi-Agent System (Minari, Ueno, Lee)

**Rationale**: These are documented failure modes in similar systems; explicit mitigation strategies prevent replication of known issues.

### Latency Concerns
**Decision**: Deprioritize live session automation for MVP.

**Rationale**: Research shows multi-agent review systems can introduce latency of >1 minute per interaction, unrealistic for live TTRPG play. Focus MVP on preparation deliverables where latency is acceptable.

---

## Document Status

**Source Files** (marked "Current" as of 06/04/2026):
- `.reflections/implementation/design_goals.md`
- `.reflections/implementation/engineering_principles.md`
- `.reflections/implementation/agent_principles.md`
- `.reflections/background/orchestration_2.md`
- `.reflections/reviews/existing_work.md`
- `.reflections/reviews/agentic_merits.md`

Files marked "Historic" were excluded per project constraints.
