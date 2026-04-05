# Agent-Oriented Campaign Development for D&D (Repo-Driven Approach)

- **Date recorded**: 03/04/2026
- **Last updated**: 05/04/2026

**Problem Statement**  
How to design a D&D campaign workflow using a structured repository and multi-agent system that reduces DM cognitive overload during sessions while maintaining long-term consistency and scalability.

---

## Key Concepts

### Campaign as a Software System  
The campaign is treated as a repository-driven system where markdown files act as source data, canon represents the source of truth, and session files are compiled artifacts. This reframes DM preparation as a build process rather than manual organisation, separating storage (data/canon) from execution (session files).

### Repo Structure for Searchability and Clarity  
A layered structure is used: data for atomic files (NPCs, quests, enemies), canon for approved world state, sessions for logs and compiled outputs, and players for state snapshots. Atomic files enable fast lookup and reduce context overload compared to large monolithic documents.

### Compiled Session Workflow  
Instead of browsing notes during play, a “compile session” step generates a single markdown file containing only actionable content. This includes TL;DR, scene flow, NPC summaries, encounters, hooks, and fallback logic. A secondary “DM screen” output provides an ultra-condensed quick-reference version.

### Historian Agent as State Manager  
The Historian agent ingests session logs and converts events into canonical state updates across timeline, NPCs, quests, and player summaries. This agent maintains a single source of truth and avoids drift between narrative and stored data.

### Continuity and Validation Layer  
A Continuity Auditor verifies changes against canon before merge, flagging contradictions and unresolved threads. A Rules/Mechanics Referee ensures system correctness (e.g. stat blocks, CR balance). A Verification Editor optionally reviews high-risk outputs against prompts, surfacing assumptions and ambiguity rather than silently correcting.

### Session Usability Agents  
The Session Compiler generates playable session files, while a DM Screen Assistant produces minimal quick-reference views. A Recap/State Snapshot agent summarises post-session changes and current world/player state to reduce prep time.

### Content Generation Agents (Proposal-Only)  
Quest Designer, NPC Writer, Encounter Designer, and Location/Battle Site Designer generate structured content linked to canon. All outputs are treated as proposals and require review before becoming canonical.

### Asset and Media Support  
Instead of direct generation, an Asset Brief Writer produces structured specifications for standees, tokens, battlemaps, and audio. Additional agents such as Battlemap Brief Agent and Audio Director define requirements without owning canon, preventing drift between content and assets.

