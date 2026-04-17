# Working Todo List

**Last updated**: 06/04/2026  
**Status**: Reflects current project state after foundational research phase

This living document exists as both a guide and record, to track the project's current working tasks, separating completed foundational research from implementation work ahead.

---

## Contents

- [Phase 0 - Setup and Research](#phase-0-setup-phase)
- [Phase 1 - MVP Implementation Phase](#phase-1---mvp-implementation-phase)
- [Phase 2 - Iterative Campaign Phase](#phase-2---iterative-campaign-phase) _(WIP)_

---

## Phase #0 Setup Phase

### 1. Repository Structure & Guidelines

- [x] Create GitHub labels (e.g. `mvp`, `architecture`, `agent`, `workflow`, `risk`, `research`, `docs`, `blocked`, `priority-high`)
- [ ] Review [/docs/potential_templates.md](https://github.com/newwby/Tabletop-Engine/blob/main/docs/potential_templates.md)
- [ ] Define issue template set (Architecture Decision, Agent Definition, Agent Review, Task/Feature, Bug, Research/Exploration)
- [ ] Add PR template aligned to project goals (scope, decision impact, validation, docs updates)
- [ ] Create baseline project board to support cognitive load when returning (Backlog, Ready, In Progress, Review, Done)
- [X] Add contribution basics (`CONTRIBUTING.md`) with quickstart guide & issue/PR workflow
- [X] Contribution guide for playtesters to explain how to write an issue and GitHub basics
- [ ] Create `.github/copilot-instructions.md` with repo-wide context and policy
- [ ] Define branch protection rules for default branch
- [ ] Establish branching policy (`proposal/*`, `session-prep/*`, `repo-maintenance/*`, `asset-briefs/*`)
- [ ] Define directory structure for campaign data (`data/`, `canon/`, `sessions/`, `players/`, `assets/`)

### 2. Documentation Review & Alignment

- [x] _Document the phase 0 setup progress (ongoing)_
- [x] Review exploratory prompts and document initial process
- [x] Review all files in [/docs/](https://github.com/newwby/Tabletop-Engine/tree/main/docs)
- [x] Compare [todo.md](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md) against current project intent and update it as needed
- [x] Ensure project README reflects current project truth and links key docs clearly
- [ ] Convert actionable doc items into tracked GitHub issues
- [ ] Confirm documentation ownership/update expectations for future updates

### 3. Update [todo.md](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md) to address active priorities

- [x] Continue progress on MVP/Core Direction items listed in [todo.md](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md)
- [x] Continue progress on Architecture Decisions listed in [todo.md](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md)
- [x] Continue progress on Agents and Workflow Design listed in [todo.md](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md)


### 4. Research and Planning

- [x] Review outcome of step 3 (working files in /docs/) for completeness and decisions to be made
  - [x] Review [`.reflections/reviews/initial_exploration.md`](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/reviews/initial_exploration.md)
  - [x] Review [`docs/development_direction.md`](https://github.com/newwby/Tabletop-Engine/blob/main/docs/development_direction.md)
- [ ] Investigate MCP projects for DND 5e referencing
  - https://github.com/procload/dnd-mcp
  - https://mcpservers.org/servers/heffrey78/dnd-mcp
- [ ] Review/revise campaign documents for IP considerations
- [x] Determine architectural decisions and relevant engineering principles for MVP

#### MVP / Core Direction
- [x] Define MVP scope; minimum viable version of the system
  - _Completed in `.reflections/implementation/design_goals.md`_
- [x] Identify initial problems the system will solve
  - _Completed in `.reflections/reviews/initial_exploration.md`_
- [x] Establish system boundaries
  - _Completed in `.reflections/implementation/design_goals.md` (MVP goals section)_

#### Architecture Decisions
- [x] Identify key system architecture and design decisions from engineering principles
  - _Completed in `.reflections/implementation/engineering_principles.md`_
- [x] Evaluate trade-offs between approaches
  - _Completed across multiple reflections (orchestration, agent principles, engineering principles)_
- [x] Review existing work in the field
  - _Completed in `.reflections/reviews/existing_work.md` and `.reflections/reviews/agentic_merits.md`_

#### Agent Design Foundation
- [x] Establish multi-agent system approach (orchestrator vs flat)
  - _Completed in `.reflections/implementation/agent_principles.md`_
- [x] Define agent types and roles (orchestrator, workers, MCP tooling)
  - _Completed in `.reflections/implementation/agent_principles.md`_
- [x] Establish agent file design principles
  - _Completed in `.reflections/implementation/agent_principles.md`_

#### Background Research
- [x] Research orchestration patterns
  - _Completed across `.reflections/background/orchestration_1.md`, `orchestration_2.md`, `orchestration_3.md`_
- [x] Review agentic workflow risks and mitigations
  - _Completed in `.reflections/background/agentic_risks.md`_
- [x] Understand prompt engineering best practices
  - _Completed in `.reflections/background/prompt_engineering.md`_

### 5. Propose agent/layer structure for MVP
- [ ] Review [/docs/candidate_agents.md](https://github.com/newwby/Tabletop-Engine/blob/main/docs/candidate_agents.md)
- [ ] Define initial agents and layers, as well as introduction order
- [ ] Add anti-overlap checks for agent responsibilities
- [ ] Add explicit controls for scope creep and overengineering
- [ ] Agent responsibilities updated to include instruction to avoid IP conflict problems
- [ ] Consider agent to record project progress for future project accounting
- [ ] Add decision logging expectations to avoid endless exploration

### 6. Determine MVP decisions

These items require explicit decisions before implementation can proceed:

- [X] **Orchestration approach**: Manual invocation vs orchestrator agent coordinating multi-step workflows?
- [ ] **Character state tooling**: Which platform/format for character data ingestion?
- [ ] **Agent permissions**: Formalize which directories each agent can modify
- [ ] **Agent expectations**: for agent outputs.
- [ ] **Agent constraints**: (e.g. over-generation, scope, continuity) for consistency.
- [ ] **Agent layer definitions**: including initial agent layers.
- [ ] **System operation diagrams**: how do agents interact? how does data move? where does validation occur?

### 7. Finalise MVP goals

- [ ] Define success criteria for MVP (practical and testable)
- [x] Define portfolio deliverables (architecture clarity + working demo + decision history)
- [x] Define first working prototype milestone
- [x] Review next steps for MVP progress through /docs/ work and issues created during this milestone
- [x] Establish MVP to-do list from exploratory work, setup, and update [todo.md](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md) as appropriate.
- [ ] Set up branches for different major tasks
- [ ] Determine exit criteria for next phase

### 8. Documentation & Review (repeated task)

- [ ] Update README to reflect current system design
- [ ] Write devlog/blog posts for "software architecture applied to tabletop campaign design"
- [ ] Review and update documentation directory structure if appropriate

### 9. Exit Criteria

- [ ] [todo.md](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md) is confirmed as baseline and referenced in active workflow
- [ ] [/docs/](https://github.com/newwby/Tabletop-Engine/tree/main/docs) files are reviewed and reconciled
- [ ] Repository setup tasks are complete
- [ ] Additional non-duplicated tasks are tracked as issues
- [ ] Initial exploration research is cemented through /docs/
- [ ] Agents are proposed and introduction timeline exists
- [ ] MVP direction is clear; solid plan exists to guide development, first step identified
- [ ] Exit criteria for MVP exists, 'definition of done' achieved
- [ ] Record of phase 0 ('setup') milestone exists on repository as devlog
- [ ] Project board or plan is set up

---

## Phase #1 - MVP Implementation Phase

### 1. Orchestration & Workflow

- [ ] Design orchestrator logic (task routing, failure handling, workflow composition)
- [ ] Define validation gate structure and canonisation criteria
- [ ] Establish observability standards (logging/debugging/auditability)
- [ ] Create reusable prompt files for common workflows
- [ ] Document agent invocation patterns

### 2. Tooling & Integration

- [ ] Evaluate MCP servers for:
  - [ ] Campaign data access
  - [ ] Rule references (D&D 5e API or similar)
  - [ ] State management
- [ ] Define which agents get which MCP tool access
- [ ] Establish character state ingestion approach (DiceCloud/Roll20/other)

### 3. Agent Implementation

#### Meta Agents (Priority 1)
- [ ] Orchestrator Agent
- [ ] AgentManager Agent

#### Core Agents (Priority 2)

- [ ] Historian Agent
  - [ ] Define input/output contract
  - [ ] Create `.github/agents/historian.agent.md`
  - [ ] Define validation expectations
- [ ] Session Compiler Agent
  - [ ] Define input/output contract
  - [ ] Create `.github/agents/session-compiler.agent.md`
  - [ ] Define session deliverable structure
- [ ] Continuity Auditor Agent
  - [ ] Define input/output contract
  - [ ] Create `.github/agents/continuity-auditor.agent.md`
  - [ ] Define validation rules
- [ ] Post-Session Snapshot Agent
  - [ ] Define input/output contract
  - [ ] Create `.github/agents/session-snapshot.agent.md`
  - [ ] Define validation rules

#### Worker Agents (Priority 3 — Proposal-only)
- [ ] Determine narrow task agents to support campaign creation

### 4. Schema & Data Model

- [ ] Define atomic file schemas for:
  - [ ] NPCs
  - [ ] Quests
  - [ ] Enemies/Encounters
  - [ ] Locations
  - [ ] Factions
  - [ ] Items
  - [ ] Timeline entries
- [ ] Define canonical state files (`campaign_state.md`, `active_threads.md`, `timeline.md`)
- [ ] Define session log output schema
- [ ] Define session primer/compilation schema

### 5. Documentation & Review (repeated task)

- [ ] Update README to reflect current system design
- [ ] Create developer guide for agents (externalized notes)
- [ ] Write devlog/blog posts for "software architecture applied to tabletop campaign design"
- [ ] Review and update documentation directory structure if appropriate

### 6. Exit Criteria

- [ ] Measure against MVP implementation goals
- [ ] MVP is ready to generate content then test against live-play

---

## Phase #2 - Iterative Campaign Phase

_Phase 2 has no defined to-do at this time._

---

## Notes

- Reflections are maintained in `.reflections/` and should be consulted before implementing features
- Focus remains on modular, human-supervised architecture (not autonomous swarm)
- Maintain separation: research/planning in reflections, implementation tracking here
- Complexity should be avoided while core features are being implemented
