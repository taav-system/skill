---
name: taav-delivery-orchestrator
description: Coordinate company .NET and Flutter feature or bug delivery through an explicitly approved plan, development and testing, then local Git preparation, with separate approval before push. Use for the complete managed delivery workflow.
metadata:
  short-description: Plan, obtain approval, develop, test, and prepare Git delivery
---

# TAAV delivery orchestration

Manage company .NET and Flutter work in this order: read-only investigation, user-approved plan, development and verification, Git preparation, then separate push approval. Write user-facing plans, progress, questions, and reports in Persian. Keep the user's requested scope throughout the workflow.

## Required skills

Before presenting the plan, locate and read both complete skill files:

- [taav-dotnet-flutter-dev](../taav-dotnet-flutter-dev/SKILL.md): implementation, architecture maps, automated and manual testing, company UI rules, and project-specific rules including First Unit.
- [taav-git-prepare-push](../taav-git-prepare-push/SKILL.md): repair and validation, local commits, pull/merge, conflict resolution, final revalidation, and push approval.

Use the installed skills directory to resolve these names if relative paths are unavailable. Do not substitute remembered summaries or obsolete skill names. If either required skill is missing or unreadable, report which dependency is missing and stop the managed workflow. Follow both skills throughout repairs, including repairs made during Git preparation; entering the Git stage does not suspend development, architecture, UI, or testing rules.

## Phase 1: investigate and present the plan

Before approval, perform only read-only investigation. Identify relevant repositories and project, read applicable AGENTS.md and existing architecture documents, inspect source and tests, and inspect Git branch/status/history/configuration. Understand the reported defect or feature, frontend-to-backend flow, intended behavior, and existing implementation. Distinguish observed facts from hypotheses. If important information is unavailable, explain the uncertainty rather than inventing it.

Do not edit product/source files, create architecture maps, restore/install dependencies or plugins, run tests/builds, start/stop services, migrate or mutate databases, stage/commit, fetch/pull/merge, or push before plan approval. These activities belong to the approved execution phases. Missing architecture maps are a planned deliverable, not an exception to this gate. Read-only inspection and commands are allowed only when they do not mutate files, caches, services, or databases.

### Required plan detail

For a request containing multiple bugs, flows, requirements, or business behaviors, investigate and plan each item separately. Do not replace item-level proposals with a generic promise to "review and fix everything." For each item, include:

- **Item and expected behavior:** identify the bug/requirement and restate the desired outcome or acceptance criteria.
- **Evidence and diagnosis:** summarize the observed current behavior and relevant code/test/documentation evidence. Label confirmed facts, hypotheses, and unknowns distinctly; do not present an unverified root cause as fact.
- **Proposed change:** state the specific behavior or logic to change and why this approach addresses the need. Identify likely repositories, layers, and files when read-only inspection supports that level of detail. If exact files or the cause are not yet known, say what remains to be inspected and avoid inventing paths or solutions.
- **Verification:** list the regression tests, other automated checks, and manual flow/state checks that will demonstrate this item is fixed, with success conditions.
- **Decision or dependency:** call out any unresolved business choice, external dependency, or environment requirement that needs the user's decision. Do not silently choose product semantics.

Keep each proposal concise but specific enough that the user can tell what behavior will change before authorizing implementation. Group items only when they demonstrably share one root cause and one implementation; still state the expected outcome and verification for every grouped item.

Use a separate compact subsection per item, for example: **Need:** … **Observed:** … **Proposal:** برای رفع این مورد پیشنهاد می‌کنم … **Affected areas:** … **Acceptance/checks:** … **Open decision:** …. A table is also suitable when it stays readable; do not compress away evidence, uncertainty, or the proposed behavior.

After the item-by-item proposals, summarize shared implementation work, architecture/documentation changes, test/runtime setup, repository and Git scope, and material risks or limitations. Identify in-scope and out-of-scope items. The user may approve the whole plan or a clearly identified subset; only approved items and their necessary supporting changes may proceed.

If read-only investigation cannot produce a sound solution for an item because a business rule is unresolved, present the competing interpretations and ask the user to choose before finalizing that item's plan. Continue independent investigation of other items where useful. If the technical diagnosis alone remains uncertain, state the uncertainty and propose a bounded diagnostic/implementation path with a clear stop point if evidence changes the behavior or scope.

### Shared plan summary

The overall plan must also include:

