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

## Risks

* building monolithic agents
* overusing agents for simple tasks
* leaving task boundaries unclear
* skipping logging and observability
* overestimating achieveability of autonomy
* striving for high agency systems early
* using large models where lightweight models will do
* allowing agents more access than required for their role

### Modularity over monoliths

Whilst premature complexity is a hinderance to a project, monolithic ('do everything') agents should be avoided to prevent context pollution and prompt confusion. A more maintainable approach is to break the system into small modules, with single responsibilities, such as agents with distinct roles for each task.

### Logging and observability

Without logs the system becomes difficult to debug or evaluate. This hinders long-term improvements and production capability. A minimum criteria for effective logging is the abillity to observe the input, prompt, and output.

Logging provides the capability to evaluate the system output and compare model effectiveness to tasks, or measure differences in agent output after changes. Without this functionality the system is essentially operating on guesswork.

### Spectrum of agency
Agency was treated as a Lower-agency systems use predefined workflows with some selective decision-making, whilst higher-agency systems attempt more open-ended reasoning and planning.

Highly autonomous systems are not a realistic starting point, and a better approach is contrained, reliable, task-specific workflows that gradually introduce limited decision logic.

---

## Further considerations

How to evaluate success beyond “it works”? Revisit concerns such as reliability, debuggability, and maintainability for agentic software and workflows.