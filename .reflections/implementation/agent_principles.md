
# Principles of agent development

- **Date recorded**: 05/04/2026
- **Last updated**: 05/04/2026

This reflection is a guideline for how to introduce effective agent files into a multi-agent workflow.

---

## Agent File Design Principles

Agent files should remain modular and task-specific, avoiding inclusion of repo-wide context or unrelated instructions.

Shared project context should instead be placed in `.github/copilot-instructions.md`. Adhering to this enables agent files to be reused both in Copilot and in GitHub Actions without introducing noise or unintended behaviour.

See [.reflections\agent_orchestration_1.md](https://github.com/newwby/Tabletop-Engine/blob/main/.reflections/orchestration_1.md) for further information ont his approach.

### Developer Notes

Whilst it would be cleaner to include developer notes inside agent files, they are not reliably isolated from the model even whilst commented out. Comments in Markdown or YAML are still part of the text context and may be read or influence the model; if these comments conflict with instructions they will confuse the model.

The best practice is to externalise developer notes into a developer guide for agents. This provides a clean separation behavioural instructions and internal design notes.

