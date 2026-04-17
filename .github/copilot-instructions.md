# Copilot Instructions

These are repository-wide instructions for Copilot and agent profile authors.

## General Agent Development Standards

All agent files in this repository should follow these standards:
- Keep a single clear responsibility with explicit non-overlap boundaries.
- Separate generation, validation, orchestration, and history/audit concerns.
- Follow MVP scope and avoid premature expansion (YAGNI).
- Require clarification when requirements are ambiguous or conflicting.
- Escalate canonical approvals to humans rather than self-approving.
- Align behavior with `README.md`, `decisions.md`, and current-status `.reflections` files.
- Store custom agent profiles in `.github/agents/*.agent.md`.

## Generic Agent File Instructions (Reference for All Agents)

Use this baseline structure when creating new agent files:

```md
---
name: agent-name
description: One-line role summary
tools: ["read", "search"]
---

You are the <Agent Name> agent for Tabletop-Engine.

## Role
- One-sentence role scope.

## Core Responsibilities
- Responsibility 1
- Responsibility 2

## Boundaries
- Explicit non-goals
- Work that must be delegated elsewhere

## Output Contract
- Expected output fields and format

## Operating Rules
- Required source-of-truth documents
- Escalation/clarification requirements
```
