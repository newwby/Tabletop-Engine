# Contributing to Tabletop Engine

This project is in early development. Most contribution guidelines, workflows, and processes are not yet finalized.

## Current Status

The project is focused on establishing:
- Core architecture and agent orchestration
- MVP deliverables (see [design goals](.reflections/implementation/design_goals.md))
- Engineering principles and patterns (see [decisions.md](decisions.md))

## How to Contribute (for now)

### Reporting Issues
- Check existing issues first to avoid duplicates
- Provide clear descriptions and context
- For feature requests, explain the use case

### Questions and Discussion
- Use GitHub issues for questions about the project
- Use the **Discussions** tab for lightweight, lower-detail conversations that do not need issue tracking
- Use issues when work should be tracked with clear actions or follow-up
- See the [GitHub primer](docs/guides/github_primer.md) if new to GitHub

### Code Contributions
Since the architecture is still being defined, please **open an issue first** before investing significant work. This helps ensure alignment with project direction.

## Development Principles

The project follows established software engineering principles:
- **YAGNI** - features implemented as needs demand them
- **Modular Monolith** - clear separation without distributed complexity
- **Human-in-the-loop** - validation gates for critical decisions

See [engineering principles](.reflections/implementation/engineering_principles.md) for details.

## Branch Strategy

1. `main` is the default branch and should always remain stable and presentable.
2. Direct commits to `main` are not permitted.
3. New work is completed in short-lived branches.
4. Changes to `main` must go through a pull request.
5. Before merging a pull request, the working branch should be updated with the latest `main` to reduce merge conflicts.

## Branch Protection

1. Prevent direct pushes and force pushes to `main`.
2. Require pull requests for changes merged into `main`.
3. Merging without review is permitted initially, but approvals should be implemented once additional contributors join the project.
4. No required status checks are enabled at this stage. Markdown linting and related documentation checks can be introduced after MVP.
