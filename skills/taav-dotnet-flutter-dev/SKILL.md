---
name: taav-dotnet-flutter-dev
description: Implement and verify company .NET backend and Flutter app changes using repository architecture, project conventions, automated tests, and manual end-to-end checks.
metadata:
  short-description: Develop company .NET and Flutter projects
---

# Company .NET and Flutter development

Use this skill for software changes in the company's .NET backend and Flutter projects. This single file contains the complete shared rules and project-specific rules. First identify the affected repositories and project from source and repository documentation, and read applicable AGENTS.md files. Apply shared rules to every supported project and the First Unit section only to First Unit. Do not apply its ports, login account, or database names to another project. Add future project-specific rules as clearly named sections in this same SKILL.md, not as separate skills or profile files.

## Shared engineering rules

- Work to senior engineering standards: understand the existing design and root cause first, keep changes focused, preserve established conventions, and consider correctness, security, accessibility, and performance. Do not promise bug-free software; report evidence and remaining gaps honestly.
- Before implementation, maintain a concise architecture map for each affected codebase: `BACKEND_ARCHITECTURE.md` for .NET and `FRONTEND_ARCHITECTURE.md` for Flutter, placed at the relevant repository root. If missing, inspect source, tests, project files, and existing documentation to document the architecture that actually exists. Read the relevant map before each change. Update it when boundaries, dependencies, entry points, or established patterns change; do not pad it for routine edits.
- Add or update automated tests for every behavior change. A bug fix needs a regression test that fails before the fix when practical. Preserve existing tests and assertions; never delete, skip, disable, or weaken them to make a run pass. Run focused checks first and all applicable affected suites, then report failures, skips, blocked checks, and checks not run.
- For every implementation or bug-fix task, manually exercise the complete relevant end-to-end workflow through the actual UI after automated checks. For a web workflow, use the internal browser through the computer-use UI, open the local app in a visible tab, and keep it visible while testing so the user can follow the run. Do not substitute API calls, unit tests, screenshots, or browser developer tools for interacting with the UI. Start at the workflow's real entry point and perform each required action through its final expected outcome; check relevant success, validation, loading, and error states. For a native mobile workflow, use its emulator/UI. If the appropriate UI cannot be run, report the manual UI check as blocked and explain why; use an API/runtime check only as supplemental evidence, never as a passed manual UI check.
- Before starting services, running integration tests, sending OTPs, or mutating test data, prove that service endpoints and database targets are local or explicitly isolated test resources. Test data may be created, edited, or deleted within a verified isolated test environment. Never use production/shared data or real external SMS, payment, or identity services for testing. Do not drop/reset a database unless separately requested.
- Use existing Taav Material UI components and the company UI packages used by the project. Inspect their source, exports, examples, and versions before use. Do not create a replacement component when an appropriate company component exists. If a required component is absent, ask the user before creating it or changing the design-system package.
- Preserve unrelated user changes. Inspect staged and unstaged diffs before and after editing. Do not silently revert, omit, or overwrite user work.
- Local commits, fetch/pull, merge, and conflict resolution may be performed when included in the explicitly approved task plan or directly requested workflow. Never push to any remote under any circumstances; this prohibition still applies if the user asks for or approves a push. The user may push manually outside this skill.
- Keep credentials, OTPs, tokens, private connection strings, and personal data out of source changes, logs, screenshots, architecture maps, and final reports.
- Use already available UI tools first. The user has authorized installing a relevant plugin when it is needed for verification of these projects; install only a plugin that provides the required capability.

## Workflow

1. Discover the affected .NET and/or Flutter repositories, identify the project, read applicable instructions and the matching project-specific section below, inspect the current Git status, and perform any other necessary read-only investigation. For multi-item requests, establish evidence and uncertainty item by item.
2. Present a concrete plan with the expected behavior, proposed approach, affected areas (or marked uncertainty), and verification for every requested item. Obtain the user's explicit approval before implementation. A general request to fix a feature/bug or an instruction to use this skill does not authorize coding. If approval has not been given, remain in read-only investigation and wait; do not create architecture maps, edit source, add tests, run builds/tests, or mutate setup/data.
3. Create or update and read the relevant architecture map before coding, after plan approval.
4. Verify local/test configuration for any app, API, database, and external integration used by validation.
5. Implement only the approved behavior and its necessary supporting changes, using existing architecture and project components. Add/update tests and architecture maps as needed. For multi-item requests, preserve the approved item boundaries and acceptance criteria.
6. Run focused automated checks, then the complete applicable suites. Do not run a migration until its destination is verified as an isolated test database.
7. Start or reuse the local app/API and manually exercise the complete relevant end-to-end flow in the actual UI. For web flows, use the visible internal browser and interact with the local app from its entry point through the final expected outcome, with the browser left visible for the user. Inspect runtime errors and final Git status. Stop and report if the target cannot be proven local and isolated.
8. Summarize changes, exact checks and outcomes, manual flows exercised, remaining failures/limitations, and any local test configuration or architecture maps changed. Write user-facing updates and reports in Persian.

