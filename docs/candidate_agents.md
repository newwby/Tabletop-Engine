**_This WIP file defines the MVP-first agent set and controlled rollout plan. Agents are introduced in layers to avoid early complexity._**

---

# Initial Agent Layers (MVP-first)

## Layer 0: Control Layer

### Orchestrator
- Single human-facing entry point
- Classifies requests and decomposes tasks
- Invokes specialist agents and integrates outputs
- Enforces routing, quality gates, and stop conditions

---

## Layer 1: Governance Layer

### Continuity Auditor
- Validates canon/world-state consistency
- Flags unresolved thread conflicts
- Blocks canon updates that fail validation checks

### Historian
- Records project progress for accounting and review
- Maintains traceable records of approved campaign/system updates
- Writes concise decision outcomes for completed task cycles

### Documentation
- Maintains README/docs/reflections alignment
- Captures policy updates and process constraints
- Ensures decision logs and operating expectations stay current

---

## Layer 2: Delivery Layer

### Session Compiler (Prep)
- Produces preparation deliverables for upcoming sessions
- Compiles approved canon/context into usable DM outputs

### Encounter Designer
- Creates encounters, quests, and scene events
- Works only from approved canon and task constraints

### Lore Builder
- Creates world details (locations, factions, NPC context)
- Produces additive draft content for review before canonization

---

# Introduction Order

1. **Orchestrator + Continuity Auditor + Historian** (minimum safe control loop)
2. **Session Compiler** (first practical MVP deliverable)
3. **Encounter Designer + Lore Builder** (controlled expansion of generation scope)
4. **Documentation** hardening pass after each added layer

No new agent is introduced until the previous layer has:
- stable invocation pattern
- clear handoff contracts
- validated logging and review outputs

---

# Anti-overlap Checks (Required)

- **Single-owner rule**: each task type has exactly one primary owning agent
- **Explicit handoff contracts**: each handoff states input, output, and acceptance criteria
- **Decline-and-route rule**: agents must decline out-of-scope work and route via orchestrator
- **Conflict gate**: overlapping recommendations are resolved by orchestrator + continuity review before merge
- **Responsibility matrix review**: agent-role matrix must be reviewed when introducing or modifying agents

---

# Scope Creep & Overengineering Controls

- MVP-first: do not add an agent without an active MVP requirement
- One-layer-at-a-time rollout
- Prefer improving prompts/contracts before adding new agents
- Require a written "why now" decision for each new agent
- Defer live-session and multimodal complexity until post-MVP validation

---

# IP Conflict Avoidance Responsibilities

All agents must:
- avoid reproducing copyrighted or trademarked third-party setting content unless user-provided and authorized
- prefer original campaign material or clearly licensed/open content
- flag uncertain provenance for human approval before persistence
- record source/provenance notes when external references influence output

---

# Decision Logging Expectations

To avoid endless exploration loops:
- each exploratory cycle must end in one of: **decision**, **deferred decision with owner/date**, or **blocked with explicit blocker**
- exploration without a logged outcome should not continue beyond one additional iteration
- decisions must be summarized in root decision tracking files for auditability
