---
name: taav-git-prepare-push
description: Prepare TAAV .NET and Flutter changes through repair, validation, local commits, and remote integration. Push only when the user explicitly requests it.
metadata:
  short-description: Validate and integrate; push on explicit request
---

# TAAV Git Preparation

Push only when the user explicitly asks for a push. Plan approval, skill invocation, successful validation, or an implied expectation to deliver does not authorize a remote update. When the user explicitly requests a push, complete all required validation and integration, verify the intended remote and branch, then push the validated branch using the configured upstream. Never force-push or push tags unless the user explicitly requests that specific operation. Do not use an API, script, or GUI to bypass these rules. If the target is ambiguous or the branch is not ready, clarify or report the blocker instead of pushing.

Use this skill only when explicitly invoked as `$taav-git-prepare-push` or when the user directly requests this Git preparation workflow. Invocation starts the workflow but does not authorize behavior-changing code edits. For orchestrated work, follow the approved plan and its scope. For standalone work, perform read-only discovery first and obtain explicit approval of a concrete plan before editing code, running tests/builds, or setting up local test infrastructure. Stage, commit, fetch, pull, merge, or resolve conflicts only when the approved task scope authorizes those actions. An explicit user request to push authorizes that remote update; do not ask for redundant push approval. Complete the approved preparation and validation before reporting the result.

Read applicable repository instructions and architecture documents before making changes. Follow the company's .NET/Flutter architecture and Taav Material UI conventions. Inspect existing company components before adding UI; ask before creating a missing component. Update repository architecture maps when a repair changes architecture.

When this skill runs as the Git phase of `taav-delivery-orchestrator`, the approved plan authorizes only its stated implementation and Git-preparation scope. Do not expand product behavior or fix unrelated work through the repair loop. If a repair would change behavior, broaden scope, or require a business decision, pause and obtain approval of a revised item-level proposal. Routine mechanical repairs needed to validate approved changes may proceed within scope. Plan approval alone does not authorize a push; an explicit user request is required.

## Discover scope and branch

For each requested repository, use `git rev-parse --show-toplevel` to identify its root; backend and frontend may have separate histories. Inspect the current branch and upstream, remotes, staged/unstaged/untracked changes, outgoing commits, and any in-progress merge. Review changed files and relevant history, not only the diff summary. Preserve unrelated work and existing staging intent.

The target branch is `dev`. Do not switch branches automatically. If a requested repository is on another branch, report the mismatch. Use its configured upstream; without one, use `origin/dev` only when repository configuration or history establishes it as the intended destination. Ask only when the destination is genuinely ambiguous. Never invent a remote or silently change its URL.

An empty worktree does not mean preparation is complete. Inspect outgoing commits and remote divergence, then integrate and validate as needed. If there are no changes or outgoing commits, check the remote and report that no preparation is required.

## Repair and validation loop

Inspect every candidate file, including untracked files, and run `git diff --check`. Diagnose and repair in-scope code, build, analyzer, test, or configuration failures instead of stopping at the first failure. Make the smallest justified repair, rerun the failed or focused check, then run the complete required validation. Continue while a concrete repair is available; do not use an arbitrary retry limit or claim success just because retries have ended.

- Fix the implementation when a test exposes a defect, and add regression coverage for bugs. Change test expectations only when supported by intended behavior and authorized by applicable instructions. Skill invocation alone does not authorize weakening tests. Never delete, skip, disable, or mock away the behavior under test, or reduce assertions/coverage just to pass.
- Follow existing test frameworks, fixtures, repository architecture, and API contracts. Do not install or change SDKs, or rewrite dependency versions, just to force a pass. Resolve demonstrated dependency/configuration issues within scope using the documented toolchain.
- Build warnings and Flutter analyzer info/warning lint diagnostics are non-blocking; record their counts and categories. A nonzero analyzer exit caused only by lint is also non-blocking. Build errors, analyzer errors, failed tests, skipped or unrun required tests, and required build failures prevent readiness.
- Review generated files, machine-specific paths, local overrides, unrelated edits, and secrets. Do not commit unintended runtime settings, credentials, tokens, logs, or build output. Preserve user files; never silently delete or revert changes. Stage only the fully reviewed task scope and explain exclusions. If excluded files prevent safe integration, use a worktree when practical; otherwise report the exact blocker without discarding work.
- Genuine blockers include unavailable safe infrastructure, unresolved requirements, irrecoverable tooling access, and conflicts whose business meaning cannot be established from code, tests, or documentation. Complete independent preparation before asking for missing information. Do not guess business behavior or conceal blocked checks.