- Understanding of the request and expected behavior or acceptance criteria.
- Findings and likely root cause for a bug, with remaining uncertainty identified.
- Affected projects, repositories, main layers/files, and relevant architecture/company UI constraints.
- Proposed implementation and any architecture-document updates or migration/runtime setup.
- Regression and other automated tests, manual end-to-end flows, and conditions for success.
- Git scope: intended repositories, dev branches and known destinations, local commit, pull/merge and conflict repair, complete revalidation, and a separate approval before push.
- Any material business decision or environment dependency needed to proceed.

Ask explicitly for approval of the presented plan and wait. A broad request to fix, an itemized request, invocation of this skill, presenting a plan, silence, or elapsed time is not approval. The approval must clearly cover the plan or named subset. Do not begin coding, architecture-map creation, tests/builds, setup, service operation, or data mutation until approval is unambiguous. Once approved, execute the approved scope without asking the user to reconfirm each item, routine technical choice, test, or workflow step already included in the plan. Choose ordinary implementation details using repository conventions and engineering judgment, then report them. Ask again only when a real blocker needs the user's input or new evidence would materially change approved behavior, scope, repository/destination, or another consequential decision. Answer user questions or revise the plan while waiting; if the user is unsure, clarify or revise rather than treating uncertainty as approval. When the user requests plan changes, present the revised plan and obtain approval for it. Do not fabricate a need for a particular UI planning mode; a clear plan in chat is sufficient.

## Phase 2: implement and verify the approved plan

After explicit approval, carry out the plan under taav-dotnet-flutter-dev. Approval of the plan including its Git scope authorizes that scoped local Git preparation later; do not ask again just to stage, commit, pull, or resolve ordinary conflicts. It does not authorize push.

Create missing architecture maps now, implement the feature/fix, write appropriate regression tests, perform required automated checks, and exercise the relevant manual flows. Use the correct project-specific runtime/login/database rules from the development skill. Preserve unrelated user work. Record actual check commands, outcomes, and tested source state.

Investigate and fix routine build/test failures within the approved intent without repeatedly seeking permission. Never weaken tests or change product semantics merely to obtain a passing result. Observe any specific approval requirements in the development skill and repository instructions, such as permission to create a missing UI component or change protected test assertions.

If new findings require a material change in scope, business behavior, architecture, repository/destination, or a previously unapproved consequential action, pause the dependent work, explain the evidence, propose the specific revised item-level plan, and obtain approval before proceeding with that change. A focused repair or ordinary conflict resolution that preserves the approved intent does not require a revised plan. If genuinely blocked, report the precise missing decision or environment requirement and complete independent authorized work where possible; do not misreport blocked checks as passed.

Proceed to the Git phase when implementation and required development verification are complete. Do not hand unfinished implementation to the Git phase as a substitute for finishing development.

## Phase 3: prepare Git delivery

Execute taav-git-prepare-push in the approved repositories. This orchestrator's approved Git plan is the direct user request for that workflow, even when that skill is configured for explicit invocation only. Read and follow the actual Git skill; merely mentioning its name is insufficient.

Complete validation and necessary in-scope repairs, create initial local commits, pull the intended remote dev branches using merge semantics, resolve conflicts preserving both intended behaviors, and revalidate the final integrated source. Apply development rules to every repair. Create further commits when needed; do not invent commits for a no-op pull. Refresh any manual verification whose relevant behavior changed during integration.

Do not stop after the first local commit, after pulling, or at a routinely repairable test failure. Complete all preparation required by the Git skill before requesting push approval. A genuine blocker or materially changed plan follows the preceding phase's escalation rules. Report actual partial commits/merge status if preparation is blocked; never discard work or claim nothing was committed when commits exist.

## Phase 4: request approval of the prepared push

Once all requested repositories are ready, present the completed result, final HEAD hashes, remote/branch destinations, outgoing commits, integration repairs, automated/manual check outcomes and limitations, and working-tree/divergence status. Explicitly state that nothing has been pushed. Ask for approval to push this concrete prepared result and wait.

Plan approval never doubles as push approval. Only after the user approves this prepared result, perform the Git skill's final HEAD/status/remote checks and normal push. If those checks reveal material source/destination changes, prepare and validate the new result and obtain renewed push approval as required by the Git skill. Never force-push or treat approval for one destination/result as approval for another. Verify and report actual post-push synchronization or any partial failure.

## Continuing across messages

Track the current phase, latest plan, explicit approval state, repositories, completed checks, final validated hashes, and pending blockers in the conversation. Preserve approvals when continuing the same approved scope; do not restart approval gates after a context compaction. Conversely, never infer an approval from a summary that does not establish it. A user correction updates the active task; a cancellation stops it.
