# TAAV Agent Skills

Private, version-controlled catalog of TAAV engineering workflows for coding agents.

## Skills

- **taav-delivery-orchestrator** — coordinates read-only investigation, an itemized user-approved plan, implementation and verification, and Git preparation. It depends on the two skills below.
- **taav-dotnet-flutter-dev** — implementation and verification rules for company .NET and Flutter projects.
- **taav-git-prepare-push** — local commits, pull/merge, conflict resolution, and validation. It never pushes.

Each skill is self-contained in its own directory. Keep the three directories together when installing the orchestrator, because it links to its sibling skills using relative paths.

The workflows may create local commits, pull changes, merge, and resolve conflicts within the approved scope. They must never push to a remote; users perform any desired push manually outside these skills.

## Layout

```text
skills/
  taav-delivery-orchestrator/SKILL.md
  taav-dotnet-flutter-dev/SKILL.md
  taav-git-prepare-push/SKILL.md
```

## Install on Windows

First clone this private repository somewhere on your machine and make sure your GitHub account can read it. The examples below assume the clone is at `C:\Ali\projects\skill`.

### Codex

Codex's global user skill directory is `%USERPROFILE%\.agents\skills`. In PowerShell, copy one skill by replacing the name below, or copy all three so the orchestrator's sibling dependencies are present:

```powershell
$SkillRepo = 'C:\Ali\projects\skill'
$SkillTarget = Join-Path $env:USERPROFILE '.agents\skills'
$SkillNames = @(
  'taav-delivery-orchestrator',
  'taav-dotnet-flutter-dev',
  'taav-git-prepare-push'
)
New-Item -ItemType Directory -Force -Path $SkillTarget | Out-Null
foreach ($SkillName in $SkillNames) {
  Copy-Item -Path (Join-Path $SkillRepo "skills\$SkillName") -Destination $SkillTarget -Recurse -Force
}
```

To install only one skill, set `$SkillNames` to that skill's folder name. If you install `taav-delivery-orchestrator`, install all three. Codex detects local skill changes; if a newly copied or updated skill does not appear, restart Codex.

### Cursor

Cursor's global user skill directory is `%USERPROFILE%\.cursor\skills`. Use the same copy command, changing the target assignment to:

```powershell
$SkillTarget = Join-Path $env:USERPROFILE '.cursor\skills'
```

Keep all three skill folders together when installing the orchestrator so its relative links resolve. Cursor detects skills from its global skill directory. To use the skills with Cursor Cloud Agents, enable **Settings → Agents → Sync Skills for Cloud Agents**.

### Update an installation

Pull the latest repository version, then run the copy command again with the same target and skill names. The command overwrites installed files with the repository versions.

## Team installation in Cursor

Copying skills installs them for the current user's local Cursor. Cursor does not install standalone skills from a plain GitHub repository URL. To distribute through Cursor's team marketplace, package skills in a Cursor-compatible plugin/marketplace and publish it; publishing and installing are separate steps.

## Notes

- These are portable `SKILL.md` files. Other coding agents may support the same format, but their discovery paths and team distribution methods differ; use the corresponding agent's documented location.
- Do not commit credentials or personal machine paths in skill content.