### .NET backend

Identify the actual solution. Run `dotnet build <solution>` and the complete test suite with `dotnet test <solution> -m:1`. The single MSBuild node reduces contention between test projects that share a database; it does not disable parallelism within test assemblies.

Before database tests or migrations, verify that the effective connection resolves to a local, isolated test database, separate from application, developer, shared, or production data. Never print credentials. If the local test database is missing, create and initialize it with the repository's migration runner only after verifying its target. Local test configuration and necessary migrations are authorized; never drop or reset an existing database. Use local, untracked overrides where supported. If a safe local SQL Server or verified migration mechanism is unavailable, report the environment blocker. Mock SMS, OTP delivery, payments, notifications, and other external integrations; do not contact live services.

### Flutter frontend

Run `flutter analyze`, the complete `flutter test` suite, and the documented application build. If a runnable `example` supports web, build it with `flutter build web --release`; do not invent an application target for a reusable package. Use the documented Flutter SDK and inspect any files changed by the tooling.

When this skill is used on its own, do not launch the application or perform manual UI, browser, or emulator testing. If another explicitly approved workflow requires manual UI testing, follow that workflow; this skill alone does not require it.

## Commit before pull

Complete required validation in every requested repository before creating initial commits. Recheck status, stage the reviewed task scope, inspect the staged diff, and commit locally in each repository that needs a commit. Use one coherent initial commit per repository unless the changes clearly call for separate commits.

Write commit subjects and bodies entirely in English and follow Conventional Commits. Describe the actual changes; never claim that unrun tests passed. Do not append or request a ticket/sequence number unless the user explicitly asks for one. Do not automatically amend, rebase, cherry-pick, tag, force-push, or reset history. A normal push to the verified upstream is allowed only when the user explicitly requests it and all required checks pass.

## Pull, resolve conflicts, and validate again

After the initial local commits, fetch the intended remote for each requested repository and inspect divergence. Integrate its intended `dev` branch using merge semantics (`git pull --no-rebase <remote> dev` or fetch followed by an explicit merge of the observed remote branch). Do not rely on a global pull strategy or merge unrelated histories. Do not perform a content-changing pull when the branch is already up to date.

Resolve conflicts by reviewing both sides, surrounding implementation, tests, and the intent of incoming commits. Preserve the intended local changes and incoming behavior together. Never choose ours/theirs blindly for all conflicts. Repair downstream build/test regressions and check relevant tracked source for conflict markers. If intent remains ambiguous, preserve the merge state and ask the user; do not abort or reset automatically.

If Git requires a merge commit before checks can run, complete it locally. Otherwise, validate the resolved tree before completing the merge. After integration, rerun the full required validation for every requested repository against the final combined source; pre-pull results are stale. Fix failures and create additional local commits for repairs. A clean fast-forward or no-op pull does not need an artificial commit. Use an English Conventional Commit message for merge commits when possible. If the user explicitly requested a push, push only after final validation and only to the verified upstream; otherwise leave the commits local.

Any change to tested source invalidates the affected earlier results. Revalidate until the final `HEAD` is the checked source, the reviewed task work is committed, no merge conflict remains unresolved, and no required check is failing, skipped, blocked, or unrun. Follow repository requirements for validating a clean worktree. Fetch again before declaring readiness; if remote `dev` has advanced, integrate and validate again. A blocker in any requested repository prevents declaring the multi-repository operation ready.

If one repository was committed before a later failure, report the actual partial results and commit hashes. Never claim that nothing was staged or committed when it was. Do not automatically roll back successful local commits.

## Final report

When preparation is complete, report in Persian: repositories; remote/branch destinations; final `HEAD` hashes; outgoing commit summaries; integration and conflict repairs; exact build, analyzer, and test results; warning/lint counts; worktree/divergence status; and whether each requested push succeeded or was blocked.

If the user explicitly asked for a push and all readiness conditions are met, push the validated branch to its verified upstream. If any condition prevents the push, explain the exact blocker and leave the commits local. Never use another tool or route to bypass the verified destination or normal Git safeguards.

Write all user-facing progress updates and final reports in Persian. Preserve command names, paths, hashes, and English commit messages as needed. Never expose secrets.
