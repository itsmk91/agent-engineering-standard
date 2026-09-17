---
name: software-engineering-build-standard
description: "Technology-independent standard for creating, extending, refactoring, restructuring, reviewing and maintaining software, making architecture, module and state ownership, dependency direction, data contracts, tests, documentation and Git hygiene part of implementation from the start without over-engineering. Use when building a new application or software project, adding or extending a substantial feature or subsystem, cleaning up or reorganizing a repository, changing architecture, persistence, databases, data models, state or target platforms, organizing tests or tooling, reducing technical debt, or reviewing engineering quality. Applies proportionally to small fixes without forcing an architecture review. Explicit task scope, project-specific instructions, accepted ADRs and owner decisions take precedence."
---

# Software Engineering Build Standard

Never build software first and organize it later. Architecture, repository structure, module ownership, dependency direction, testing, documentation, Git hygiene, maintainability and extensibility are part of implementation from the beginning. Working code with poor engineering structure is not complete.

Professional engineering is not maximum abstraction, excessive folders, unnecessary interfaces, premature frameworks or fashionable patterns. The architecture must be proportional to the actual product.

**Goal (§65):** the software works today, its structure explains itself, another engineer can safely change it tomorrow, and the architecture is no more complicated than the product actually requires.

Rule IDs §1–§65 are stable anchors for this standard; findings, plans and handoffs may cite them.

## Precedence (§61)

This standard is general. It does not replace, and it yields to:

- explicit task scope and explicit owner decisions, including the owner's standing instructions that apply across projects;
- repository-local instructions (for example AGENTS.md, CLAUDE.md, contributing, engineering, layout or design guides);
- accepted ADRs and accepted project-specific architecture;
- security requirements.

When a project has a more specific accepted rule — a mandated folder layout, file-size limit, finding format, approval step, commit policy or delivery format — follow the project rule. When a project rule appears unsafe or contradictory, report the conflict with evidence instead of silently overriding it. Commits, pushes and approvals follow the project's and owner's rules: where you may not commit yourself, ask for the baseline or rollback point rather than skipping it.

Narrower technique skills and guides (testing, incremental delivery, interface design, migrations, debugging) complement this standard rather than compete with it: this standard decides placement, boundaries, proportion and gates; they supply technique.

## Size the work before acting (§33–§35, §62–§63)

| Work | Examples | Required depth |
|---|---|---|
| Tiny | typo, small UI adjustment, small isolated bug, simple configuration change | Inspect enough to avoid damage, make the smallest coherent change, run the relevant verification. No audit, no architecture report, no new structure. |
| Substantial | new feature, extension of an existing subsystem | The matching workflow below at a depth that fits; design questions answered before building. |
| Architectural | new application or project, major subsystem, architecture change, database or persistence change, data-model or state-model change, large refactor, repository restructuring, platform expansion, high-frequency or historical data feature | The full workflow; no implementation until the design questions are answered and any Human Gate is passed. |

Project scale sets how much structure is right:

- **Small:** simple repository, few clear modules, lightweight tests, concise README, minimal tooling. Never manufacture enterprise architecture.
- **Medium:** clearer feature boundaries, domain or shared logic, a persistence boundary, organized tests and tools, documentation, platform separation where relevant.
- **Large or long-lived:** architecture documentation, explicit subsystem ownership, dependency direction, data contracts, migration strategy, test architecture, operational tooling, ADRs, release and recovery procedures.

Complexity follows product needs. A declared throwaway prototype or spike is a scope decision: keep it minimal and add no foundation work, but the secret and private-data rules still apply (§27). If a prototype starts becoming the product, say so and treat it as an existing project from then on.

Do not produce a ceremonial architecture report for a small, obvious task, and do not use planning to avoid implementation once the required decisions are resolved.

## Workflows

Substantial work follows this order, with planning proportional to the task (§1):

UNDERSTAND → DESIGN BOUNDARIES → DEFINE STRUCTURE → PLAN → IMPLEMENT → TEST → VERIFY → REVIEW → HUMAN GATE

### Existing project (§2, §34, §58, §63)

Never assume its architecture, and do not start changing files until you understand enough of the real project: repository root, Git state, source layout, entry points, application composition, major features, domain rules, state ownership, persistence, platform-specific code, tests, tooling, documentation, dependencies, build and release path, and its own instructions and ADRs.

INSPECT → AUDIT IF NECESSARY → PLAN → HUMAN GATE IF ARCHITECTURAL → IMPLEMENT IN COHERENT SLICES → TEST → VERIFY → REVIEW

Determine the current architecture before proposing a target. Do not rebuild a functioning project because its structure is imperfect, and never reorganize blindly. Before a major feature, check whether the existing architecture can support it safely; if it cannot, do the minimum necessary foundation work first.

### New project (§3, §34, §63)

REQUIREMENTS → ARCHITECTURE → REPOSITORY FOUNDATION → DATA/STATE MODEL → TEST STRATEGY → IMPLEMENTATION

Before substantial feature development, decide: repository boundary, source structure, application composition, domain strategy, feature strategy, persistence boundary, platform boundaries, state ownership, dependency direction, testing strategy, tooling location, documentation location, generated-artifact policy and Git baseline. Decide where responsibilities belong before the project grows, but create only the minimum foundation the product needs — never dozens of speculative feature files, and no architecture a tiny project does not need.

### Engineering-quality audit or cleanup (§28)

Do not refactor immediately:

AUDIT → CURRENT ARCHITECTURE → FINDINGS → TARGET ARCHITECTURE → HUMAN GATE → INCREMENTAL MIGRATION

Distinguish actual correctness risk, structural debt, maintainability debt, cleanup and optional improvement. Report findings in the §29 format and never inflate severity to justify restructuring.

### Refactoring or restructuring (§20, §21, §54)

