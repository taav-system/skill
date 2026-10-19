---
name: taav-git-prepare-push
description: Repair and validate TAAV .NET and Flutter changes, create local dev commits, pull and resolve remote changes, and revalidate everything until ready for push; request explicit approval before pushing.
metadata:
  short-description: Repair, commit, pull, and prepare validated changes for push
---

# TAAV Git preparation and push approval

Activate only when explicitly invoked as `$taav-git-prepare-push` or when the user directly requests this Git preparation workflow. Invocation starts the workflow but is not approval for behavioral code changes. In an orchestrated task, use the already approved plan for its defined scope. In a standalone task, perform read-only discovery first and get explicit approval of a concrete plan before code edits, tests/builds, or local test setup. Staging, local commits, fetching, pulling, merging, and conflict resolution are authorized only when included in the approved task scope. Complete every authorized preparation step before requesting permission for push. Never push automatically, even if the initial task includes a push request: first present the final validated commits and destination, then obtain explicit approval for that prepared result.

This skill is self-contained. Read applicable repository instructions and architecture documents. Apply the company's existing .NET/Flutter architecture and Taav Material UI conventions to repairs; inspect existing company components before adding UI. Ask before inventing a missing UI component. Keep repository architecture maps accurate when a repair changes architecture.

When this skill runs as the Git phase of taav-delivery-orchestrator, the orchestrator's explicitly approved plan authorizes only its stated implementation and Git-preparation scope. Do not use this skill's repair loop to expand product behavior or fix unrelated work. A Git-preparation repair that changes behavior, broadens scope, or requires a new business decision must pause for a revised item-level proposal and user approval before that repair. Routine mechanical repairs needed to validate the approved change may proceed within the authorized scope. The plan approval does not authorize push; push always requires separate approval of the concrete prepared result.

## Discover scope and branch

Discover the requested repositories using `git rev-parse --show-toplevel`; backend and frontend may have separate histories. Inspect branch, tracking branch, remote, staged/unstaged/untracked changes, outgoing commits, and any merge already in progress. Review changed files and relevant history, not just the diff summary. Preserve unrelated user work and existing staging intent.

This workflow targets `dev`. Do not switch branches automatically. If a requested repository is on another branch, report the mismatch. Use the configured upstream; if none exists, use `origin/dev` only when repository configuration/history establishes it as the intended destination. Ask only if the destination is genuinely ambiguous. Never invent a remote or silently change its URL.

No uncommitted changes does not mean preparation is complete: inspect existing outgoing commits and remote divergence, and integrate/validate as necessary. If there are neither changes nor outgoing commits, report that fact after checking the remote.

## Repair and validation loop

Inspect every candidate file, including untracked files, and run `git diff --check`. Correct workflow-related code, build, analyzer, test, and configuration problems rather than stopping at the first failure. Diagnose the cause, make the smallest justified repair, rerun the failed/focused check, then run the complete required validation. Continue while a concrete repair is available; do not impose an arbitrary retry count or claim success merely because retries ended.

- Fix the implementation when a test reveals a defect. Add regression coverage for a bug. Change test expectations only when supported by the intended behavior and authorized by applicable instructions; invocation alone does not authorize weakening tests. Never delete, skip, disable, mock away the behavior under test, or lower assertions/coverage merely to get a pass.
- Follow existing test frameworks, fixtures, repository architecture, and API contracts. Do not install/change SDKs or rewrite dependency versions just to force a pass. Resolve demonstrated dependency/configuration issues in scope using the documented toolchain.
- Build warnings and Flutter analyzer info/warning lint diagnostics are non-blocking; record counts/categories. A nonzero analyzer exit caused only by lint is also non-blocking. Build errors, analyzer errors, failed tests, skipped/unrun required tests, or required build failures prevent readiness.
- Review generated files, machine-specific paths, local overrides, unrelated edits, and secrets. Do not commit unintended runtime settings, credentials, tokens, logs, or build output. Preserve user files; do not silently delete/revert changes. Stage only the complete reviewed task scope, and explain exclusions. If an excluded file makes the required clean integration impossible, isolate the task in a worktree when practical; otherwise report the precise blocker without discarding work.
- Genuine blockers include unavailable safe infrastructure, unresolved requirements, irrecoverable tooling access, or conflicts whose business meaning cannot be established from code/tests/docs. Finish independent preparation before asking for the missing information. Do not guess business behavior or hide blocked checks.

