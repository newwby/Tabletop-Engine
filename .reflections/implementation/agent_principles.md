
# Principles of agent development

- **Date recorded**: 03/04/2026
- **Last updated**: 05/04/2026

This reflection is a discussion of the multi-agent system approach to utilise and guidelines for how to introduce effective agent files.

---

## Context

### Related project principles

The best approach is treating the idea as a software design and orchestration problem. Thinking in terms of agent roles such as generator, editor, validator, asset creator, and session assistant, coordinated by a central orchestration layer handling task delegation, validation, and iteration.

### GitHub support

GitHub supports repository-wide instructions in .github/copilot-instructions.md, custom agent profiles in .github/agents, and MCP integration to extend available tools. With optional nested AGENTS.md or instruction files for specific subfolders if parts of the repo have different conventions (such as the campaign folder).

---

## Agent File Design Principles

Agent files should remain modular and task-specific, avoiding inclusion of repo-wide context or unrelated instructions.

Shared project context should instead be placed in `.github/copilot-instructions.md`. Adhering to this enables agent files to be reused both in Copilot and in GitHub Actions without introducing noise or unintended behaviour.

See [agent_orchestration_1.md](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/background/orchestration_1.md) for further information ont his approach.

### Developer Notes

Whilst it would be cleaner to include developer notes inside agent files, they are not reliably isolated from the model even whilst commented out. Comments in Markdown or YAML are still part of the text context and may be read or influence the model; if these comments conflict with instructions they will confuse the model.

The best practice is to externalise developer notes into a developer guide for agents. This provides a clean separation behavioural instructions and internal design notes.

---

## Multi-agent system approach

### Problem
By invoking agents independently their individual actions overlap to produce emergent behaviour. Flat-structured multi-agent systems (or 'bags of agents') frequently lead to ambiguous responsibility, conflicting outputs, and task deadlocks.

A campaign task like “prepare next session” is long-horizon and cross-cutting: it touches continuity, world state, player consequences, encounter prep, retrieval, and presentation. A flat swarm makes that messy, and the workflow is slowed by having to invoke agents for each subtask in the flow. In a D&D campaign system, that creates exactly the failure mode the project highlighted in initial exploration: duplicated work, overlapping authority, inconsistent canon, and weak auditability.

### Proposal

Implement a hierarchical workflow where one top-level agent decides the task shape and specialist agents execute narrow pieces of it. This planner-worker model reduces context bloat and keeps each worker scoped; separating strategic planning from task execution. GitHub Copilot subagents enable this structured delegation.

See [agent_orchestration_2.md](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/background/orchestration_2.md) for further information ont his approach.

---

## Agent Types

### Orchestrator

The orchestrator agent is the only agent the human invokes directly. It's job is not to produce content but determine the task/s, pass the task/s to sub-agent/s as appropriate, then check and merge results.

**Orchestrator workflow**:
* interpret:
	* classify the request
	* decompose it into subtasks
* implement:
	* decide sequence vs parallel work
	* select which specialist agent or MCP tool is needed
	* invoke worker(s)
* integrate:
	* workers return defined structured outputs
	* merge outputs
	* validate against project rules before composing final output

### Workers (Sub-Agents)

Narrow worker agents are specialists with clear bounded authority, not overlapping mandates. Some agents in early consideration are:

* **Continuity Agent** (checks canon, world state, unresolved threads, entity consistency)
* **Prep Agent** (produces session deliverables)
* **Encounter Agent** (creates encounters and events)
* **Lore Agent** (record keeper, arc/faction/NPC creator)
* **Documentation Agent** (reviews docs, readme, devlogs, prompt summaries)

### Flat

Flat agents are for singular purpose, invoked when a task should not touch multiple agents. This approach should not utilise flat agents regularly, except for minor generation tasks.

### MCP Tooling

Instead of a rules expert, utilise MCP servers for campaign data, rule references, storage, retrieval, maybe generation helpers. This then enables planner agents, continuity agent, any other agents (such as those for content/encounter/session), to have access to the same capabilities.

---

## Final consideration

The project is not just content generation, it needs to also concern itself with persistence, audibility, and world state. A planner-worker architecture better supports these gaols because it leaves a clearer record of tasks requested, specialists invoked, context utilised, validation, and final output.
