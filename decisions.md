# Architectural and Design Decisions

This document tracks confirmed design decisions for the Tabletop-Engine project.

**Last Updated**: 17/04/2026  
**Source**: .reflections/ directory (current-status documents only)

## Table of Contents

- [Project Goals](#project-goals)
  - [MVP Scope](#mvp-scope)
- [Architecture Decisions](#architecture-decisions)
  - [Core Architecture Pattern](#core-architecture-pattern)
  - [Orchestration Strategy](#orchestration-strategy)
  - [Workflow Pattern](#workflow-pattern)
  - [Event-Driven Extensions](#event-driven-extensions)
  - [Post-Session Intake Workflow](#post-session-intake-workflow)
- [Design Principles](#design-principles)
  - [Human-in-the-Loop](#human-in-the-loop)
  - [Single Responsibility](#single-responsibility)
  - [Separation of Concerns](#separation-of-concerns)
  - [Design by Contract](#design-by-contract)
  - [YAGNI](#yagni)
  - [DRY](#dry)
- [Canonical Truth Management](#canonical-truth-management)
  - [Source of Truth](#source-of-truth)
  - [Observability](#observability)
- [Agent System Design](#agent-system-design)
  - [Agent File Principles](#agent-file-principles)
  - [Agent Orchestration Mechanism](#agent-orchestration-mechanism)
  - [Orchestrator implementation](#orchestrator-implementation)
  - [Worker agent implementation](#worker-agent-implementation)
  - [MCP Tooling](#mcp-tooling)
- [MVP Deliverables](#mvp-deliverables)
- [Scope Boundaries](#scope-boundaries)
  - [Out of Scope for MVP](#out-of-scope-for-mvp)
  - [Post-MVP Considerations](#post-mvp-considerations)
- [Known Issues from Existing Work](#known-issues-from-existing-work)
  - [Multi-Agent GM System Risks](#multi-agent-gm-system-risks)
  - [Latency Concerns](#latency-concerns)
- [Document Status](#document-status)

---

## Project Goals

**Decision**: Build an effective and reproducible multi-agent system for supporting tabletop DMs.

### MVP Scope
* DM preparation as a build process; primer deliverables and wiki structure to support sessions
* Explicit multi-agent orchestration with iterative review
* TTRPG-specific grounding for rules/state/spoiler control
* Long-term campaign memory (evolving versioned canon through bible, world state, consistent NPCs)
* Established workflow; approved canon maintained on main & session changes pushed to branch first
* Session deliverables: structured output artefacts generated per session (summary, notable events, state changes, unresolved questions)
* Post-session intake workflow: GitHub Issue Forms as the standard intake surface, with automated branch and pull request scaffolding for review

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
**Decision**: Defer broad GitHub Actions and hooks integration for post-MVP, with the exception of the post-session intake routing workflow.

**Rationale**: Full event-driven automation is not suitable for MVP complexity and should be revisited after core orchestration is stable. However, a targeted GitHub Actions workflow for detecting post-session intake issues and scaffolding session branches and pull requests is within MVP scope, as it is the primary intake routing mechanism.

### Post-Session Intake Workflow
**Decision**: Adopt GitHub Issue Forms as the standard post-session intake surface. Use a GitHub Actions workflow to detect intake issues, prepare a session-specific branch, generate draft session artefacts, and open a pull request for review.

**Key rule**: Issue intake produces draft evidence and proposed changes only. Canon remains a reviewed and approved layer; raw player input must not directly overwrite canonical state.

**Workflow shape**:
1. Player or DM submits a post-session issue via GitHub Issue Form (short, mobile-friendly fields)
2. GitHub Action detects the issue by label (`post-session`) and triggers session-processing workflow
3. Summarisation step extracts structured draft artefacts from issue content
4. Draft artefacts are written to a session-specific branch with an immediate pull request for review
5. Developer reviews output and determines whether continuity, lore, or canon updates are needed

**Intake form fields**: session identifier or date, player/character name, favourite moment, biggest concern or correction, character's next intention, optional notes.

**Rationale**: Balances low player effort, structured intake, minimal DM administration, and clear separation between draft evidence and approved world state. Preserves audit trail and aligns with branch-first review practices.

**Deferred**: Automatic downstream agent fan-out (continuity, lore) from intake. Early versions should produce follow-up tasks or recommendations rather than chaining autonomous updates.

**See**: `.reflections/implementation/post_session_intake.md` for full workflow analysis and options.

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

### Orchestrator implementation
**Decision**: One orchestrator agent invoked directly by humans.

**Responsibilities**:
1. Classify request and decompose into subtasks
2. Select specialist agents or MCP tools and invoke workers
3. Merge outputs and validate against project rules

**Note**: Does not produce content directly; manages delegation and integration only.

### Worker agent implementation
**Decision**: Narrow specialist agents with clear bounded authority, non-overlapping mandates.

**Defined Agents**:
- **Continuity Agent**: Checks canon, world state, unresolved threads, entity consistency
- **Prep Agent**: Produces session deliverables
- **Encounter Agent**: Creates arcs/quests, encounters, locations, events
- **Lore Agent**: Creates world geography & history, cultures, factions, NPCs
- **Documentation Agent**: Reviews reflection/campaign docs, readme, devlogs, audit logs
- **Historian Agent**: Maintains change history record of campaign updates

### MCP Tooling
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
* Session deliverables schema: structured artefacts per session (summary, notable events, proposed state changes, unresolved questions, contradictions)
* Post-session intake workflow: GitHub Issue Form schema, routing labels, automated branch and pull request scaffolding, and intake-to-draft artefact pipeline
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

**Source Files** (marked "Current" as of 17/04/2026):
- `.reflections/implementation/design_goals.md`
- `.reflections/implementation/engineering_principles.md`
- `.reflections/implementation/agent_principles.md`
- `.reflections/implementation/post_session_intake.md`
- `.reflections/background/orchestration_2.md`
- `.reflections/reviews/existing_work.md`
- `.reflections/reviews/agentic_merits.md`

Files marked "Historic" were excluded per project constraints.
