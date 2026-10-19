---
name: taav-dotnet-flutter-dev
description: Implement and verify the company's .NET backend and Flutter applications, following repository architecture, project conventions, automated tests, and required manual UI checks.
metadata:
  short-description: Implement and verify .NET and Flutter changes
---

# Company .NET and Flutter Development

Use this skill when changing the company's .NET backend or Flutter applications. This file contains the shared engineering requirements and project-specific guidance. Identify the affected repositories and projects from their source and documentation, then read applicable `AGENTS.md` files. Apply the shared requirements to every project and the First Unit guidance only to First Unit. Never reuse its ports, test account, or database names for another project. Add future project-specific guidance as clearly named sections in this file rather than as separate skills or profile files.

## Shared engineering rules

- Follow senior engineering practice: understand the current design and root cause, keep changes focused, preserve project conventions, and account for correctness, security, accessibility, and performance. Do not promise defect-free software; report evidence and remaining gaps accurately.
- Before implementation, maintain a concise architecture map in each affected repository: `BACKEND_ARCHITECTURE.md` for .NET and `FRONTEND_ARCHITECTURE.md` for Flutter. If a map is missing, inspect source, tests, project files, and documentation and record the architecture that exists. Read the relevant map before making changes. Update it when boundaries, dependencies, entry points, or established patterns change; routine edits do not need map updates.
- Add or update automated tests for every behavior change. When practical, a bug fix must include a regression test that fails before the fix. Preserve existing tests and assertions; never delete, skip, disable, or weaken them to pass a run. Run focused checks first, then all applicable affected suites. Report failures, skips, blocked checks, and checks not run.
- After automated checks, manually exercise the complete relevant end-to-end workflow in the actual UI for every implementation or bug-fix task. For web, use the computer-use UI to open the local application in a visible browser tab and leave it visible during the test. Interact with the application instead of substituting API calls, unit tests, screenshots, or developer tools. Start from the workflow's real entry point and complete each required action through the expected outcome; verify relevant success, validation, loading, and error states. For native mobile, use the emulator/UI. If the appropriate UI is unavailable, report the manual check as blocked and explain why. An API/runtime check is supplemental and does not count as a passed UI check.
- Before starting services, running integration tests, sending OTPs, or changing test data, verify that service endpoints and database targets are local or explicitly isolated test resources. Test data may be created, changed, or deleted only in a verified isolated test environment. Never use production/shared data or live SMS, payment, or identity services. Do not drop or reset a database unless separately requested.
- Reuse the project's existing Taav Material UI components and company UI packages. Inspect their source, exports, examples, and versions before use. Do not replace an existing suitable company component. If a required component is missing, ask the user before creating a substitute or changing the design-system package.
- Preserve unrelated user changes. Inspect staged and unstaged diffs before and after editing; never silently revert, omit, or overwrite user work.
- Perform local commits, fetch/pull, merges, and conflict resolution only when included in the explicitly approved task plan or a directly requested workflow. Never push to a remote, even if the user requests or approves it. The user may push manually outside this skill.
- Keep credentials, OTPs, tokens, private connection strings, and personal data out of source changes, logs, screenshots, architecture maps, and final reports.
- Prefer already available UI tools. The user has authorized installing a relevant plugin when needed to verify these projects; install only a plugin that provides the required capability.

## Workflow

1. Discover the affected .NET and/or Flutter repositories and projects. Read applicable instructions and the matching project-specific section below, inspect Git status, and complete necessary read-only investigation. For multi-item requests, record evidence and uncertainty per item.
2. Present a concrete plan for every requested item, including expected behavior, proposed approach, affected areas (or explicit uncertainty), and verification. Obtain the user's explicit approval before implementation. A general request to fix a feature/bug or an instruction to use this skill does not authorize coding. Until approval, remain read-only: do not create architecture maps, edit source, add tests, run builds/tests, or mutate setup or data.
3. After approval, create or update and read the relevant architecture map before coding.
4. Verify local/test configuration for every app, API, database, and external integration used during validation.
5. Implement only approved behavior and necessary supporting changes, following existing architecture and project components. Add/update tests and architecture maps as needed. For multi-item requests, preserve approved item boundaries and acceptance criteria.
6. Run focused checks, then the complete applicable automated suites. Verify the destination is an isolated test database before running any migration.
7. Start or reuse the local app/API and manually complete the relevant end-to-end flow in the actual UI. For web, use the visible internal browser and exercise the local app from its entry point through the expected outcome, leaving the browser visible. Check runtime errors and final Git status. Stop and report if the target cannot be verified as local and isolated.
8. Report changes, exact check commands and outcomes, manual flows exercised, remaining failures/limitations, and any local test configuration or architecture maps changed. Write all user-facing updates and reports in Persian.

### Plan and approval boundary

The approved plan is the implementation contract. Before coding, ensure it defines the intended behavior and acceptance criteria, proposed approach, affected project/layers (or explicit uncertainty), and verification for every requested bug, flow, or business requirement. For multi-item work, follow the orchestrator's item-level format. A generic "fix all" request, skill invocation, or plan presentation is not approval. After approval, work autonomously within scope; do not ask the user to reconfirm items, routine technical decisions, tests, or steps already covered by the plan. Use repository conventions and engineering judgment for routine choices, then report them.

