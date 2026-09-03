# RulesLLM

Canonical reusable rules template for software projects developed with LLM assistance.

The goal of this repository is to keep the development process simple, safe, observable, and economical without reducing implementation quality.

## Usage

1. Copy `PROJECT_RULES.md` into the project's instruction set or repository documentation.
2. Fill only the `Project-Specific Overrides` section with facts that are truly specific to that project.
3. Keep durable architecture, decisions, testing instructions, and current project state in the project repository rather than in chat history.
4. Add new process rules only after a real recurring problem has been observed. Do not add rules for hypothetical future problems.
5. Project-specific rules may override this template only when the override is explicit and justified by the project.

## Files

- `PROJECT_RULES.md` — reusable development rules and project-specific override template.

## Design Principle

Use the smallest process that reliably produces the required result:

`ChatGPT -> Codex -> OMC -> SJC`

This is not a mandatory pipeline. Each layer is used only when the task actually needs it.