Prefer small coherent slices to a big-bang rewrite unless there is a compelling reason. Each slice has one objective, explicit scope, expected behavior change, risk, tests, a rollback point and acceptance criteria. Purely structural work carries ZERO intended behavior change (§54). After each significant slice: IMPLEMENT → VERIFY → REVIEW → HUMAN GATE, and never continue into the next slice automatically when the workflow requires approval. Move modules that are coherent, well tested, appropriately isolated and stable with minimal logic changes; architectural cleanup must never become a hidden product rewrite.

### Defect found in review (§53)

Do not immediately rewrite the surrounding architecture. First isolate the exact defect, its root cause, the affected contract, a reproduction, the required correction and the regression tests; then fix the smallest coherent scope that closes the defect. Record architecture work discovered during the fix separately unless it is necessary for correctness.

## Design questions before substantial building (§4, §34)

Derive the answers from the product, never from an arbitrary folder template:

- What responsibility is being introduced, and where should it live?
- Who owns its state, and who is allowed to depend on it?
- What external systems does it depend on?
- How will it be tested? How will it be persisted?
- What happens if it becomes much larger, and what future work is likely to extend it?

For architectural work, also establish: what exists now; what problem is being solved; which boundary owns it; what dependencies it introduces; what data and state change; the persistence implications; migration or compatibility risks; the rollback point; and whether a Human Gate is required.

## Always-on rules

- **Structure reflects responsibility (§5, §6).** Folders show real responsibility, never a popular template. A composition root wires systems together; it does not implement them.
- **No God files (§7).** Central files such as App, main, index, server, controller or manager must not become the default home of new behavior. Judge by responsibilities and change coupling, not size alone, and never split a cohesive file just to meet a line count.
- **Domain logic has a home (§8, §9, §10).** Product rules stay independent of UI, storage, platform wrappers and global clock access where practical; dependencies point toward stable product logic; each substantial feature has an obvious home; shared rules are not duplicated.
- **Boundaries are explicit (§11–§14, §39–§42, §49).** Persistence, platforms and configuration sit behind intentional boundaries; canonical data contracts, units and time models are defined; every important piece of state has an owner; failures are owned, not scattered as UI messages.
- **Data model before time-based features (§12).** Before movement, telemetry, history, replay, collaboration, event streams, background automation or time-based simulation, verify the data model can carry identity, time, ordering and history.
- **Repository hygiene (§15–§19, §25–§27, §47, §48).** Tests are architecture, tooling is not testing, source is not evidence, Git tells the truth, documentation is part of engineering. Never commit or expose keys, passwords, secrets, tokens or private user or device data.
- **Do not over-engineer (§22).** Without a concrete product need, do not add interfaces for every class, repositories, services or managers for appearance, dependency-injection frameworks, global state libraries, empty future feature folders, dozens of tiny files, fashionable design patterns, microservices, event buses, databases or complex build systems. Every abstraction must solve a real problem of responsibility, dependency direction, testing, change isolation, platform isolation, maintainability or extensibility. Prefer the simplest architecture that remains structurally sound.
- **Realistic roadmap (§23, §58).** Test the architecture against the next realistic roadmap, not hypothetical infinite scale, and build no infrastructure for speculative features.
- **Repository over memory (§59).** Conversation context is not a substitute for repository engineering. Important project knowledge lives in source, tests, documentation, ADRs, Git history and explicit project plans, so the project stays understandable to an agent that never saw earlier conversations.

## Human Gate (§30, §44)

Major architecture decisions belong to the human owner. Never silently decide to:

- replace the architecture;
- change the persistence strategy, data-model semantics or platform strategy;
- restructure the repository at scale;
- break the compatibility of released software (persisted data, public APIs, package or application identity, signing identity, file formats, configuration, user workflows);
- delete important historical or recovery material.

Instead: present the current state → explain the problem → propose the target → explain the trade-offs → explain the migration risk → wait for the Human Gate. Minor implementation choices inside an already approved architecture need no extra approval.

## Stop conditions (§56)

Stop and report instead of improvising when:

- required data is missing, or repository identity is uncertain;
- a migration behaves differently than predicted;
- signing or package identity differs from what was expected;
- private data may be at risk;
- tests reveal an unexplained regression;
- the architecture requires an unapproved major decision;
- a supposedly structural change requires a behavior change;
- evidence contradicts the task's assumptions.

Never "make it work" by destroying data or bypassing safeguards.

## Completion (§31, §32, §60, §64)

Working code alone is not done. Before declaring substantial work complete, explicitly verify the requested behavior, architectural placement, tests, build/type/lint status where relevant, Git state where relevant, documentation impact, known risks, and whether a Human Gate is required. Then apply the definition of done, the self-check and the handoff in [references/review-and-completion.md](references/review-and-completion.md), in proportion to the task. Report any concerning self-check answer with the smallest appropriate correction. Never claim completion for verification that was not performed — say what remains unverified.

## References — load only what the task needs

| File | Load when the work involves |
|---|---|
| [references/architecture-and-boundaries.md](references/architecture-and-boundaries.md) | where code lives, feature or module boundaries, God-file risk, dependency direction, platform wrappers, change coupling, configuration, naming, comments |
| [references/data-state-and-runtime.md](references/data-state-and-runtime.md) | persistence or databases, data contracts, units, time, state ownership, migrations, compatibility, error handling, async or concurrency, performance |
| [references/repository-and-delivery.md](references/repository-and-delivery.md) | tests, tooling, evidence and artifacts, Git and commits, documentation, ADRs, dependencies, generated files, build and release, secrets and private data, dead code |
| [references/review-and-completion.md](references/review-and-completion.md) | findings, reviews, zero-behavior-change refactors, technical debt, definition of done, self-check, handoff |
