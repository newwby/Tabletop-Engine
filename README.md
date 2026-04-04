# Tabletop Engine
### Designing a Structured AI System for Tabletop RPG Development

---

## Overview

This project explores how **software engineering principles** can be applied to tabletop RPG (TTRPG) workflows through a **multi-agent AI system**. This repository is intended first and foremost as a **system design project** utilising GitHub Copilot to orchestrate agent development and narrative content generation. Over time this repository will additionally contain a sample campaign generated with the developed workflow and playtested alongside supporting system deliverables.

### Goals

- Reduce DM cognitive load during sessions through session deliverables.
- Reduce session preparation time whilst maintaining human creative control.
- Simplify session organisation and access to reference files.
- Maintain consistency in long-running campaigns.

---

## Design Principles & Architecture Direction

The system is composed of:

- Multiple specialised AI agents operating in different layers.
- Clearly defined responsibilities per agent, with minimal overlap.

Agents do not operate independently, they are orchestrated by the human DM who acts as project architect.  AI agents are treated as system components, not tools.

### Core Principles

- **Separation of Concerns**
- **Single Responsibility Principle**
- **Modularity**
- **Human-in-the-loop decision making**
- **Iterative output over time and controlled evolution**
- **Long-term maintainability**

---
