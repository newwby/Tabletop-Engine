# Design direction for minimum viable product and beyond

- **Date recorded**: 04/04/2026
- **Last updated**: 06/04/2026
- **Status**: Current

This reflection records the design goals of the MVP, as determined from existing work and other reflections, as well as development risks not related to agentic development or software engineering principles.

---

## Contents

- [Project goals](#project-goals)
- [MVP goals](#mvp-goals)
- [Project Risks and Mitigations](#project-risks-and-mitigations)
- [Further development](#further-development)

---

## Project goals

* Determine an effective and reproducible multi-agent system for supporting tabletop DMs.

---

## MVP goals

* DM preparation as a build process; primer deliverables and wiki structure to support sessions
* Explicit multi-agent orchestration with iterative review
* TTRPG-specific grounding for rules/state/spoiler control
* Long-term campaign memory (evolving versioned canon through bible, world state, consistent NPCs)
* Established workflow; approved canon maintained on main & session changes pushed to branch first of all

### MVP Deliverables

* Narrow task agents managed through an orchestration layer, with defined constraints and roles
* Formal branching policy (enabling rollback/versioning), enforced by repo instructions and PR workflows
* Global agent instructions, agent usage guidance, and repo-directory specific agent instructions
* Atomic files to support wiki, fast lookup, and reduced context overload.
* Atomic schemas per file type (npc/quest/enemy/encounter/location)
* Reusable prompts for content generation
* Structured validation schemas for agents
* Issue/pr templates for contributors along with GitHub quickstart guide
* Session primer generation for live workflow (TL;DR, scene flow, relevant file links and fallback logic)
* Session log output schema and intake for session recording with standardised post-session schema
* Observability of campaign changes (historical record)
* Devlog write-ups from reflection notes, framing 'applying software architecture to tabletop campaign design'

---

## Project Risks and Mitigations

### Lack of decisions

Endless exploration without commitment will delay project implementation - orchestrated agent workflows can involve additional setup and research.

- **Mitigation**: Utilise decision logs, record reflections to prevent re-reviewing existing work and to force explicit decisions. Time-box exploration to prevent endless exploration.

### Unclear value

If the system cannot meaningfully improve workflow, the additional setup demands would be unwise for reproduction.

- **Mitigation**: Define success criteria early, iterate based on feedback, and test against real use cases. This can take the form o implementing a tabletop system through the MVP of the project and playtesting after MVP delivery. Determining a means of measuring, even qualatively, the effectiveness of running a campaign utilising the project vs without, would provide effective data for proving value.

### Time constraints

If the project grows beyond developer available time it may very well stall.

- **Mitigation**: Prioritise core functionality/MVP, and treat as an iterative project. Cut any non-essential feature early. Maintain repository and contributor guides to support both third-party involvement and return to the project if time demands prevent continued work.

---

## Further development

### Future considerations

* Scalability and longevity concerns (canon drift, campaign/wiki expansion)
* Performance considerations (especially during live sessions)
* Developer experience (inc. onboarding)
* Governance and control

### Potential post-MVP goals

**Generated wiki documenting relationships between atomic files & narrative structure**

**Application companion (lightweight) mobile app for pulling information from server-hosted campaign docs quickly through natural language search interface, or providing updates during a session**

**Campaign IDE built around agent workflows**; an analogue to editor-native agent writing systems with deep document state integration and version control, but adapted to campaign bibles, session logs, world graphs etc.

**Reliable live-session prompting**; research indicates multi-agent loops harm conversational smoothness. Deterministic orchestration (with latency budgets, retry strategies, single-agent degradation) could support effortless AI companions for tabletop DMs. 

**Multimodal asset pipeline**; coordinating map/token/audio generation via downstream agents and tools, producing versioned assets according to canonical state. Potentially beginning with specification output for external tools.

**Evolving NPC agents**; utilising agent files as persistent NPC memory, creating ensembles of unique characters evolving over campaigns.

**Narrative layer standardisation across rulesets**; tool-grounded rules engines already have strong interoperability through MCP-based mechanics servers. Narrative agents struggle with this.
