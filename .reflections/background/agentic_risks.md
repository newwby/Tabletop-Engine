# Summary of agentic workflow concerns and risks

- **Date recorded**: 05/04/2026
- **Last updated**: 06/04/2026
- **Status**: Current

## What “agentic” means
Agentic software is defined as software that can decide its own next steps rather than following only a rigid fixed script.

In practical terms, this means choosing tasks, gathering context, and performing actions dynamically. Many modern systems are not fully autonomous, but structured workflow automations with limited agency - preserving the human-in-the-loop principle.

---

## Terminology

* MCP - model context protocol, a standard interface for an LLM to interact with a tool or data source.
* Structured output - a format assurance to ensure tool response or model action is usable JSON.
* RAG - retrieval augmented generation, improves LLM accuracy by retrieving data from external sources.

---

## Agent Risks and Mitigations

### Agency scope creep

Too many agents, features, or systems introduced early. An overly complex system produces unpredictable results. Lower-agency systems use predefined workflows with some selective decision-making, whilst higher-agency systems attempt more open-ended reasoning and planning.

- **Mitigation**: Define a strict MVP. Limit the initial agent set, with incremental feature addition, and avoid the overuse of agents for simple tasks or overestimating autonomous achievability early. Do not strive for high agency initially, i.e. avoid development complexity at first; strive for proof of concept first and then an MVP. Highly autonomous systems are not a realistic starting point, and a better approach is constrained, reliable, task-specific workflows that gradually introduce limited decision logic.

### Agent overlap

Agents duplicating responsibilities.

- **Mitigation**: Enforce strict boundaries, Define responsibilities clearly and review potential for agent overlap regularly. Utilise an orchestrator to call sub-agents and manage task decomposition.

### Unexpected or poor outputs

Agents produce inconsistent or incorrect results, or produce unexpected results due to unbounded access.

- **Mitigation**: Require human review during initial development loop and whenever the campaign canon is touched. Determine clear task boundaries and determine agent capability/tool access by role to prevent unexpected results.

### Inability to debug results

Without logs the system becomes difficult to debug or evaluate. This hinders long-term improvements and production capability.

- **Mitigation**: Introduce validation layer early, with logging/observability and utilising an editor/validator agent. Logging provides the capability to evaluate the system output and compare model effectiveness to tasks, or measure differences in agent output after changes. Without this functionality the system is essentially operating on guesswork. A minimum criteria for effective logging is the ability to observe the input, prompt, and output.

### Monolithic agents

Whilst premature complexity is a hinderance to a project, monolithic ('do everything') agents should be avoided to prevent context pollution and prompt confusion.

- **Mitigation**: A more maintainable approach is to break the system into small modules, with single responsibilities, such as agents with distinct roles for each task.

### Model-task misalignment

Overprovisioning model choice increases cost/latency without proportional quality gains. Using large models where lightweight would do will make workflows slower, less scalable, and harder to iterate on.

- **Mitigation**: Some tasks are not as complex as others, model size should be matched to common agent task complexity. Determining model strengths and weaknesses will support informed decision making. Reserve larger models for high-ambiguity or judgment heavy steps, and smaller models for classification, extraction, formatting and straightforward transformations. Evaluate trade-offs between quality/latency/cost at each workflow stage.


---

## Further considerations

- How to evaluate success beyond “it works”?
- How to measure effectiveness against traditional human-driven campaign development?
- Revisit concerns such as reliability, debuggability, and maintainability for agentic software and workflows