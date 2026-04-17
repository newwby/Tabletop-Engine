# Tabletop Engine
### Designing a Structured AI System for Tabletop RPG Development

---

## Contents

- [Overview](#overview)
- [Design Principles & Architecture Direction](#design-principles-architecture-direction)
- [Key Documents](#key-documents)

## Key Documents

- **[Working to-do list](https://github.com/newwby/Tabletop-Engine/blob/main/todo.md)** - _living document, broadly covers tasks for each phase._
- **[Architectural Decisions](https://github.com/newwby/Tabletop-Engine/blob/main/decisions.md)** - _Details confirmed design decisions and rationale._
- **[GitHub guide for new users](https://github.com/newwby/Tabletop-Engine/blob/main/docs/guides/github_primer.md)** - _GitHub repository structure/issue basics, for non-technical contributors._
- **[Contribution guide](https://github.com/newwby/Tabletop-Engine/blob/main/CONTRIBUTING.md)** - _Guide for contributors to the project._
- **[Project context docs](https://github.com/newwby/Tabletop-Engine/blob/main/docs/)** - _agent campaign and development context_
- **[Project reflection docs](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/.contents.md)** - _personal reflections and decision logic_
- **[Devlogs](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/.devlogs/)** - _(WIP) directory containing record of devlogs for the project._


---

## Overview

This project explores how **software engineering principles** can be applied to tabletop RPG (TTRPG) workflows through a **multi-agent AI system**. This repository is intended first and foremost as a **system design project** utilising GitHub Copilot to orchestrate agent development and narrative content generation. Over time this repository will additionally contain a sample campaign generated with the developed workflow and playtested alongside supporting system deliverables.

### Goals

- Reduce session preparation time whilst maintaining human creative control.
- Support sessions through preparation deliverables (primers, references, structured files), reducing DM cognitive load.
- Simplify session organisation and access to reference files.
- Maintain consistency in long-running campaigns through versioned canonical state.

**MVP Focus**: Preparation workflow. Live session support is a post-MVP consideration.

---

## Design Principles & Architecture Direction

The system uses a **hierarchical planner-worker architecture** with specialized AI agents:

- **Orchestrator**: Single entry point that decomposes tasks and delegates to specialist agents
- **Worker Agents**: Narrow specialists (Continuity, Prep, Encounter, Lore, Documentation, Historian)
- **MCP Tooling**: Campaign data, rule references, and shared capabilities

Orchestration happens through **GitHub Copilot subagents** (VSCode) with context isolation per agent. The human DM maintains oversight through validation gates for canonical changes.

### Core Principles

- **Modular Monolith** - clear separation without distributed system complexity
- **Orchestration over Flat Swarms** - structured delegation prevents responsibility confusion
- **Separation of Concerns** - distinct agents for generation, validation, session support
- **Single Responsibility Principle** - one clear, bounded role per agent
- **Human-in-the-loop decision making** - checkpoints for canonical changes
- **Canonical Truth** - validation gates prevent unreliable outputs becoming canon
- **Long-term maintainability** - versioning, observability, clear contracts

### Implementation

Agents are configured via `.agent.md` files with VSCode subagent orchestration. Shared context in `.github/copilot-instructions.md`, agent-specific instructions in individual agent files. See [decisions.md](https://github.com/newwby/Tabletop-Engine/blob/main/decisions.md) for detailed architectural decisions.

---
