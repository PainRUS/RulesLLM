# LLM-Assisted Project Rules

## 1. Purpose

These rules exist to help produce working software with the minimum process required for reliable results.

Do not optimize the development workflow for its own sake. Internal tooling and process changes must solve observed recurring friction, reliability risk, or meaningful manual work.

## 2. Communication and Documentation

- Technical documentation stored in the repository is written in technical English.
- User-facing communication is written in Russian unless explicitly requested otherwise.
- User-facing updates should explain state, decisions, risks, and the next meaningful step without unnecessary low-level detail.
- Important technical knowledge must not exist only in chat history. Persist durable decisions and project knowledge in the repository.

## 3. Roles

### User

The user defines product goals and makes decisions that materially affect product direction, architecture trade-offs, risk acceptance, credentials/access, or subjective final validation.

Do not use the user as a manual transport layer between ChatGPT, Codex, OMC, Git, logs, and SJC when available tools can perform the transfer or verification directly.

### ChatGPT

ChatGPT acts as co-architect, analyst, planner, reviewer, and technical partner.

Use ChatGPT directly for architecture, analysis, research, planning, documentation, review, troubleshooting, and small technical tasks that do not justify substantial repository work.

ChatGPT should challenge materially weak solutions instead of merely agreeing with them.

### Codex

Codex executes bounded implementation missions inside a repository.

Use Codex when meaningful repository changes, codebase exploration, implementation, or automated verification are required.

### OMC

OMC is the orchestration layer for complex repository implementation.

Use OMC when the work benefits from multiple dependent bounded missions, parallel execution, or a coordinated implementation-test-review flow.

Do not use OMC when one bounded Codex mission is sufficient.

### SJC

SJC is the execution observability and lifecycle layer for long-running or unattended AI jobs.

Use SJC for state tracking, progress reporting, durable notifications, result reporting, quota waiting, and automatic continuation when execution can resume.

SJC must not become an architecture engine, task planner, or duplicate AI orchestrator.

## 4. Task Routing

Choose the smallest execution path that can reliably complete the task.

### ChatGPT-only

Use when the task is primarily:

- architecture or design;
- analysis or research;
- planning;
- documentation;
- review;
- explanation or troubleshooting;
- a small local technical task that does not require substantial repository work.

### Direct Codex

Use when:

- the architecture or intended behavior is already sufficiently clear;
- the task is self-contained enough to express as one bounded mission;
- repository changes and verification are required.

### OMC + Codex

Use when:

- implementation naturally splits into multiple dependent missions;
- parallel work provides real value;
- a coordinated implementation and review workflow is materially useful;
- one open-ended Codex mission would otherwise become too large or ambiguous.

### Architecture First

Before substantial architectural, behavioral, cross-cutting, migration, security-sensitive, or difficult-to-reverse changes, resolve the direction in ChatGPT first.

Implementation should begin only when the chosen approach, important constraints, risks, and acceptance criteria are sufficiently defined.

Experiments may use a lighter process when the cost of failure is low.

## 5. Mission Contract

Every substantial Codex or OMC mission should define:

- **Goal** — the intended outcome;
- **In scope** — what may be changed;
- **Out of scope** — what must not be changed;
- **Acceptance criteria** — how PASS is determined;
- **Required evidence** — what must be shown or verified;
- **Important constraints** — compatibility, architecture, safety, or product constraints;
- **Stop conditions** — when the agent should stop instead of expanding or repeatedly patching the task.

Avoid open-ended instructions such as "improve everything necessary" or "fix whatever you find" unless broad exploration is explicitly the task.

Agents may inspect enough of the repository to validate implementation assumptions, but they should not re-solve settled product or architectural decisions without evidence that those decisions are invalid.

## 6. Model and Reasoning Economy

Choose the minimum model capability and reasoning effort that can reliably complete the mission without reducing result quality.

Typical guidance:

- mechanical edits, formatting, straightforward documentation: low effort;
- normal feature implementation and understood bugs: medium effort;
- difficult unknown bugs, concurrency, migrations, security, data-loss risk, or high-impact review: high effort.

Use stronger reasoning only where uncertainty or consequence justifies it. Do not run an entire workflow at maximum effort when only one stage requires it.

Quota savings must never be achieved by knowingly lowering required implementation quality or verification quality.

## 7. Context Economy

Keep durable project context in the repository and provide agents with just-in-time context for the current mission.

Do not repeatedly send the entire project history when the task needs only a small subset of it.

Prefer repository artifacts such as architecture documents, decision records, test instructions, and current-state notes over reconstructing context from chat memory.

## 8. Git Safety

Substantial AI implementation must use an isolated branch, worktree, or equivalent safe workspace.

Do not make substantial implementation changes directly on the stable `main` working copy.

Before accepting or merging work, inspect the actual repository state and relevant diff.

A statement from an AI agent that work is complete or tests passed is not evidence by itself.

## 9. Verification

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

Do not run unrelated verification solely to satisfy process ceremony.

If behavior changes materially, update the relevant repository documentation in the same workstream.

## 10. Review Policy

Use independent LLM review when the expected value justifies the additional quota and time.

The default maximum is two LLM review/fix waves:

1. implementation plus independent review;
2. requested fixes plus final verification.

If fundamental problems remain after the second wave, stop iterative patching and return the evidence to ChatGPT for diagnosis, replanning, or architectural reconsideration.

A new plan may start a new bounded mission; this rule prevents blind patch loops, not deliberate rework after diagnosis.

Use cross-model review or Best-of-N primarily for high-risk, expensive-to-reverse, or architecturally important decisions.

## 11. Stop-Loss Rule

If repeated AI attempts fail to solve the same underlying problem, stop the execution loop.

Collect the current evidence, identify what is still unknown, reassess the diagnosis in ChatGPT, and create a new bounded mission only after the problem framing changes materially.

Do not spend additional quota on agent thrashing.

## 12. SJC Usage

Long-running or unattended Codex/LLM work should use SJC when observability, notification, quota waiting, or automatic continuation is materially useful.

SJC responsibilities may include:

- authoritative execution state;
- progress and terminal reporting;
- durable notification delivery;
- result artifacts;
- quota state;
- pause on quota exhaustion;
- automatic resume when execution becomes possible again.

Do not extend SJC into functionality that belongs to ChatGPT, Codex, or OMC unless a repeated real-world need demonstrates that the existing separation is insufficient.

## 13. Internal Tooling Firewall

Do not build internal infrastructure for hypothetical future problems.

Before adding a new workflow layer or internal capability, require at least one of the following:

- repeated friction observed in real project work;
- a meaningful reliability or safety risk;
- recurring manual work that automation would materially remove;
- a capability that existing tools cannot reasonably provide.

Prefer improving how existing tools are used over building a new orchestrator or abstraction layer.

After an internal tool reaches the minimum capability needed for real use, prioritize dogfooding it on actual product work before expanding it further.

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

## 16. Guiding Principle

Use the smallest process that reliably produces the required result:

- ChatGPT when ChatGPT is enough;
- Codex when one bounded coding mission is enough;
- OMC when implementation genuinely requires orchestration;
- SJC when execution genuinely requires observability or unattended continuation;
- human intervention only when a human decision is genuinely required.

## Project-Specific Overrides

Keep this section short. Add only facts that materially change how the generic rules should be applied.

- **Project name:**
- **Repository:**
- **Product goal:**
- **Critical constraints:**
- **Required verification gates:**
- **Project-specific architecture or workflow rules:**
- **Explicit overrides to the generic rules:**
