# QA Due Diligence (Quick Notes PR Stack)

This repository is a **merge simulation** of `Yakitrak/obsidian-cli` with the “quick notes” PR stack merged on top.

- `main` is intended to represent: “upstream + all quick notes PRs merged”.
- The `pr-*` branches represent the individual PR slices in the stack.

## What We Verified

We re-ran a full “merge sim” against the latest upstream `origin/main`, merging each PR branch sequentially (non-fast-forward) and running `go test` + `go build` after each merge.

All verification was run in an isolated per-repo sandbox directory:

- `QA_ROOT="$PWD/.qa"`
- `HOME="$QA_ROOT/home"`
- `GOCACHE="$QA_ROOT/cache"`
- `GOPATH="$QA_ROOT/go"`
- `GOMODCACHE="$QA_ROOT/mod"`

Artifacts under `.qa/` are **not committed** (no binaries/logs checked in).

## Commands Run

- Per-PR merge sim (recorded in `.qa/logs/*.log`):
  - `git merge --no-ff --no-edit quicknotes/<pr-branch>`
  - `go test -mod=vendor ./...`
  - `go build -mod=vendor -o "$QA_ROOT/bin/obsidian-cli_<pr>_<run_id>" .`
- Help/alias sanity checks (captured as text under `.qa/logs/*_help.txt`):
  - `obsidian-cli delete --help` / `obsidian-cli del --help`
  - `obsidian-cli target --help` / `obsidian-cli t --help`
  - `obsidian-cli open --help` / `create --help` / `print --help` / `frontmatter --help`

## Results (Recorded)

Environment + run metadata:

- Go version: `go version go1.25.5 darwin/arm64` (confirmed from `go version` output during QA run)
- Latest upstream merge-sim run ID: `20251227T060241Z` (confirmed from `.qa/QA_SUMMARY_latest_upstream_20251227T060241Z.md`)

### Latest Upstream Merge Sim (PASS)

Baseline: `origin/main` @ `1b06c32570b17684ba08b958f86368577e87fd20` (confirmed from `.qa/QA_SUMMARY_latest_upstream_20251227T060241Z.md`)

All PR merges passed `go test` and `go build` when merged sequentially onto the latest upstream baseline (confirmed from `.qa/QA_SUMMARY_latest_upstream_20251227T060241Z.json`):

- `pr-01-ux` → merge commit `e3b525bdc9c016793194bc8e0358dfe0ea274db1`
- `pr-02-links` → merge commit `697ca6857e19aa17fb8f54addf69ca7c00f5ae0d`
- `pr-03-delete` → merge commit `74d06c1bc4c4fb50869e241e39fc8b8d2f91b70c`
- `pr-04a-settings` → merge commit `7d88a862396c35ef70c1f188a166d3d5c811ffce`
- `pr-04b-append` → merge commit `bdadc6622f2acbf7d72e7892356e47c1b70fc387`
- `pr-04d-init` → merge commit `c67a1331cd8587dd040e6b61f1f235b13a208e8d`

### Behavior Spot Checks (PASS)

Verified behavior from captured `--help` output under `.qa/logs/20251227T060241Z_*_help.txt`:

- `delete` aliases: `delete, del` (confirmed from `.qa/logs/20251227T060241Z_pr-04d-init_delete_help.txt`)
- `target` has alias `t` (confirmed from `.qa/logs/20251227T060241Z_pr-04d-init_target_help.txt`)
- Note picker flags are present where expected:
  - `open` supports `--ls/--select` (confirmed from `.qa/logs/20251227T060241Z_pr-04d-init_open_help.txt`)
  - `create` supports `--ls/--select` (confirmed from `.qa/logs/20251227T060241Z_pr-04d-init_create_help.txt`)
  - `print` supports `--ls/--select` (confirmed from `.qa/logs/20251227T060241Z_pr-04d-init_print_help.txt`)
  - `frontmatter` supports `--ls/--select` (confirmed from `.qa/logs/20251227T060241Z_pr-04d-init_frontmatter_help.txt`)
