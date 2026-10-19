# TAAV Agent Skills

Private, version-controlled catalog of TAAV engineering workflows for coding agents.

## Skills

- **taav-delivery-orchestrator** — coordinates read-only investigation, an itemized user-approved plan, implementation and verification, and Git preparation. It depends on the two skills below.
- **taav-dotnet-flutter-dev** — implementation and verification rules for company .NET and Flutter projects.
- **taav-git-prepare-push** — local Git preparation, integration, validation, and separate approval before push.

Each skill is self-contained in its own directory. Keep the three directories together when installing the orchestrator, because it links to its sibling skills using relative paths.

## Layout

```text
skills/
  taav-delivery-orchestrator/SKILL.md
  taav-dotnet-flutter-dev/SKILL.md
  taav-git-prepare-push/SKILL.md
```

The canonical files use the Agent Skills `SKILL.md` format. Installation and update steps vary by coding agent; follow the target agent's supported private-repository or local skill installation workflow. For Codex, copy the selected skill directory under `~/.codex/skills/`. Install all three together when using `taav-delivery-orchestrator`.
