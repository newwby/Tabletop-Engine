# Software Engineering Principles for Agent-Based TTRPG System
//TODO
- **Date recorded**: 04/04/2026
- **Last updated**: 05/04/2026

A structured evaluation of software engineering principles and architectural patterns applicable to a multi-agent AI system designed for tabletop campaign preparation and live session support, with emphasis on reliability, maintainability, and reduced cognitive load.

## Key Concepts

### Architecture Patterns
The system is best framed as a modular monolith rather than microservices, balancing simplicity with separation of concerns. Layered architecture separates interface, orchestration, domain logic, and infrastructure. Pipeline/workflow architecture is highly relevant, structuring tasks as staged transformations (e.g. generation → validation → persistence). Orchestration is preferred over choreography to ensure control, traceability, and reduced coordination complexity. Event-driven extensions are viable later for decoupled background processes but introduce debugging complexity. Plugin-style extensibility and hexagonal (ports-and-adapters) architecture support long-term flexibility, especially where model providers or tools may change.

### Design Principles
Single Responsibility Principle is critical for defining agent boundaries, preventing overlap and ambiguity. Separation of concerns ensures distinct handling of generation, validation, memory, and session support. Composition over inheritance supports building agents from reusable capabilities such as model invocation, validation, and formatting. KISS and YAGNI are essential to avoid premature complexity, especially around autonomy and distributed systems. DRY applies to schemas and validation logic but should not drive early over-abstraction. Explicit contracts (clear inputs, outputs, and failure modes per agent) are foundational for reliability and testability.

### System Reliability
Validation layers (quality gates) are central to preventing unreliable outputs from becoming canonical state. Fault tolerance should ensure agent failure does not cascade across workflows. Error handling must distinguish between model failure, tool failure, invalid output, and canonical conflicts. Observability (logging, tracing, audit trails) is necessary for debugging and trust. Determinism is more important in live session workflows than in preparation workflows. Graceful degradation ensures partial functionality when components fail, especially during live play.

### State and Data Management
A canonical source of truth is required to prevent lore drift and inconsistency across agents. State machines can formalise lifecycle transitions such as Drafted → Validated → Approved → Canonised. Context management determines what information each agent can access, preventing overload and leakage of hidden information. Memory should be segmented into durable canon, working notes, drafts, and temporary session state. Versioning is required to track changes and avoid destructive overwrites. Schema design underpins structured entities such as NPCs, locations, and quests. Idempotency ensures safe retries without duplication or corruption.

### Workflow and Coordination
Task decomposition avoids overloading single agents and supports structured workflows. A supervisor/orchestrator pattern centralises control, delegating tasks and enforcing validation. Review/approval stages ensure generated outputs are not immediately trusted. Human-in-the-loop checkpoints are required for canonical changes. Workflow modes differ between preparation (asynchronous, depth-focused) and live sessions (synchronous, speed-focused). Priority handling shifts system behaviour depending on context (accuracy vs responsiveness).

### Testing and Quality Assurance
Testing should include unit testing of orchestration logic, contract testing of agent interfaces, scenario testing for end-to-end campaign flows, and regression testing for prompt/model changes. Evaluation harnesses and golden test cases support consistent behaviour over time and enable comparison of system iterations.

### Maintainability and Evolution
Modularity ensures components such as agents, validators, and storage systems can evolve independently. Configuration over hardcoding allows flexible tuning of workflows and model usage. Documentation must capture architecture, agent roles, workflows, and state rules. Refactoring should be expected and supported by clean boundaries. External dependencies (models, APIs) should be abstracted behind adapters.

### UX and Cognitive Load
The system should minimise operator burden by handling coordination internally. Progressive disclosure limits visible complexity. Confidence signalling communicates reliability of outputs. Interruptibility and recovery mechanisms are critical for live session usability. The system must support quick overrides and rollback of AI-generated content.

## Architectural Decisions or Options Surfaced

A modular monolith with a central orchestrator is the preferred baseline, providing strong control and simplicity without infrastructure overhead. A workflow engine model is a viable extension for structured, multi-step preparation pipelines with traceability and resumability. Event-driven patterns may be introduced later for optional or asynchronous processes but should not form the core architecture initially. Microservices are not justified at this stage due to operational complexity and limited benefit.

The recommended direction combines a modular monolith, orchestrated workflows, strict validation gates, a canonical state store, and distinct workflow modes for preparation and live sessions. This aligns with goals of maintainability, reliability, and reduced cognitive load.

## Further Research / Links to Explore

Explore workflow engine patterns and task orchestration systems for structuring multi-step processes. Investigate hexagonal architecture for isolating external dependencies such as LLM providers. Review state machine design patterns for managing campaign entity lifecycles. Examine observability tooling patterns for tracing multi-step workflows. Study evaluation frameworks for LLM output consistency and regression testing.

## Open Questions / Next Steps

Define the canonical data model and source of truth for campaign state, including schema design and versioning strategy. Formalise agent contracts, including input/output schemas and validation expectations. Design the orchestrator logic, including task routing, failure handling, and workflow composition. Determine validation gate structure and criteria for canonisation. Define differences between preparation and live-session workflows in terms of latency, strictness, and fallback behaviour. Establish observability and logging standards for debugging and auditability.