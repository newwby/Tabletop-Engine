# Agent Workflows for Creative Tabletop Development

- **Date recorded**: 03/04/2026
- **Last updated**: 05/04/2026

Problem statement

---

## Scope and working definitions

This research focuses on **structured, role-separated, multi-agent AI workflows** applied to **tabletop RPG creation and play support**, where “multi-agent” means **multiple distinct agent roles** (e.g., narrator, rules judge, lore builder, validator) coordinated by explicit orchestration logic (routing, shared state, iteration/approval loops), rather than a single prompt with tool calls. citeturn7view0turn6view1turn18view4turn26view0

Within tabletop workflows, there are two practically different targets:

- **Authoring/prep workflows**: generating/refining campaign artefacts (NPCs, locations, encounters, handouts, continuity notes) over time, with memory and validation. citeturn7view4turn17view0turn14view1  
- **Runtime workflows**: real-time assistance during sessions (rules lookup, initiative/combat mechanics, state tracking, fast content generation) with tight latency constraints. citeturn6view1turn22view0turn26view1

A recurring theme in both is *separating narrative from mechanics* (and often separating “thinking” from “speaking”), because rules correctness, spoilers, and continuity failures are the dominant failure modes for single-LLM “DM chat” approaches. citeturn6view1turn14view1turn26view1

## Confirmed examples of multi-agent systems in TTRPG workflows

The following are **confirmed implementations or published systems** that clearly meet the “multi-agent + orchestration + TTRPG/narrative GM support” bar.

**TRPG Game Mastering Using LLM‑Based Multi‑Agent System** (academic)  
Category: academic research prototype.  
What it is: a GM agent produces a provisional response, then specialised agents (notably a **rule agent** and **scenario agent**) review and approve/disapprove; the GM revises iteratively until all approve or a max iteration count is hit. citeturn6view1turn5view1turn6view0  
Why it matters: it’s a direct study of the exact “generator + validators + iteration loop” pattern for tabletop GMing, including measured trade-offs: scenario progression improved, but rule violations/spoilers and latency issues can increase when agent feedback is wrong or conflicting. citeturn4view1turn5view2turn6view1  
Relevance: **direct**.

**AI Dungeon Master** (ITMO-Agentic-AI, GitHub) (hobby/open-source)  
Category: hobby/open-source (implementation-focused).  
What it is: a LangGraph/LangChain-based **multi-agent** D&D GM system with a **single shared GameState** and **eight specialised agents** (e.g., Story Architect, World Engine, Action Resolver, Director, Lore Builder, Rule Judge). citeturn7view0turn25search0  
Why it matters: it explicitly models: (a) **shared state** across agents and (b) **role-level decomposition** including a **Rule Judge**—both central components of your target architecture. citeturn7view0  
Relevance: **direct**.

**Autonomous Multi‑Agent RAG Game Master** (Devpost) (prototype/hackathon)  
Category: prototype/hackathon project.  
What it is: a modular architecture with a **World Builder agent**, **multiple NPC agents with independent goals**, a **Game Master agent** for arbitration/consistency, plus a **RAG memory system** and an API-driven orchestration layer (FastAPI + React front-end). citeturn18view4  
Why it matters: it closely matches the “dynamic world + independently reasoning NPCs + central GM resolution + memory” design pattern. citeturn18view4  
Relevance: **direct**.

**Claude‑Code‑Game‑Master** (GitHub) (hobby/open-source)  
Category: hobby/open-source (system-building).  
What it is: a persistent adventure system that (1) vectors imported source documents and (2) spawns **extraction agents** to convert text into structured data; during play, **specialist agents** (e.g., rules, monster manual, loot, world building) “spin up” contextually. It also publishes an explicit “specialist agents” list and triggers. citeturn26view0  
Why it matters: it demonstrates a **multi-agent content pipeline** (import → extract → structure) plus **runtime specialist delegation**. This is close to long-term campaign development where sourcebooks/notes are continuously ingested and referenced. citeturn26view0  
Relevance: **direct**.

**agent-dungeonmaster-langchain** (GitHub) (hobby/open-source)  
Category: hobby/open-source.  
What it is: a multi-agent structure with a **main/orchestrator agent**, a **task agent** (categorises player actions and assigns difficulty), and a **location agent** backed by document ingestion + retrieval. citeturn7view1  
Why it matters: it shows a minimal but real separation of responsibilities (classification/mechanics vs lore retrieval vs GM interaction) and an ingestion step for adventure PDFs. citeturn7view1  
Relevance: **direct** (small-scale).

## Near matches with strong overlap

These systems overlap strongly with the target design, but either (a) do not clearly implement multi-agent role separation, or (b) are “agentic” with tools/memory but not a coordinated team of agents.

