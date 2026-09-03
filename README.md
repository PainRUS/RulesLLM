# RulesLLM

Reusable rules template for software projects developed with LLM-assisted workflows.

## Purpose

This repository contains a compact baseline for projects where ChatGPT, Codex, OMC, SJC, Git, and human review may participate in the development workflow.

The template is intentionally opinionated about role boundaries, evidence, quota economy, and stopping conditions, while avoiding project-specific implementation details.

## Canonical Template

Use [`PROJECT_RULES.md`](PROJECT_RULES.md) as the starting point for a project's LLM working rules.

When adopting it:

1. copy the template into the target project's authoritative documentation or project instructions;
2. fill only the `Project-Specific Overrides` section with constraints that materially affect that project;
3. keep durable technical decisions in the target repository rather than in chat history;
4. remove references to tools that the project genuinely does not use instead of inventing placeholder process around them.

The template also defines a mandatory SJC execution contract for long-running, unattended, quota-sensitive, or notification-critical AI jobs. SJC manages execution lifecycle and evidence delivery; it does not replace planning, implementation orchestration, or verification.

## Design Goals

- use the smallest reliable execution path;
- keep architecture decisions separate from implementation orchestration;
- minimize unnecessary model and reasoning cost without lowering required quality;
- prevent agent thrashing and endless review loops;
- keep humans out of routine transport and status work;
- verify results from repository state, tests, runtime, and other factual evidence;
- avoid building internal tooling for hypothetical problems.

## Status

The initial template is under review before adoption as the shared baseline for LLM-assisted projects.
