# Post-Session Input Workflow - GitHub Issue Intake, Summarisation, and Review

- **Date recorded**: 17/04/2026
- **Last updated**: 17/04/2026
- **Status**: Current

This reflection explores how to capture low-friction player feedback after sessions in a structured, reproducible way that supports AI summarisation, campaign state review, and long-term historical record keeping without adding heavy manual bookkeeping for the DM.

---

## Problem Context

A core workflow problem in the project is how to capture live session outcomes from players in a way that is consistent enough for automation while remaining easy enough that players will actually engage with it. This sits alongside atomic file structure and session deliverables as one of the key workflow cornerstones for the system.

The challenge is not simply collecting notes. The system needs a reliable intake point that can feed session summarisation, preserve provenance, support later continuity review, and create a historical record without allowing raw player recollections to directly overwrite approved campaign canon.

The workflow must therefore balance four pressures: low player effort, structured intake, minimal DM administration, and clear separation between draft evidence and approved world state.

## Proposed Direction

A strong candidate workflow is to use a specialised GitHub Issue Form for post-session input. This form can either be filled directly by players through a link or populated manually by the DM from Discord or text responses when direct GitHub participation is unrealistic.

The issue form acts as the standard intake surface for post-session feedback. A GitHub Action can then detect issues created under the relevant template or label and trigger a session-processing workflow. That workflow can prepare a session-specific branch, call a session summarisation step, generate draft session artefacts, and open a pull request for human review.

This preserves a clear split between intake and approval. The issue is the evidence source, the generated artefacts are draft outputs, and the pull request is the review boundary before any approved campaign record is updated.

## Why This Fits the Project Direction

This approach fits the wider project direction well because it supports structured and reproducible intake, preserves an audit trail, and aligns with the repository model where changes should move through branches and pull requests before being treated as accepted state.

It also matches the broader architecture preference for explicit orchestration, validation gates, and historical record keeping. Rather than treating live player input as direct truth, it treats it as source material that can be processed, reviewed, and then either accepted, corrected, or deferred.

Using issues as the intake surface also reduces friction. Players are not asked to edit repository files, understand branch workflows, or manage campaign documentation directly. At the same time, the workflow remains developer-friendly because every submission is timestamped, attributable, and available for later automation.

## Workflow Shape

### 1. Post-Session Intake

A player or DM submits a specialised post-session issue. The form should remain short and mobile-friendly. Suggested fields are:

- session identifier or date
- player name or character name
- favourite moment
- biggest concern, confusion, or correction
- what the character intends to do next
- optional additional notes

This keeps the player contribution short while still producing structured enough input for downstream extraction.

### 2. Intake Detection and Routing

A GitHub Action watches for issues using the relevant template or label, such as `post-session`. When triggered, it identifies the relevant session and starts the intake-processing workflow.

At this stage the workflow should not attempt to update canon directly. It should only prepare draft processing outputs.

### 3. Session Summarisation Step

The summarisation step consumes the raw issue content and transforms it into structured draft artefacts such as:

- session summary
- notable events
- proposed state changes
- unresolved questions
- contradictions or low-confidence statements
- follow-up items for continuity or lore review

This stage should be treated as extraction and proposal generation, not final acceptance.

### 4. Session Branch and Pull Request

The workflow writes draft files into a session-specific branch and opens a pull request for review. This creates a clear review checkpoint and preserves the project rule that approved state changes should not land silently.

The pull request can also reference the original issue or issues as provenance.

### 5. Review and Follow-Up

Once the draft session artefacts exist, the developer can review the output and decide whether continuity, lore, or canon updates are needed. These may happen in the same branch if the scope is small, but should not be assumed to happen automatically in the first version of the workflow.

A structured review step is important because player recollections may conflict, omit details, or overstate uncertain events.

## Architectural Decisions / Options Surfaced

### Option 1 - Player Form as Primary Intake

A structured post-session issue form is the strongest primary intake option because it is reproducible, easy to route through automation, and supports low-friction submissions.

### Option 2 - Manual Message Conversion into Issue Form

Where players are unlikely to engage with GitHub directly, Discord or text responses can be manually converted into the same issue form by the DM. This preserves the workflow shape even when the submission channel varies.

### Option 3 - Automatic Session Processing via GitHub Actions

GitHub Actions are a good fit for routing, branch preparation, pull request scaffolding, and issue state updates. They work well as an event-driven automation layer around the intake and review process.

### Option 4 - Copilot Web Task as Summarisation Engine

Using Copilot web tasks for the summarisation step is attractive but should be treated as an implementation option rather than an architectural dependency. If Copilot invocation is brittle or constrained, the workflow should still survive with a different summarisation runner.

### Option 5 - Automatic Downstream Agent Fan-Out

Automatically triggering continuity or lore agents from intake is appealing but should be deferred initially. Early versions should prefer producing follow-up tasks or recommendations rather than chaining multiple autonomous updates.

## Recommended Decision

The recommended direction is to adopt GitHub Issue Forms as the standard post-session intake surface, then use automation to transform those submissions into draft session artefacts inside a session-specific branch with an immediate pull request for review.

The key rule is that issue intake should produce draft evidence and proposed changes, not direct canon updates. Canon should remain a reviewed and approved layer.

This gives the project a workflow that is:

- low-friction for players
- structured enough for summarisation
- reproducible across sessions
- auditable through GitHub history
- aligned with branch-first review practices
- compatible with future continuity and lore workflows

## Risks and Constraints

The main risk is over-automating too early. If the workflow attempts to classify, summarise, reconcile continuity, update lore, and modify canon in one chain, it will become difficult to debug and hard to trust.

Another risk is over-reliance on a specific execution environment such as Copilot web tasking. If that invocation path proves limited, the workflow should still function with a different summarisation step.

There is also a data quality risk. Player recollections are useful but imperfect. The system should preserve uncertainty and contradictions rather than flattening everything into false confidence.

A final practical risk is participation drop-off if the issue form becomes too long. The form should be kept extremely short and optimised for consistent completion rather than rich narrative detail.

## Implementation Guidance

A sensible implementation order is:

1. define the post-session issue form schema
2. define the output schema for draft session summaries
3. define routing labels and naming conventions
4. implement branch and pull request scaffolding
5. add summarisation automation
6. add issue comment, closure, or status update behaviour
7. add optional follow-up generation for continuity or lore review

This preserves a stable workflow foundation before introducing deeper automation.

## Open Questions / Next Steps

A decision is still needed on whether player submissions should go directly through GitHub or whether DM-mediated transcription from Discord should be the default expected route.

The exact output schema for session draft artefacts also needs to be defined. In particular, the project should determine which session outputs are mandatory, which are optional, and where uncertainty or contradictions should be represented.

A further open question is whether continuity and lore review should occur in the same session branch or be surfaced as separate follow-up work items.

Finally, Copilot web tasking should be validated experimentally before it is assumed to be a reliable orchestration component. If it proves too constrained, the issue intake and branch-review model should remain intact while the summarisation runner is replaced.
