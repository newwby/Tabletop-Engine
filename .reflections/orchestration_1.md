# GitHub Copilot Multi-Agent Orchestration - Execution Wrappers vs GitHub Actions

**Date recorded**: 05/04/2026
**Last updated**: 05/04/2026

GitHub Copilot web does not allow switching agents mid-task or orchestrating natively. This reflection explores alternative approaches to orchestrate multi-agent workflows.

---

## Orchestration

When people refer to orchestration, they mean a separate layer deciding which agent or model to call, in what order, and how outputs move between steps.

Manual orchestration is the simplest and matches the current approach: a human runs one agent, takes its output, and feeds it into the next task or agent.

Structured prompt chaining simulates orchestration by telling one agent to perform a sequence of stages in one run, though this gives weaker control between steps.

External orchestration uses code or workflow tooling to determine which agent to call, pass outputs forward, and validate results between stages. In this model, Copilot is the execution unit and the orchestrator is the decision-making layer around it.

## Copilot Agent Constraints & Implications

GitHub Copilot agents cannot be dynamically invoked or switched mid-task, and there is no native support for agent-to-agent delegation or orchestration. This prevents true multi-agent execution flows within a single run. As a result, any orchestration must be simulated either manually (human-in-the-loop) or via prompt composition. Custom agents defined in `.github/agents/*.md` are not executable entities and cannot be invoked programmatically by GitHub Actions or other agents.

---

## Lightweight Orchestration Approaches

Manual orchestration involves using an orchestrator agent that classifies a task and outputs a recommended next step, after which the user switches to the appropriate specialist agent; this workflow can be streamlined via agent handover, but otherwise this introduces friction due to repeated interaction cycles.

To reduce friction while preserving modularity, a composed execution pattern is used. This involves dynamically combining multiple agent instruction files into a single prompt at runtime, allowing staged execution of roles (e.g. generator → validator → continuity checker) within one model call. This avoids monolithic agents while maintaining separation of concerns.

---

## Execution Wrapper Model

The execution wrapper is not an agent but a runtime composition mechanism. It loads multiple agent instruction files and executes them in a defined sequence within a single prompt. This enables multi-role behaviour without requiring agent switching.

To prevent role interference and context bleed, strict controls are required. Each stage must explicitly define its role, operate only on structured inputs from previous stages, and avoid overlapping responsibilities. Outputs should be wrapped in clearly defined markers (e.g. `[LORE_OUTPUT]`) and each stage should be instructed to only act on its designated input. Role boundaries must be reasserted at each stage to ensure deterministic behaviour.

This model is a single-model staged execution, not true multi-agent orchestration.

```
https://moveo.ai/blog/wrappers-vs-multi-agent-systems
This approach creates an opaque and fragile system. The prompt's complexity makes it difficult to maintain and audit, potentially introduces context pollution and prompt confusion.
```

---

## GitHub Actions as Validation Layer

GitHub Actions cannot invoke Copilot agents directly, but they can access agent definition files as plain text and reuse them as prompt templates. During a workflow run, the repository is checked out into a temporary environment, allowing `.github/agents/*.md` files to be read and injected into an LLM call.

Agent behaviour in Actions is implemented by:
- loading agent instruction files
- combining them with task input (e.g. PR title, body, changed files)
- sending to an LLM via a script (e.g. Python, Node.js)
- returning output (e.g. PR comment)

A script is presented as the best starting point as it offers low overhead, full control, and a straightforward way to chain analysis, generation, and validation steps. 

Files created during the workflow exist only in the ephemeral runner environment and are not persisted unless explicitly committed or uploaded.

GitHub Actions is best positioned as a post-generation automation layer for validation and review, not as an orchestration engine.

### PR-Based Validation Workflow

A command-driven workflow is used to trigger validation. A GitHub Action listens for an `issue_comment` event containing `/review` on a pull request. When triggered, the workflow retrieves PR metadata and changed files, runs a validation script, and posts the result back as a PR comment.

This enables automated validation agents such as:
- continuity checker (detects lore conflicts)
- format validator (ensures structure compliance)
- content reviewer (flags issues and suggests changes)

The recommended approach is review-only output rather than automatic code changes, to avoid permission and safety complexity.

### Authentication & Permissions

The built-in `GITHUB_TOKEN` is sufficient for:
- reading repository content
- commenting on PRs
- updating issues

Workflow permissions should follow least-privilege principles, typically including `contents: read`, `pull-requests: write`, and `issues: write`.

### Drawbacks

* GitHub Actions require script development and calling in order to execute LLM calls.
* Agent files are injected into LLM prompts in their entirety.
* Agent files must be stripped of project context in order to prevent prompt confusion, reducing agent usefulness in the rest of the proejct.

---

## Composed Execution - Subagents vs Handovers

Composed execution can be achieved by dynamically combining agents via strict stage controls, such as with subagents or handovers.

This manages context, allows parallel tasking, and follows a modular architecture principle.

See [.reflections\agent_orchestration_2.md](.reflections\agent_orchestration_2.md) for further information ont his approach.

---

## Architectural Decisions / Options Surfaced

1. A monolithic agent approach was rejected due to scalability issues and lack of transparency in failure points.
2. Manual orchestration was identified as functional but too high-friction for frequent use.
3. True multi-agent orchestration was deemed infeasible within current Copilot constraints.

---

## Further Research / Links to Explore

* LangChain and LangGraph for future orchestration once workflows require stateful, branching, or long-running execution. Does complexity justify introducing an orchestration service?
* GitHub Actions advanced patterns, including `issue_comment` triggers, workflow dispatch inputs, automated PR suggestions, and PR review APIs.
* LLM integration within GitHub Actions for scalable validation pipelines.

### Orchestration Services

LangChain and LangGraph were surfaced as deeper workflow options, with LangGraph noted as the better fit for deterministic multi-step flows.

**see**: https://docs.langchain.com/oss/python/langchain/quickstart
**see**: https://www.geeksforgeeks.org/artificial-intelligence/langchain-vs-langgraph/

Microsoft Semantic Kernel was identified as structurally aligned with the Copilot ecosystem and useful for planners, skills, and orchestration logic.

**see**: https://devblogs.microsoft.com/agent-framework/semantic-kernel-multi-agent-orchestration/

AutoGen was noted as the option closest to true multi-agent collaboration because it supports orchestrated agent-to-agent interactions through an external coordination layer.

**see**: https://microsoft.github.io/autogen/stable//user-guide/core-user-guide/design-patterns/mixture-of-agents.html

