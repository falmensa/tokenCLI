# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

`tokless` is a single Go CLI (binary name `tokless`, entrypoint `cmd/tokless`) that installs a set of token-saving tools — RTK, Caveman, CodeGraph, Context-Mode — and wires each of them into AI coding agents (Claude Code, OpenCode, Codex) by editing those agents' own config files. It installs nothing of its own runtime; it orchestrates upstream tools and mutates agent configs.

## Build, test, run

```bash
go build ./...                        # build everything
go vet ./...                          # static checks (CI gate)
go test ./...                         # unit + sandbox integration + idempotency
go test ./internal/tools -run TestName   # a single test/package
go run ./cmd/tokless doctor           # run the CLI without installing
bash scripts/build-release.sh v0.2.0  # cross-compile all platform binaries into dist/release
```

CI (`.github/workflows/ci.yml`) runs `go vet`, `go test`, `go build` on every push to `main` and every PR. Pushing a `v*` tag triggers `release.yml`. The release version is stamped via ldflags into `internal/util.Version`.

Note: `go.mod` contains a stale local `replace empty-name => /home/hoangp/empty-name` directive with a phantom require. It is not imported by any code and builds/tests pass regardless; leave it unless you're deliberately cleaning up module metadata.

### TOKLESS_TEST sandbox mode

Tests set `TOKLESS_TEST=1`, which flips nearly every tool into a hermetic mode: instead of downloading/installing real upstream binaries or shelling out to real agent CLIs, each tool writes fake shims and config into a temp `HOME`. This is what lets `go test` run offline and deterministically. When adding or changing a tool, you must add the matching `TOKLESS_TEST` branch (see `rtkEnsureInstalled`, `codegraphTestShim`) or the sandbox integration test in `internal/commands/init_integration_test.go` will not exercise your code. That test wires all three agents under a temp `HOME` and asserts byte-for-byte idempotency across repeated runs.

## Architecture

The whole design is a **registry of manifests iterated by generic command handlers** — there is deliberately no per-agent or per-tool branching in the command layer.

- **`internal/core`** defines two data types and the global registry: `AgentManifest` (id, label, config-dir, detection probe) and `ToolManifest` (id, install fn, plus `WireFor` / `UnwireFor` / `VerifyFor` maps keyed by agent id, and an optional `IndexProject` fn). `RegisterAgent` / `RegisterTool` preserve registration order, which is the order shown in all output.
- **`internal/agents`** — one file per agent (`claude.go`, `opencode.go`, `codex.go`), each declaring an `AgentManifest` var plus that agent's config-mutation helpers (e.g. `ConfigureClaudeMcp`, `ConfigureCodexMcp`). `agents.Register()` (in `codex.go`) registers all three.
- **`internal/tools`** — one file per tool (`rtk.go`, `caveman.go`, `codegraph.go`, `contextmode.go`), each declaring a `ToolManifest` var. `tools.Register()` (in `contextmode.go`) registers all four. `autoindex.go` holds the shared SessionStart-hook plumbing that CodeGraph uses to auto-build a per-project index.
- **`internal/commands`** — the verbs: `init` (default; install + interactively pick agents + wire), `update`, `doctor`, `index`, `disable`, `uninstall`, `selfupdate`. Every handler loops over `core.ListTools()` / `core.ListAgents()` and calls the manifest functions — it never names a specific tool or agent.
- **`internal/util`** — all config I/O and platform glue: ordered-map JSON/JSONC (`jsonc.go`), TOML block editing (`toml.go`), agent config paths (`paths.go`), process exec (`exec.go`), MCP spawn resolution (`mcpspawn.go`), npm/cargo install (`npminstall.go`, `deps.go`), version checking (`versions.go`), progress bars, prompts, colors, PATH self-healing.

`cmd/tokless/main.go` is a thin dispatcher: it calls `agents.Register()` + `tools.Register()`, parses args (`--k=v`, `--k v`, `--k`, `-x`), then switches on the command into a `commands.RunXxx` function. Default command with no verb is `init`.

### Request flow (init)

`RunInit` → for each tool call `tool.Install(RunOpts)` with a progress reporter → detect which agents are present → (interactively or via `--agents`) pick agents → for each chosen agent, for each tool, call `tool.WireFor[agentID]`, then in real (non-dry, non-test) runs call `tool.VerifyFor[agentID]` to confirm the wire actually took. A tool counts as wired only if both wire and verify succeed.

## Conventions that matter

- **Idempotency is a hard requirement.** Config writes must be byte-stable across repeated runs and must never reorder a user's existing keys. This is asserted by the integration test via sha256 comparison.
- **JSON/JSONC edits go through the ordered-map helpers** (`util.NewOrderedMap`, `util.TryParseJsonc`, `util.StringifyJSON`) so existing key order is preserved. Never `encoding/json`-marshal a config back out.
- **TOML edits (Codex config) go through the block helpers** (`util.UpsertBlock`, `util.RemoveBlock`, `util.HasBlock`) which edit `[section]` blocks in place.
- **Every wired entry needs a matching `VerifyFor` step** so `tokless doctor` can validate it independently, and a matching `UnwireFor` so `uninstall` fully reverses it.
- **Spawning upstream binaries**: prefer a real binary on PATH, fall back to `npx --no-install` — use `util.PickMcpSpawn` rather than hardcoding.
- `ToolManifest.NotTrackable` marks tools with no standalone binary (installed per-agent) so version checks in doctor/update skip them instead of looping.

## Adding a tool or agent

Copy the nearest existing file as a template — the manifests are self-documenting.

- **Tool**: create `internal/tools/<name>.go` defining a `ToolManifest` (Install + WireFor/UnwireFor/VerifyFor per agent, plus a `TOKLESS_TEST` branch), and add one `core.RegisterTool(...)` line to `Register()` in `contextmode.go`.
- **Agent**: create `internal/agents/<name>.go` defining an `AgentManifest`, and add one `core.RegisterAgent(...)` line to `Register()` in `codex.go`.

Cite the upstream tool's README URL in a comment for any config shape you write, since that shape is dictated by the upstream tool, not this repo.
