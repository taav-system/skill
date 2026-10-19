---
name: taav-delivery-orchestrator
description: Coordinate approved .NET and Flutter feature or bug work from investigation through implementation, verification, and local Git integration. Supports local commits, pulls, merges, and conflict resolution; never pushes.
metadata:
  short-description: Coordinate approved .NET and Flutter delivery
---

# TAAV Delivery Orchestration

Coordinate company .NET and Flutter work through four stages: read-only investigation, an explicitly approved plan, implementation and verification, and local Git preparation. Write all user-facing plans, progress updates, questions, and reports in Persian. Keep the work within the user's requested and approved scope. Local commits, fetch/pull, merges, and conflict resolution are allowed only within the approved Git scope. Never push to a remote, even if the user requests or approves it; the user may push manually outside this workflow.

## Required skills

Before presenting a plan, locate and read both complete dependency skills:

- [taav-dotnet-flutter-dev](../taav-dotnet-flutter-dev/SKILL.md): implementation, architecture maps, automated and manual testing, company UI conventions, and project-specific guidance, including First Unit.
- [taav-git-prepare-push](../taav-git-prepare-push/SKILL.md): repair and validation, local commits, pull/merge, conflict resolution, and final revalidation. It never pushes.

If relative paths are unavailable, resolve the dependencies from the installed skills directory. Do not rely on memory, summaries, or obsolete skill names. If either dependency is missing or unreadable, identify it and stop the managed workflow. Follow both skills throughout the work, including repairs during Git preparation; entering the Git stage does not suspend development, architecture, UI, or testing requirements.

## Phase 1: investigate and present the plan

Before approval, perform read-only investigation only. Identify the relevant repositories and projects; read applicable `AGENTS.md` files and architecture documents; inspect source, tests, and Git branch, status, history, and configuration. Understand the reported defect or feature, its frontend-to-backend flow, intended behavior, and current implementation. Separate observed facts from hypotheses. State material unknowns instead of filling them with assumptions.

Do not edit product or source files, create architecture maps, restore or install dependencies/plugins, run tests or builds, start or stop services, migrate or mutate databases, stage or commit changes, fetch/pull/merge, or push before plan approval. These actions belong to the approved execution phases. A missing architecture map is a planned deliverable, not an exception to this gate. Read-only commands are allowed only if they do not change files, caches, services, or databases.

### Required plan detail

For requests covering multiple bugs, flows, requirements, or business behaviors, investigate and plan each item separately. A generic promise to "review and fix everything" is not an adequate plan. For each item, include:

- **Request and expected behavior:** identify the bug or requirement and state the desired outcome or acceptance criteria.
- **Evidence and diagnosis:** summarize the observed behavior and relevant code, test, or documentation evidence. Distinguish confirmed facts, hypotheses, and unknowns; do not state an unverified root cause as fact.
- **Proposed change:** describe the behavior or logic to change and why the approach addresses the request. Name likely repositories, layers, and files only when inspection supports it. If the cause or exact files remain unknown, say what must be investigated; do not invent paths or solutions.
- **Verification:** list the regression tests, other automated checks, and manual flow/state checks, with clear pass conditions.
- **Open decision or dependency:** identify unresolved business choices, external dependencies, or environment requirements that need the user's input. Do not choose product behavior silently.

Keep each proposal concise while making the expected behavior change clear before implementation is authorized. Group items only when evidence shows they share a root cause and implementation; state the expected outcome and verification for each item in the group.

Use a compact subsection for each item, for example: **Request**, **Observed behavior**, **Evidence**, **Proposal**, **Affected areas**, **Acceptance checks**, and **Open decisions**. A table is also suitable if it remains readable. Do not omit evidence, uncertainty, or the proposed behavior to save space.

After the item-level proposals, summarize shared implementation work, architecture/documentation updates, test/runtime setup, repository and Git scope, and material risks or limitations. Identify what is in scope and out of scope. The user may approve the full plan or a clearly identified subset; proceed only with approved items and necessary supporting changes.

If an unresolved business rule prevents a sound proposal, present the competing interpretations and ask the user to choose before finalizing that item. Continue investigating independent items where useful. If only the technical diagnosis is uncertain, state that uncertainty and propose a bounded investigation and implementation path, with a clear stop condition if new evidence changes behavior or scope.

### Shared plan summary

The overall plan must also state:

