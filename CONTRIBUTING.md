# Tabletop Engine Contribution Guide

This project is in early development. Any clarifications required should be directed to <a href="https://github.com/newwby">@newwby</a>.

1. <a href="#current-status">Current Status</a>
2. <a href="#how-to-contribute">How to Contribute</a>
3. <a href="#development-principles">Development Principles</a>
4. <a href="#branch-strategy">Branch Strategy</a>
5. <a href="#branch-protection">Branch Protection</a>

---

<h2 id="current-status">Current Status</h2>

**The project is focused on establishing:**
- Core architecture and agent orchestration
- MVP deliverables (see <a href=".reflections/implementation/design_goals.md">design goals</a>)
- Engineering principles and patterns (see <a href="decisions.md">decisions.md</a>)

**The purpose of this project is to reduce DM cognitive load and preparation time by:**
- Streamlining content generation whilst preserving human involvement
- Creating easily accessed and organised reference files
- Producing deliverables to support running a session

---

<h2 id="how-to-contribute">How to Contribute</h2>

### Reporting Issues
- Check existing issues first to avoid duplicates
- Provide clear descriptions and context
- For feature requests, explain the use case

### Questions and Discussion
- Use GitHub issues for questions about the project
- Use the **Discussions** tab for lightweight, lower-detail conversations that do not need issue tracking
- Use issues when work should be tracked with clear actions or follow-up
- See the <a href="docs/guides/github_primer.md">GitHub primer</a> if new to GitHub

### Contributions
Since the architecture is still being defined, please **open an issue first** before investing significant work. This helps ensure alignment with project direction.

Once your goal is clarified and you are ready to begin work, follow branch strategy rules and create a working branch.

---

<h2 id="development-principles">Development Principles</h2>

The project follows established software engineering principles:
- **YAGNI** - features implemented as needs demand them
- **Modular Monolith** - clear separation without distributed complexity
- **Human-in-the-loop** - validation gates for critical decisions

See <a href=".reflections/implementation/engineering_principles.md">engineering principles</a> for details.

---

# Branch Rules

<h2 id="branch-strategy">Branch Strategy</h2>

1. `main` is the default branch and should always remain stable and presentable.
2. Direct commits to `main` are not permitted.
3. New work is completed in short-lived branches.
4. Changes to `main` must go through a pull request.
5. Before merging a pull request, the working branch should be updated with the latest `main` to reduce merge conflicts.

<h2 id="branch-protection">Branch Protection</h2>

1. Prevent direct pushes and force pushes to `main`.
2. Require pull requests for changes merged into `main`.
3. Merging without review is permitted initially, but approvals should be implemented once additional contributors join the project.
4. No required status checks are enabled at this stage. Markdown linting and related documentation checks can be introduced after MVP.