**CALYPSO: LLMs as Dungeon Masters’ Assistants** (academic + code artifact)  
Category: academic system deployed “in the wild”.  
What it is: a set of LLM-powered interfaces to support human DMs during play (encounter understanding + focused brainstorming + open chat baseline), designed explicitly to be low-friction mid-session. citeturn14view0turn14view1turn12search9  
Why it’s near (not direct): CALYPSO is more “co-pilot interfaces” than a multi-agent orchestration team; however, it directly targets **cognitive load reduction** and **session-time usability**, and it explicitly frames campaigns as long-running (months/years), which is crucial to your long-term workflow goal. citeturn13view0turn14view0  
Relevance: **partial**.

**Simulacrum: AI Campaign Copilot for Foundry VTT** (GitHub module)  
Category: hobby/productised plugin (VTT integration).  
What it is: an in-VTT “campaign copilot” that can plan multi-step operations and directly manipulate campaign documents (NPCs, items, journal entries), search/read existing content, and run automation. citeturn7view4  
Why it’s near: it looks like a **tool-using agent embedded in the live tabletop UI**, which is a major missing piece in many prototypes. It is not clearly multi-agent, but it is clearly designed for **multi-step orchestration inside a persistent campaign workspace**. citeturn7view4  
Relevance: **partial** (strong runtime overlap).

**EverTavern: persistent NPC as a live Discord character** (indie project write-up)  
Category: hobby/indie.  
What it is: a persistent NPC acting in a live game, with a rule-based triage step and a LangGraph pipeline connecting to multiple MCP servers (rules lookup via Open5e + RAG, episodic memory, game state, Discord tooling). It uses “two-phase” design (reason → respond) and tool-call iteration limits for mechanics/combat tools. citeturn22view0  
Why it’s near: it is primarily **single-agent + rich tooling**, rather than multiple collaborating agents; however, it demonstrates (a) campaign-grade memory integration, (b) explicit separation of reasoning vs roleplay output, and (c) real-time play-by-post ergonomics. citeturn22view0  
Relevance: **partial** (especially for “session assistant” and “persistent NPC” roles).

**Friends & Fables** (commercial product)  
Category: commercial.  
What it is: an AI GM + worldbuilding tools + virtual tabletop integrated into one platform, including a “Quest Editor”/structured quest tooling and a live GM persona (“Franz”). citeturn15search0turn15search1turn15search11  
Why it’s near: it plausibly addresses the *product goal* (integrated platform with long-term content and in-play GM support), but public materials don’t confirm multi-agent orchestration (role-based agents, validation loops, etc.). citeturn15search0turn15search11  
Relevance: **partial** (integration/UX overlap; architecture unconfirmed).

**DM Co‑Pilot** (open-source app + community build)  
Category: hobby/open-source progressing towards product.  
What it is: a Streamlit-driven toolkit for campaign prep and live utilities (e.g., monster tools, session summaries, VTT export), with an emphasis on workflow automation and deployment architecture; public posts and README describe modules and a backend “engine”, but do not clearly document a multi-agent internal architecture. citeturn17view0turn17view1  
Relevance: **partial**.

**mnehmos.rpg.mcp** (MCP server for rules-grounded play)  
Category: hobby/open-source infrastructure.  
What it is: an MCP server designed so the AI “DM” narrates, but mechanics are enforced in a database (dice, AC, damage, spell slots, quests, and persistent NPC memory). citeturn26view1  
Why it’s near: it’s not positioned as multi-agent, but it is a strong building block for **mechanics grounding + persistence**, which is one of the hardest parts to stabilise in long-running TTRPG agentic systems. citeturn26view1  
Relevance: **partial**.

## Frameworks and orchestration foundations used in practice

Most real implementations above are built from a small set of recurring primitives: (a) a **workflow graph** or a **handoff model** to coordinate roles, and (b) a **tool protocol** to connect to state, rules, and assets.

**Agent orchestration libraries**
- OpenAI’s modern approach is a small set of agent primitives enabling **tool use and handoffs to specialised agents**, packaged as an SDK and supported by platform documentation (including visual workflow building). citeturn8search0turn8search5turn8search12  
- The earlier Swarm project demonstrates a lightweight “agents + handoffs” model for controllable coordination. citeturn8search1  
- Microsoft’s AutoGen is explicitly a framework for **multi-agent AI applications** (agents that converse, coordinate, and can include humans in the loop), and is accompanied by research/project documentation. citeturn8search3turn8search19  
- LangGraph is positioned as a **low-level framework for long-running, stateful (and typically graph-structured) agent workflows**, including cycles and persistence—features that map directly to campaign continuity and iterative “revise until approved” loops. citeturn20search0turn20search12turn20search19  
- LlamaIndex documents common multi-agent patterns and provides an AgentWorkflow approach for chaining specialist agents (e.g., research → write → review), which is structurally similar to “generate → validate → polish” creative pipelines. citeturn20search2turn20search5  
- CrewAI explicitly positions itself as a framework for orchestrating role-playing agents with roles and tasks, and is widely used in “writer/critic” examples (though not TTRPG-specific). citeturn9search3turn9search11  
- Strands Agents (from entity["company","Amazon Web Services","cloud provider"] open-source) provides multi-agent patterns including swarms, graphs, workflows, and an “agent-to-agent” approach, and is documented as production-oriented (tooling, observability, deployment). citeturn19search0turn19search3turn19search1turn19search5  
- Microsoft’s Agent Framework and Semantic Kernel documentation describe multi-agent orchestration patterns and agent workflow management as a first-class engineering concern. citeturn8search7turn20search6  