### .NET backend

Identify the actual solution and run `dotnet build <solution>` and complete `dotnet test <solution> -m:1`. The single MSBuild node reduces contention between test projects sharing a database; it does not disable assembly-level parallel tests.

Before database tests or migrations, verify the effective connection resolves to a local isolated test database, separate from application/developer/shared/production data. Do not print credentials. If the local test database is absent, create and initialize it through the repository's migration runner after confirming its target. Local test configuration and necessary migrations are authorized; do not drop/reset existing databases. Use local, untracked overrides where supported. If a safe local SQL Server or verified migration mechanism is unavailable, report the environment blocker. Mock real SMS, OTP delivery, payments, notifications, and external integrations rather than contacting live services.

### Flutter frontend

Run `flutter analyze`, complete `flutter test`, and the documented application build. When a runnable `example` has web as a supported target, build from it using `flutter build web --release`; do not invent an application target for a reusable package. Use the documented Flutter SDK and inspect any files tooling changes.

For source repairs affecting behavior, manually verify the affected flow through the available browser/emulator/computer-use UI in an isolated local environment in addition to automated tests. Use repository/project login and runtime instructions. If unavailable, report that check as blocked, not passed. Do not publish just to obtain a test runtime.

## Commit before pull

Complete required validation in all requested repositories before creating the initial commits. Recheck status, stage the reviewed task scope, inspect the staged diff, and commit locally in each repository that needs a commit. Use one coherent initial commit per repository unless the changes clearly require separate commits.

Commit subject and body must be entirely English and follow Conventional Commits. Describe actual changes; do not claim unrun tests passed. Do not append a ticket/sequence number or request one unless the user explicitly asked for it. Do not amend, rebase, cherry-pick, tag, force-push, or reset history automatically.

## Pull, resolve conflicts, and validate again

After the initial local commits, fetch the intended remote for every requested repository and inspect divergence. Pull/integrate the intended remote `dev` using merge semantics (`git pull --no-rebase <remote> dev` or fetch followed by an explicit merge of the observed remote branch). Do not rely on a globally configured pull strategy and do not merge unrelated histories. Skip a content-changing pull when already up to date.

Resolve conflicts by reading both sides, surrounding implementation, tests, and remote commit intent. Preserve the intended local change and incoming behavior together. Do not blindly choose ours/theirs for all conflicts. Repair downstream build/test regressions and check for conflict markers in relevant tracked source. If intent is genuinely ambiguous, preserve the merge state and ask for that decision; do not abort/reset automatically.

If Git requires completing the merge commit before running checks, complete it locally; never push it until validation passes. Otherwise validate the resolved tree before completing the merge. After integration, rerun full required validation for all requested repositories against the final combined source; pre-pull results are stale. Fix failures and create additional local commits for repairs. A clean fast-forward or no-op pull needs no artificial extra commit. Use an English Conventional Commit message for merge commits where controllable.

Any fix or commit that changes tested source invalidates affected earlier results. Revalidate until the final HEAD is the checked source, the reviewed task work is committed, no merge remains unresolved, and no required check is failing, skipped, blocked, or unrun. Respect repository requirements for validation in a clean worktree. Re-fetch before declaring ready; if remote dev advanced, integrate and validate again. A blocker in one requested repository prevents declaring the multi-repository operation ready.

If an earlier repository was committed before a later failure, report actual partial results and hashes. Never falsely state nothing was staged/committed. Do not roll back successful local commits automatically.

## Final push approval

When preparation is complete, present in Persian the repositories, remote/branch destinations, final HEAD hashes, outgoing commit summaries, integration/conflict repairs, exact check outcomes, warning/lint counts, manual checks, and working-tree/divergence status. State explicitly that no repository has been pushed. Ask for push approval only now, with the concrete prepared result ready for review.

After explicit approval of that result, recheck branch, status, HEAD, and remote divergence. If source or destination changed, finish integration/repair/validation and present the new prepared hashes for approval before pushing. If nothing material changed, push only the approved commits using a normal push; never force. Verify remote hashes and synchronization afterward. Precheck every requested repository before pushing any; if a push nevertheless fails partway through, report exactly which succeeded and which failed without resetting published history.

All user-facing progress and final reports are Persian; preserve command names, paths, hashes, and English commit messages as needed. Never expose secrets.
