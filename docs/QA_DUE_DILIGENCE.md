# QA Due Diligence (Quick Notes PR Stack)

This repository is a **merge simulation** of `Yakitrak/obsidian-cli` with the “quick notes” PR stack merged on top.

- `main` is intended to represent: “upstream + all quick notes PRs merged”.
- The `pr-*` branches represent the individual PR slices in the stack.

## What We Verified

All verification was run in an isolated per-repo sandbox directory:

- `QA_ROOT="$PWD/.qa"`
- `HOME="$QA_ROOT/home"`
- `GOCACHE="$QA_ROOT/cache"`
- `GOPATH="$QA_ROOT/go"`
- `GOMODCACHE="$QA_ROOT/mod"`

Artifacts under `.qa/` are **not committed** (no binaries/logs checked in).

## Commands Run

- `go test -mod=vendor ./...`
- `go build -mod=vendor -o "$QA_ROOT/bin/obsidian-cli_<branch>" .`
- Help/alias sanity checks:
  - `obsidian-cli delete --help`
  - `obsidian-cli del --help`
  - `obsidian-cli d --help`

## Results (Recorded)

Environment + run metadata:

- Go version: `go version go1.25.5 darwin/arm64` (confirmed from `.qa/logs/go_version.txt`)
- Audit: `{ "verification_status": "complete", "unverified_claims": 0, "timestamp": "2025-12-27T00:37:29Z" }` (confirmed from `.qa/QA_AUDIT_post_ba966c7.json`)

Summary (confirmed from `.qa/QA_SUMMARY_post_ba966c7.json`):

- `pr-03-delete` @ `c20affdfe1aad365e123839c2fc8c1d299cf4786` — `go test` passed; `delete` aliases: `delete, del`
- `pr-04d-init` @ `ff8b67374a80d03d145413699e5c039ec5bd062d` — `go test` passed; `delete` aliases: `delete, del`
- `all-prs-merged` @ `dba51b42062fabd52b255140958e5297245fae95` — `go test` passed; `delete` aliases: `delete, del`

Behavior spot-check (confirmed from `.qa/logs/*_d_help.txt` and `.qa/logs/*_del_help.txt`):

- `d` resolves to `daily`
- `del` resolves to `delete`