**Tool and interoperability protocols**
- Model Context Protocol (MCP) standardises how agent hosts connect to external tools/data via a client-server model; many tabletop-adjacent builds (including EverTavern and the Hugging Face “LLM Game Master Agent”) explicitly use MCP to decouple tool implementations from the agent. citeturn8search6turn7view2turn22view0  
- Agent-to-agent interoperability is an explicit emerging concern in platform guidance, often discussed alongside MCP for tool access. citeturn16search0turn19search1turn18view1  

The net effect is that the “software design problem” framing is already mainstream: orchestration, state, tool boundaries, retries, traceability, and governance are foundational topics in today’s agent frameworks, not just prompting tricks. citeturn8search0turn20search1turn19search3  

## Academic research and adjacent creative multi-agent systems

Academic work spans two overlapping directions: **(a) TTRPG as a testbed for long-horizon, rules-bound multi-agent behaviour**, and **(b) multi-agent systems as creative collaborators** (writing, character creation, drama scripts).

**TTRPG-specific multi-agent work**
- “TRPG Game Mastering Using LLM‑Based Multi‑Agent System” directly implements and evaluates a GM + specialised critics (rule/scenario) with iterative refinement, reporting improvements and failure modes (conflicting feedback loops and slower response times). citeturn6view1turn5view2turn4view1  
- “Static vs. Agentic Game Master AI…” describes an “agentic” version that splits responsibilities into two agents (Narrator + Archivist) using tool calls and structured memory/state updates—the architecture is explicitly framed as multi-agent division of labour. citeturn6view3turn23search0  
- “Setting the DC: Tool‑Grounded D&D Simulations to Test LLM Agents” introduces a multi-agent simulator where LLM agents take DM/player/monster roles and make tool calls that update formal game state; it is primarily an evaluation environment, but it demonstrates how tool-grounding can enforce rule truth while keeping narration flexible. citeturn6view4turn6view5  
- “Towards LLM‑Agents That Play D&D Using Iterative Prompting” uses Concordia (below) to run multi-agent D&D scenarios and iteratively adjust shared memories/prompts to improve collaborative behaviour and narrative compliance. citeturn6view6turn6view7  

**Adjacent creative multi-agent systems relevant to campaign/NPC/lore generation**
- entity["organization","Google DeepMind","ai research lab"]’s Concordia library explicitly uses a tabletop-inspired interaction pattern: a “Game Master” simulates the environment and translates agent-described actions into outcomes; it is a general-purpose foundation for multi-entity narrative simulation and evaluation. citeturn21search0turn21search1turn21search9  
- Constella is an LLM-based multi-agent tool for storywriters focused on *interconnected character creation* (discovering related characters, exposing multiple characters’ “journals”, modelling relationships via inter-character responses). citeturn10search1  
- Co‑DIRECT is described as a knowledge-augmented multi-agent framework for interactive drama script generation with roles such as Writer/Actor/Critic in a director-in-the-loop setup—close to “writer room” workflows that map well to campaign writing rooms. citeturn10search2  
- PaperDebugger is an in-editor multi-agent writing/review/editing system designed around an explicit workflow pipeline (research → critique → revision), highlighting the engineering needs of scheduling, state synchronisation, and structured patching—useful analogues for “campaign bible” and long-term narrative editing pipelines. citeturn10search3turn10search23  
- Surveys specifically on creativity in LLM multi-agent systems now exist and emphasise recurring techniques like iterative refinement and collaborative synthesis, and recurring challenges like coordination conflicts and evaluation gaps. citeturn10search0turn10search4  

Overall, academia strongly supports the conclusion that (1) TTRPG is a natural stress test for long-horizon agent systems and (2) multi-agent decomposition is a plausible remedy, but performance/latency and cross-agent conflict remain open problems. citeturn6view1turn6view5turn21search0  

## Gap analysis, common patterns, and white space

### What clearly exists

**Fragmented but real “direct match” implementations exist**: multiple open implementations and peer-reviewed prototypes explicitly implement multi-agent GMing patterns (GM + critics; DM/player agent swarms; world/NPC/GM decompositions; extraction agents for source material). citeturn7view0turn6view1turn18view4turn26view0  