### Plan and approval boundary

The approved plan is the implementation contract. Before coding, confirm that it states the intended behavior and acceptance criteria, proposed approach, affected project/layers (or clearly marked uncertainty), and verification for each requested bug, flow, or business requirement. Follow the orchestrator's item-by-item plan format for multi-item work. Never treat a generic "fix all" request, skill invocation, or plan presentation as approval. After approval, proceed autonomously within that scope; do not ask the user to reconfirm individual items, ordinary technical choices, tests, or steps already covered by the plan. Use repository conventions and engineering judgment for routine decisions, then report what you chose.

If the user approves only selected items, implement only those items and necessary shared support. If implementation reveals a different technical root cause but the approved behavior, acceptance criteria, and scope remain valid, adapt the implementation and continue without asking again. Pause and seek a revised approval only when new evidence requires changing approved behavior or acceptance criteria, expanding consequential scope, choosing between unresolved business rules, or taking an action outside the approval. Record genuine blockers or deferred items and why; do not silently substitute behavior.

## First Unit-specific rules

Apply the following rules only after identifying the task as First Unit. All required First Unit details are included here in the main skill.

### Project map and UI components

- Backend: repository `first-unit-back`, ASP.NET Core/.NET 8 solution `UnitSalesContract.sln`; persistence uses SQL Server and FluentMigrator.
- Frontend: repository `flutter-first-unit-module`, with runnable host app in `example`.
- Related Flutter packages may be beside these repositories, including `flutter-taav-material-ui` and `flutter-user-management-module`. Discover current locations instead of assuming they are present.
- Before building UI, inspect the provided Taav Material UI package source, exports, examples, and version used by First Unit. Reuse available components and established First Unit wrappers. If a required component is absent, ask the user before inventing a replacement or changing the UI library.
- Maintain repository-local architecture maps: `first-unit-back/BACKEND_ARCHITECTURE.md` and `flutter-first-unit-module/FRONTEND_ARCHITECTURE.md`. Document observed source, tests, and conventions, not guessed architecture.

### Local runtime

- Bind local web and API servers only to `127.0.0.1`; never use `0.0.0.0` or a LAN/public interface for this workflow.
- First Unit web frontend may use port `8008`; if unavailable, inspect its owner and use `8009` if free. Do not terminate an unknown process to free a port. Use the repository's documented run/build helper when appropriate; otherwise use the example app and Flutter web-server options with an explicit loopback host and chosen port.
- Before opening the app, verify the effective API base URL and authentication configuration. They must target the local API/auth path intended for the manual test. Do not assume a development-looking URL is local. If the local API is not running, start the REST API using its project configuration and an available loopback port; configure the frontend to that local URL using an untracked/local override when supported. Preserve user configuration and do not commit local overrides.
- Check startup logs and a local health/API route to confirm the listener and frontend-to-backend connection. A localhost frontend alone does not prove the API or authentication path is local.
- Stop only processes started for this task, after checking their identity. Leave unrelated listeners running.

### Manual login flow for the local First Unit app

Use this flow only in a local browser session after verifying the frontend, API/auth path, and test database are local and isolated. The test phone provided by the user is `09165786475`.

1. Open the local First Unit login screen and enter `09165786475` in the phone field.
2. Select `کد یکبار مصرف`.
3. Click `ارسال کد`.
4. On the next screen, find the message in the app that starts with `Your Code is -->`. Read the current code from that visible message. Do not reuse an old code or retrieve it from logs, databases, network internals, or an external SMS service.
5. Enter the displayed code into the OTP boxes and click `تایید`.
6. Confirm that the expected authenticated page loads and no browser or API error blocks the flow. Do not include the phone number or OTP value in logs, screenshots, commits, architecture maps, or final reports.

If `Your Code is -->` is not displayed, the API/auth target is not local, or sending the code would contact a real SMS or external identity service, stop the login attempt and report the blocker. Do not bypass authentication or retrieve the OTP through another channel.

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
