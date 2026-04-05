# An introduction to Prompt Engineering

- **Date recorded**: 05/04/2026
- **Last updated**: 05/04/2026

This reflection explores structuring task prompts in order to optimise for clearer and constrained outputs that are more useful on the first pass.

---

## Core Prompt Structure

A good agent prompt was distilled into six core components, compressed in order to streamline the prompting process: Role & Objective, Task Context, Constraints, Output Format, Guidelines, and Exit Criteria.

The final understanding was that these map well to a function-definition mental model: the role and goal define what the function is for, task context provides the relevant inputs, constraints and guidelines shape behaviour and limit drift, output defines the expected return structure, and exit criteria define what counts as success.

### Reusable Prompt Template

```
Role & Goal:
Task Context (state/input):
Constraints (scope/rules): 
Output (deliverable/structure):
Guidelines (ask/proceed/avoid):
Exit Criteria:
```

## Prompt Structure

A key idea was that prompt quality is less about wording style and more about reducing implicit intent (assumptions the user has but didn't state). If multiple valid interpretations exist, that ambiguity should be treated as implicit intent and clarified in the prompt where it matters.

A high-level request may communicate a goal, but if the implied constraints, structure, or quality bar are left unstated, the model fills gaps inconsistently. Better prompts therefore make the executable shape of the task explicit rather than relying on the model to infer what the human intended.

## Open Questions / Next Steps

* Prompt pattern types (different tasks benefit from different shapes).
* Context engineering (compressing, ordering, scope)
* Prompt debugging (determining failure causes and modes)
* Evaluation methods (defined quality rubrics, repeatable tests, variants)
* Model differences
* Tool prompting
