# Design direction for minimum viable product and beyond

- **Date recorded**: 2026-04-05
- **Last updated**: 2026-05-05

This reflection records the design goals of the MVP, as determined from existing work and other reflections.

---

## MVP goals

* Explicit multi-agent orchestration with iterative review
* TTRPG-specific grounding for rules/state/spoiler control
* Long-term campaign memory (bible, world state, consistent NPCs)

---

## Potential post-MVP goals

**Campaign IDE built around agent workflows**; an analogue to editor-native agent writing systems with deep document state integration and version control, but adapted to campaign bibles, session logs, world graphs etc.

**Reliable live-session prompting**; research indicates multi-agent loops harm conversational smoothness. Deterministic orchestration (with latency budgets, retry strategies, single-agent degradation) could support effortless AI companions for tabletop DMs. 

**Multimodal asset pipeline**; coordinating map/token/audio generation via downstream agents and tools, producing versioned assets according to canonical state.

**Evolving NPC agents**; utilising agent files as persistent NPC memory, creating ensembles of unique characters evolving over campaigns.

**Narrative layer standardisation across rulesets**; tool-grounded rules engines already have strong interoperability through MCP-based mechanics servers. Narrative agents struggle with this.