If the user approves only selected items, implement those items and necessary shared support only. If implementation reveals a different technical root cause but the approved behavior, acceptance criteria, and scope remain valid, adapt and continue without asking again. Pause for revised approval only if new evidence requires a change to approved behavior or acceptance criteria, expands consequential scope, raises an unresolved business choice, or requires an action outside the approval. Record genuine blockers and deferred items with their reasons; do not silently substitute behavior.

## First Unit-specific rules

Apply these rules only after confirming the task concerns First Unit. All required First Unit guidance is maintained in this skill.

### Project map and UI components

- Backend: `first-unit-back`, an ASP.NET Core/.NET 8 solution named `UnitSalesContract.sln`; persistence uses SQL Server and FluentMigrator.
- Frontend: `flutter-first-unit-module`, with a runnable host application in `example`.
- Related Flutter packages may be located alongside these repositories, including `flutter-taav-material-ui` and `flutter-user-management-module`. Discover their current locations instead of assuming they exist.
- Before building UI, inspect the Taav Material UI package source, exports, examples, and version used by First Unit. Reuse available components and established First Unit wrappers. If a required component is missing, ask before creating a replacement or changing the UI library.
- Maintain repository-local architecture maps at `first-unit-back/BACKEND_ARCHITECTURE.md` and `flutter-first-unit-module/FRONTEND_ARCHITECTURE.md`. Document observed source, tests, and conventions; do not guess the architecture.

### Local runtime

- Bind local web and API servers to `127.0.0.1` only. Never bind to `0.0.0.0` or a LAN/public interface.
- The First Unit web frontend may use port `8008`. If it is unavailable, inspect the owning process and use `8009` if free. Do not terminate an unknown process to free a port. Use the repository's documented run/build helper when available; otherwise run the example app with Flutter web-server options that specify the loopback host and selected port.
- Before opening the app, verify that the effective API base URL and authentication configuration target the local API/auth path used by the manual test. A development-looking URL is not proof that a target is local. If the local API is not running, start the REST API using project configuration and an available loopback port. Configure the frontend to use that URL through an untracked/local override when supported. Preserve user configuration and never commit local overrides.
- Inspect startup logs and a local health/API route to verify the listener and frontend-to-backend connection. A localhost frontend alone does not prove the API or authentication path is local.
- Stop only processes started for this task, after verifying their identity. Leave unrelated listeners running.

### Manual login flow for the local First Unit app

Use this flow only in a local browser session after verifying the frontend, API/auth path, and test database are local and isolated. The test phone provided by the user is `09165786475`.

1. Open the local First Unit login screen and enter `09165786475` in the phone field.
2. Select `کد یکبار مصرف`.
3. Select `ارسال کد`.
4. On the next screen, locate the in-app message beginning with `Your Code is -->` and read the current code from that visible message. Do not reuse an old code or retrieve it from logs, databases, network internals, or an external SMS service.
5. Enter the displayed code in the OTP fields and select `تایید`.
6. Confirm that the expected authenticated page loads without a blocking browser or API error. Do not include the phone number or OTP in logs, screenshots, commits, architecture maps, or final reports.

If `Your Code is -->` is not displayed, the API/auth target is not local, or sending the code would contact a live SMS or external identity service, stop the login attempt and report the blocker. Do not bypass authentication or retrieve the OTP through another channel.

### Backend tests and local test database

- Read the test project's effective connection configuration without printing credentials. The checked-in `UnitSalesContract.TestTools` configuration currently names local SQL Server database `FirstUnitTest`; verify the current configuration at execution time because it can change.
- Before any database test or migration, prove the server is local, the target database is explicitly test-only, and tests cannot reach DEV, production, or a shared database. The configured database name alone is not proof.
- If the verified local `FirstUnitTest` database is absent, create only that local test database and initialize it with the repository's FluentMigrator project and configured migration entry point. Inspect migration runner arguments and the target connection first. Never drop, reset, or overwrite an existing database implicitly. If the runner or target cannot be proven safe, stop without migrating and report the blocker.
- Run the backend solution build and complete test suite with one MSBuild node (`dotnet test UnitSalesContract.sln -m:1`) so projects that share SQL Server do not contend. Use narrower commands for focused diagnostics, then run the complete applicable suite.
- Do not use real SMS, OTP delivery, payment, push notification, or other external service calls in automated tests. Use the repository's safe local test path or mocks.

### Flutter checks and manual flows

- From the Flutter package root, run `flutter analyze` and the full `flutter test` suite, respecting the repository's Flutter SDK and dependency setup. Do not alter SDKs, dependencies, lockfiles, or generated configuration just to force a pass.
- For the runnable web host, use `example` and bind the web server to `127.0.0.1` on port `8008` or `8009`. If using `tool/run_local_web_release.ps1`, confirm it serves only the built First Unit example and its API target remains local.
- For a web flow, open the local First Unit app in the visible internal browser and keep that tab visible while manually completing the entire relevant flow from its real starting screen through its expected final screen/result. Exercise every required interaction yourself; do not count automated checks or API-only checks as completing the UI flow. For data-entry flows, create, edit, and delete records freely in the verified isolated local test database, including existing rows if needed to exercise the requested workflow. This authorization never applies to shared, staging, or production data. Do not drop/reset the database itself unless the user separately requests that operation.
- Report the frontend URL/port, local API target (without secrets), the visible-browser flow steps and observed outcomes, and any unavailable manual checks. Never claim the full flow passed if any required step was blocked or skipped.