**The enabling foundation is mature**: orchestration frameworks support graphs, handoffs, multi-agent dialogue, tool calling, persistence, and observability as first-class concepts (notably in LangGraph- and Strands-style ecosystems, plus vendor SDKs). citeturn20search0turn19search3turn8search0turn8search3  

### What does not appear to exist as a mature, unified product

Based on publicly documented architectures, there is **no clearly documented, broadly adopted, commercial-grade system** that simultaneously provides:

- Explicit **multi-agent role orchestration** (generator/editor/validator/etc.) with tight iteration loops, *and*
- Deep **TTRPG-specific grounding** (rules + state + spoiler control), *and*
- Seamless **long-term campaign memory** (campaign bible, evolving world state, consistent NPCs), *and*
- Reliable **real-time usability during live play** with low latency and stable failure handling, *and*
- First-class integration for **multimodal assets** (maps/tokens/audio) as coordinated agent outputs, not just separate tools.

Commercial platforms do exist that integrate an AI GM with worldbuilding and play surfaces, but their public materials do not confirm multi-agent orchestration or the kinds of validation/quality loops described above. citeturn15search0turn15search11  

Academic prototypes demonstrate the orchestration patterns, but also document serious frictions for live play (especially latency and incorrect/interacting “validator” feedback). citeturn4view1turn6view1  

### Patterns that show up repeatedly

**Supervisor + specialists with a shared state**  
This appears both in open-source D&D GM projects and in the broader agent-framework literature: maintain one authoritative state object (world, party, quests, combat) and have specialist agents read/write it through orchestrated turns. citeturn7view0turn20search12turn6view5  

**Narration separated from mechanics grounded by tools**  
Many systems enforce “truth” (dice, HP, slots, inventory) in code/tools, while narrative is free-form—reducing hallucinated mechanics. citeturn6view5turn26view1turn22view0  

**Critique/approval loops are effective but fragile**  
Validator agents can improve adherence and progression but can also introduce errors and coordination deadlocks; explicit max-iteration caps and mediation logic become necessary. citeturn6view1turn5view2  

**Two-phase reasoning (think → speak) for roleplay contexts**  
Separating private deliberation from public in-character output reduces “thinking out loud” leakage and helps keep channel boundaries (IC vs OOC). citeturn22view0turn6view3  

**Campaign continuity is increasingly implemented as “systems + memory”, not just longer prompts**  
Episodic memory stores, retrieval pipelines, and structured state updates are common, but stitching them into a stable user experience still looks more like a research/prototype landscape than a standardised product category. citeturn22view0turn26view0turn26view1  

### Opportunities and white space

**A “campaign IDE” built around agent workflows (not a chatbox)**  
The strongest analogue is editor-native multi-agent writing systems: deep integration with document state, diff/patch operations, versioning, and structured review workflows. Adapting that to campaign bibles, session logs, and world graphs is still largely open. citeturn10search3turn10search23turn17view0  

**Live-session reliability engineering for multi-agent tabletop**  
Academic work explicitly shows that multi-agent loops can harm conversational smoothness; a differentiated portfolio project could focus on deterministic orchestration, latency budgets, backoff/retry strategies, and graceful degradation to “single-agent fallback”. citeturn4view1turn6view1turn17view0  

**Asset pipeline agents tied to canonical world state**  
Most tabletop-adjacent systems focus on text + mechanics. Coordinating map/token/audio generation as downstream agents that consume the same canonical state and produce versioned assets (with validation) appears underdeveloped in confirmed tabletop-specific systems. (This is implied by what is present—rules/state/memory—and what is missing—coordinated multimodal artefact generation—in the documented systems.) citeturn7view0turn26view1turn15search0  

**Portable, tool-grounded rules engines with strong interoperability**  
MCP-based mechanics servers (or similar) suggest a path where “rules truth” is portable across UIs (Discord/VTT/web) while narrative agents vary. Standardising this layer for multiple rulesets (not just one) remains an open engineering niche. citeturn26view1turn22view0turn8search6  

**Multi-agent NPC ensembles as long-lived “cast members”**  
Concordia-style GM world simulation plus persistent NPC memory and role constraints points toward ensembles of long-lived NPC agents that can evolve over campaigns—this exists as building blocks, but not as a clearly mature, integrated tabletop product category. citeturn21search0turn22view0turn18view4  

In summary: the idea is **not novel in concept**—there are direct matches in academia and open-source prototypes—but a **fully integrated, low-latency, long-term tabletop product with explicit multi-agent workflow design and robust validation** does not appear to exist in a clearly mature, well-documented form, suggesting meaningful space for a portfolio-quality system that treats orchestration, state, memory, and validation as first-class software architecture concerns. citeturn6view1turn7view0turn15search0turn20search0