- Request understanding and expected behavior or acceptance criteria.
- Findings and likely root cause, with any remaining uncertainty identified.
- Affected projects, repositories, layers/files, and relevant architecture or company UI constraints.
- Proposed implementation, architecture-document updates, and any migration/runtime setup.
- Regression and other automated tests, manual end-to-end flows, and their pass conditions.
- Git scope: repositories, `dev` branches/remotes, local commits, pull/merge and conflict resolution, and final revalidation. State explicitly that this skill will not push.
- Any material business decision or environment dependency.

Request explicit approval of the presented plan and wait for it. A broad or itemized request, invocation of this skill, presentation of a plan, silence, or elapsed time does not count as approval. Approval must clearly cover the full plan or a named subset. Until approval is unambiguous, do not code, create architecture maps, run tests/builds, configure setup, operate services, or mutate data. After approval, execute the approved scope without asking the user to reconfirm included items, routine technical choices, tests, or workflow steps. Make ordinary implementation decisions using repository conventions and engineering judgment, then report them. Ask again only if a genuine blocker requires user input or new evidence would materially change approved behavior, scope, repository/destination, or another consequential decision. Answer questions and revise the plan while waiting. If the user is uncertain, clarify or revise the proposal; do not treat uncertainty as approval. Obtain approval for any requested plan revision. A clear plan in chat is sufficient; do not require a particular UI planning mode.

## Phase 2: implement and verify the approved plan

After explicit approval, implement the plan under `taav-dotnet-flutter-dev`. Approval of a plan that includes Git scope also authorizes that local Git preparation; do not ask again to stage, commit, pull, or resolve routine conflicts within that scope. No approval can authorize a push: never run `git push` or an equivalent remote-write operation.

Create any missing architecture maps, implement the approved feature or fix, add appropriate regression tests, run required automated checks, and exercise relevant manual flows. Follow the development skill's project-specific runtime, login, and database rules. Preserve unrelated user changes. Record the actual commands, outcomes, and source revision covered by each check.

Diagnose and fix routine build/test failures within the approved scope without repeatedly seeking permission. Never weaken tests or change product behavior just to obtain a passing result. Follow any additional approval requirements in the development skill or repository instructions, such as approval to create a missing UI component or change protected test assertions.

If new evidence requires a material change to scope, business behavior, architecture, repository/destination, or another consequential action not yet approved, pause dependent work. Explain the evidence, propose a specific item-level revision, and obtain approval before making that change. A focused repair or routine conflict resolution that preserves the approved intent does not require a revised plan. If blocked, identify the missing decision or environment requirement and complete independent authorized work where possible. Never report a blocked check as passed.

Begin the Git phase only after implementation and required development verification are complete. Do not use Git preparation as a substitute for finishing development work.

## Phase 3: prepare Git delivery

Run `taav-git-prepare-push` in the approved repositories. The orchestrator's approved Git plan counts as the direct request to run that workflow, even if the Git skill requires explicit invocation. Read and follow the current skill file; mentioning its name is not sufficient.

Complete validation and necessary in-scope repairs, create initial local commits, pull the intended remote `dev` branches using merge semantics, resolve conflicts while preserving intended behavior on both sides, and revalidate the integrated source. Apply the development skill to every repair. Create additional commits when needed; do not create an artificial commit for a no-op pull. Repeat any manual check whose relevant behavior changed during integration.

Do not stop after the first local commit, after pulling, or at a routinely repairable test failure. Complete all approved preparation and validation before reporting the local result. Handle genuine blockers or material plan changes under the escalation rules above. If blocked, report actual commits and merge status; never discard work or claim that nothing was committed when commits exist.

## Phase 4: report locally prepared result; never push

When all requested repositories are ready, report the final HEAD hashes, remote/branch destinations, outgoing commits, integration repairs, automated and manual check results and limitations, and working-tree/divergence status. State explicitly that no push was attempted and this skill will never push. Do not ask for push approval.

If the user asks for a push, explain that this skill prohibits it and leave the prepared commits local. Do not use GitHub APIs, scripts, GUIs, or other tools to make an equivalent remote update.

## Continuing across messages

Track the current phase, latest plan, approval state, repositories, completed checks, validated commit hashes, and blockers across messages. Preserve approval when continuing the same approved scope; do not restart approval gates after context compaction. Do not infer approval from a summary that does not establish it. Treat a user correction as an update to the active task and a cancellation as a stop instruction.
