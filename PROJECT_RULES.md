# LLM-Assisted Project Rules

## 1. Purpose

These rules exist to produce working software with the minimum process required for reliable results.

Do not optimize the development workflow for its own sake. Process and internal tooling must solve observed recurring friction, a meaningful reliability or safety risk, recurring manual work, or a capability gap that existing tools cannot reasonably cover.

## 2. Communication and Documentation

- Technical documentation stored in the repository is written in technical English.
- User-facing communication is written in Russian unless explicitly requested otherwise.
- User-facing updates should explain state, decisions, risks, and the next meaningful step without unnecessary low-level detail unless deeper technical detail is requested.
- Important technical knowledge must not exist only in chat history. Persist durable architecture, decisions, testing instructions, and project state in the repository.

## 3. Roles

### User

The user defines product goals and makes decisions that materially affect product direction, architecture trade-offs, risk acceptance, credentials/access, or subjective final validation.

Do not use the user as a manual transport layer between ChatGPT, Codex, OMC, Git, logs, and SJC when available tools can perform the transfer or verification directly.

### ChatGPT

ChatGPT acts as co-architect, analyst, planner, reviewer, and technical partner.

Use ChatGPT directly for architecture, analysis, research, planning, documentation, review, troubleshooting, and small technical tasks that do not justify substantial repository execution.

When connected repository tools are sufficient, ChatGPT may inspect repository state directly for analysis or review without launching Codex.

ChatGPT should challenge materially weak solutions instead of merely agreeing with them.

### Codex

Codex executes bounded implementation missions inside a repository.

Use Codex when the task requires meaningful repository execution such as implementation, substantial codebase exploration, automated changes, or repository-level verification that is not efficient to perform directly in ChatGPT.

### OMC

OMC is the orchestration layer for complex repository implementation.

Use OMC when the work materially benefits from multiple dependent bounded missions, parallel execution, or a coordinated implementation-test-review flow.

Do not use OMC when one bounded Codex mission is sufficient.

### SJC

SJC is the execution observability and lifecycle layer for long-running or unattended AI jobs.

Use SJC for state tracking, progress reporting, durable notifications, result reporting, quota waiting, and automatic continuation when execution can resume.

SJC must not become an architecture engine, task planner, or duplicate AI orchestrator.

## 4. Task Routing

Choose the smallest execution path that can reliably complete the task.

### ChatGPT-only

Use when the task is primarily architecture, analysis, research, planning, documentation, review, explanation, troubleshooting, or a small technical task that does not require substantial repository execution.

### Direct Codex

Use when:

- the architecture or intended behavior is sufficiently clear;
- the task can be expressed as one bounded mission;
- meaningful repository execution and verification are required.

### OMC + Codex

Use when:

- implementation naturally splits into multiple dependent missions;
- parallel work provides real value;
- coordinated implementation and review materially reduce risk or manual work;
- one Codex mission would otherwise become too large or ambiguous.

Do not route a task through additional layers merely because those tools are available.

## 5. Architecture Before Substantial Implementation

Before substantial architectural, behavioral, cross-cutting, migration, security-sensitive, or difficult-to-reverse changes, resolve the direction in ChatGPT first.

Implementation should begin only when the chosen approach, important constraints, material risks, and acceptance criteria are sufficiently defined.

Do not make Codex rediscover or renegotiate settled product and architecture decisions without evidence that those decisions are invalid or incompatible with the repository state.

Experiments may use a lighter process when the cost of failure is low.

## 6. Mission Contract

Every substantial Codex or OMC mission must define at minimum:

- **Goal** — the intended outcome;
- **Scope boundaries** — what is in scope and what must not be changed;
- **Acceptance criteria** — how success is determined;
- **Required evidence** — what must be shown or verified;
- **Stop conditions** — when the agent should stop instead of expanding or repeatedly patching the task.

Add compatibility, architecture, safety, product, or other constraints when they materially affect implementation.

Avoid open-ended instructions such as "improve everything necessary" or "fix whatever you find" unless broad exploration is explicitly the mission.

Agents may inspect enough of the repository to validate implementation assumptions. Exploration must support the mission rather than silently expanding it.

## 7. Model and Reasoning Economy

Choose the minimum model capability and reasoning effort that can reliably complete the mission without reducing result quality.

Typical guidance:

