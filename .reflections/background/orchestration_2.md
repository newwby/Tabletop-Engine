# GitHub Copilot Multi-Agent Orchestration — Subagents vs Handoffs

- **Date recorded**: 05/04/2026
- **Last updated**: 06/04/2026
- **Status**: Current

Multi-agent workflows solve the problem of context pollution, degraded output/hallucinations, and monolithic single agents. The GitHub Copilot Orchestra workflow promotes an orchestration layer to utilise multiple Copilot agents without manual oversight.

---

## Contents

- [Subagent Key Concepts](#subagent-key-concepts)
- [Setup](#setup)
- [Handoffs vs Subagents](#handoffs-vs-subagents)
- [Architectural Decisions / Options Surfaced](#architectural-decisions-options-surfaced)
- [Conclusion](#conclusion)
- [Further Research / Links to Explore](#further-research-links-to-explore)

---

## Subagent Key Concepts

### Context Isolation via Subagents

Subagents in GitHub Copilot (VS Code Chat) spawn a genuinely separate LLM session with its own context window. When a subagent finishes, only its final output message is returned to the main (orchestrating) agent.

The same property has a cost: if you need detailed information from a previous step in the next step, separating them into subagents will lose that information. The isolation is real in both directions.

### What runSubagent Actually Does

To use runSubagent, enable it in the Tool Picker in the chat panel, and declare it in the tools frontmatter.

It does not copy the .agent.md file text into the calling agent's prompt. It genuinely spawns a separate configured session.

When called without a named agent, the subagent simply inherits the parent session's model and tools.

Because named subagents are fully configured from their own .agent.md files, each stage in a pipeline can run a different model. 

---

## Setup

### The .agent.md Format

Key frontmatter properties:
- name: display name
- model: which LLM to use
- tools: list of tools available to this agent
- agents: restrict which custom agents this agent may invoke as subagents
- handoffs: define transition buttons to other agents after completion
- disable-model-invocation: true blocks this agent from being used as a subagent by default (can be overridden by an explicit agents: list)

The body of the file (below frontmatter) is the agent's system prompt/instructions, written in Markdown.

### SubAgent Prerequisites

- VS Code stable v1.107+ (or Insiders)
- GitHub Copilot subscription (Individual or Business)
- Git initialised in the workspace
- .agent.md files placed in your workspace or User Data

---

## Handoffs vs Subagents

These are two different mechanisms for multi-agent workflows. Choosing between them is an architectural decision.

### Subagents (Orchestra pattern)
- Fully isolated context window per subagent
- Triggered automatically by the orchestrating agent
- Only final result returns to the orchestrator
- Intermediate reasoning is discarded
- Low risk of context pollution across stages
- Suited to long automated pipelines, TDD enforcement, quality gates

### Handoffs
- Defined in the agent's frontmatter under handoffs:
- When an agent finishes, a labelled button appears in the chat
- Clicking it switches to the target agent and pre-fills the prompt
- send: false means the prompt does not fire automatically — you can review it first
- The chat thread continues — the new agent can see prior context
- Context accumulates across stages (not isolated)
- Suited to 2–3 stage flows where you want deliberate human checkpoints
- Closest to the GitHub web "create task" mental model for users transitioning from web-based Copilot

### Practical Comparison

Subagents: autonomous, isolated, lower context risk, higher setup complexity
Handoffs: human-paced, context-accumulating, lower setup, familiar UX

---

## Architectural Decisions / Options Surfaced

Option A — (VSCode) Full Orchestra
Conductor + named subagents via runSubagent. Fully automated pipeline. You interact only at plan approval and phase commits. Lowest manual switching, highest setup. Best for sustained development work with quality gates.

Option B — (Single context) Handoffs
Staged agents connected by handoff buttons. You click to advance each stage. One chat thread carries context forward. Lower isolation, more control. Best transition path from GitHub web issue assignment workflow.

Option C — (Manual) Singular Specialist Agents
Select the right agent for each discrete task. No orchestration, no pipeline. Pick Code Reviewer, give it the diff. Simplest to use, no context management overhead. Best for well-scoped one-shot tasks.

--- 

## Conclusion

VSCode runAgent orchestration presents the best measure for achieving a transform pipeline whilst utilising agents with a clearly defined role, and removing context pollution. Setup is slower - requiring a conductor, narrow task agents, and quality gates - but a stated goal of this project is to 'determine an effective and reproducable multi-agent system for supporting tabletop DMs'. Once a workflow is determined and documented the setup should be streamlined for future implementation.

---

## Further Research / Links to Explore

* github.com/ShepAlderson/copilot-orchestra
* code.visualstudio.com/docs/copilot/agents/subagents
* code.visualstudio.com/docs/copilot/customization/custom-agents
* docs.github.com/en/copilot/concepts/agents/coding-agent/about-custom-agents
* docs.github.com/en/copilot/reference/custom-agents-configuration
* code.visualstudio.com/blogs/2025/11/03/unified-agent-experience
* code.visualstudio.com/docs/copilot/best-practices
* https://www.oneusefulthing.org/p/a-guide-to-which-ai-to-use-in-the
