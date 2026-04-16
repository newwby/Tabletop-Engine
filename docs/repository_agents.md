# Repository Agents

This file defines the current minimum agent set for Tabletop-Engine orchestration and proposes future agents for phased expansion.

## Current Agents (Implemented Now)

The project currently uses a minimum planner-worker baseline to support orchestrated preparation workflows while avoiding early complexity.

### Orchestrator (`agents/orchestrator.agent.md`)
Single human-facing entry point. It classifies requests, delegates tasks to specialist agents, integrates outputs, and enforces MVP scope boundaries.

### Continuity Auditor (`agents/continuity-auditor.agent.md`)
Validation specialist for canon consistency. It checks continuity, unresolved threads, and contradiction risks before any canonical acceptance.

### Session Compiler (`agents/session-compiler.agent.md`)
Preparation specialist. It converts approved inputs into structured session prep deliverables without taking canon-approval responsibility.

### Historian (`agents/historian.agent.md`)
Audit specialist. It records approved changes and rationale so campaign and workflow evolution remain traceable.

## Why this is the minimum set

This set is the smallest group that supports a controlled orchestration loop:
1. Plan and route work (Orchestrator)
2. Produce prep output (Session Compiler)
3. Validate canon safety (Continuity Auditor)
4. Preserve history and audit trail (Historian)

Anything beyond this for current setup would increase overlap risk and violate YAGNI.

## Proposed Future Agents (Not Implemented Yet)

### Encounter Designer (Future)
**Purpose:** Generate arcs, encounters, locations, and event beats when campaign content generation begins at scale.

**Justification:** Session prep will eventually require dedicated encounter composition beyond generic session compilation.

**Alternative considered:** Keep encounter generation inside Session Compiler.

**Why deferred:** Current MVP setup benefits from tighter boundaries and fewer active agents.

### Lore Builder (Future)
**Purpose:** Produce world geography, cultures, factions, and NPC foundations within defined canon constraints.

**Justification:** Long-running campaigns need structured lore expansion that should remain separate from session-specific prep.

**Alternative considered:** Merge lore work into Encounter Designer.

**Why deferred:** Strong risk of overlap and scope creep before canon schemas are finalized.

### Post-Session Snapshot (Future)
**Purpose:** Transform session outcomes into structured update candidates for continuity review and history recording.

**Justification:** Reduces manual post-session intake effort and improves consistency of state update proposals.

**Alternative considered:** Fold post-session intake into Historian.

**Why deferred:** Requires stable session-log schema first.

### Documentation Steward (Future)
**Purpose:** Maintain repository docs alignment (README, reflections summaries, process docs) with approved changes.

**Justification:** Documentation drift will increase as workflow complexity grows and contributor count rises.

**Alternative considered:** Keep docs maintenance as ad hoc human/manual work.

**Why deferred:** Current document volume is manageable without a dedicated agent.

### Agent Manager (Future, Optional)
**Purpose:** Help maintain and review agent definitions, boundaries, and anti-overlap contracts.

**Justification:** Useful when the agent set becomes large and contract governance overhead rises.

**Alternative considered:** Keep this responsibility with human maintainers plus Orchestrator checks.

**Why deferred:** Premature for current scale and can encourage unnecessary self-referential complexity.