- mechanical edits, formatting, straightforward documentation: low effort;
- normal feature implementation and understood bugs: medium effort;
- difficult unknown bugs, concurrency, migrations, security, data-loss risk, or high-impact review: high effort.

Use stronger reasoning only where uncertainty or consequence justifies it. Do not run an entire workflow at maximum effort when only one stage requires it.

Quota savings must never be achieved by knowingly lowering required implementation or verification quality.

## 8. Context Economy

Keep durable project context in the repository and provide agents with just-in-time context for the current mission.

Do not repeatedly send the entire project history when the task needs only a subset of it.

Prefer repository artifacts such as architecture documents, decision records, test instructions, and current-state notes over reconstructing context from chat memory.

## 9. Git Safety

Substantial AI implementation must use an isolated branch, worktree, or equivalent safe workspace.

Do not make substantial implementation changes directly on the stable `main` working copy.

Before accepting or merging work, inspect the actual repository state and relevant diff.

An AI statement that work is complete or tests passed is not evidence by itself.

## 10. Verification

Verification must be proportional to the task and based on factual evidence.

Relevant evidence may include:

- branch and commit state;
- actual diff;
- automated tests;
- lint or type checks;
- runtime checks;
- generated artifacts;
- logs;
- visual verification for user-facing behavior.

Run the checks required to establish confidence in the changed behavior. Do not add unrelated verification solely to satisfy process ceremony.

If behavior changes materially, update the relevant repository documentation in the same workstream.

## 11. Review and Stop-Loss

Use independent LLM review when the expected reduction in risk justifies the additional quota and time.

The normal limit is two review/fix waves:

1. review the implementation and identify required fixes;
2. apply the fixes and perform final verification or final independent review when justified by risk.

If fundamental problems remain after the second wave, stop iterative patching. Return the evidence to ChatGPT for diagnosis, replanning, or architectural reconsideration before starting a new bounded mission.

If repeated AI attempts fail to solve the same underlying problem, stop the execution loop even before the review limit is reached. Collect the evidence, identify what remains unknown, and materially change the diagnosis or plan before spending more quota.

Use cross-model review or Best-of-N primarily for high-risk, expensive-to-reverse, or architecturally important decisions.

## 12. SJC Usage

Use SJC when Codex or another LLM job is expected to be long-running, unattended, quota-sensitive, or requires reliable progress/terminal notification.

SJC responsibilities may include:

- authoritative execution state;
- progress and terminal reporting;
- durable notification delivery;
- result artifacts;
- quota state;
- pause on quota exhaustion;
- automatic resume when execution becomes possible again.

SJC is an observability and lifecycle layer, not another reasoning stage. Do not extend it into functionality that belongs to ChatGPT, Codex, or OMC unless repeated real-world evidence shows the separation is insufficient.

## 13. Internal Tooling Firewall

Do not build infrastructure for hypothetical future problems.

Before adding a new workflow layer or internal capability, require at least one of the following:

- repeated friction observed in real project work;
- a meaningful reliability or safety risk;
- recurring manual work that automation would materially remove;
- a capability that existing tools cannot reasonably provide.

Prefer improving how existing tools are used over creating another orchestrator or abstraction layer.

After an internal tool reaches the minimum capability needed for real use, prioritize using it on actual product work before expanding it further.

## 14. Human Decision Boundary

Do not ask the user to decide low-level technical details when the choice can be made safely without changing product meaning or accepting significant risk.

Escalate primarily when a decision affects:

- product direction;
- architecture with meaningful trade-offs;
- irreversible or expensive changes;
- security or data-loss risk;
- credentials, permissions, or external access;
- subjective product validation.

## 15. Definition of Done

A substantial task is complete only when:

- the requested scope is implemented;
- acceptance criteria are satisfied;
- relevant verification has passed or failures are explicitly documented;
- the actual diff has been inspected;
- required documentation is updated;
- unresolved risks or deviations are stated clearly;
- subjective or visual checks are performed when the task requires them.

Do not declare completion based only on an agent summary.

## Project-Specific Overrides

Keep this section short. Add only facts that materially change how the generic rules should be applied.

- **Project name:**
- **Repository:**
- **Product goal:**
- **Critical constraints:**
- **Required verification gates:**
- **Project-specific architecture or workflow rules:**
- **Explicit overrides to the generic rules:**