### Branching and Workflow Control  
Branching is used to separate proposal content from approved canon. Patterns include proposal/* for generated content, session-prep/* for upcoming sessions, and repo-maintenance/* for agent/template changes. Copilot can assist in PR-based workflows, but cannot reliably enforce automatic branch switching purely through agent instructions.

### Agent Scope and Overlap Minimisation  
Agents are deliberately constrained to avoid duplication and long-term drift. Core agents focus on truth and usability, while content agents are proposal-only. Avoided agent types include vague “worldbuilder” or “creative improver” roles due to poor long-term reliability.

### Copilot Integration  
GitHub Copilot features used include .github/copilot-instructions.md for repo-wide context, .github/agents/*.agent.md for custom agents, and prompt files for reusable workflows. Copilot supports PR creation and agent selection but relies on explicit workflow instructions rather than autonomous branch management.

### Software Engineering Principles Applied  
The system maps directly to engineering concepts: source of truth (canon), derived artifacts (session files), change history (session logs), review gates (continuity/mechanics validation), modular architecture (atomic files), and build pipelines (session compilation). This forms the basis for a portfolio/blog narrative.

---

## Architectural Decisions / Options Surfaced

- Single Historian agent handling both session ingestion and repo consistency to avoid drift, rather than splitting responsibilities.
- Introduction of a Continuity Auditor instead of overloading the Historian with validation responsibilities.
- Replacement of a universal “assistant editor” with a lighter model: mandatory output contracts plus selective Verification Editor for high-risk outputs.
- Separation of asset generation into briefing/specification agents rather than direct generation to maintain canon integrity.
- Limiting initial implementation to core agents (Historian, Session Compiler, DM Screen Assistant) before expanding to content generation.
- Use of compiled session files as the primary runtime artifact to eliminate mid-session navigation of raw notes.

---

## Further Research / Links to Explore

- GitHub Copilot custom instructions: https://docs.github.com/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot  
- GitHub Copilot custom agents: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents  
- GitHub Copilot PR workflows: https://docs.github.com/copilot/using-github-copilot/coding-agent/asking-copilot-to-create-a-pull-request  
- GitHub prompt storage: https://docs.github.com/en/github-models/use-github-models/storing-prompts-in-github-repositories  

---

## Open Questions / Next Steps

- Define strict schemas for atomic files (NPCs, quests, encounters) to ensure agent consistency.  
- Implement Historian.agent.md and Session Compiler.agent.md as the first working agents.  
- Establish a formal branching policy and enforce it via repo instructions and PR workflows.  
- Determine toolchain for asset generation based on Asset Brief outputs (e.g. map tools, token generators).  
- Evaluate DiceCloud vs Roll20 for character state ingestion and standardise post-session snapshot format.  
- Decide how orchestration is handled: manual invocation vs orchestrator agent coordinating multi-step workflows.  
- Define constraints for agent permissions (which directories each agent can modify).  
- Develop a blog/portfolio write-up framing this as “software architecture applied to tabletop campaign design.”


# Agent-Oriented D&D Campaign Development in a Repo

- **Date recorded**: 2026-04-05
- **Last updated**: 2026-04-05

This summary records an exploratory conversation about using a structured repository and multi-agent workflow to design, maintain, and run a long-term D&D campaign with lower cognitive load. The discussion focused on repo architecture, agent responsibilities, session-compilation workflows, branching/review practices, GitHub Copilot integration, and how this can be framed as a portfolio/blog example of software engineering principles applied to tabletop campaign development.

---

## Problem Statement

How to build and maintain a long-form D&D campaign in a repository using agents, so that campaign information stays coherent and searchable over time while session prep and live DM usage become dramatically less overwhelming.

## Key Concepts

### Campaign Repo as a Source-of-Truth System

The campaign was reframed as a software-style system rather than a loose set of notes. Atomic markdown files act as structured source material, canonical files represent approved truth, and playable session packets are compiled artifacts derived from that truth. This separation reduces the need to navigate large notes collections mid-session and makes the campaign easier to search, update, and maintain.

### Repo Structure for Searchability and Cognitive Relief

A layered repo structure was surfaced as central to the approach. `data/` contains atomic files such as NPCs, enemies, quests, factions, locations, items, audio cues, and asset briefs. `canon/` stores approved world state and merged truths such as `timeline.md`, `campaign_state.md`, and `active_threads.md`. `sessions/` contains raw logs, compiled session files, and DM screen outputs. `players/` stores character and party summaries. `assets/` stores standees, battlemaps, handouts, and audio. The key idea is that searchable atomic files reduce overload compared to a monolithic campaign document.

### Compiled Session Files as Build Artifacts

The most important runtime concept was the idea of compiling a session file from repo content. Instead of using raw notes during play, the system generates a single markdown session file containing only live-usable material such as TL;DR, scene flow, condensed NPC summaries, encounters, hooks, off-script handling, and end states. A second ultra-condensed output, comparable to a DM screen or quick-view sheet, exists for panic-proof reference during play. This moves the DM from browsing notes to using a purpose-built execution artifact.

### Historian Agent as State Manager

A Historian agent was identified as a core piece of the system. Its purpose is to ingest post-session updates and convert events into canonical state changes. This includes updating NPCs, quests, factions, locations, player state, and timeline records. The final understanding was that this agent should not be split into separate “history” and “repo change” agents, because that would create drift; one Historian should own state update responsibilities.

### Continuity, Mechanics, and Verification

A Continuity Auditor was proposed as a distinct agent that checks proposals against canon before approval, flagging contradictions, dead threads, duplicate roles, and timeline errors. A Rules / Mechanics Referee was suggested for stat blocks, encounter legality, CR sanity checks, condition tracking, and rules correctness. The idea of an always-on universal assistant editor was discussed and refined into a more selective Verification Editor model: rather than wrapping every agent all the time, all agents should produce clear output contracts stating what changed, assumptions made, uncertainties, and whether review is required, while a verification-focused reviewer is only used for high-risk outputs such as canon changes, mechanics-heavy work, session compilation, and agent/template modification.

### Session Usability Agents

Several agents were framed as directly reducing mental strain while running sessions. The Session Compiler builds the playable session packet from current canon, active quests, and player state. A DM Screen Assistant produces ultra-condensed quick-reference material. A Recap / State Snapshot agent summarises post-session changes, what players know, unresolved hooks, party status, and current consequences, reducing the startup burden for the next session.

### Content Production Agents

Proposal-only content agents were recommended for expanding and fleshing out the campaign without giving them authority over canon. These included a Quest Designer, NPC Writer, Encounter Designer, and Location / Battle Site Designer. Their role is to generate structured, linkable content tied back to factions, locations, quests, and current state, but all outputs remain proposals until reviewed and accepted.

### Asset and Media Support

The conversation expanded into support for standee or placeable graphics, battlemap graphics, and audio assets. The recommended pattern was not to let asset tools or art-generation logic define campaign truth. Instead, an Asset Brief Writer should turn campaign content into structured specifications for standees, tokens, placeables, battlemaps, ambience packs, music cues, and handouts. Additional specialist roles such as a Battlemap Brief Agent and an Audio Director can define requirements, but they should remain non-authoritative and asset-focused rather than canon-owning.

### GitHub Copilot Integration

The workflow was anchored to GitHub Copilot repo features. `.github/copilot-instructions.md` serves as repo-wide context and policy. `.github/agents/*.agent.md` stores custom agents. Prompt files store repeatable workflow instructions. Copilot can help with PR/task-based flows and custom agent selection, but it cannot be relied on to autonomously enforce branch switching purely through instructions. Branching must be treated as an explicit workflow rule rather than assumed agent behaviour.

### Branching, Review, and Workflow Safety

Branching was surfaced as important both for repo cleanliness and long-term agent safety. Suggested lanes included `proposal/*` for generated content, `session-prep/*` for upcoming sessions, `repo-maintenance/*` for agent and template changes, and `asset-briefs/*` for asset-spec work. The idea was to keep `main` as approved canon only, with proposals isolated until continuity, mechanics, and human review are complete. This was explicitly recognised as useful both practically and as a portfolio demonstration of review gates and separation of concerns.

### Software Engineering Principles Applied to Campaign Design

A recurring theme was that this project has strong value as a case study in applying software development principles to D&D campaign development. The mapping surfaced clearly: canon as source of truth, compiled session packs as derived artifacts, session logs as change history, continuity/mechanics checks as review gates, branching strategy as isolation of proposals, and atomic files as modular architecture. This gives the project a strong crossover appeal between software developers and D&D players, and makes it suitable for a blog or portfolio write-up focused on human-centred workflow design and cognitive load reduction.

### Disney Campaign Context

The uploaded WIP Disney tabletop campaign document showed why structure and continuity tooling matter. It contains many factions, dynasties, delegations, named characters, regions, and quest lines, including Yendis dynasties, the Unfounded Archipelago, the Nautic Channel, the Neverwonder, a very large cast, and a dense quest list. This scale makes continuity management, searchability, and compiled session artifacts especially important. :contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1} :contentReference[oaicite:2]{index=2}

## Architectural Decisions or Options Surfaced

The strongest final direction was to keep the agent system relatively small, specialised, and layered. Core agents prioritise campaign truth and session usability: Historian, Session Compiler, DM Screen Assistant, and Continuity Auditor. Content agents such as Quest Designer, NPC Writer, Encounter Designer, and Location / Battle Site Designer are proposal-only. Support agents include Asset Brief Writer, Verification Editor, and Agent Manager.

The Historian remains a single state-management agent rather than being split, to avoid drift. The Continuity Auditor exists separately from the Historian so that update and verification concerns stay distinct. The Verification Editor is selective rather than universal. The Agent Manager may modify agent files, prompts, and shared instructions, but should not touch canon content. Media-oriented agents should generate briefs/specifications rather than authoritative world changes.

A recommended implementation order was surfaced: Historian first, then Session Compiler, DM Screen Assistant, Continuity Auditor, followed by NPC Writer, Quest Designer, Encounter Designer, Asset Brief Writer, Verification Editor, and Agent Manager.

## Further Research / Links to Explore

GitHub Copilot repo custom instructions: https://docs.github.com/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot

GitHub Copilot custom agents: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents

GitHub Copilot PR/task workflows: https://docs.github.com/copilot/using-github-copilot/coding-agent/asking-copilot-to-create-a-pull-request

GitHub prompt storage in repositories: https://docs.github.com/en/github-models/use-github-models/storing-prompts-in-github-repositories

The Disney campaign file itself remains a useful internal reference for testing continuity, repo structure, and compiled session outputs because of its scale and density. :contentReference[oaicite:3]{index=3} :contentReference[oaicite:4]{index=4}

## Open Questions / Next Steps

The next practical step is to define strict schemas for atomic files such as NPCs, quests, factions, encounters, and locations so that agents can operate consistently. The Historian and Session Compiler should be implemented first as working agent files and tested against a small subset of campaign content before wider rollout.

Branching policy still needs to be formalised as repo rules and paired with clear edit-path permissions for each agent. The exact review flow for continuity, mechanics, and human approval also needs to be codified.

The media pipeline remains open: asset briefs, battlemaps, standees, placeables, and audio support were identified as valuable, but the specific tools for generation and storage were not yet selected.

Character-state ingestion also remains open. Free tools such as DiceCloud or Roll20 were previously considered for maintaining character data, with a reduced post-session snapshot format proposed for repo storage rather than full sheets.

Finally, the project has clear potential as a blog/meta portfolio piece focused on “software architecture applied to tabletop campaign design,” and a future write-up could structure that around problem framing, architecture, workflow, review gates, compiled artifacts, and lessons about where agents help versus where they quietly create long-term problems.