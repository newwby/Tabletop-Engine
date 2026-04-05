# Software Engineering Principles for Agent-Based TTRPG System

- **Date recorded**: 04/04/2026
- **Last updated**: 05/04/2026

This reflection reviews relevant software engineer principles and architectural patterns that may be applicable to the project.

---

## Key Concepts

### Architecture Patterns

Approaches considered:

* **Modular monolith over traditional monolith or microservices**; balances simplicity with a clearer separation of concerns than a tightly coupled monolith, without the operational overhead of distributed services.
* **Orchestration over choreography or single-agent**; utilising structured delegation through subagents and orchestrator task decomposition, avoiding overloading single agents. This improves control and traceability and reduces coordination complexity, preventing context pollution and responsibility confusion.
* **Pipeline workflow**; highly relevant, tasks are structured as staged transformations through orchestration.
* **Event-driven extensions (GitHub actions)**; supports the async approach of GitHub Copilot web. Viable for later (decoupled) background processes but introduces complexity early on, not suitable for MVP development.

### Design Principles

* **Human-in-the-loop** - checkpoints required for canonical changes (review/approval stages) and preserving system understanding/control.
* **Single-repoonsibility** - critical for defining agent boundaries, preventing overlap and ambiguity.
* **Separation of concerns** - distinct handling of generation, validation, session support, etc - through distinct agent specialists.
* **Design by contract** - clear inputs, outputs, and failure modes per agents - to support reliability and testability.
* **YAGNI (you aren't going to need it)** - features implemented as needs demand them, avoiding contributor work drift. Important for premature complexity in autonomous systems.
* **DRY (don't repeat yourself)** - Avoid early over-abstraction but identify reusable elements, such as through developing reusable prompts, issue/pr templates, and repeatable schemas for validation logic.

---

## System Reliability

### Observability
Observability and audibility, through logging/tracing/trails, supports debugging and system trust. Error handling can support this further by distinguishing between model/tool failure, invalid output, and canon conflicts.

Observability can be supported by unit testing - isolating tool choice, file updates, prompts, etc. This is especially important if models change over time. Evaluation harnesses can support comparison of system iterations.

### Canonical truth
Validation layers/agents as quality gates are central to preventing unreliable outputs from becoming canonical state. Agent failure should not be permitted to cascade across workflows, and the source of truth for the campaign needs to remain absolute.

A canonical source of truth is required to prevent lore drift and inconsistency across agents. State machines can formalise lifecycle transitions such as Drafted → Validated → Approved → Canonised. Context management determines what information each agent can access, preventing overload and leakage of hidden information. Memory should be segmented into durable canon, working notes, drafts, and temporary session state. Versioning is required to track changes and avoid destructive overwrites. Schema design underpins structured entities such as NPCs, locations, and quests. Idempotency ensures safe retries without duplication or corruption.

### Maintainability and evolution

Ensure modularity by abstracting external dependencies (such as models/APIs) behind adapters. Documentation must capture decisions, architecture, agent roles, and workflows - evolving as these requirements change. The system should support rollback, store campaign versions, and workflows should involve consistent branching to prevent loss of history.

### Live workflow specific considerations
**Live workflow is not part of the MVP scope**, however, there are important reliability concerns to consider for this. One such concern is latency of response; multiple agents and validation passes will reduce the effectiveness of reecall.

Implementation of graceful degradation ensures partial functionality when errors/failures occur, especially important during live workflow. Optimise for consistent and predictable outputs from the same inputs - this is especially important for any live session workflow, more so than in preparation workflow.

--- 

## Next steps

### Todo
* Define the structure of the campaign canonical data model (source of truth)
* Formalise agent contracts (including input/output schemas and validation expectations)
* Design orchestrator logic (task routing, failure handling, workflow composition)
* Determine validation gate structure and canonisation criteria
* Establish MVP implementation of observability (logging/debugging/audability standards)

### Further Research

* Evaluation frameworks for LLM output consistency
* Observability tooling patterns for tracing multi-step workflows
* **[Hexagonal architecture for isolating external dependencies such as LLM providers](https://dev.to/dyarleniber/hexagonal-architecture-and-clean-architecture-with-examples-48oi)**
