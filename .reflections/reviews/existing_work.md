# Reviewing Existing Work Around Multi-Agent Creative Systems

- **Date recorded**: 05/04/2026
- **Last updated**: 06/04/2026
- **Status**: Current

This reflection explores whether structured multi-agent AI workflows exist for creative content development, especially tabletop GM/DM support.

---

## Contents

- [Existing projects reviewed](#existing-projects-reviewed)
- [Key concepts determined](#key-concepts-determined)
- [Conclusion](#conclusion)
- [Further research / links to explore](#further-research-links-to-explore)

---

## Existing projects reviewed

Several projects exploring similar, but not identical, goals were identified. These implement multi-agent GMing patterns, world/npc decompositions, and utilise extraction tools for source materials.

These are undergoing deeper review to confirm the strengths and drawbacks of each approach. These projects often leaned towards session automation with LLM agents, a goal not within the scope of the Tabletop Engine project, but there are insights to be drawn from each approach.

### [TRPG Game Mastering Using LLM-Based Multi-Agent System](https://wordplay-workshop.github.io/pdfs/20.pdf)
_(Yukito Minari, Sei Ueno and Akinobu Lee)_

The multi-agent approach however outlined three clear issues:
1. Misunderstanding rules
2. Deviating outside existing story and world
3. Giving spoilers to players

Hypothesises for this include insufficient long-context understanding and lack of reasoning ability when referring to multiple pieces of information from different sources.

The paper outlined a means to use LLM agents to run sessions (not a goal of this project). Multi-agent approach had latency of up to more than a minute from input, which is unrealistic for live TTRPG play. The paper mentions this could potentially be addressed by limiting the length and frequency of agent interactions.

Proposals to fix this included a review system (resulting in the aforementioned latency) using agents with different responsibilities.

### [ai-dungeon-master](https://github.com/ITMO-Agentic-AI/ai-dungeon-master)
_(Timur Bavshin, Umar Ahmed, Shiwei Zhang, Dmitriy)_

A multi-agent AI system that automates the role of a Dungeon Master in Dungeons & Dragons. This repository explored using a combination of agents linked to a shared game state, hosted locally within a docker container with a CLI interface.

This work is another solution for using agentic workflows to automate gameplay, rather than as a tool to support creating and running a tabletop campaign. A notable difference is that it creates a campaign from story prompts, automating everything from LLM agents, and omits a human in the loop. Some persistence exists within the project, but the campaign is generated at runtime if no storage can be located from the orchestrator layer.

The agent strategy and tools (LangChain tools for the D&D 5e API, game mechanics, and player data.) are worth consideration for their approach, but the project does not seem to support long-term campaign play currently.

### other projects to review

- **[CALYPSO](https://www.cis.upenn.edu/~ccb/publications/calypso-llms-for-dms.pdf)** _(LLMs as Dungeon Masters’ Assistants)_
- **[Simulacrum](https://github.com/Daxiongmao87/simulacrum-foundry)** _(Campaign Copilot for Foundry Virtual Tabletop supporting OpenAI-compatible API endpoints.)_
- **[Claude-Code-Game-Master](https://github.com/Sstobo/Claude-Code-Game-Master)** _(Total conversion for Claude Code. Uses RAG and the RPG ruleset apis.)_
- **[agent-dungeonmaster-langchain](https://github.com/Ghazalehdelfi/agent-dungeonmaster-langchain)** _(LangChain DM assistant for managing tabletop sessions.)_
- **[EverTavern](https://kzfubar.com/blog/evertavern)** _(persistent NPC as a live Discord character)_
- **[Friends & Fables](https://fables.gg/)** _(commercial product)_
- **[DM Co‑Pilot](https://github.com/Cmccombs01/DM-Copilot-App/blob/main/README.md)** _(open-source app)_
- **[mnehmos.rpg.mcp](https://github.com/Mnehmos/rpg-mcp)** _(MCP server for rules-grounded play)_
- **[AI dungeon](https://aidungeon.com/)** _(A text-based adventure-story game run by AI.)_

------------------

## Key concepts determined

### Current understanding of the field

The project's key traits include:

* multi-agent role-based orchestration
* tabletop creative content generation for DMs
* real-time runtime support through pre-made deliverables and references

This combination is not exactly represented by any project reviewed. The current understanding is that related work exists in neighbouring areas, and adjacent projects do exist, but the particular framing of the problem as a software design problem is comparatively uncommon.

### Common patterns

* Workflow graph or handoff model to coordinate roles
	* such as an orchestration layer
* Tool protocol to connect to state/rules/assets
	* such as model context protocol
* Supervisor + specialists with a shared (authoritative) state
* Free-form narrative but mechanics grounded by code/tools
* Validator agents
	* can improve adherence/progression
	* introduce errors and coordination deadlocks
	* utilise explicit max-iteration caps and mediation logic
* Campaign continuity
	* implemented as 'systems + memory' not longer prompts
	* episodic memory stores
	* retrieval pipelines
	* structured state updates stitched together

---

## Conclusion

The project is not necessarily novel in concept, with close matches existing, but the exact implementation does not appear to exist in a mature and documented form. This suggest meaningful space for a system that treats orchestration, state, memory, and validation as software architecture concerns.

---

## Further research / links to explore

### Topics to review

* Orchestration (see [file 1](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/background/orchestration_1.md) and [file2](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/background/orchestration_2.md) here)
* Tool boundaries
* Agent state _(what agents know/are doing)_
* Agent retries _(how agents recover)_
* Agent traceability _(how agents behaved)_
* Agent governance _(how agents are controlled)_
* Multi-agent software systems
* Creative writing tooling
* Game development tooling pipeliens
* TTRPG assistant tools
* HCI work around cognitive-load reduction
* HCI work around usability under pressure

### Tools to review

* AutoGPT — https://github.com/Significant-Gravitas/AutoGPT
* CrewAI — https://github.com/joaomdmoura/crewAI
* LangGraph — https://github.com/langchain-ai/langgraph
* Sudowrite — https://www.sudowrite.com/
* NovelAI — https://novelai.net/
* Scenario — https://scenario.com/
* Promethean AI — https://www.prometheanai.com/
* AI Dungeon — https://aidungeon.io/
* AI-Dungeon-Master example repository — https://github.com/Torantulino/AI-Dungeon-Master

### Papers to review

* **[Generative Agents](https://arxiv.org/abs/2304.03442)**
* **[ReAct](https://arxiv.org/abs/2210.03629)**
* **[Procedural narrative generation survey](https://arxiv.org/abs/1907.06424)**
* **[Static vs. Agentic Game Master AI](https://arxiv.org/pdf/2502.19519)**
* **[Setting the DC](https://openreview.net/pdf?id=3Op7kJOvaD)** _(Tool‑Grounded D&D Simulations to Test LLM Agents)_
* **[Towards LLM‑Agents That Play D&D Using Iterative Prompting](https://ceur-ws.org/Vol-4097/paper7.pdf)**
* **[Constella](https://arxiv.org/abs/2507.05820)**
* **[Paper Debugger](https://arxiv.org/abs/2512.02589)**
