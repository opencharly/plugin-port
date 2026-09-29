# AGENTS.md — plugin-port

Standalone plugin repo for the `port` capability (`verb:port`). The plugin is a
Go module at `candy/plugin-port/` (module path
`github.com/opencharly/plugin-port/candy/plugin-port`); the root `charly.yml`
only declares `discover: candy` so the repo is a project and its candy is
scanned.

Canonical files:

- `candy/plugin-port/charly.yml` — the `plugin-port:` candy entity (`plugin:`
  block, `plan:` check).
- `candy/plugin-port/plugin.go` — the `verb` implementation (`RunVerb` via
  `sdk/kit.CheckContext`), `NewCheckVerb()` + `NewMeta()`.
- `candy/plugin-port/schema/port.cue` — the self-contained `#PortInput`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, the per-plugin CUE-schema contract,
  placement. Load before touching the provider or schema.
- `/charly-check:check` — the declarative check-verb surface the `port:` verb is
  authored through.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-port/` — compile the plugin module.
- `go test ./...` in `candy/plugin-port/` — the plugin's Go tests.
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- There is no live bed: the verb is host-coupled and its evidence is its `plan:`
  `check:` step plus the Go tests.

## Modify this repo

- Edit the `plugin-port:` candy entity, the Go source, and `schema/port.cue`
  **together** — the schema is the single source for the `params/` struct, so a
  field change not mirrored in the schema desyncs the generated types.
- The verb is placement-agnostic (compiled-in **or** out-of-process); do not
  describe it as one or the other.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
