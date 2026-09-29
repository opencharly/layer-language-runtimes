# AGENTS.md — layer-language-runtimes

Standalone candy repo for the `language-runtimes` layer — the polyglot
system-runtime meta-layer (.NET 9 SDK, PHP CLI, system Python 3, plus Go and
Node.js via dependencies). The candy lives in `charly.yml` at the repo root: the
per-distro package sections, the `dotnet-install.sh` cross-distro plan step, the
`check:` assertions, and the embedded `skill:` entity projected into the
marketplace corpus as `/charly-coder:language-runtimes`.

Canonical files:

- `charly.yml` — the `language-runtimes:` candy entity and the
  `language-runtimes-skill:` skill entity.
- `CHANGELOG/` — per-CalVer release history; read it before changing baked checks.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:language-runtimes` — the owning skill. The runtime meta-layer,
  the packages per distro, the system-Python-vs-pixi distinction, and the
  `dotnet-install.sh` parity story. Load before editing or troubleshooting the
  layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `command:`/`check:`, per-distro `distro:` arms,
  package/repo sections, and service declarations). Load before editing any
  entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; they must stay
  valid on every distro arm they run on. Scope a distro-specific check in the
  command itself — the check runner does not honour runner-level
  `exclude-distro` fields.
- The `dotnet-install.sh` plan step is idempotent: its outer `command -v dotnet`
  guard makes it a no-op where the distro package already installed dotnet.
  Preserve that guard.

## Modify this repo

- Edit the `language-runtimes:` candy entity AND the
  `language-runtimes-skill:` skill entity in `charly.yml` together. The skill is
  the projected usage source, so a package or behaviour change not mirrored in
  the skill leaves the corpus stale.
- This candy deliberately does NOT depend on `python`/`pixi`; do not add a
  defensive dependency — see the rulebook's "don't declare defensive deps" rule.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
