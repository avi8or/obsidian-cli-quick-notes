# pr-04d-init

Baseline: origin/main (1b06c32570b17684ba08b958f86368577e87fd20)
Previous: pr-04b-append (900d9baec9c05b5d63d7fe6999ee7509ddcbe782)
This PR: pr-04d-init (226e6c873e833c61a03558231a21dc6d6f918d3c)

## Incremental Diff (vs previous in stack)

Compare: pr-04b-append..pr-04d-init

### Commits
```
226e6c8 docs: update README for note picker and targets
39a9c56 feat(target): allow invoking targets by name
f083982 feat: add --ls/--select note picker
a251ea4 feat(append): select target via --select/--ls
b177d5d docs: add versioning note (pr-04d-init)
1f4a6eb docs: refresh README (pr-04d-init)
a6934da docs: note PR #59 compatibility
52f8fdb docs: README document frontmatter command
9eb89f6 feat: add frontmatter command for viewing and editing note metadata
6b4e28b docs: dedupe append section in README
1263511 docs: add acknowledgements for link-update work
783807c docs: add target/remove/test and append time-format examples
dbadbf1 docs: update README for init/append/targets and dry-run
7625bce chore(cli): improve usage/help text
4eb45d1 feat(cli): add dry-run to append/daily/delete
5be92e6 feat(target): improve edit prompt
b41c946 feat(wizard): add back/skip/help prompts
469f685 fix(init): add wizard UI helpers
4787252 feat(append): apply daily note template on create
0bf2948 chore(target): preserve targets.yaml header on save
2aaafd0 feat(cli): add interactive target editor
b5740c7 feat(cli): add target command
aff3c5b feat(target): add targets config and execution
368ef12 feat(obsidian): support obsidian-style date patterns
50c1a11 docs: README document delete confirmation and --force
abe6eb2 docs: acknowledge upstream PR #58 for link updates
382b61c docs: README clarify move link updates
```

### Diff Stat (all files)
```
 README.md                                          |  601 ++++-
 cmd/append.go                                      |  101 +-
 cmd/create.go                                      |   29 +-
 cmd/daily.go                                       |   21 +-
 cmd/delete.go                                      |   48 +-
 cmd/frontmatter.go                                 |   92 +
 cmd/init.go                                        |  734 ++++++
 cmd/move.go                                        |   67 +-
 cmd/note_picker.go                                 |  103 +
 cmd/open.go                                        |   32 +-
 cmd/print.go                                       |   29 +-
 cmd/root.go                                        |   42 +
 cmd/search.go                                      |   38 +-
 cmd/search_content.go                              |    2 +-
 cmd/set_default.go                                 |    2 +-
 cmd/target.go                                      | 1326 ++++++++++
 cmd/wizard_prompt.go                               |   44 +
 cmd/wizard_ui.go                                   |   28 +
 go.mod                                             |    3 +
 go.sum                                             |    6 +
 mocks/note.go                                      |    9 +
 pkg/actions/append.go                              |  143 +-
 pkg/actions/append_test.go                         |   44 +
 pkg/actions/daily.go                               |   15 +-
 pkg/actions/delete.go                              |   38 +-
 pkg/actions/delete_test.go                         |    7 +
 pkg/actions/frontmatter.go                         |  111 +
 pkg/actions/frontmatter_test.go                    |  237 ++
 pkg/actions/search_content_test.go                 |    9 +-
 pkg/actions/target.go                              |   94 +
 pkg/actions/target_test.go                         |   89 +
 pkg/config/targets_path.go                         |   15 +
 pkg/config/targets_path_test.go                    |   34 +
 pkg/frontmatter/frontmatter.go                     |  145 ++
 pkg/frontmatter/frontmatter_test.go                |  172 ++
 pkg/obsidian/date_format.go                        |  164 ++
 pkg/obsidian/date_format_test.go                   |   44 +
 pkg/obsidian/note.go                               |   40 +
 pkg/obsidian/path_safety.go                        |   37 +
 pkg/obsidian/path_safety_test.go                   |   37 +
 pkg/obsidian/targets.go                            |  298 +++
 pkg/obsidian/targets_test.go                       |   63 +
 pkg/obsidian/template.go                           |   61 +
 pkg/obsidian/template_test.go                      |   20 +
 vendor/github.com/BurntSushi/toml/.gitignore       |    5 +
 vendor/github.com/BurntSushi/toml/.travis.yml      |   15 +
 vendor/github.com/BurntSushi/toml/COMPATIBLE       |    3 +
 vendor/github.com/BurntSushi/toml/COPYING          |   21 +
 vendor/github.com/BurntSushi/toml/Makefile         |   19 +
 vendor/github.com/BurntSushi/toml/README.md        |  218 ++
 vendor/github.com/BurntSushi/toml/decode.go        |  509 ++++
 vendor/github.com/BurntSushi/toml/decode_meta.go   |  121 +
 vendor/github.com/BurntSushi/toml/doc.go           |   27 +
 vendor/github.com/BurntSushi/toml/encode.go        |  568 ++++
 .../github.com/BurntSushi/toml/encoding_types.go   |   19 +
 .../BurntSushi/toml/encoding_types_1.1.go          |   18 +
 vendor/github.com/BurntSushi/toml/lex.go           |  953 +++++++
 vendor/github.com/BurntSushi/toml/parse.go         |  592 +++++
 vendor/github.com/BurntSushi/toml/session.vim      |    1 +
 vendor/github.com/BurntSushi/toml/type_check.go    |   91 +
 vendor/github.com/BurntSushi/toml/type_fields.go   |  242 ++
 .../github.com/adrg/frontmatter/CODE_OF_CONDUCT.md |   77 +
 vendor/github.com/adrg/frontmatter/CONTRIBUTING.md |  135 +
 vendor/github.com/adrg/frontmatter/LICENSE         |   21 +
 vendor/github.com/adrg/frontmatter/README.md       |  170 ++
 vendor/github.com/adrg/frontmatter/format.go       |   70 +
 vendor/github.com/adrg/frontmatter/frontmatter.go  |   47 +
 vendor/github.com/adrg/frontmatter/parser.go       |  132 +
 vendor/gopkg.in/yaml.v2/.travis.yml                |   16 +
 vendor/gopkg.in/yaml.v2/LICENSE                    |  201 ++
 vendor/gopkg.in/yaml.v2/LICENSE.libyaml            |   31 +
 vendor/gopkg.in/yaml.v2/NOTICE                     |   13 +
 vendor/gopkg.in/yaml.v2/README.md                  |  133 +
 vendor/gopkg.in/yaml.v2/apic.go                    |  740 ++++++
 vendor/gopkg.in/yaml.v2/decode.go                  |  815 ++++++
 vendor/gopkg.in/yaml.v2/emitterc.go                | 1685 ++++++++++++
 vendor/gopkg.in/yaml.v2/encode.go                  |  390 +++
 vendor/gopkg.in/yaml.v2/parserc.go                 | 1095 ++++++++
 vendor/gopkg.in/yaml.v2/readerc.go                 |  412 +++
 vendor/gopkg.in/yaml.v2/resolve.go                 |  258 ++
 vendor/gopkg.in/yaml.v2/scannerc.go                | 2711 ++++++++++++++++++++
 vendor/gopkg.in/yaml.v2/sorter.go                  |  113 +
 vendor/gopkg.in/yaml.v2/writerc.go                 |   26 +
 vendor/gopkg.in/yaml.v2/yaml.go                    |  466 ++++
 vendor/gopkg.in/yaml.v2/yamlh.go                   |  739 ++++++
 vendor/gopkg.in/yaml.v2/yamlprivateh.go            |  173 ++
 vendor/modules.txt                                 |    9 +
 87 files changed, 19261 insertions(+), 113 deletions(-)
```

### Diff Stat (vendor only)
```
 vendor/github.com/BurntSushi/toml/.gitignore       |    5 +
 vendor/github.com/BurntSushi/toml/.travis.yml      |   15 +
 vendor/github.com/BurntSushi/toml/COMPATIBLE       |    3 +
 vendor/github.com/BurntSushi/toml/COPYING          |   21 +
 vendor/github.com/BurntSushi/toml/Makefile         |   19 +
 vendor/github.com/BurntSushi/toml/README.md        |  218 ++
 vendor/github.com/BurntSushi/toml/decode.go        |  509 ++++
 vendor/github.com/BurntSushi/toml/decode_meta.go   |  121 +
 vendor/github.com/BurntSushi/toml/doc.go           |   27 +
 vendor/github.com/BurntSushi/toml/encode.go        |  568 ++++
 .../github.com/BurntSushi/toml/encoding_types.go   |   19 +
 .../BurntSushi/toml/encoding_types_1.1.go          |   18 +
 vendor/github.com/BurntSushi/toml/lex.go           |  953 +++++++
 vendor/github.com/BurntSushi/toml/parse.go         |  592 +++++
 vendor/github.com/BurntSushi/toml/session.vim      |    1 +
 vendor/github.com/BurntSushi/toml/type_check.go    |   91 +
 vendor/github.com/BurntSushi/toml/type_fields.go   |  242 ++
 .../github.com/adrg/frontmatter/CODE_OF_CONDUCT.md |   77 +
 vendor/github.com/adrg/frontmatter/CONTRIBUTING.md |  135 +
 vendor/github.com/adrg/frontmatter/LICENSE         |   21 +
 vendor/github.com/adrg/frontmatter/README.md       |  170 ++
 vendor/github.com/adrg/frontmatter/format.go       |   70 +
 vendor/github.com/adrg/frontmatter/frontmatter.go  |   47 +
 vendor/github.com/adrg/frontmatter/parser.go       |  132 +
 vendor/gopkg.in/yaml.v2/.travis.yml                |   16 +
 vendor/gopkg.in/yaml.v2/LICENSE                    |  201 ++
 vendor/gopkg.in/yaml.v2/LICENSE.libyaml            |   31 +
 vendor/gopkg.in/yaml.v2/NOTICE                     |   13 +
 vendor/gopkg.in/yaml.v2/README.md                  |  133 +
 vendor/gopkg.in/yaml.v2/apic.go                    |  740 ++++++
 vendor/gopkg.in/yaml.v2/decode.go                  |  815 ++++++
 vendor/gopkg.in/yaml.v2/emitterc.go                | 1685 ++++++++++++
 vendor/gopkg.in/yaml.v2/encode.go                  |  390 +++
 vendor/gopkg.in/yaml.v2/parserc.go                 | 1095 ++++++++
 vendor/gopkg.in/yaml.v2/readerc.go                 |  412 +++
 vendor/gopkg.in/yaml.v2/resolve.go                 |  258 ++
 vendor/gopkg.in/yaml.v2/scannerc.go                | 2711 ++++++++++++++++++++
 vendor/gopkg.in/yaml.v2/sorter.go                  |  113 +
 vendor/gopkg.in/yaml.v2/writerc.go                 |   26 +
 vendor/gopkg.in/yaml.v2/yaml.go                    |  466 ++++
 vendor/gopkg.in/yaml.v2/yamlh.go                   |  739 ++++++
 vendor/gopkg.in/yaml.v2/yamlprivateh.go            |  173 ++
 vendor/modules.txt                                 |    9 +
 43 files changed, 14100 insertions(+)
```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index 58ff503..6b0d9f2 100644
--- a/README.md
+++ b/README.md
@@ -19,6 +19,7 @@ Available Commands:
   daily          Creates or opens daily note in vault
   delete         Delete note in vault
   help           Help about any command
+  init           Interactive setup wizard
   move           Move or rename note in vault and update corresponding links
   open           Opens note in vault by note name
   print          Print contents of note
@@ -26,6 +27,7 @@ Available Commands:
   search         Fuzzy searches and opens note in vault
   search-content Search note content for search term
   set-default    Sets default vault
+  target         Append text to a configured target note
 
 Flags:
   -h, --help      help for obsidian-cli
@@ -36,8 +38,18 @@ Use "obsidian-cli [command] --help" for more information about a command.
 
 ## Description
 
-Obsidian is a powerful and extensible knowledge base application
-that works on top of your local folder of plain text notes. This CLI tool (written in Go) will let you interact with the application using the terminal. You are currently able to open, search, move, create, update and delete notes.
+Obsidian is a powerful and extensible knowledge base application that works on top of your local folder of plain text notes.
+This CLI tool (written in Go) lets you interact with Obsidian from the terminal:
+
+- Open/search/create/move/delete notes
+- Open daily notes
+- Append to daily notes from the CLI
+- Capture into named “targets” configured in `targets.yaml`
+- Guided `init` wizard to set up defaults
+
+## Versioning (Maintainer Note)
+
+This PR keeps `obsidian-cli` at `v0.2.0` to avoid unnecessary merge/rebase churn across a stacked PR chain. If you merge the full chain (or the “just merge it” option), consider doing a single version bump afterward (for example to `v0.3.0`) in a follow-up PR/release.
 
 ---
 
@@ -78,12 +90,60 @@ For full installation instructions, see [Mac and Linux manual](https://yakitrak.
 obsidian-cli --help
 ```
 
-For detailed help (including examples) for a specific command:
+### Quickstart (Recommended)
+
+Run the interactive wizard:
 
 ```bash
-obsidian-cli <command> --help
+obsidian-cli init
 ```
 
+The wizard:
+
+- Selects and saves your default vault (`set-default`)
+- Configures per-vault daily note settings (used by `append`)
+- Offers to set up/migrate `targets.yaml`, and optionally add your first target
+
+Most wizard prompts accept:
+
+- `?` for help
+- `back` to go back / cancel
+- `skip` to accept defaults where applicable
+
+<details>
+<summary><code>init</code> command reference (help, flags, examples)</summary>
+
+```text
+$ obsidian-cli init --help
+Interactive setup wizard to configure your default vault, daily note settings, and targets.
+
+Usage:
+  obsidian-cli init [flags]
+
+Examples:
+  obsidian-cli init
+
+Flags:
+  -h, --help   help for init
+```
+
+</details>
+
+### Interactive Workflows
+
+Many commands support “guided” behavior when you omit arguments/flags:
+
+- `init` is a full interactive wizard (and supports a fuzzy finder when choosing a vault).
+- `append` and `target` read content from stdin (piped), or prompt for multi-line input until EOF (Ctrl-D to save, Ctrl-C to cancel).
+- `target add` runs a guided workflow when you omit the `[name]`.
+- `delete` prompts for confirmation if the note has incoming links (use `--force` to skip).
+- `open`, `create`, and `print` can prompt you to pick a note (or type a new one) when you omit the note path.
+- `delete`, `frontmatter`, and `move` support `--ls` / `--select` to pick an existing note interactively.
+
+The fuzzy finder (used by `search`, `search-content`, `open`, `create`, `print`, `frontmatter --ls`, `delete --ls`, `move --ls`, and `target --select`) lets you type to filter, then press Enter to choose a result. Press Esc, Ctrl-C, or Ctrl-D to abort the selection.
+
+In the note picker used by `search`, `open`, and `create`, there’s a “Create new note…” option which lets you choose an existing folder (or create a new folder), then type the note name.
+
 ### Command Shortcut (Alias)
 
 If you want a shorter command name (for example `obsi`), you can either:
@@ -244,6 +304,7 @@ Then you can use `obs_cd` to navigate to the default vault directory within your
 `obsidian-cli` reads and writes configuration under your OS user config directory (`os.UserConfigDir()`):
 
 - `obsidian-cli/preferences.json` (default vault name + optional per-vault `vault_settings`)
+- `obsidian-cli/targets.yaml` (capture targets, used by `target`)
 
 It also reads Obsidian’s vault list from:
 
@@ -253,22 +314,60 @@ Note: when writing `preferences.json`, the CLI attempts to create the config dir
 
 ### Open Note
 
-Open given note name in Obsidian. Note can also be an absolute path from top level of vault.
+Open a note in Obsidian by vault-relative note path (or pick one interactively with `--ls` / `--select`).
 
 ```bash
 # Opens note in obsidian vault
 obsidian-cli open "{note-name}"
 
+# Pick (or create) a note path interactively
+obsidian-cli open --ls
+
 # Opens note in specified obsidian vault
 obsidian-cli open "{note-name}" --vault "{vault-name}"
 
 ```
 
+<details>
+<summary><code>open</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli open --help
+Opens a note in Obsidian by name or path.
+
+The note name can be just the filename or a path relative to the vault root.
+The .md extension is optional.
+
+Usage:
+  obsidian-cli open [note-path] [flags]
+
+Aliases:
+  open, o
+
+Examples:
+  # Open a note by name
+  obsidian-cli open "Meeting Notes"
+
+  # Open a note in a subfolder
+  obsidian-cli open "Projects/my-project"
+
+  # Open in a specific vault
+  obsidian-cli open "Daily" --vault "Work"
+
+Flags:
+  -h, --help           help for open
+      --ls             select a note interactively
+      --select         select a note interactively
+  -v, --vault string   vault name (not required if default is set)
+```
+
+</details>
+
 ### Daily Note
 
 Open the daily note in Obsidian (via Obsidian URI).
 
-Note: creation/templates are controlled by Obsidian’s daily note settings/plugins.
+Note: creation/templates are controlled by Obsidian’s daily note settings/plugins. Use `append` (below) if you want the CLI to write the daily note Markdown file directly.
 
 ```bash
 # Creates / opens daily note in obsidian vault
@@ -277,16 +376,18 @@ obsidian-cli daily
 # Creates / opens daily note in specified obsidian vault
 obsidian-cli daily --vault "{vault-name}"
 
+# Print the Obsidian URI (does not open Obsidian)
+obsidian-cli daily --dry-run
+
 ```
 
 ### Append to Daily Note
 
-PR04b adds an `append` command which **writes the daily note Markdown file directly** (no Obsidian URI). The daily note path is derived from per-vault settings in `preferences.json`:
+Append text to today’s daily note **by writing the Markdown file directly** using your per-vault settings in `preferences.json` (`daily_note.folder`, `daily_note.filename_pattern`, and optional `daily_note.template_path`).
 
-- `vault_settings.{vault}.daily_note.folder` (required)
-- `vault_settings.{vault}.daily_note.filename_pattern` (optional; defaults to `{YYYY-MM-DD}`)
+If you provide no text, content is read from stdin (piped) or entered interactively until EOF (Ctrl-D to save, Ctrl-C to cancel).
 
-If you omit the `[text]` argument, `append` reads content from stdin (piped) or prompts for multi-line input until EOF (Ctrl-D).
+Tip: if you already use targets, `append --select` / `append --ls` is a convenience shortcut that selects a target from `targets.yaml` and appends to that target instead of the daily note.
 
 ```bash
 # Append a one-liner
@@ -295,15 +396,22 @@ obsidian-cli append "Meeting notes: discussed roadmap"
 # Multi-line content interactively (Ctrl-D to save, Ctrl-C to cancel)
 obsidian-cli append
 
+# Select a target, then append to it (shortcut for: obsidian-cli target --select)
+obsidian-cli append --select
+obsidian-cli append --ls "Buy milk"
+
 # Pipe content
 printf "line1\nline2\n" | obsidian-cli append
 
-# Append with a timestamp prefix (default format 15:04)
+# Append with timestamp
 obsidian-cli append --timestamp "Started work on feature X"
 
-# Append with a custom timestamp format (Go time format)
+# Append with timestamp + custom format (Go time format)
 obsidian-cli append --timestamp --time-format "15:04:05" "Did the thing"
 
+# Preview which file would be written (does not write)
+obsidian-cli append --dry-run "hello"
+
 # Append in a specific vault
 obsidian-cli append --vault "{vault-name}" "Daily standup notes"
 ```
@@ -318,6 +426,8 @@ Appends text to today's daily note.
 This command writes to a daily note path derived from your per-vault settings
 in preferences.json (daily_note.folder and daily_note.filename_pattern).
 
+If you prefer, you can use --select/--ls to pick a target from targets.yaml and append to that note instead.
+
 If no text argument is provided, content is read from stdin (piped) or entered
 interactively until EOF.
 
@@ -334,6 +444,9 @@ Examples:
   # Append multi-line content interactively (Ctrl-D to save)
   obsidian-cli append
 
+  # Pick a target interactively, then enter content
+  obsidian-cli append --select
+
   # Append with timestamp
   obsidian-cli append --timestamp "Started work on feature X"
 
@@ -341,7 +454,10 @@ Examples:
   obsidian-cli append --vault "Work" "Daily standup notes"
 
 Flags:
+      --dry-run              preview which note would be written without writing
   -h, --help                 help for append
+      --ls                   select a target interactively (targets.yaml)
+      --select               select a target interactively (targets.yaml)
       --time-format string   custom timestamp format (Go time format, default: 15:04)
   -t, --timestamp            prepend a timestamp to the content
   -v, --vault string         vault name (not required if default is set)
@@ -349,9 +465,245 @@ Flags:
 
 </details>
 
+### Target (Quick Capture)
+
+Targets let you define named shortcuts for capturing into specific notes.
+
+Targets are configured in `targets.yaml` (stored next to `preferences.json`), and can point at:
+
+- A fixed file path (always append to the same note)
+- A folder + filename pattern (append to a dated note based on the current time)
+
+Common workflows:
+
+```bash
+# Guided target creation workflow
+obsidian-cli target add
+
+# Short alias for `target`
+obsidian-cli t inbox "Buy milk"
+
+# You can also invoke a target name directly (the CLI routes it to `target <name> ...`)
+obsidian-cli inbox "Buy milk"
+
+# Capture a one-liner to a target
+obsidian-cli target inbox "Buy milk"
+
+# Multi-line content (Ctrl-D to save, Ctrl-C to cancel)
+obsidian-cli target inbox
+
+# Pick a target interactively, then enter content
+obsidian-cli target --select
+
+# Alias for --select
+obsidian-cli target --ls
+
+# Preview which file would be used (does not write)
+obsidian-cli target inbox --dry-run
+
+# Preview resolved paths for one or all targets
+obsidian-cli target test inbox
+obsidian-cli target test
+
+# List targets
+obsidian-cli target list
+
+# Remove a target
+obsidian-cli target remove inbox
+obsidian-cli target rm inbox
+
+# Edit targets (choose CLI mode or open targets.yaml in your editor)
+obsidian-cli target edit
+
+# Run a target using a specific vault (unless the target has its own vault override)
+obsidian-cli target --vault "{vault-name}" inbox "hello"
+```
+
+Minimal `targets.yaml` examples:
+
+```yaml
+# Fixed-file target
+inbox:
+  type: file
+  file: Inbox.md
+
+# Folder + pattern target
+log:
+  type: folder
+  folder: Log
+  pattern: YYYY-MM-DD
+
+# Folder target with a template and per-target vault override
+worklog:
+  type: folder
+  folder: Log
+  pattern: YYYY-MM-DD
+  template: Templates/Daily
+  vault: Work
+```
+
+Notes:
+
+- A simplified scalar form is also accepted and can be migrated by `init` / `target edit`:
+  - `inbox: Inbox.md`
+- The legacy `note` key is treated as `file` (file target).
+- Target names cannot contain whitespace, and some names are reserved:
+  - `add`, `remove`, `rm`, `list`, `ls`, `edit`, `validate`, `test`, `help`
+
+<details>
+<summary><code>target</code> command reference (help, flags, subcommands)</summary>
+
+```text
+$ obsidian-cli target --help
+Appends text to a note configured in targets.yaml.
+
+Targets can point at:
+  - a fixed file path (always append to the same note)
+  - a folder + filename pattern (append to a dated note based on the current time)
+
+If no text is provided, content is read from stdin (piped) or entered interactively until EOF.
+
+Usage:
+  obsidian-cli target [id] [text] [flags]
+  obsidian-cli target [command]
+
+Aliases:
+  target, t
+
+Examples:
+  # Append a one-liner to a target
+  obsidian-cli target inbox "Buy milk"
+
+  # Multi-line content (Ctrl-D to save, Ctrl-C to cancel)
+  obsidian-cli target inbox
+
+  # Pick a target interactively, then enter content
+  obsidian-cli target --select
+
+  # Preview which file would be used
+  obsidian-cli target inbox --dry-run
+
+Available Commands:
+  add         Add a new target
+  edit        Edit targets in CLI or open targets.yaml
+  list        List configured targets
+  remove      Remove a target
+  test        Preview the resolved path for a target
+
+Flags:
+      --dry-run        preview the resolved target path without writing
+  -h, --help           help for target
+      --ls             select a target interactively
+      --select         select a target interactively
+  -v, --vault string   vault name (not required if default is set)
+
+Use "obsidian-cli target [command] --help" for more information about a command.
+```
+
+</details>
+
+<details>
+<summary><code>target</code> subcommand references (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli target add --help
+Add a new capture target.
+
+Run without a name to start a guided workflow.
+
+Usage:
+  obsidian-cli target add [name] [flags]
+
+Examples:
+  # Guided workflow
+  obsidian-cli target add
+
+  # Add a fixed-file target
+  obsidian-cli target add inbox
+
+  # Add a folder+pattern target
+  obsidian-cli target add log
+
+Flags:
+  -h, --help   help for add
+```
+
+```text
+$ obsidian-cli target remove --help
+Remove a target
+
+Usage:
+  obsidian-cli target remove [name] [flags]
+
+Aliases:
+  remove, rm
+
+Flags:
+  -h, --help   help for remove
+```
+
+```text
+$ obsidian-cli target list --help
+List configured targets
+
+Usage:
+  obsidian-cli target list [flags]
+
+Flags:
+  -h, --help   help for list
+```
+
+```text
+$ obsidian-cli target edit --help
+Edit targets in CLI or open targets.yaml
+
+Usage:
+  obsidian-cli target edit [flags]
+
+Flags:
+  -h, --help   help for edit
+```
+
+```text
+$ obsidian-cli target test --help
+Shows which file would be created or appended to for the given target.
+
+If no name is provided, previews all targets.
+
+Usage:
+  obsidian-cli target test [name] [flags]
+
+Flags:
+  -h, --help   help for test
+```
+
+</details>
+
+### Date Patterns and Template Variables
+
+Date patterns (used by daily note filename patterns and folder targets) support Obsidian-style tokens, legacy `{brace}` patterns, and `[literal]` blocks:
+
+- Tokens (curated subset): `YYYY`, `YY`, `MMMM`, `MMM`, `MM`, `M`, `DD`, `D`, `dddd`, `ddd`, `HH`, `H`, `hh`, `h`, `mm`, `m`, `ss`, `s`, `A`, `a`, `ZZ`, `Z`, `z`
+- Legacy brace patterns: `{YYYY-MM-DD}` and `{YYYY-MM-DD-HHmmss}` (braces are ignored)
+- Zettel timestamp: `YYYYMMDDHHmmss` (with or without braces)
+- Literal blocks: wrap text in `[brackets]`, e.g. `YYYY-[log]-MM`
+
+Templates (used when `append` or `target` creates a note that doesn’t exist yet) support:
+
+- `{{title}}`
+- `{{date}}` / `{{date:FORMAT}}`
+- `{{time}}` / `{{time:FORMAT}}`
+
+Example template snippet:
+
+```text
+Title={{title}}
+Created={{date:YYYY-MM-DD}} {{time:HH:mm}}
+```
+
 ### Search Note
 
-Starts a fuzzy search displaying notes in the terminal from the vault. You can hit enter on a note to open that in Obsidian.
+Starts a fuzzy search displaying notes in the terminal from the vault. Press Enter on a note to open it in Obsidian (or choose “Create new note…” to pick/create a folder and type a new note name).
 
 ```bash
 # Searches in default obsidian vault
@@ -383,12 +735,15 @@ obsidian-cli search-content "search term" --editor
 
 ### Print Note
 
-Prints the contents of given note name or path in Obsidian.
+Prints the contents of a note to stdout (useful for piping to other commands).
 
 ```bash
 # Prints note in default vault
 obsidian-cli print "{note-name}"
 
+# Pick a note interactively
+obsidian-cli print --ls
+
 # Prints note by path in default vault
 obsidian-cli print "{note-path}"
 
@@ -397,15 +752,58 @@ obsidian-cli print "{note-name}" --vault "{vault-name}"
 
 ```
 
+<details>
+<summary><code>print</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli print --help
+Prints the contents of a note to stdout.
+
+Useful for piping note contents to other commands, or quickly viewing
+a note without opening Obsidian.
+
+Usage:
+  obsidian-cli print [note-path] [flags]
+
+Aliases:
+  print, p
+
+Examples:
+  # Print a note
+  obsidian-cli print "Meeting Notes"
+
+  # Print note in subfolder
+  obsidian-cli print "Projects/readme"
+
+  # Pipe to grep
+  obsidian-cli print "Todo" | grep "TODO"
+
+  # Copy to clipboard (macOS)
+  obsidian-cli print "Template" | pbcopy
+
+Flags:
+  -h, --help           help for print
+      --ls             select a note interactively
+      --select         select a note interactively
+  -v, --vault string   vault name
+```
+
+</details>
+
 ### Create / Update Note
 
-Creates note (can also be a path with name) in vault. By default, if the note exists, it will create another note but passing `--overwrite` or `--append` can be used to edit the named note.
+Creates a note (can be a path from the top level of the vault). By default, if the note exists, it will create another note; passing `--overwrite` or `--append` changes that behavior.
+
+Note: `--editor` only applies when `--open` is also provided.
 
 ```bash
-# Creates empty note in default obsidian and opens it
+# Creates empty note in default obsidian (does not open unless --open is used)
 obsidian-cli create "{note-name}"
 
-# Creates empty note in given obsidian and opens it
+# Pick (or create) a note path interactively
+obsidian-cli create --ls
+
+# Creates empty note in given obsidian
 obsidian-cli create "{note-name}"  --vault "{vault-name}"
 
 # Creates note in default obsidian with content
@@ -425,24 +823,166 @@ obsidian-cli create "{note-name}" --content "abcde" --open --editor
 
 ```
 
+<details>
+<summary><code>create</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli create --help
+Creates a new note in your Obsidian vault.
+
+By default, if the note already exists, Obsidian will create a new note
+with a numeric suffix. Use --append to add to an existing note, or
+--overwrite to replace its contents.
+
+Usage:
+  obsidian-cli create [note-path] [flags]
+
+Aliases:
+  create, c
+
+Examples:
+  # Create an empty note
+  obsidian-cli create "New Note"
+
+  # Create with content
+  obsidian-cli create "Ideas" --content "My brilliant idea"
+
+  # Append to existing note
+  obsidian-cli create "Log" --content "Entry" --append
+
+  # Create and open in Obsidian
+  obsidian-cli create "Draft" --open
+
+  # Create and open in $EDITOR
+  obsidian-cli create "Draft" --open --editor
+
+Flags:
+  -a, --append           append to note
+  -c, --content string   text to add to note
+  -e, --editor           open in editor instead of Obsidian (requires --open flag)
+  -h, --help             help for create
+      --ls               select a note interactively
+      --open             open created note
+  -o, --overwrite        overwrite note
+      --select           select a note interactively
+  -v, --vault string     vault name
+```
+
+</details>
+
 ### Move / Rename Note
 
-Moves a given note(path from top level of vault) with new name given (top level of vault). If given same path but different name then its treated as a rename. All links inside vault are updated to match new name.
+Moves a note to a new path (or renames it) and updates links inside the vault to match.
+
+Note: `--editor` only applies when `--open` is also provided.
 
 ```bash
 # Renames a note in default obsidian
 obsidian-cli move "{current-note-path}" "{new-note-path}"
 
+# Pick the note to move interactively
+obsidian-cli move --ls "{new-note-path}"
+
 # Renames a note and given obsidian
 obsidian-cli move "{current-note-path}" "{new-note-path}" --vault "{vault-name}"
 
 # Renames a note in default obsidian and opens it
 obsidian-cli move "{current-note-path}" "{new-note-path}" --open
 
-# Renames a note and opens it in your default editor
-obsidian-cli move "{current-note-path}" "{new-note-path}" --open --editor
+	# Renames a note and opens it in your default editor
+	obsidian-cli move "{current-note-path}" "{new-note-path}" --open --editor
+```
+
+<details>
+<summary><code>move</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli move --help
+Moves or renames a note and updates all links pointing to it.
+
+This command safely renames notes by also updating any [[wikilinks]]
+or [markdown](links) that reference the moved note.
+
+Usage:
+  obsidian-cli move [from-note-path] [to-note-path] [flags]
+
+Aliases:
+  move, m
+
+Examples:
+  # Rename a note
+  obsidian-cli move "Old Name" "New Name"
+
+  # Move to a different folder
+  obsidian-cli move "Inbox/note" "Projects/note"
+
+  # Move and open the result
+  obsidian-cli move "temp" "Archive/temp" --open
+
+Flags:
+  -e, --editor         open in editor instead of Obsidian (requires --open flag)
+  -h, --help           help for move
+      --ls             select the note to move interactively
+  -o, --open           open new note
+      --select         select the note to move interactively
+  -v, --vault string   vault name
+```
+
+</details>
+
+### Frontmatter
+
+View or edit a note’s YAML frontmatter.
+
+```bash
+# Print frontmatter
+obsidian-cli frontmatter "My Note" --print
+
+# Edit a key
+obsidian-cli frontmatter "My Note" --edit --key "status" --value "done"
+
+# Delete a key
+obsidian-cli frontmatter "My Note" --delete --key "draft"
+
+# Pick a note interactively
+obsidian-cli frontmatter --ls
 ```
 
+<details>
+<summary><code>frontmatter</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli frontmatter --help
+View or modify YAML frontmatter in a note.
+
+Use --print to display frontmatter, --edit to modify a key,
+or --delete to remove a key.
+
+Examples:
+  obsidian-cli frontmatter "My Note" --print
+  obsidian-cli frontmatter "My Note" --edit --key "status" --value "done"
+  obsidian-cli frontmatter "My Note" --delete --key "draft"
+
+Usage:
+  obsidian-cli frontmatter [note] [flags]
+
+Aliases:
+  frontmatter, fm
+
+Flags:
+  -d, --delete         delete a frontmatter key
+  -e, --edit           edit a frontmatter key
+  -h, --help           help for frontmatter
+  -k, --key string     key to edit or delete
+      --ls             select a note interactively
+  -p, --print          print frontmatter
+      --select         select a note interactively
+      --value string   value to set (required for --edit)
+  -v, --vault string   vault name
+```
+
+</details>
+
 ### Delete Note
 
 Deletes a given note (path from top level of vault).
@@ -452,14 +992,20 @@ If other notes link to the note, `delete` prints the incoming links and prompts
 Use `--force` (`-f`) to skip confirmation (recommended for scripts). Alias: `delete, del`. Heads up: `daily` uses alias `d`, so `delete` uses `del` to avoid ambiguity.
 
 ```bash
-# Delete a note in default obsidian
+# Delete a note in default obsidian vault
 obsidian-cli delete "{note-path}"
 
+# Pick a note interactively (existing notes only)
+obsidian-cli delete --ls
+
+# Delete a note in given obsidian vault
+obsidian-cli delete "{note-path}" --vault "{vault-name}"
+
 # Force delete without prompt
 obsidian-cli delete "{note-path}" --force
 
-# Delete a note in given obsidian
-obsidian-cli delete "{note-path}" --vault "{vault-name}"
+# Preview which file would be deleted (does not delete)
+obsidian-cli delete --dry-run "{note-path}"
 ```
 
 <details>
@@ -473,7 +1019,7 @@ If other notes link to the note, you'll be prompted to confirm.
 Use --force to skip confirmation (recommended for scripts).
 
 Usage:
-  obsidian-cli delete <note> [flags]
+  obsidian-cli delete [note-path] [flags]
 
 Aliases:
   delete, del
@@ -489,8 +1035,11 @@ Examples:
   obsidian-cli delete "note" --vault "Archive"
 
 Flags:
+      --dry-run        preview which file would be deleted without deleting it
   -f, --force          skip confirmation if the note has incoming links
   -h, --help           help for delete
+      --ls             select a note interactively
+      --select         select a note interactively
   -v, --vault string   vault name
 ```
 
@@ -500,6 +1049,10 @@ Flags:
 
 Fork the project, add your feature or fix and submit a pull request. You can also open an [issue](https://github.com/yakitrak/obsidian-cli/issues/new/choose) to report a bug or request a feature.
 
+## Acknowledgements
+
+The note move/rename link-update improvements proposed in this repo build on the approach from Logan McDuffie’s fix for issue #44 (`tidalstudio/fix/issue-44-link-updates`).
+
 ## License
 
 Available under [MIT License](./LICENSE)
diff --git a/cmd/append.go b/cmd/append.go
index cf40752..17b3c03 100644
--- a/cmd/append.go
+++ b/cmd/append.go
@@ -1,7 +1,10 @@
 package cmd
 
 import (
+	"errors"
 	"fmt"
+	"io"
+	"os"
 	"strings"
 	"time"
 
@@ -13,17 +16,21 @@ import (
 var (
 	appendTimestamp bool
 	appendTimeFmt   string
+	appendDryRun    bool
+	appendSelect    bool
 )
 
 var appendCmd = &cobra.Command{
 	Use:     "append [text]",
 	Aliases: []string{"a"},
-	Short:   "Append text to today's daily note",
+	Short:   "Append text to today's daily note (or select a target)",
 	Long: `Appends text to today's daily note.
 
 This command writes to a daily note path derived from your per-vault settings
 in preferences.json (daily_note.folder and daily_note.filename_pattern).
 
+If you prefer, you can use --select/--ls to pick a target from targets.yaml and append to that note instead.
+
 If no text argument is provided, content is read from stdin (piped) or entered
 interactively until EOF.`,
 	Example: `  # Append a one-liner
@@ -32,6 +39,9 @@ interactively until EOF.`,
   # Append multi-line content interactively (Ctrl-D to save)
   obsidian-cli append
 
+  # Pick a target interactively, then enter content
+  obsidian-cli append --select
+
   # Append with timestamp
   obsidian-cli append --timestamp "Started work on feature X"
 
@@ -41,11 +51,68 @@ interactively until EOF.`,
 	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 
+		if appendSelect {
+			targetName, err := pickTargetName()
+			if err != nil {
+				return err
+			}
+			if strings.TrimSpace(targetName) == "" {
+				return errors.New("no target selected")
+			}
+
+			content := strings.TrimSpace(strings.Join(args, " "))
+			if content == "" {
+				stat, err := os.Stdin.Stat()
+				if err == nil && (stat.Mode()&os.ModeCharDevice) == 0 {
+					b, err := io.ReadAll(os.Stdin)
+					if err != nil {
+						return err
+					}
+					content = strings.TrimSpace(string(b))
+				} else if !appendDryRun {
+					content, err = actions.PromptForContentIfEmpty(content)
+					if err != nil {
+						return err
+					}
+				}
+			}
+
+			if appendTimestamp {
+				format := appendTimeFmt
+				if strings.TrimSpace(format) == "" {
+					format = "15:04"
+				}
+				content = fmt.Sprintf("- %s %s", time.Now().Format(format), content)
+			}
+
+			if appendDryRun {
+				return printTargetPlan(&vault, targetName)
+			}
+
+			plan, err := actions.AppendToTarget(&vault, targetName, content, time.Now(), false)
+			if err != nil {
+				return err
+			}
+			fmt.Printf("Wrote to: %s\n", plan.AbsoluteNotePath)
+			return nil
+		}
+
 		content := strings.TrimSpace(strings.Join(args, " "))
-		var err error
-		content, err = actions.PromptForContentIfEmpty(content)
-		if err != nil {
-			return err
+		if content == "" {
+			stat, err := os.Stdin.Stat()
+			if err == nil && (stat.Mode()&os.ModeCharDevice) == 0 {
+				b, err := io.ReadAll(os.Stdin)
+				if err != nil {
+					return err
+				}
+				content = strings.TrimSpace(string(b))
+			} else if !appendDryRun {
+				var err error
+				content, err = actions.PromptForContentIfEmpty(content)
+				if err != nil {
+					return err
+				}
+			}
 		}
 
 		if appendTimestamp {
@@ -56,13 +123,37 @@ interactively until EOF.`,
 			content = fmt.Sprintf("- %s %s", time.Now().Format(format), content)
 		}
 
+		if appendDryRun {
+			plan, err := actions.PlanDailyAppend(&vault, time.Now())
+			if err != nil {
+				return err
+			}
+			fmt.Println("Append dry-run:")
+			fmt.Printf("  vault: %s\n", plan.VaultName)
+			fmt.Printf("  note: %s\n", plan.AbsoluteNotePath)
+			fmt.Printf("  create_dirs: %t\n", plan.WillCreateDirs)
+			fmt.Printf("  create_file: %t\n", plan.WillCreateFile)
+			if plan.WillApplyTemplate {
+				fmt.Printf("  template: %s\n", plan.TemplateAbs)
+			}
+			if strings.TrimSpace(content) == "" {
+				fmt.Println("  content: (none supplied; would prompt interactively without --dry-run)")
+			} else {
+				fmt.Printf("  append_bytes: %d\n", len([]byte(content)))
+			}
+			return nil
+		}
+
 		return actions.AppendToDailyNote(&vault, content)
 	},
 }
 
 func init() {
+	appendCmd.Flags().BoolVar(&appendSelect, "ls", false, "select a target interactively (targets.yaml)")
+	appendCmd.Flags().BoolVar(&appendSelect, "select", false, "select a target interactively (targets.yaml)")
 	appendCmd.Flags().BoolVarP(&appendTimestamp, "timestamp", "t", false, "prepend a timestamp to the content")
 	appendCmd.Flags().StringVar(&appendTimeFmt, "time-format", "", "custom timestamp format (Go time format, default: 15:04)")
+	appendCmd.Flags().BoolVar(&appendDryRun, "dry-run", false, "preview which note would be written without writing")
 	appendCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
 	rootCmd.AddCommand(appendCmd)
 }
diff --git a/cmd/create.go b/cmd/create.go
index f283518..8cad0c3 100644
--- a/cmd/create.go
+++ b/cmd/create.go
@@ -1,6 +1,7 @@
 package cmd
 
 import (
+	"errors"
 	"fmt"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
@@ -11,8 +12,9 @@ import (
 var shouldAppend bool
 var shouldOverwrite bool
 var content string
+var createSelect bool
 var createNoteCmd = &cobra.Command{
-	Use:     "create <note>",
+	Use:     "create [note-path]",
 	Aliases: []string{"c"},
 	Short:   "Creates note in vault",
 	Long: `Creates a new note in your Obsidian vault.
@@ -34,11 +36,30 @@ with a numeric suffix. Use --append to add to an existing note, or
 
   # Create and open in $EDITOR
   obsidian-cli create "Draft" --open --editor`,
-	Args: cobra.ExactArgs(1),
+	Args: cobra.MaximumNArgs(1),
 	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
-		noteName := args[0]
+		noteName := ""
+		if len(args) > 0 && !createSelect {
+			noteName = args[0]
+		} else {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickNotePathOrNew(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+		if noteName == "" {
+			return errors.New("no note selected")
+		}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
 			return fmt.Errorf("failed to parse --editor flag: %w", err)
@@ -57,6 +78,8 @@ with a numeric suffix. Use --append to add to an existing note, or
 
 func init() {
 	createNoteCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	createNoteCmd.Flags().BoolVar(&createSelect, "ls", false, "select a note interactively")
+	createNoteCmd.Flags().BoolVar(&createSelect, "select", false, "select a note interactively")
 	createNoteCmd.Flags().BoolVarP(&shouldOpen, "open", "", false, "open created note")
 	createNoteCmd.Flags().StringVarP(&content, "content", "c", "", "text to add to note")
 	createNoteCmd.Flags().BoolVarP(&shouldAppend, "append", "a", false, "append to note")
diff --git a/cmd/daily.go b/cmd/daily.go
index 4105495..8dafc58 100644
--- a/cmd/daily.go
+++ b/cmd/daily.go
@@ -1,29 +1,40 @@
 package cmd
 
 import (
-	"log"
+	"fmt"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
 )
 
+var dailyDryRun bool
+
 var DailyCmd = &cobra.Command{
 	Use:     "daily",
 	Aliases: []string{"d"},
 	Short:   "Creates or opens daily note in vault",
 	Args:    cobra.ExactArgs(0),
-	Run: func(cmd *cobra.Command, args []string) {
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
-		err := actions.DailyNote(&vault, &uri)
-		if err != nil {
-			log.Fatal(err)
+
+		if dailyDryRun {
+			u, err := actions.PlanDailyNote(&vault, &uri)
+			if err != nil {
+				return err
+			}
+			fmt.Println("Daily dry-run:")
+			fmt.Println(u)
+			return nil
 		}
+
+		return actions.DailyNote(&vault, &uri)
 	},
 }
 
 func init() {
+	DailyCmd.Flags().BoolVar(&dailyDryRun, "dry-run", false, "print the Obsidian URI without opening it")
 	DailyCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
 	rootCmd.AddCommand(DailyCmd)
 }
diff --git a/cmd/delete.go b/cmd/delete.go
index 6a0b63e..b486c2f 100644
--- a/cmd/delete.go
+++ b/cmd/delete.go
@@ -1,6 +1,9 @@
 package cmd
 
 import (
+	"errors"
+	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 
@@ -8,9 +11,11 @@ import (
 )
 
 var deleteForce bool
+var deleteDryRun bool
+var deleteSelect bool
 
 var deleteCmd = &cobra.Command{
-	Use:     "delete <note>",
+	Use:     "delete [note-path]",
 	Aliases: []string{"del"},
 	Short:   "Delete note in vault",
 	Long: `Delete a note from the vault.
@@ -25,21 +30,58 @@ Use --force to skip confirmation (recommended for scripts).`,
 
   # Delete from specific vault
   obsidian-cli delete "note" --vault "Archive"`,
-	Args: cobra.ExactArgs(1),
+	Args: cobra.MaximumNArgs(1),
 	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		note := obsidian.Note{}
-		notePath := args[0]
+		notePath := ""
+
+		if len(args) > 0 && !deleteSelect {
+			notePath = args[0]
+		} else {
+			if !deleteSelect {
+				return errors.New("note path required (or use --ls)")
+			}
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			notePath = selected
+		}
+		if notePath == "" {
+			return errors.New("no note selected")
+		}
 		params := actions.DeleteParams{
 			NotePath: notePath,
 			Force:    deleteForce,
 		}
+
+		if deleteDryRun {
+			plan, err := actions.PlanDeleteNote(&vault, params)
+			if err != nil {
+				return err
+			}
+			fmt.Println("Delete dry-run:")
+			fmt.Printf("  vault: %s\n", plan.VaultName)
+			fmt.Printf("  path: %s\n", plan.AbsoluteNotePath)
+			return nil
+		}
 		return actions.DeleteNote(&vault, &note, params)
 	},
 }
 
 func init() {
+	deleteCmd.Flags().BoolVar(&deleteDryRun, "dry-run", false, "preview which file would be deleted without deleting it")
 	deleteCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
 	deleteCmd.Flags().BoolVarP(&deleteForce, "force", "f", false, "skip confirmation if the note has incoming links")
+	deleteCmd.Flags().BoolVar(&deleteSelect, "ls", false, "select a note interactively")
+	deleteCmd.Flags().BoolVar(&deleteSelect, "select", false, "select a note interactively")
 	rootCmd.AddCommand(deleteCmd)
 }
diff --git a/cmd/frontmatter.go b/cmd/frontmatter.go
new file mode 100644
index 0000000..e7a611d
--- /dev/null
+++ b/cmd/frontmatter.go
@@ -0,0 +1,92 @@
+package cmd
+
+import (
+	"errors"
+	"fmt"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/spf13/cobra"
+)
+
+var fmPrint bool
+var fmEdit bool
+var fmDelete bool
+var fmKey string
+var fmValue string
+var fmSelect bool
+
+var frontmatterCmd = &cobra.Command{
+	Use:     "frontmatter [note]",
+	Aliases: []string{"fm"},
+	Short:   "View or modify note frontmatter",
+	Long: `View or modify YAML frontmatter in a note.
+
+Use --print to display frontmatter, --edit to modify a key,
+or --delete to remove a key.
+
+Examples:
+  obsidian-cli frontmatter "My Note" --print
+  obsidian-cli frontmatter "My Note" --edit --key "status" --value "done"
+  obsidian-cli frontmatter "My Note" --delete --key "draft"`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		vault := obsidian.Vault{Name: vaultName}
+		note := obsidian.Note{}
+		noteName := ""
+
+		if len(args) > 0 && !fmSelect {
+			noteName = args[0]
+		} else {
+			if !fmSelect {
+				return errors.New("note path required (or use --ls)")
+			}
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+		if noteName == "" {
+			return errors.New("no note selected")
+		}
+
+		params := actions.FrontmatterParams{
+			NoteName: noteName,
+			Print:    fmPrint,
+			Edit:     fmEdit,
+			Delete:   fmDelete,
+			Key:      fmKey,
+			Value:    fmValue,
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, params)
+		if err != nil {
+			return err
+		}
+
+		if output != "" {
+			fmt.Print(output)
+		}
+		return nil
+	},
+}
+
+func init() {
+	frontmatterCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	frontmatterCmd.Flags().BoolVar(&fmSelect, "ls", false, "select a note interactively")
+	frontmatterCmd.Flags().BoolVar(&fmSelect, "select", false, "select a note interactively")
+	frontmatterCmd.Flags().BoolVarP(&fmPrint, "print", "p", false, "print frontmatter")
+	frontmatterCmd.Flags().BoolVarP(&fmEdit, "edit", "e", false, "edit a frontmatter key")
+	frontmatterCmd.Flags().BoolVarP(&fmDelete, "delete", "d", false, "delete a frontmatter key")
+	frontmatterCmd.Flags().StringVarP(&fmKey, "key", "k", "", "key to edit or delete")
+	frontmatterCmd.Flags().StringVar(&fmValue, "value", "", "value to set (required for --edit)")
+	rootCmd.AddCommand(frontmatterCmd)
+}
diff --git a/cmd/init.go b/cmd/init.go
new file mode 100644
index 0000000..f7324fe
--- /dev/null
+++ b/cmd/init.go
@@ -0,0 +1,734 @@
+package cmd
+
+import (
+	"bufio"
+	"bytes"
+	"encoding/json"
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/ktr0731/go-fuzzyfinder"
+	"github.com/spf13/cobra"
+	"gopkg.in/yaml.v3"
+)
+
+var initCmd = &cobra.Command{
+	Use:     "init",
+	Short:   "Interactive setup wizard",
+	Long:    "Interactive setup wizard to configure your default vault, daily note settings, and targets.",
+	Args:    cobra.NoArgs,
+	Example: "  obsidian-cli init",
+	RunE: func(cmd *cobra.Command, args []string) error {
+		in := bufio.NewReader(os.Stdin)
+
+		fmt.Println()
+		fmt.Println(DoubleLine(DefaultBorderWidth))
+		fmt.Println("                            OBSIDIAN-CLI SETUP WIZARD                        ")
+		fmt.Println(DoubleLine(DefaultBorderWidth))
+		fmt.Println()
+
+		return runInitWizard(in)
+	},
+}
+
+func init() {
+	rootCmd.AddCommand(initCmd)
+}
+
+type discoveredVault struct {
+	Name string
+	Path string
+	ID   string
+}
+
+func runInitWizard(in *bufio.Reader) error {
+	var vaultName string
+	var vaultPath string
+
+	var settings obsidian.VaultSettings
+	var daily obsidian.DailyNoteSettings
+
+vaultSelection:
+	// Step 1: vault selection
+	for {
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 1: Default vault")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("Type '?' for help, or 'back' to cancel.")
+		fmt.Println()
+
+		vaults, err := discoverVaults()
+		if err != nil || len(vaults) == 0 {
+			if err != nil {
+				fmt.Printf("Could not discover vaults: %v\n", err)
+			} else {
+				fmt.Println("No vaults discovered.")
+			}
+			fmt.Println("Enter the vault name (usually the folder name), or type '?' for help.")
+			name, action, err := promptLine(in, "> ")
+			if err != nil {
+				return err
+			}
+			switch action {
+			case actionBack:
+				return errors.New("cancelled")
+			case actionHelp:
+				printVaultHelp()
+				continue
+			default:
+			}
+			name = strings.TrimSpace(name)
+			if name == "" {
+				fmt.Println("Vault name is required.")
+				continue
+			}
+			vault := obsidian.Vault{Name: name}
+			path, err := vault.Path()
+			if err != nil {
+				fmt.Printf("Vault not found: %v\n", err)
+				fmt.Println("Tip: ensure Obsidian has created an obsidian.json with your vaults.")
+				continue
+			}
+			vaultName = name
+			vaultPath = path
+			break
+		}
+
+		fmt.Println("Discovered vaults:")
+		for i, v := range vaults {
+			fmt.Printf("  %d) %s\n", i+1, v.Name)
+			fmt.Printf("     %s\n", v.Path)
+		}
+		fmt.Println()
+		fmt.Println("Choose a vault:")
+		fmt.Println("  - Enter a number")
+		fmt.Println("  - Type the exact name")
+		fmt.Println("  - Type 'select' to choose with fuzzy finder")
+
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		switch action {
+		case actionBack:
+			return errors.New("cancelled")
+		case actionSkip:
+			fmt.Println("Skip is not available here. Choose a vault or type 'back' to cancel.")
+			continue
+		case actionHelp:
+			printVaultHelp()
+			continue
+		default:
+		}
+
+		line = strings.TrimSpace(line)
+		if strings.EqualFold(line, "select") {
+			name, path, err := pickVaultFuzzy(vaults)
+			if err != nil {
+				fmt.Printf("Selection cancelled: %v\n", err)
+				continue
+			}
+			vaultName = name
+			vaultPath = path
+			break
+		}
+
+		if n, ok := parseNumber(line); ok {
+			if n < 1 || n > len(vaults) {
+				fmt.Printf("Invalid selection: enter 1-%d\n", len(vaults))
+				continue
+			}
+			vaultName = vaults[n-1].Name
+			vaultPath = vaults[n-1].Path
+			break
+		}
+
+		if name, path, ok := matchVaultName(vaults, line); ok {
+			vaultName = name
+			vaultPath = path
+			break
+		}
+
+		fmt.Printf("Vault '%s' not found. Try again.\n", line)
+	}
+
+	// Confirm and write default vault name
+	for {
+		fmt.Println()
+		fmt.Printf("Set default vault to: %s\n", vaultName)
+		fmt.Println("This writes to preferences.json in your user config directory.")
+		fmt.Println("Continue? (y/N)  Type 'back' to re-select vault.")
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			goto vaultSelection
+		}
+		if action == actionHelp {
+			fmt.Println("This sets which vault is used when --vault is not provided.")
+			continue
+		}
+		if action == actionSkip {
+			fmt.Println("Skip is not available here. Answer 'y' to write preferences.json, or type 'back' to re-select.")
+			continue
+		}
+		if !isYes(line) {
+			return errors.New("cancelled")
+		}
+
+		v := obsidian.Vault{}
+		if err := v.SetDefaultName(vaultName); err != nil {
+			fmt.Printf("Failed to write preferences.json: %v\n", err)
+			fmt.Println("Tip: check permissions on your user config directory.")
+			continue
+		}
+		break
+	}
+
+	vault := obsidian.Vault{Name: vaultName}
+
+dailySettings:
+	// Step 2: daily folder
+	settings, _ = vault.Settings()
+	daily = settings.DailyNote
+
+	folder, err := promptDailyFolder(in, vaultPath, daily.Folder)
+	if err != nil {
+		if errors.Is(err, errBack) {
+			goto vaultSelection
+		}
+		return err
+	}
+	daily.Folder = folder
+
+	// Step 3: pattern
+	pattern, err := promptDailyPattern(in, daily.FilenamePattern)
+	if err != nil {
+		if errors.Is(err, errBack) {
+			goto dailySettings
+		}
+		return err
+	}
+	daily.FilenamePattern = pattern
+
+	// Step 4: template
+	templatePath, err := promptDailyTemplate(in, vaultPath, daily.TemplatePath)
+	if err != nil {
+		if errors.Is(err, errBack) {
+			goto dailySettings
+		}
+		return err
+	}
+	daily.TemplatePath = templatePath
+
+	// Step 5: save daily settings
+	daily.CreateIfMissing = true
+	settings.DailyNote = daily
+
+	fmt.Println()
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Println("  Step 2: Daily note settings summary")
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Printf("  folder:   %s\n", daily.Folder)
+	fmt.Printf("  pattern:  %s\n", daily.FilenamePattern)
+	fmt.Printf("  example:  %s.md\n", obsidian.ExpandDatePattern(daily.FilenamePattern, time.Now()))
+	if strings.TrimSpace(daily.TemplatePath) == "" {
+		fmt.Println("  template: (none)")
+	} else {
+		fmt.Printf("  template: %s\n", daily.TemplatePath)
+	}
+	fmt.Println()
+	fmt.Println("Save these daily note settings? (y/N)  Type 'back' to edit.")
+
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			goto dailySettings
+		}
+		if action == actionHelp {
+			printDailyHelp()
+			continue
+		}
+		if !isYes(line) {
+			return errors.New("cancelled")
+		}
+		if err := vault.SetSettings(settings); err != nil {
+			return err
+		}
+		break
+	}
+
+targetsStep:
+	// Step 6: targets migration / setup
+	if err := runTargetsMigration(in); err != nil {
+		if errors.Is(err, errBack) {
+			goto dailySettings
+		}
+		return err
+	}
+
+	// Step 7: optionally add a first target
+	if err := runInitAddTargets(in, vaultPath); err != nil {
+		if errors.Is(err, errBack) {
+			goto targetsStep
+		}
+		return err
+	}
+
+	// Summary
+	fmt.Println()
+	fmt.Println(DoubleLine(DefaultBorderWidth))
+	fmt.Println("                                  SETUP COMPLETE                              ")
+	fmt.Println(DoubleLine(DefaultBorderWidth))
+	fmt.Println()
+	fmt.Printf("Default vault: %s\n", vaultName)
+	fmt.Printf("Vault path:    %s\n", vaultPath)
+	fmt.Println()
+	fmt.Println("Next steps:")
+	fmt.Println("  - Append to today's daily note:")
+	fmt.Println("      obsidian-cli append \"some text\"")
+	fmt.Println("      obsidian-cli a \"some text\"")
+	fmt.Println("  - Append multi-line content (Ctrl-D to save, Ctrl-C to cancel):")
+	fmt.Println("      obsidian-cli append")
+	fmt.Println("  - Capture to a target:")
+	fmt.Println("      obsidian-cli target --select")
+	fmt.Println("      obsidian-cli target inbox \"some text\"")
+	return nil
+}
+
+func parseNumber(s string) (int, bool) {
+	var n int
+	_, err := fmt.Sscanf(strings.TrimSpace(s), "%d", &n)
+	return n, err == nil
+}
+
+func discoverVaults() ([]discoveredVault, error) {
+	obsidianConfigFile, err := config.ObsidianFile()
+	if err != nil {
+		return nil, err
+	}
+	content, err := os.ReadFile(obsidianConfigFile)
+	if err != nil {
+		return nil, err
+	}
+	var cfg struct {
+		Vaults map[string]struct {
+			Path string `json:"path"`
+		} `json:"vaults"`
+	}
+	if err := json.Unmarshal(content, &cfg); err != nil {
+		return nil, err
+	}
+	if len(cfg.Vaults) == 0 {
+		return nil, errors.New("no vaults found in obsidian.json")
+	}
+	var out []discoveredVault
+	for id, v := range cfg.Vaults {
+		p := v.Path
+		if strings.TrimSpace(p) == "" {
+			continue
+		}
+		base := filepath.Base(filepath.Clean(p))
+		if base == "." || base == string(filepath.Separator) || base == "" {
+			continue
+		}
+		out = append(out, discoveredVault{Name: base, Path: p, ID: id})
+	}
+	sort.Slice(out, func(i, j int) bool { return strings.ToLower(out[i].Name) < strings.ToLower(out[j].Name) })
+	return out, nil
+}
+
+func matchVaultName(vaults []discoveredVault, input string) (string, string, bool) {
+	in := strings.TrimSpace(input)
+	if in == "" {
+		return "", "", false
+	}
+	for _, v := range vaults {
+		if strings.EqualFold(v.Name, in) || (v.ID != "" && strings.EqualFold(v.ID, in)) {
+			return v.Name, v.Path, true
+		}
+	}
+	return "", "", false
+}
+
+func pickVaultFuzzy(vaults []discoveredVault) (string, string, error) {
+	items := make([]string, 0, len(vaults))
+	for _, v := range vaults {
+		items = append(items, fmt.Sprintf("%s  (%s)", v.Name, v.Path))
+	}
+	idx, err := fuzzyfinder.Find(items, func(i int) string { return items[i] })
+	if err != nil {
+		return "", "", err
+	}
+	return vaults[idx].Name, vaults[idx].Path, nil
+}
+
+func promptDailyFolder(in *bufio.Reader, vaultPath string, existing string) (string, error) {
+	const defaultDailyFolder = "Daily"
+	for {
+		fmt.Println()
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 2: Daily note folder")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		if strings.TrimSpace(existing) != "" {
+			fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+		}
+		fmt.Println("Enter a folder path relative to the vault, or type 'ls' to browse.")
+		fmt.Println("Type 'skip' to accept a default, '?' for help, or 'back' to go back.")
+
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			fmt.Printf("Using default: %s\n", defaultDailyFolder)
+			return defaultDailyFolder, nil
+		}
+		if action == actionHelp {
+			printDailyHelp()
+			continue
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		if strings.EqualFold(line, "ls") {
+			p, err := pickOrCreateFolderPath(vaultPath)
+			if err != nil {
+				fmt.Printf("Selection error: %v\n", err)
+				continue
+			}
+			return p, nil
+		}
+		if strings.TrimSpace(line) == "" {
+			fmt.Println("Folder is required.")
+			continue
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid folder path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptDailyPattern(in *bufio.Reader, existing string) (string, error) {
+	const defaultDailyPattern = "YYYY-MM-DD"
+	for {
+		fmt.Println()
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 3: Daily note filename pattern")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		if strings.TrimSpace(existing) != "" {
+			fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+		}
+		if strings.TrimSpace(existing) == "" {
+			fmt.Printf("Tip: type 'skip' to accept the default (%s).\n", defaultDailyPattern)
+		}
+
+		pattern, err := promptForTargetPattern(in, existing)
+		if err != nil {
+			if errors.Is(err, errBack) {
+				return "", errBack
+			}
+			return "", err
+		}
+		if strings.TrimSpace(pattern) == "" {
+			fmt.Println("Pattern is required.")
+			continue
+		}
+		return pattern, nil
+	}
+}
+
+func promptDailyTemplate(in *bufio.Reader, vaultPath string, existing string) (string, error) {
+	for {
+		fmt.Println()
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 4: Daily note template (optional)")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		if strings.TrimSpace(existing) != "" {
+			fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+		}
+		fmt.Println("Press Enter for none, type a path, or type 'ls' to browse.")
+		fmt.Println("Type 'skip' to skip this step, '?' for help, or 'back' to go back.")
+
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		switch action {
+		case actionBack:
+			return "", errBack
+		case actionSkip:
+			return existing, nil
+		case actionHelp:
+			printTemplateHelp()
+			continue
+		default:
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		if line == "" {
+			return "", nil
+		}
+		if strings.EqualFold(line, "ls") {
+			p, err := pickOrCreateNotePath(vaultPath)
+			if err != nil {
+				fmt.Printf("Selection error: %v\n", err)
+				continue
+			}
+			return p, nil
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func runTargetsMigration(in *bufio.Reader) error {
+	fmt.Println()
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Println("  Step 5: Targets (optional)")
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Println("Type 'skip' to skip, 'back' to go back, '?' for help.")
+
+	_, targetsFile, err := config.TargetsPath()
+	if err != nil {
+		return err
+	}
+	raw, err := os.ReadFile(targetsFile)
+	if err != nil {
+		if os.IsNotExist(err) {
+			fmt.Println("No targets.yaml found.")
+			fmt.Println("Create it now? (y/N)  Type 'skip' to skip, 'back' to go back.")
+			for {
+				line, action, err := promptLine(in, "> ")
+				if err != nil {
+					return err
+				}
+				if action == actionBack {
+					return errBack
+				}
+				if action == actionSkip {
+					return nil
+				}
+				if action == actionHelp {
+					printTargetsHelp()
+					continue
+				}
+				if isYes(line) {
+					path, err := obsidian.EnsureTargetsFileExists()
+					if err != nil {
+						return err
+					}
+					fmt.Printf("Created: %s\n", path)
+				}
+				return nil
+			}
+		}
+		return err
+	}
+
+	scalarNames, err := detectScalarTargets(raw)
+	if err != nil {
+		fmt.Printf("Could not parse targets.yaml: %v\n", err)
+		fmt.Println("Options:")
+		fmt.Println("  - Run: obsidian-cli target edit")
+		fmt.Println("  - Or open it now in your editor")
+		fmt.Println("Open in editor now? (y/N)  Type 'back' to go back, 'skip' to skip.")
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			return errBack
+		}
+		if action == actionSkip {
+			return nil
+		}
+		if isYes(line) {
+			return obsidian.OpenInEditor(targetsFile)
+		}
+		return nil
+	}
+	if len(scalarNames) == 0 {
+		fmt.Println("targets.yaml looks OK (no simplified scalar targets detected).")
+		return nil
+	}
+
+	fmt.Printf("Found %d simplified targets (e.g. inbox: Inbox.md).\n", len(scalarNames))
+	fmt.Println("Migrate them to the canonical format (type/file fields)?")
+	fmt.Println("This will rewrite targets.yaml and may remove custom comments (a standard header will be added).")
+	fmt.Println("Continue? (y/N)  Type 'skip' to skip, 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			return errBack
+		}
+		if action == actionSkip {
+			return nil
+		}
+		if action == actionHelp {
+			printTargetsHelp()
+			continue
+		}
+		if !isYes(line) {
+			return nil
+		}
+		break
+	}
+
+	cfg, err := obsidian.LoadTargets()
+	if err != nil {
+		return err
+	}
+
+	before := bytes.TrimSpace(raw)
+	if err := obsidian.SaveTargets(cfg); err != nil {
+		return err
+	}
+	afterRaw, _ := os.ReadFile(targetsFile)
+	after := bytes.TrimSpace(afterRaw)
+	if bytes.Equal(before, after) {
+		fmt.Println("No changes made.")
+	} else {
+		fmt.Println("Migrated targets.yaml.")
+	}
+	return nil
+}
+
+func detectScalarTargets(raw []byte) ([]string, error) {
+	var node yaml.Node
+	if err := yaml.Unmarshal(raw, &node); err != nil {
+		return nil, err
+	}
+	if len(node.Content) == 0 {
+		return nil, nil
+	}
+	root := node.Content[0]
+	if root.Kind != yaml.MappingNode {
+		return nil, nil
+	}
+
+	var names []string
+	for i := 0; i+1 < len(root.Content); i += 2 {
+		k := root.Content[i]
+		v := root.Content[i+1]
+		if k.Kind == yaml.ScalarNode && v.Kind == yaml.ScalarNode {
+			n := strings.TrimSpace(k.Value)
+			if n != "" {
+				names = append(names, n)
+			}
+		}
+	}
+	sort.Strings(names)
+	return names, nil
+}
+
+func runInitAddTargets(in *bufio.Reader, vaultPath string) error {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return err
+	}
+
+	fmt.Println()
+	fmt.Println("Add a target now? (y/N)  Type 'skip' to skip, 'back' to go back, '?' for help.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			return errBack
+		}
+		if action == actionSkip {
+			return nil
+		}
+		if action == actionHelp {
+			printTargetsHelp()
+			continue
+		}
+		if !isYes(line) {
+			return nil
+		}
+		break
+	}
+
+	for {
+		name, target, err := runTargetAddWizard(in, vaultPath, "", cfg)
+		if err != nil {
+			fmt.Printf("Error: %v\n", err)
+		} else {
+			cfg[name] = target
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Saved target: %s\n", name)
+		}
+
+		fmt.Println("Add another? (y/N)")
+		line, _, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if !isYes(line) {
+			break
+		}
+	}
+	return nil
+}
+
+func printVaultHelp() {
+	fmt.Println()
+	fmt.Println("Vault help:")
+	fmt.Println("- Vault name is usually the folder name of your vault path.")
+	fmt.Println("- obsidian-cli reads vaults from Obsidian's obsidian.json in your user config directory.")
+	fmt.Println("- If no vaults are discovered, open Obsidian once and ensure at least one vault is configured.")
+}
+
+func printDailyHelp() {
+	fmt.Println()
+	fmt.Println("Daily note help:")
+	fmt.Println("- Folder is a path relative to the vault root (example: Daily).")
+	fmt.Println("- Pattern controls the daily note filename and supports Obsidian-style tokens like YYYY, MM, DD, HH, mm, ss.")
+	fmt.Println("- [literal] blocks are preserved, e.g. YYYY-[log]-MM.")
+}
+
+func printTemplateHelp() {
+	fmt.Println()
+	fmt.Println("Template help:")
+	fmt.Println("- Template is a note path relative to the vault root (example: Templates/Daily).")
+	fmt.Println("- When a daily note is created for the first time, template content is copied and variables like {{date}}, {{time}}, {{title}} are expanded.")
+}
+
+func printTargetsHelp() {
+	fmt.Println()
+	fmt.Println("Targets help:")
+	fmt.Println("- Targets let you capture quickly to a configured note or dated note pattern.")
+	fmt.Println("- Use: obsidian-cli target add    (guided workflow)")
+	fmt.Println("- Use: obsidian-cli target --select")
+	fmt.Println("- targets.yaml is stored next to preferences.json.")
+}
diff --git a/cmd/move.go b/cmd/move.go
index 8e23fa5..6a6ffb6 100644
--- a/cmd/move.go
+++ b/cmd/move.go
@@ -1,6 +1,7 @@
 package cmd
 
 import (
+	"errors"
 	"fmt"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
@@ -10,8 +11,9 @@ import (
 )
 
 var shouldOpen bool
+var moveSelect bool
 var moveCmd = &cobra.Command{
-	Use:     "move <source> <destination>",
+	Use:     "move [from-note-path] [to-note-path]",
 	Aliases: []string{"m"},
 	Short:   "Move or rename note in vault and update corresponding links",
 	Long: `Moves or renames a note and updates all links pointing to it.
@@ -26,13 +28,68 @@ or [markdown](links) that reference the moved note.`,
 
   # Move and open the result
   obsidian-cli move "temp" "Archive/temp" --open`,
-	Args: cobra.ExactArgs(2),
+	Args: cobra.MaximumNArgs(2),
 	RunE: func(cmd *cobra.Command, args []string) error {
-		currentName := args[0]
-		newName := args[1]
 		vault := obsidian.Vault{Name: vaultName}
 		note := obsidian.Note{}
 		uri := obsidian.Uri{}
+		currentName := ""
+		newName := ""
+
+		var vaultPath string
+		needVaultPath := moveSelect || len(args) < 2
+		if needVaultPath {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			p, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			vaultPath = p
+		}
+
+		switch len(args) {
+		case 2:
+			currentName = args[0]
+			newName = args[1]
+		case 1:
+			if moveSelect {
+				newName = args[0]
+				selected, err := pickExistingNotePath(vaultPath)
+				if err != nil {
+					return err
+				}
+				currentName = selected
+			} else {
+				currentName = args[0]
+				selected, err := promptNewNotePath(vaultPath)
+				if err != nil {
+					return err
+				}
+				newName = selected
+			}
+		case 0:
+			if !moveSelect {
+				return errors.New("from-note-path required (or use --ls)")
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			currentName = selected
+			selected, err = promptNewNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			newName = selected
+		default:
+			return errors.New("expected 0, 1, or 2 arguments")
+		}
+
+		if currentName == "" || newName == "" {
+			return errors.New("both from-note-path and to-note-path are required")
+		}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
 			return fmt.Errorf("failed to parse --editor flag: %w", err)
@@ -50,6 +107,8 @@ or [markdown](links) that reference the moved note.`,
 func init() {
 	moveCmd.Flags().BoolVarP(&shouldOpen, "open", "o", false, "open new note")
 	moveCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	moveCmd.Flags().BoolVar(&moveSelect, "ls", false, "select the note to move interactively")
+	moveCmd.Flags().BoolVar(&moveSelect, "select", false, "select the note to move interactively")
 	moveCmd.Flags().BoolP("editor", "e", false, "open in editor instead of Obsidian (requires --open flag)")
 	rootCmd.AddCommand(moveCmd)
 }
diff --git a/cmd/note_picker.go b/cmd/note_picker.go
new file mode 100644
index 0000000..2613389
--- /dev/null
+++ b/cmd/note_picker.go
@@ -0,0 +1,103 @@
+package cmd
+
+import (
+	"bufio"
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/ktr0731/go-fuzzyfinder"
+)
+
+const (
+	notePickerNewNote   = "(Create new note...)"
+	notePickerVaultRoot = "(Vault root)"
+)
+
+func pickExistingNotePath(vaultPath string) (string, error) {
+	notes, err := listVaultNotes(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	if len(notes) == 0 {
+		return "", errors.New("no notes found in vault")
+	}
+	sort.Strings(notes)
+
+	idx, err := fuzzyfinder.Find(notes, func(i int) string { return notes[i] })
+	if err != nil {
+		return "", err
+	}
+	return notes[idx], nil
+}
+
+func pickNotePathOrNew(vaultPath string) (string, error) {
+	notes, err := listVaultNotes(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	notes = append(notes, notePickerNewNote)
+	sort.Strings(notes)
+
+	idx, err := fuzzyfinder.Find(notes, func(i int) string { return notes[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := notes[idx]
+	if choice != notePickerNewNote {
+		return choice, nil
+	}
+	return promptNewNotePath(vaultPath)
+}
+
+func promptNewNotePath(vaultPath string) (string, error) {
+	folders, err := listVaultFolders(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	folders = append(folders, notePickerVaultRoot, "(Create new folder...)")
+	sort.Strings(folders)
+
+	idx, err := fuzzyfinder.Find(folders, func(i int) string { return folders[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := folders[idx]
+	switch choice {
+	case "(Create new folder...)":
+		created, err := promptCreateFolder(vaultPath)
+		if err != nil {
+			return "", err
+		}
+		choice = created
+	case notePickerVaultRoot:
+		choice = ""
+	}
+
+	in := bufio.NewReader(os.Stdin)
+	fmt.Println("Enter new note name (relative to the selected folder; .md optional):")
+	fmt.Print("> ")
+	name, err := in.ReadString('\n')
+	if err != nil {
+		return "", err
+	}
+	name = strings.TrimSpace(name)
+	if name == "" {
+		return "", errors.New("note name is required")
+	}
+	name = strings.TrimSuffix(name, ".md")
+
+	rel := strings.TrimSpace(filepath.ToSlash(filepath.Join(choice, name)))
+	if rel == "" {
+		return "", errors.New("note path is required")
+	}
+	if _, err := obsidian.SafeJoinVaultPath(vaultPath, rel); err != nil {
+		return "", err
+	}
+	return rel, nil
+}
+
diff --git a/cmd/open.go b/cmd/open.go
index d4ac325..61bca0f 100644
--- a/cmd/open.go
+++ b/cmd/open.go
@@ -1,14 +1,17 @@
 package cmd
 
 import (
+	"errors"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
 )
 
 var vaultName string
+var openSelect bool
 var OpenVaultCmd = &cobra.Command{
-	Use:     "open <note>",
+	Use:     "open [note-path]",
 	Aliases: []string{"o"},
 	Short:   "Opens note in vault by note name",
 	Long: `Opens a note in Obsidian by name or path.
@@ -23,11 +26,32 @@ The .md extension is optional.`,
 
   # Open in a specific vault
   obsidian-cli open "Daily" --vault "Work"`,
-	Args: cobra.ExactArgs(1),
+	Args: cobra.MaximumNArgs(1),
 	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
-		noteName := args[0]
+		noteName := ""
+
+		if len(args) > 0 && !openSelect {
+			noteName = args[0]
+		} else {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickNotePathOrNew(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+
+		if noteName == "" {
+			return errors.New("no note selected")
+		}
 		params := actions.OpenParams{NoteName: noteName}
 		return actions.OpenNote(&vault, &uri, params)
 	},
@@ -35,5 +59,7 @@ The .md extension is optional.`,
 
 func init() {
 	OpenVaultCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
+	OpenVaultCmd.Flags().BoolVar(&openSelect, "ls", false, "select a note interactively")
+	OpenVaultCmd.Flags().BoolVar(&openSelect, "select", false, "select a note interactively")
 	rootCmd.AddCommand(OpenVaultCmd)
 }
diff --git a/cmd/print.go b/cmd/print.go
index 652d5e1..a638b38 100644
--- a/cmd/print.go
+++ b/cmd/print.go
@@ -1,6 +1,7 @@
 package cmd
 
 import (
+	"errors"
 	"fmt"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
@@ -10,8 +11,9 @@ import (
 )
 
 var shouldRenderMarkdown bool
+var printSelect bool
 var printCmd = &cobra.Command{
-	Use:     "print <note>",
+	Use:     "print [note-path]",
 	Aliases: []string{"p"},
 	Short:   "Print contents of note",
 	Long: `Prints the contents of a note to stdout.
@@ -29,10 +31,29 @@ a note without opening Obsidian.`,
 
   # Copy to clipboard (macOS)
   obsidian-cli print "Template" | pbcopy`,
-	Args: cobra.ExactArgs(1),
+	Args: cobra.MaximumNArgs(1),
 	RunE: func(cmd *cobra.Command, args []string) error {
-		noteName := args[0]
 		vault := obsidian.Vault{Name: vaultName}
+		noteName := ""
+		if len(args) > 0 && !printSelect {
+			noteName = args[0]
+		} else {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+		if noteName == "" {
+			return errors.New("no note selected")
+		}
 		note := obsidian.Note{}
 		params := actions.PrintParams{
 			NoteName: noteName,
@@ -48,5 +69,7 @@ a note without opening Obsidian.`,
 
 func init() {
 	printCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	printCmd.Flags().BoolVar(&printSelect, "ls", false, "select a note interactively")
+	printCmd.Flags().BoolVar(&printSelect, "select", false, "select a note interactively")
 	rootCmd.AddCommand(printCmd)
 }
diff --git a/cmd/root.go b/cmd/root.go
index 3aed97e..20fd5ee 100644
--- a/cmd/root.go
+++ b/cmd/root.go
@@ -3,7 +3,9 @@ package cmd
 import (
 	"fmt"
 	"os"
+	"strings"
 
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
 )
 
@@ -15,8 +17,48 @@ var rootCmd = &cobra.Command{
 }
 
 func Execute() {
+	maybeRewriteArgsForTargetNames()
 	if err := rootCmd.Execute(); err != nil {
 		fmt.Fprintf(os.Stderr, "Whoops. There was an error while executing your CLI '%s'", err)
 		os.Exit(1)
 	}
 }
+
+func maybeRewriteArgsForTargetNames() {
+	args := os.Args[1:]
+	if len(args) == 0 {
+		return
+	}
+	first := strings.TrimSpace(args[0])
+	if first == "" || strings.HasPrefix(first, "-") {
+		return
+	}
+	if isKnownRootCommand(first) {
+		return
+	}
+
+	cfg, err := obsidian.LoadTargets()
+	if err != nil {
+		return
+	}
+	if _, ok := cfg[first]; !ok {
+		return
+	}
+
+	// Treat: obsidian-cli <targetName> ... as: obsidian-cli target <targetName> ...
+	rootCmd.SetArgs(append([]string{"target"}, args...))
+}
+
+func isKnownRootCommand(name string) bool {
+	for _, c := range rootCmd.Commands() {
+		if c.Name() == name {
+			return true
+		}
+		for _, a := range c.Aliases {
+			if a == name {
+				return true
+			}
+		}
+	}
+	return false
+}
diff --git a/cmd/search.go b/cmd/search.go
index 571b243..c6c4836 100644
--- a/cmd/search.go
+++ b/cmd/search.go
@@ -2,6 +2,7 @@ package cmd
 
 import (
 	"fmt"
+	"strings"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
@@ -28,14 +29,43 @@ note in Obsidian, or use --editor to open in your $EDITOR.`,
 	Args: cobra.NoArgs,
 	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
-		note := obsidian.Note{}
-		uri := obsidian.Uri{}
-		fuzzyFinder := obsidian.FuzzyFinder{}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
 			return fmt.Errorf("failed to retrieve 'editor' flag: %w", err)
 		}
-		return actions.SearchNotes(&vault, &note, &uri, &fuzzyFinder, useEditor)
+
+		if _, err := vault.DefaultName(); err != nil {
+			return err
+		}
+		vaultPath, err := vault.Path()
+		if err != nil {
+			return err
+		}
+
+		notePath, err := pickNotePathOrNew(vaultPath)
+		if err != nil {
+			return err
+		}
+		if strings.TrimSpace(notePath) == "" {
+			return fmt.Errorf("no note selected")
+		}
+
+		if useEditor {
+			fmt.Printf("Opening note: %s\n", notePath)
+			rel := notePath
+			if !strings.HasSuffix(strings.ToLower(rel), ".md") {
+				rel += ".md"
+			}
+			abs, err := obsidian.SafeJoinVaultPath(vaultPath, rel)
+			if err != nil {
+				return err
+			}
+			return obsidian.OpenInEditor(abs)
+		}
+
+		uri := obsidian.Uri{}
+		params := actions.OpenParams{NoteName: notePath}
+		return actions.OpenNote(&vault, &uri, params)
 	},
 }
 
diff --git a/cmd/search_content.go b/cmd/search_content.go
index 6cfd2d1..fe6340d 100644
--- a/cmd/search_content.go
+++ b/cmd/search_content.go
@@ -10,7 +10,7 @@ import (
 )
 
 var searchContentCmd = &cobra.Command{
-	Use:   "search-content <term>",
+	Use:   "search-content <search-term>",
 	Short: "Search note content for search term",
 	Long: `Searches the contents of all notes for a term.
 
diff --git a/cmd/set_default.go b/cmd/set_default.go
index 0c855f6..75a7c85 100644
--- a/cmd/set_default.go
+++ b/cmd/set_default.go
@@ -8,7 +8,7 @@ import (
 )
 
 var setDefaultCmd = &cobra.Command{
-	Use:     "set-default <vault>",
+	Use:     "set-default <vault-name>",
 	Aliases: []string{"sd"},
 	Short:   "Sets default vault",
 	Long: `Sets the default vault for all commands.
diff --git a/cmd/target.go b/cmd/target.go
new file mode 100644
index 0000000..720e19e
--- /dev/null
+++ b/cmd/target.go
@@ -0,0 +1,1326 @@
+package cmd
+
+import (
+	"bufio"
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/ktr0731/go-fuzzyfinder"
+	"github.com/spf13/cobra"
+)
+
+var (
+	targetSelect bool
+	targetDryRun bool
+)
+
+var targetCmd = &cobra.Command{
+	Use:     "target [id] [text]",
+	Aliases: []string{"t"},
+	Short: "Append text to a configured target note",
+	Long: `Appends text to a note configured in targets.yaml.
+
+Targets can point at:
+  - a fixed file path (always append to the same note)
+  - a folder + filename pattern (append to a dated note based on the current time)
+
+If no text is provided, content is read from stdin (piped) or entered interactively until EOF.`,
+	Example: `  # Append a one-liner to a target
+  obsidian-cli target inbox "Buy milk"
+
+  # Multi-line content (Ctrl-D to save, Ctrl-C to cancel)
+  obsidian-cli target inbox
+
+  # Pick a target interactively, then enter content
+  obsidian-cli target --select
+
+  # Preview which file would be used
+  obsidian-cli target inbox --dry-run`,
+	Args: cobra.ArbitraryArgs,
+	RunE: func(cmd *cobra.Command, args []string) error {
+		vault := obsidian.Vault{Name: vaultName}
+
+		if len(args) == 0 || targetSelect {
+			name, err := pickTargetName()
+			if err != nil {
+				return err
+			}
+			if strings.TrimSpace(name) == "" {
+				return errors.New("no target selected")
+			}
+
+			if targetDryRun {
+				return printTargetPlan(&vault, name)
+			}
+
+			content, err := actions.PromptForContentIfEmpty("")
+			if err != nil {
+				return err
+			}
+			plan, err := actions.AppendToTarget(&vault, name, content, time.Now(), false)
+			if err != nil {
+				return err
+			}
+			fmt.Printf("Wrote to: %s\n", plan.AbsoluteNotePath)
+			return nil
+		}
+
+		name := strings.TrimSpace(args[0])
+		content := strings.TrimSpace(strings.Join(args[1:], " "))
+
+		if targetDryRun {
+			return printTargetPlan(&vault, name)
+		}
+
+		var err error
+		content, err = actions.PromptForContentIfEmpty(content)
+		if err != nil {
+			return err
+		}
+
+		plan, err := actions.AppendToTarget(&vault, name, content, time.Now(), false)
+		if err != nil {
+			return err
+		}
+		fmt.Printf("Wrote to: %s\n", plan.AbsoluteNotePath)
+		return nil
+	},
+}
+
+var targetListCmd = &cobra.Command{
+	Use:   "list",
+	Short: "List configured targets",
+	RunE: func(cmd *cobra.Command, args []string) error {
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+		names := obsidian.ListTargetNames(cfg)
+		if len(names) == 0 {
+			fmt.Println("No targets configured.")
+			fmt.Println("Run: obsidian-cli target add")
+			return nil
+		}
+		for _, n := range names {
+			t := cfg[n]
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+				fmt.Printf("- %s (folder: %s, pattern: %s)\n", n, t.Folder, t.Pattern)
+			} else {
+				fmt.Printf("- %s (file: %s)\n", n, firstNonEmpty(t.File, t.Note))
+			}
+		}
+		return nil
+	},
+}
+
+var targetAddCmd = &cobra.Command{
+	Use:   "add [name]",
+	Short: "Add a new target",
+	Long: `Add a new capture target.
+
+Run without a name to start a guided workflow.`,
+	Example: `  # Guided workflow
+  obsidian-cli target add
+
+  # Add a fixed-file target
+  obsidian-cli target add inbox
+
+  # Add a folder+pattern target
+  obsidian-cli target add log`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		in := bufio.NewReader(os.Stdin)
+
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+
+		var name string
+		if len(args) == 1 {
+			name = strings.TrimSpace(args[0])
+		}
+
+		vault := obsidian.Vault{Name: vaultName}
+		vaultPath, err := vault.Path()
+		if err != nil {
+			return err
+		}
+
+		name, target, err := runTargetAddWizard(in, vaultPath, name, cfg)
+		if err != nil {
+			return err
+		}
+
+		cfg[name] = target
+		if err := obsidian.SaveTargets(cfg); err != nil {
+			return err
+		}
+
+		fmt.Printf("Saved target: %s\n", name)
+		return nil
+	},
+}
+
+var targetRemoveCmd = &cobra.Command{
+	Use:     "remove [name]",
+	Aliases: []string{"rm"},
+	Short:   "Remove a target",
+	Args:    cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+
+		var name string
+		if len(args) == 1 {
+			name = strings.TrimSpace(args[0])
+		} else {
+			name, err = pickTargetNameFromConfig(cfg)
+			if err != nil {
+				return err
+			}
+		}
+		if strings.TrimSpace(name) == "" {
+			return errors.New("target name is required")
+		}
+
+		if _, ok := cfg[name]; !ok {
+			return fmt.Errorf("target not found: %s", name)
+		}
+		delete(cfg, name)
+		if err := obsidian.SaveTargets(cfg); err != nil {
+			return err
+		}
+		fmt.Printf("Removed target: %s\n", name)
+		return nil
+	},
+}
+
+var targetTestCmd = &cobra.Command{
+	Use:   "test [name]",
+	Short: "Preview the resolved path for a target",
+	Long: `Shows which file would be created or appended to for the given target.
+
+If no name is provided, previews all targets.`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		vault := obsidian.Vault{Name: vaultName}
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+		names := obsidian.ListTargetNames(cfg)
+		if len(names) == 0 {
+			return errors.New("no targets configured")
+		}
+
+		if len(args) == 1 {
+			name := strings.TrimSpace(args[0])
+			return printTargetPlan(&vault, name)
+		}
+
+		// Preview all targets.
+		for _, name := range names {
+			fmt.Printf("%s:\n", name)
+			if err := printTargetPlan(&vault, name); err != nil {
+				fmt.Printf("  error: %v\n", err)
+			}
+		}
+		return nil
+	},
+}
+
+var targetEditCmd = &cobra.Command{
+	Use:   "edit",
+	Short: "Edit targets in CLI or open targets.yaml",
+	RunE: func(cmd *cobra.Command, args []string) error {
+		in := bufio.NewReader(os.Stdin)
+
+		path, err := obsidian.EnsureTargetsFileExists()
+		if err != nil {
+			return err
+		}
+
+		for {
+			fmt.Println("Edit targets:")
+			fmt.Println("  1) Stay in CLI (recommended)")
+			fmt.Println("  2) Open in editor")
+			fmt.Println("Press Enter for CLI, type '?' for help, or 'back' to cancel.")
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return err
+			}
+			switch action {
+			case actionBack:
+				return nil
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Edit help:")
+				fmt.Printf("- CLI mode provides guided add/edit/remove/test/list workflows.\n")
+				fmt.Printf("- Editor mode opens: %s\n", path)
+				fmt.Println()
+				continue
+			case actionSkip:
+				return runTargetEditor(in)
+			default:
+			}
+			switch strings.ToLower(strings.TrimSpace(line)) {
+			case "", "1", "cli":
+				return runTargetEditor(in)
+			case "2", "editor":
+				return obsidian.OpenInEditor(path)
+			default:
+				fmt.Println("Invalid selection. Enter 1 or 2, or type '?' for help.")
+			}
+		}
+	},
+}
+
+func init() {
+	targetCmd.Flags().BoolVar(&targetSelect, "ls", false, "select a target interactively")
+	targetCmd.Flags().BoolVar(&targetSelect, "select", false, "select a target interactively")
+	targetCmd.Flags().BoolVar(&targetDryRun, "dry-run", false, "preview the resolved target path without writing")
+	targetCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
+
+	targetCmd.AddCommand(targetListCmd)
+	targetCmd.AddCommand(targetAddCmd)
+	targetCmd.AddCommand(targetRemoveCmd)
+	targetCmd.AddCommand(targetTestCmd)
+	targetCmd.AddCommand(targetEditCmd)
+
+	rootCmd.AddCommand(targetCmd)
+}
+
+func loadTargetsOrEmpty() (obsidian.TargetsConfig, error) {
+	_, err := obsidian.EnsureTargetsFileExists()
+	if err != nil {
+		return nil, err
+	}
+	cfg, err := obsidian.LoadTargets()
+	if err != nil {
+		return nil, err
+	}
+	return cfg, nil
+}
+
+func pickTargetName() (string, error) {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return "", err
+	}
+	return pickTargetNameFromConfig(cfg)
+}
+
+func pickTargetNameFromConfig(cfg obsidian.TargetsConfig) (string, error) {
+	names := obsidian.ListTargetNames(cfg)
+	if len(names) == 0 {
+		return "", errors.New("no targets configured (run: obsidian-cli target add)")
+	}
+
+	idx, err := fuzzyfinder.Find(names, func(i int) string {
+		return names[i]
+	})
+	if err != nil {
+		return "", err
+	}
+	return names[idx], nil
+}
+
+func printTargetPlan(vault *obsidian.Vault, name string) error {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return err
+	}
+	target, ok := cfg[name]
+	if !ok {
+		return fmt.Errorf("target not found: %s", name)
+	}
+
+	effectiveVault := vault
+	if strings.TrimSpace(target.Vault) != "" {
+		effectiveVault = &obsidian.Vault{Name: strings.TrimSpace(target.Vault)}
+	}
+
+	vaultName, err := effectiveVault.DefaultName()
+	if err != nil {
+		return err
+	}
+	vaultPath, err := effectiveVault.Path()
+	if err != nil {
+		return err
+	}
+
+	plan, err := obsidian.PlanTargetAppend(vaultPath, name, target, time.Now())
+	if err != nil {
+		return err
+	}
+
+	fmt.Printf("  vault: %s\n", vaultName)
+	fmt.Printf("  note: %s\n", plan.AbsoluteNotePath)
+	fmt.Printf("  create_dirs: %t\n", plan.WillCreateDirs)
+	fmt.Printf("  create_file: %t\n", plan.WillCreateFile)
+	if plan.WillApplyTemplate {
+		fmt.Printf("  template: %s\n", plan.AbsoluteTemplate)
+	}
+	return nil
+}
+
+func runTargetAddWizard(in *bufio.Reader, vaultPath string, existingName string, existing obsidian.TargetsConfig) (string, obsidian.Target, error) {
+	step := 0
+	var name string
+	var t obsidian.Target
+
+	for {
+		switch step {
+		case 0:
+			if strings.TrimSpace(existingName) != "" {
+				name = existingName
+			} else {
+				fmt.Println("Target name")
+				fmt.Println("Type '?' for help, or 'back' to cancel.")
+				line, action, err := promptLine(in, "> ")
+				if err != nil {
+					return "", obsidian.Target{}, err
+				}
+				switch action {
+				case actionBack:
+					return "", obsidian.Target{}, errors.New("cancelled")
+				case actionHelp:
+					fmt.Println()
+					fmt.Println("Target name help:")
+					fmt.Println("- Names cannot contain whitespace.")
+					fmt.Println("- Reserved names: add, rm, ls, edit, validate, test, help.")
+					fmt.Println("- Examples: inbox, todo, log")
+					fmt.Println()
+					continue
+				case actionSkip:
+					fmt.Println("Target name is required (skip is not available here).")
+					continue
+				default:
+				}
+				if err := obsidian.ValidateTargetName(line); err != nil {
+					fmt.Printf("Invalid name: %v\n", err)
+					continue
+				}
+				if _, ok := existing[line]; ok {
+					fmt.Println("A target with that name already exists.")
+					continue
+				}
+				name = line
+			}
+			step = 1
+		case 1:
+			fmt.Println("Target type:")
+			fmt.Println("  1) file   (append to one fixed note)")
+			fmt.Println("  2) folder (append to a dated note based on a pattern)")
+			fmt.Println("Type a number, 'skip' for file (recommended), '?' for help, or 'back' to change the name.")
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return "", obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				step = 0
+				existingName = ""
+				continue
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Type help:")
+				fmt.Println("- file: always appends to the same note")
+				fmt.Println("- folder: appends to a note path derived from folder + date pattern")
+				fmt.Println()
+				continue
+			case actionSkip:
+				t.Type = "file"
+				step = 2
+				continue
+			default:
+			}
+			switch line {
+			case "1":
+				t.Type = "file"
+				step = 2
+			case "2":
+				t.Type = "folder"
+				step = 2
+			default:
+				fmt.Println("Invalid selection.")
+			}
+		case 2:
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "file" {
+				p, err := promptForTargetFile(in, vaultPath, "", name+".md")
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return "", obsidian.Target{}, err
+				}
+				t.File = p
+				step = 4
+			} else {
+				folder, err := promptForTargetFolder(in, vaultPath, "", name)
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return "", obsidian.Target{}, err
+				}
+				t.Folder = folder
+				step = 3
+			}
+		case 3:
+			pattern, err := promptForTargetPattern(in, "")
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 2
+					continue
+				}
+				return "", obsidian.Target{}, err
+			}
+			t.Pattern = pattern
+			step = 4
+		case 4:
+			template, err := promptForTemplatePath(in, vaultPath, t.Template)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+						step = 3
+					} else {
+						step = 2
+					}
+					continue
+				}
+				return "", obsidian.Target{}, err
+			}
+			t.Template = template
+			step = 5
+		case 5:
+			vaultOverride, err := promptForVaultOverride(in, t.Vault)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 4
+					continue
+				}
+				return "", obsidian.Target{}, err
+			}
+			t.Vault = vaultOverride
+			step = 6
+		case 6:
+			fmt.Println()
+			fmt.Println("Save this target? (y/N)  Type 'back' to edit, '?' for help, or 'skip' to cancel.")
+			fmt.Printf("  name: %s\n", name)
+			fmt.Printf("  type: %s\n", t.Type)
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+				fmt.Printf("  folder: %s\n", t.Folder)
+				fmt.Printf("  pattern: %s\n", t.Pattern)
+				fmt.Printf("  example: %s\n", obsidian.ExpandDatePattern(t.Pattern, time.Now()))
+			} else {
+				fmt.Printf("  file: %s\n", t.File)
+			}
+			if strings.TrimSpace(t.Template) != "" {
+				fmt.Printf("  template: %s\n", t.Template)
+			}
+			if strings.TrimSpace(t.Vault) != "" {
+				fmt.Printf("  vault: %s\n", t.Vault)
+			}
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return "", obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				step = 5
+				continue
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Save help:")
+				fmt.Println("- Answer 'y' to save to targets.yaml.")
+				fmt.Println("- Answer 'n' or press Enter to cancel without saving.")
+				fmt.Println()
+				continue
+			case actionSkip:
+				return "", obsidian.Target{}, errors.New("cancelled")
+			default:
+			}
+			if isYes(line) {
+				return name, t, nil
+			}
+			return "", obsidian.Target{}, errors.New("cancelled")
+		default:
+			return "", obsidian.Target{}, errors.New("invalid wizard state")
+		}
+	}
+}
+
+var errBack = errors.New("back")
+
+func promptForTargetFile(in *bufio.Reader, vaultPath string, existing string, defaultPath string) (string, error) {
+	fmt.Println("Target file path (relative to vault).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	if strings.TrimSpace(existing) == "" && strings.TrimSpace(defaultPath) != "" {
+		fmt.Printf("Default: %s\n", defaultPath)
+	}
+	fmt.Println("Type a path, 'ls' to browse, 'skip' to accept default/keep current, '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionHelp {
+			fmt.Println()
+			fmt.Println("File help:")
+			fmt.Println("- Path is relative to the vault root (example: Inbox/Capture.md).")
+			fmt.Println("- A .md extension is optional.")
+			fmt.Println()
+			continue
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultPath) == "" {
+				fmt.Println("No default available; enter a file path.")
+				continue
+			}
+			fmt.Printf("Using default: %s\n", defaultPath)
+			return defaultPath, nil
+		}
+		if line == "" {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultPath) != "" {
+				fmt.Printf("Using default: %s\n", defaultPath)
+				return defaultPath, nil
+			}
+			fmt.Println("File path is required.")
+			continue
+		}
+		if strings.EqualFold(line, "ls") {
+			return pickOrCreateNotePath(vaultPath)
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptForTargetFolder(in *bufio.Reader, vaultPath string, existing string, defaultFolder string) (string, error) {
+	fmt.Println("Target folder path (relative to vault).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	if strings.TrimSpace(existing) == "" && strings.TrimSpace(defaultFolder) != "" {
+		fmt.Printf("Default: %s\n", defaultFolder)
+	}
+	fmt.Println("Type a folder path, 'ls' to browse, 'skip' to accept default/keep current, '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionHelp {
+			fmt.Println()
+			fmt.Println("Folder help:")
+			fmt.Println("- Path is relative to the vault root (example: Inbox).")
+			fmt.Println("- A date-based filename will be appended using your selected pattern.")
+			fmt.Println()
+			continue
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultFolder) == "" {
+				fmt.Println("No default available; enter a folder path.")
+				continue
+			}
+			fmt.Printf("Using default: %s\n", defaultFolder)
+			return defaultFolder, nil
+		}
+		if line == "" {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultFolder) != "" {
+				fmt.Printf("Using default: %s\n", defaultFolder)
+				return defaultFolder, nil
+			}
+			fmt.Println("Folder path is required.")
+			continue
+		}
+		if strings.EqualFold(line, "ls") {
+			return pickOrCreateFolderPath(vaultPath)
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptForVaultOverride(in *bufio.Reader, existing string) (string, error) {
+	fmt.Println("Vault override (optional).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	fmt.Println("Press Enter for default vault, type a vault name, or type 'skip' to keep current.")
+	fmt.Println("Type '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		switch action {
+		case actionBack:
+			return "", errBack
+		case actionHelp:
+			fmt.Println()
+			fmt.Println("Vault override help:")
+			fmt.Println("- Leave empty to use the default vault.")
+			fmt.Println("- Set to a vault name (as it appears in Obsidian) to route this target elsewhere.")
+			fmt.Println()
+			continue
+		case actionSkip:
+			return existing, nil
+		default:
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		return line, nil
+	}
+}
+
+func promptForTemplatePath(in *bufio.Reader, vaultPath string, existing string) (string, error) {
+	fmt.Println("Template note path (optional, relative to vault).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	fmt.Println("Press Enter for none, type a path, type 'ls' to browse, or type 'skip' to keep current.")
+	fmt.Println("Type '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		switch action {
+		case actionBack:
+			return "", errBack
+		case actionHelp:
+			fmt.Println()
+			fmt.Println("Template help:")
+			fmt.Println("- Applied only when creating a new note for the first time.")
+			fmt.Println("- Supports variables like {{date}}, {{time}}, and {{title}}.")
+			fmt.Println()
+			continue
+		case actionSkip:
+			return existing, nil
+		default:
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		if line == "" {
+			return "", nil
+		}
+		if strings.EqualFold(line, "ls") {
+			path, err := pickOrCreateNotePath(vaultPath)
+			if err != nil {
+				return "", err
+			}
+			return path, nil
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptForTargetPattern(in *bufio.Reader, existing string) (string, error) {
+	now := time.Now()
+	const defaultPattern = "YYYY-MM-DD"
+	options := []struct {
+		label   string
+		pattern string
+	}{
+		{"Daily (YYYY-MM-DD)", "YYYY-MM-DD"},
+		{"Hourly (YYYY-MM-DD_HH)", "YYYY-MM-DD_HH"},
+		{"Minute (YYYY-MM-DD_HHmm)", "YYYY-MM-DD_HHmm"},
+		{"Second (YYYY-MM-DD_HHmmss)", "YYYY-MM-DD_HHmmss"},
+		{"Zettel (YYYYMMDDHHmmss)", "YYYYMMDDHHmmss"},
+		{"Daily + weekday (YYYY-MM-DD_ddd)", "YYYY-MM-DD_ddd"},
+		{"Daily + weekday full (YYYY-MM-DD_dddd)", "YYYY-MM-DD_dddd"},
+		{"Month name (YYYY-MMMM-DD)", "YYYY-MMMM-DD"},
+		{"Month abbrev (YYYY-MMM-DD)", "YYYY-MMM-DD"},
+		{"AM/PM (YYYY-MM-DD_A)", "YYYY-MM-DD_A"},
+		{"Literal blocks (YYYY-[log]-MM)", "YYYY-[log]-MM"},
+		{"Custom pattern...", ""},
+	}
+
+	fmt.Println("Filename pattern (controls when a new file is created).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	fmt.Printf("Type a number, type a pattern directly, 'skip' for default (%s), '?' for help, or 'back' to go back.\n", defaultPattern)
+	for i, o := range options {
+		ex := o.pattern
+		if ex != "" {
+			ex = obsidian.ExpandDatePattern(o.pattern, now)
+			ex = ex + ".md"
+		}
+		if o.pattern == "" {
+			fmt.Printf("  %d) %s\n", i+1, o.label)
+		} else {
+			fmt.Printf("  %d) %s -> %s\n", i+1, o.label, ex)
+		}
+	}
+
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionHelp {
+			printPatternHelp()
+			continue
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			return defaultPattern, nil
+		}
+		if line == "" {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			return defaultPattern, nil
+		}
+		// allow direct entry of a pattern
+		if line != "" && (strings.ContainsAny(line, "YMDHmsd") || strings.Contains(line, "{") || strings.Contains(line, "[") || strings.Contains(line, "]")) && !isDigits(line) {
+			return line, nil
+		}
+		n, convErr := parseChoice(line, len(options))
+		if convErr != nil {
+			fmt.Println(convErr.Error())
+			continue
+		}
+		chosen := options[n-1]
+		if chosen.pattern == "" {
+			fmt.Println("Enter custom pattern (Obsidian-style tokens; supports [literal] blocks).")
+			fmt.Println("Examples: YYYY-MM-DD_HHmm, YYYYMMDDHHmmss, YYYY-[ToDo]-MM")
+			fmt.Printf("Type 'skip' for default (%s), '?' for help, or 'back' to go back.\n", defaultPattern)
+			for {
+				custom, customAction, err := promptLine(in, "> ")
+				if err != nil {
+					return "", err
+				}
+				switch customAction {
+				case actionBack:
+					goto continuePatternMenu
+				case actionHelp:
+					printPatternHelp()
+					continue
+				case actionSkip:
+					if strings.TrimSpace(existing) != "" {
+						return existing, nil
+					}
+					return defaultPattern, nil
+				default:
+				}
+				custom = strings.TrimSpace(custom)
+				if strings.TrimSpace(custom) == "" {
+					fmt.Println("Pattern is required.")
+					continue
+				}
+				return custom, nil
+			}
+		}
+		return chosen.pattern, nil
+
+	continuePatternMenu:
+		continue
+	}
+}
+
+func printPatternHelp() {
+	fmt.Println()
+	fmt.Println("Pattern help:")
+	fmt.Println("- Tokens (curated subset): YYYY, YY, MM, M, DD, D, HH, H, mm, m, ss, s, ddd, dddd, MMM, MMMM, A, a")
+	fmt.Println("- Zettel timestamp: YYYYMMDDHHmmss")
+	fmt.Println("- Literal blocks: wrap text in [brackets], e.g. YYYY-[log]-MM")
+	fmt.Println("- Examples:")
+	fmt.Println("    YYYY-MM-DD")
+	fmt.Println("    YYYY-MM-DD_HH")
+	fmt.Println("    YYYY-MM-DD_HHmmss")
+	fmt.Println("    YYYYMMDDHHmmss")
+	fmt.Println()
+}
+
+func parseChoice(line string, max int) (int, error) {
+	if line == "" {
+		return 0, errors.New("enter a selection")
+	}
+	var n int
+	_, err := fmt.Sscanf(line, "%d", &n)
+	if err != nil || n < 1 || n > max {
+		return 0, fmt.Errorf("invalid selection: enter 1-%d", max)
+	}
+	return n, nil
+}
+
+func isDigits(s string) bool {
+	for _, r := range s {
+		if r < '0' || r > '9' {
+			return false
+		}
+	}
+	return s != ""
+}
+
+func pickOrCreateFolderPath(vaultPath string) (string, error) {
+	dirs, err := listVaultFolders(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	dirs = append(dirs, "(Create new folder...)")
+	sort.Strings(dirs)
+
+	idx, err := fuzzyfinder.Find(dirs, func(i int) string { return dirs[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := dirs[idx]
+	if choice != "(Create new folder...)" {
+		return choice, nil
+	}
+	return promptCreateFolder(vaultPath)
+}
+
+func pickOrCreateNotePath(vaultPath string) (string, error) {
+	files, err := listVaultNotes(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	files = append(files, "(Create new note...)")
+	sort.Strings(files)
+
+	idx, err := fuzzyfinder.Find(files, func(i int) string { return files[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := files[idx]
+	if choice != "(Create new note...)" {
+		return choice, nil
+	}
+	return promptCreateNote(vaultPath)
+}
+
+func listVaultFolders(vaultPath string) ([]string, error) {
+	var out []string
+	err := filepath.WalkDir(vaultPath, func(path string, d os.DirEntry, err error) error {
+		if err != nil {
+			return err
+		}
+		if !d.IsDir() {
+			return nil
+		}
+		if path == vaultPath {
+			return nil
+		}
+		base := filepath.Base(path)
+		if strings.HasPrefix(base, ".") {
+			return filepath.SkipDir
+		}
+		rel, err := filepath.Rel(vaultPath, path)
+		if err != nil {
+			return err
+		}
+		out = append(out, filepath.ToSlash(rel))
+		return nil
+	})
+	return out, err
+}
+
+func listVaultNotes(vaultPath string) ([]string, error) {
+	var out []string
+	err := filepath.WalkDir(vaultPath, func(path string, d os.DirEntry, err error) error {
+		if err != nil {
+			return err
+		}
+		base := filepath.Base(path)
+		if d.IsDir() {
+			if strings.HasPrefix(base, ".") {
+				return filepath.SkipDir
+			}
+			return nil
+		}
+		if strings.HasPrefix(base, ".") {
+			return nil
+		}
+		if strings.ToLower(filepath.Ext(base)) != ".md" {
+			return nil
+		}
+		rel, err := filepath.Rel(vaultPath, path)
+		if err != nil {
+			return err
+		}
+		out = append(out, filepath.ToSlash(strings.TrimSuffix(rel, ".md")))
+		return nil
+	})
+	return out, err
+}
+
+func promptCreateFolder(vaultPath string) (string, error) {
+	in := bufio.NewReader(os.Stdin)
+	fmt.Println("Enter folder path to create (relative to vault):")
+	fmt.Print("> ")
+	line, err := in.ReadString('\n')
+	if err != nil {
+		return "", err
+	}
+	line = strings.TrimSpace(line)
+	if line == "" {
+		return "", errors.New("folder path is required")
+	}
+	abs, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line))
+	if err != nil {
+		return "", err
+	}
+	if err := os.MkdirAll(abs, 0750); err != nil {
+		return "", err
+	}
+	return line, nil
+}
+
+func promptCreateNote(vaultPath string) (string, error) {
+	in := bufio.NewReader(os.Stdin)
+	fmt.Println("Enter note path to create (relative to vault):")
+	fmt.Print("> ")
+	line, err := in.ReadString('\n')
+	if err != nil {
+		return "", err
+	}
+	line = strings.TrimSpace(line)
+	if line == "" {
+		return "", errors.New("note path is required")
+	}
+	abs, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line))
+	if err != nil {
+		return "", err
+	}
+	if !strings.HasSuffix(strings.ToLower(abs), ".md") {
+		abs += ".md"
+	}
+	if err := os.MkdirAll(filepath.Dir(abs), 0750); err != nil {
+		return "", err
+	}
+	if err := os.WriteFile(abs, []byte{}, 0600); err != nil {
+		return "", err
+	}
+	return line, nil
+}
+
+func firstNonEmpty(values ...string) string {
+	for _, v := range values {
+		if strings.TrimSpace(v) != "" {
+			return v
+		}
+	}
+	return ""
+}
+
+func runTargetEditor(in *bufio.Reader) error {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return err
+	}
+
+	vault := obsidian.Vault{Name: vaultName}
+	vaultPath, err := vault.Path()
+	if err != nil {
+		return err
+	}
+
+	for {
+		fmt.Println()
+		fmt.Println("Target editor:")
+		fmt.Println("  1) Add target")
+		fmt.Println("  2) Edit target")
+		fmt.Println("  3) Remove target")
+		fmt.Println("  4) Test target")
+		fmt.Println("  5) List targets")
+		fmt.Println("  6) Back")
+		fmt.Println("Type '?' for help, or 'back'/'skip' to exit.")
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		switch action {
+		case actionBack, actionSkip:
+			return nil
+		case actionHelp:
+			fmt.Println()
+			fmt.Println("Editor help:")
+			fmt.Println("- Add/Edit prompts support 'back', 'skip', and '?' to help you continue without restarting.")
+			fmt.Println()
+			continue
+		default:
+		}
+		switch line {
+		case "1":
+			name, target, err := runTargetAddWizard(in, vaultPath, "", cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			cfg[name] = target
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Saved target: %s\n", name)
+		case "2":
+			name, err := pickTargetNameFromConfig(cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			current := cfg[name]
+			updated, err := runTargetEditWizard(in, vaultPath, name, current)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			cfg[name] = updated
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Updated target: %s\n", name)
+		case "3":
+			name, err := pickTargetNameFromConfig(cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			delete(cfg, name)
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Removed target: %s\n", name)
+		case "4":
+			name, err := pickTargetNameFromConfig(cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			if err := printTargetPlan(&vault, name); err != nil {
+				fmt.Printf("Error: %v\n", err)
+			}
+		case "5":
+			names := obsidian.ListTargetNames(cfg)
+			if len(names) == 0 {
+				fmt.Println("No targets configured.")
+				continue
+			}
+			for _, n := range names {
+				t := cfg[n]
+				if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+					fmt.Printf("- %s (folder: %s, pattern: %s)\n", n, t.Folder, t.Pattern)
+				} else {
+					fmt.Printf("- %s (file: %s)\n", n, firstNonEmpty(t.File, t.Note))
+				}
+			}
+		case "6", "back":
+			return nil
+		default:
+			fmt.Println("Invalid selection.")
+		}
+	}
+}
+
+func runTargetEditWizard(in *bufio.Reader, vaultPath string, name string, current obsidian.Target) (obsidian.Target, error) {
+	step := 0
+	t := current
+
+	for {
+		switch step {
+		case 0:
+			fmt.Printf("Editing target: %s\n", name)
+			step = 1
+		case 1:
+			fmt.Println("Target type:")
+			fmt.Printf("Current: %s (press Enter to keep)\n", strings.TrimSpace(t.Type))
+			fmt.Println("  1) file   (append to one fixed note)")
+			fmt.Println("  2) folder (append to a dated note based on a pattern)")
+			fmt.Println("Type a number, press Enter to keep current, 'skip' to keep current, '?' for help, or 'back' to cancel.")
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				return obsidian.Target{}, errors.New("cancelled")
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Type help:")
+				fmt.Println("- file: always appends to the same note")
+				fmt.Println("- folder: appends to a note path derived from folder + date pattern")
+				fmt.Println()
+				continue
+			case actionSkip:
+				step = 2
+				continue
+			default:
+			}
+			if line == "" && strings.TrimSpace(t.Type) != "" {
+				step = 2
+				continue
+			}
+			switch line {
+			case "1":
+				t.Type = "file"
+				step = 2
+			case "2":
+				t.Type = "folder"
+				step = 2
+			default:
+				fmt.Println("Invalid selection.")
+			}
+		case 2:
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "file" {
+				p, err := promptForTargetFile(in, vaultPath, firstNonEmpty(t.File, t.Note), name+".md")
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return obsidian.Target{}, err
+				}
+				t.File = p
+				t.Note = ""
+				t.Folder = ""
+				t.Pattern = ""
+				step = 4
+			} else {
+				folder, err := promptForTargetFolder(in, vaultPath, t.Folder, name)
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return obsidian.Target{}, err
+				}
+				t.Folder = folder
+				step = 3
+			}
+		case 3:
+			pattern, err := promptForTargetPattern(in, t.Pattern)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 2
+					continue
+				}
+				return obsidian.Target{}, err
+			}
+			t.Pattern = pattern
+			step = 4
+		case 4:
+			template, err := promptForTemplatePath(in, vaultPath, t.Template)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+						step = 3
+					} else {
+						step = 2
+					}
+					continue
+				}
+				return obsidian.Target{}, err
+			}
+			t.Template = template
+			step = 5
+		case 5:
+			vaultOverride, err := promptForVaultOverride(in, t.Vault)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 4
+					continue
+				}
+				return obsidian.Target{}, err
+			}
+			t.Vault = vaultOverride
+			step = 6
+		case 6:
+			fmt.Println()
+			fmt.Println("Save changes? (y/N)  Type 'back' to edit, '?' for help, or 'skip' to cancel.")
+			fmt.Printf("  name: %s\n", name)
+			fmt.Printf("  type: %s\n", t.Type)
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+				fmt.Printf("  folder: %s\n", t.Folder)
+				fmt.Printf("  pattern: %s\n", t.Pattern)
+				fmt.Printf("  example: %s\n", obsidian.ExpandDatePattern(t.Pattern, time.Now()))
+			} else {
+				fmt.Printf("  file: %s\n", t.File)
+			}
+			if strings.TrimSpace(t.Template) != "" {
+				fmt.Printf("  template: %s\n", t.Template)
+			}
+			if strings.TrimSpace(t.Vault) != "" {
+				fmt.Printf("  vault: %s\n", t.Vault)
+			}
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				step = 5
+				continue
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Save help:")
+				fmt.Println("- Answer 'y' to save to targets.yaml.")
+				fmt.Println("- Answer 'n' or press Enter to cancel without saving.")
+				fmt.Println()
+				continue
+			case actionSkip:
+				return obsidian.Target{}, errors.New("cancelled")
+			default:
+			}
+			if isYes(line) {
+				return t, nil
+			}
+			return obsidian.Target{}, errors.New("cancelled")
+		default:
+			return obsidian.Target{}, errors.New("invalid wizard state")
+		}
+	}
+}
diff --git a/cmd/wizard_prompt.go b/cmd/wizard_prompt.go
new file mode 100644
index 0000000..e55327b
--- /dev/null
+++ b/cmd/wizard_prompt.go
@@ -0,0 +1,44 @@
+package cmd
+
+import (
+	"bufio"
+	"fmt"
+	"strings"
+)
+
+type promptAction int
+
+const (
+	actionNone promptAction = iota
+	actionBack
+	actionSkip
+	actionHelp
+)
+
+func promptLine(in *bufio.Reader, prompt string) (string, promptAction, error) {
+	fmt.Print(prompt)
+	line, err := in.ReadString('\n')
+	if err != nil {
+		return "", actionNone, err
+	}
+	line = strings.TrimSpace(line)
+	switch strings.ToLower(line) {
+	case "back":
+		return "", actionBack, nil
+	case "skip":
+		return "", actionSkip, nil
+	case "?":
+		return "", actionHelp, nil
+	default:
+		return line, actionNone, nil
+	}
+}
+
+func isYes(s string) bool {
+	switch strings.ToLower(strings.TrimSpace(s)) {
+	case "y", "yes":
+		return true
+	default:
+		return false
+	}
+}
diff --git a/cmd/wizard_ui.go b/cmd/wizard_ui.go
new file mode 100644
index 0000000..90aded0
--- /dev/null
+++ b/cmd/wizard_ui.go
@@ -0,0 +1,28 @@
+package cmd
+
+import "strings"
+
+// wizard_ui.go defines shared constants and functions for interactive wizard UI styling.
+// This ensures consistent border styling across all wizard prompts.
+
+// DefaultBorderWidth is the standard width for wizard borders
+const DefaultBorderWidth = 76
+
+// DoubleLine returns a double-line border of the specified width.
+// Used for major section headers (wizard start, completion).
+func DoubleLine(width int) string {
+	if width < 0 {
+		width = 0
+	}
+	return strings.Repeat("═", width)
+}
+
+// SingleLine returns a single-line border of the specified width.
+// Used for step headers within a wizard.
+func SingleLine(width int) string {
+	if width < 0 {
+		width = 0
+	}
+	return strings.Repeat("─", width)
+}
+
diff --git a/go.mod b/go.mod
index 656a5b7..a7631c0 100644
--- a/go.mod
+++ b/go.mod
@@ -10,6 +10,8 @@ require (
 )
 
 require (
+	github.com/BurntSushi/toml v0.3.1 // indirect
+	github.com/adrg/frontmatter v0.2.0 // indirect
 	github.com/davecgh/go-spew v1.1.1 // indirect
 	github.com/gdamore/encoding v1.0.1 // indirect
 	github.com/gdamore/tcell/v2 v2.7.4 // indirect
@@ -25,5 +27,6 @@ require (
 	golang.org/x/sys v0.28.0 // indirect
 	golang.org/x/term v0.27.0 // indirect
 	golang.org/x/text v0.21.0 // indirect
+	gopkg.in/yaml.v2 v2.3.0 // indirect
 	gopkg.in/yaml.v3 v3.0.1 // indirect
 )
diff --git a/go.sum b/go.sum
index 2339b50..294ac6d 100644
--- a/go.sum
+++ b/go.sum
@@ -1,3 +1,7 @@
+github.com/BurntSushi/toml v0.3.1 h1:WXkYYl6Yr3qBf1K79EBnL4mak0OimBfB0XUf9Vl28OQ=
+github.com/BurntSushi/toml v0.3.1/go.mod h1:xHWCNGjB5oqiDr8zfno3MHue2Ht5sIBksp03qcyfWMU=
+github.com/adrg/frontmatter v0.2.0 h1:/DgnNe82o03riBd1S+ZDjd43wAmC6W35q67NHeLkPd4=
+github.com/adrg/frontmatter v0.2.0/go.mod h1:93rQCj3z3ZlwyxxpQioRKC1wDLto4aXHrbqIsnH9wmE=
 github.com/cpuguy83/go-md2man/v2 v2.0.4/go.mod h1:tgQtvFlXSQOSOSIRvRPT7W67SCa46tRHOmNcaadrF8o=
 github.com/davecgh/go-spew v1.1.0/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
 github.com/davecgh/go-spew v1.1.1 h1:vj9j/u1bqnvCEfJOwUhtlOARqs3+rkHYY13jYWTU97c=
@@ -90,6 +94,8 @@ golang.org/x/tools v0.6.0/go.mod h1:Xwgl3UAJ/d3gWutnCtw505GrjyAbvKui8lOU390QaIU=
 golang.org/x/xerrors v0.0.0-20190717185122-a985d3407aa7/go.mod h1:I/5z698sn9Ka8TeJc9MKroUUfqBBauWjQqLJ2OPfmY0=
 gopkg.in/check.v1 v0.0.0-20161208181325-20d25e280405 h1:yhCVgyC4o1eVCa2tZl7eS0r+SDo693bJlVdllGtEeKM=
 gopkg.in/check.v1 v0.0.0-20161208181325-20d25e280405/go.mod h1:Co6ibVJAznAaIkqp8huTwlJQCZ016jof/cbN4VW5Yz0=
+gopkg.in/yaml.v2 v2.3.0 h1:clyUAQHOM3G0M3f5vQj7LuJrETvjVot3Z5el9nffUtU=
+gopkg.in/yaml.v2 v2.3.0/go.mod h1:hI93XBmqTisBFMUTm0b8Fm+jr3Dg1NNxqwp+5A1VGuI=
 gopkg.in/yaml.v3 v3.0.0-20200313102051-9f266ea9e77c/go.mod h1:K4uyk7z7BCEPqu6E+C64Yfv1cQ7kz7rIZviUmN+EgEM=
 gopkg.in/yaml.v3 v3.0.1 h1:fxVm/GzAzEWqLHuvctI91KS9hhNmmWOoWu0XTYJS7CA=
 gopkg.in/yaml.v3 v3.0.1/go.mod h1:K4uyk7z7BCEPqu6E+C64Yfv1cQ7kz7rIZviUmN+EgEM=
diff --git a/mocks/note.go b/mocks/note.go
index efb9295..407e788 100644
--- a/mocks/note.go
+++ b/mocks/note.go
@@ -7,7 +7,9 @@ type MockNoteManager struct {
 	MoveErr          error
 	UpdateLinksError error
 	GetContentsError error
+	SetContentsError error
 	NoMatches        bool
+	Contents         string
 }
 
 func (m *MockNoteManager) Delete(string) error {
@@ -23,9 +25,16 @@ func (m *MockNoteManager) UpdateLinks(string, string, string) error {
 }
 
 func (m *MockNoteManager) GetContents(string, string) (string, error) {
+	if m.Contents != "" {
+		return m.Contents, m.GetContentsError
+	}
 	return "example contents", m.GetContentsError
 }
 
+func (m *MockNoteManager) SetContents(string, string, string) error {
+	return m.SetContentsError
+}
+
 func (m *MockNoteManager) GetNotesList(string) ([]string, error) {
 	return []string{"note1", "note2", "note3"}, m.GetContentsError
 }
diff --git a/pkg/actions/append.go b/pkg/actions/append.go
index fc1427e..0293eee 100644
--- a/pkg/actions/append.go
+++ b/pkg/actions/append.go
@@ -13,43 +13,122 @@ import (
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 )
 
-// AppendToDailyNote appends content to today's daily note, using per-vault settings.
-func AppendToDailyNote(vault *obsidian.Vault, content string) error {
-	content = strings.TrimSpace(content)
-	if content == "" {
-		return errors.New("no content provided")
+type DailyAppendPlan struct {
+	VaultName         string
+	VaultPath         string
+	Folder            string
+	Pattern           string
+	Filename          string
+	RelativeNotePath  string
+	AbsoluteNotePath  string
+	WillCreateDirs    bool
+	WillCreateFile    bool
+	TemplateRel       string
+	TemplateAbs       string
+	WillApplyTemplate bool
+}
+
+func PlanDailyAppend(vault *obsidian.Vault, now time.Time) (DailyAppendPlan, error) {
+	vaultName, err := vault.DefaultName()
+	if err != nil {
+		return DailyAppendPlan{}, err
 	}
 
 	vaultPath, err := vault.Path()
 	if err != nil {
-		return err
+		return DailyAppendPlan{}, err
 	}
 
 	settings, err := vault.Settings()
 	if err != nil {
-		return err
+		return DailyAppendPlan{}, err
 	}
 
 	folder := strings.TrimSpace(settings.DailyNote.Folder)
 	if folder == "" {
-		return errors.New("daily note is not configured (missing daily_note.folder in preferences.json)")
+		return DailyAppendPlan{}, errors.New("daily note is not configured (missing daily_note.folder in preferences.json)")
 	}
 
 	pattern := strings.TrimSpace(settings.DailyNote.FilenamePattern)
 	if pattern == "" {
 		pattern = "{YYYY-MM-DD}"
 	}
+	filename, err := obsidian.FormatDatePattern(pattern, now)
+	if err != nil {
+		return DailyAppendPlan{}, err
+	}
 
-	filename := expandDailyFilename(pattern, time.Now())
 	relNotePath := filepath.ToSlash(filepath.Join(folder, filename))
 	abs, err := safeJoinVaultPath(vaultPath, relNotePath)
 	if err != nil {
-		return err
+		return DailyAppendPlan{}, err
 	}
 	if !strings.HasSuffix(strings.ToLower(abs), ".md") {
 		abs += ".md"
 	}
 
+	plan := DailyAppendPlan{
+		VaultName:        vaultName,
+		VaultPath:        vaultPath,
+		Folder:           folder,
+		Pattern:          pattern,
+		Filename:         filename,
+		RelativeNotePath: relNotePath,
+		AbsoluteNotePath: abs,
+	}
+
+	if _, err := os.Stat(filepath.Dir(abs)); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateDirs = true
+		} else {
+			return DailyAppendPlan{}, err
+		}
+	}
+
+	if _, err := os.Stat(abs); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateFile = true
+		} else {
+			return DailyAppendPlan{}, err
+		}
+	}
+
+	templateRel := strings.TrimSpace(settings.DailyNote.TemplatePath)
+	if templateRel != "" && plan.WillCreateFile {
+		templateAbs, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(templateRel))
+		if err != nil {
+			return DailyAppendPlan{}, fmt.Errorf("invalid template path: %w", err)
+		}
+		if !strings.HasSuffix(strings.ToLower(templateAbs), ".md") {
+			templateAbs += ".md"
+		}
+		if _, err := os.Stat(templateAbs); err != nil {
+			return DailyAppendPlan{}, fmt.Errorf("failed to read template: %w", err)
+		}
+
+		plan.TemplateRel = templateRel
+		plan.TemplateAbs = templateAbs
+		plan.WillApplyTemplate = true
+	}
+
+	return plan, nil
+}
+
+// AppendToDailyNote appends content to today's daily note, using per-vault settings.
+func AppendToDailyNote(vault *obsidian.Vault, content string) error {
+	content = strings.TrimSpace(content)
+	if content == "" {
+		return errors.New("no content provided")
+	}
+
+	now := time.Now()
+
+	plan, err := PlanDailyAppend(vault, now)
+	if err != nil {
+		return err
+	}
+	abs := plan.AbsoluteNotePath
+
 	if err := os.MkdirAll(filepath.Dir(abs), 0750); err != nil {
 		return fmt.Errorf("failed to create note directory: %w", err)
 	}
@@ -65,6 +144,16 @@ func AppendToDailyNote(vault *obsidian.Vault, content string) error {
 			return fmt.Errorf("failed to read note: %w", err)
 		}
 		existing = []byte{}
+
+		if plan.WillApplyTemplate {
+			b, err := os.ReadFile(plan.TemplateAbs)
+			if err != nil {
+				return fmt.Errorf("failed to read template: %w", err)
+			}
+			title := filepath.Base(abs)
+			b = obsidian.ExpandTemplateVariablesAt(b, title, now)
+			existing = b
+		}
 	}
 
 	next := appendWithSeparator(string(existing), content)
@@ -108,13 +197,6 @@ func PromptForContentIfEmpty(content string) (string, error) {
 	return s, nil
 }
 
-func expandDailyFilename(pattern string, now time.Time) string {
-	out := pattern
-	out = strings.ReplaceAll(out, "{YYYY-MM-DD}", now.Format("2006-01-02"))
-	out = strings.ReplaceAll(out, "YYYY-MM-DD", now.Format("2006-01-02"))
-	return out
-}
-
 func appendWithSeparator(existing string, addition string) string {
 	existing = strings.TrimRight(existing, "\n")
 	addition = strings.TrimSpace(addition)
@@ -129,30 +211,5 @@ func appendWithSeparator(existing string, addition string) string {
 
 // safeJoinVaultPath joins a vault root and a relative note path and ensures the result stays within the vault.
 func safeJoinVaultPath(vaultPath string, relativePath string) (string, error) {
-	if filepath.IsAbs(relativePath) {
-		return "", fmt.Errorf("absolute paths are not allowed: %s", relativePath)
-	}
-	cleaned := filepath.Clean(strings.TrimSpace(relativePath))
-	cleaned = strings.TrimPrefix(cleaned, string(filepath.Separator))
-	cleaned = strings.TrimPrefix(cleaned, "./")
-	if cleaned == "" || cleaned == "." {
-		return "", fmt.Errorf("note path cannot be empty")
-	}
-
-	absVault, err := filepath.Abs(vaultPath)
-	if err != nil {
-		return "", fmt.Errorf("failed to resolve vault path: %w", err)
-	}
-
-	joined := filepath.Join(absVault, filepath.FromSlash(cleaned))
-	absJoined, err := filepath.Abs(joined)
-	if err != nil {
-		return "", fmt.Errorf("failed to resolve note path: %w", err)
-	}
-
-	if absJoined != absVault && !strings.HasPrefix(absJoined, absVault+string(filepath.Separator)) {
-		return "", fmt.Errorf("note path escapes vault: %s", relativePath)
-	}
-
-	return absJoined, nil
+	return obsidian.SafeJoinVaultPath(vaultPath, relativePath)
 }
diff --git a/pkg/actions/append_test.go b/pkg/actions/append_test.go
index 52de227..5ee4280 100644
--- a/pkg/actions/append_test.go
+++ b/pkg/actions/append_test.go
@@ -46,6 +46,34 @@ func TestAppendToDailyNoteCreatesAndAppends(t *testing.T) {
 	})
 }
 
+func TestAppendToDailyNoteAppliesTemplateOnCreate(t *testing.T) {
+	withTempVaultAndConfig(t, func(vault *obsidian.Vault, vaultPath string) {
+		now := time.Now()
+
+		assert.NoError(t, os.MkdirAll(filepath.Join(vaultPath, "Templates"), 0750))
+		assert.NoError(t, os.WriteFile(filepath.Join(vaultPath, "Templates", "T.md"), []byte("Title={{title}}\nDate={{date:YYYYMMDDHHmmss}}\n"), 0600))
+
+		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
+			DailyNote: obsidian.DailyNoteSettings{
+				Folder:          "Daily",
+				FilenamePattern: "{YYYY-MM-DD}",
+				TemplatePath:    "Templates/T",
+			},
+		}))
+
+		assert.NoError(t, AppendToDailyNote(vault, "hello"))
+
+		filename := now.Format("2006-01-02") + ".md"
+		noteAbs := filepath.Join(vaultPath, "Daily", filename)
+		b, err := os.ReadFile(noteAbs)
+		assert.NoError(t, err)
+		s := string(b)
+		assert.Contains(t, s, "Title="+now.Format("2006-01-02")+"\n") // title is the note filename
+		assert.Contains(t, s, "Date=")
+		assert.Contains(t, s, "hello\n")
+	})
+}
+
 func TestAppendToDailyNoteRejectsEscapePaths(t *testing.T) {
 	withTempVaultAndConfig(t, func(vault *obsidian.Vault, _ string) {
 		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
@@ -60,6 +88,22 @@ func TestAppendToDailyNoteRejectsEscapePaths(t *testing.T) {
 	})
 }
 
+func TestPlanDailyAppendSupportsTimeBasedPatterns(t *testing.T) {
+	withTempVaultAndConfig(t, func(vault *obsidian.Vault, _ string) {
+		now := time.Date(2025, 12, 25, 17, 31, 45, 0, time.UTC)
+		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
+			DailyNote: obsidian.DailyNoteSettings{
+				Folder:          "Daily",
+				FilenamePattern: "YYYY-MM-DD_HHmmss",
+			},
+		}))
+
+		plan, err := PlanDailyAppend(vault, now)
+		assert.NoError(t, err)
+		assert.Equal(t, "2025-12-25_173145", plan.Filename)
+	})
+}
+
 func withTempVaultAndConfig(t *testing.T, fn func(vault *obsidian.Vault, vaultPath string)) {
 	t.Helper()
 
diff --git a/pkg/actions/daily.go b/pkg/actions/daily.go
index d45914e..1b3f3b9 100644
--- a/pkg/actions/daily.go
+++ b/pkg/actions/daily.go
@@ -4,15 +4,22 @@ import (
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 )
 
-func DailyNote(vault obsidian.VaultManager, uri obsidian.UriManager) error {
+func PlanDailyNote(vault obsidian.VaultManager, uri obsidian.UriManager) (string, error) {
 	vaultName, err := vault.DefaultName()
 	if err != nil {
-		return err
+		return "", err
 	}
 
-	obsidianUri := uri.Construct(OnsDailyUrl, map[string]string{
+	return uri.Construct(OnsDailyUrl, map[string]string{
 		"vault": vaultName,
-	})
+	}), nil
+}
+
+func DailyNote(vault obsidian.VaultManager, uri obsidian.UriManager) error {
+	obsidianUri, err := PlanDailyNote(vault, uri)
+	if err != nil {
+		return err
+	}
 
 	err = uri.Execute(obsidianUri)
 	if err != nil {
diff --git a/pkg/actions/delete.go b/pkg/actions/delete.go
index 1b606bf..d079f89 100644
--- a/pkg/actions/delete.go
+++ b/pkg/actions/delete.go
@@ -14,20 +14,46 @@ type DeleteParams struct {
 	Force    bool
 }
 
-func DeleteNote(vault obsidian.VaultManager, note obsidian.NoteManager, params DeleteParams) error {
-	_, err := vault.DefaultName()
+type DeletePlan struct {
+	VaultName        string
+	VaultPath        string
+	RelativeNotePath string
+	AbsoluteNotePath string
+}
+
+func PlanDeleteNote(vault obsidian.VaultManager, params DeleteParams) (DeletePlan, error) {
+	vaultName, err := vault.DefaultName()
 	if err != nil {
-		return err
+		return DeletePlan{}, err
 	}
 
 	vaultPath, err := vault.Path()
+	if err != nil {
+		return DeletePlan{}, err
+	}
+
+	rel := filepath.ToSlash(params.NotePath)
+	abs, err := obsidian.SafeJoinVaultPath(vaultPath, rel)
+	if err != nil {
+		return DeletePlan{}, err
+	}
+
+	return DeletePlan{
+		VaultName:        vaultName,
+		VaultPath:        vaultPath,
+		RelativeNotePath: rel,
+		AbsoluteNotePath: abs,
+	}, nil
+}
+
+func DeleteNote(vault obsidian.VaultManager, note obsidian.NoteManager, params DeleteParams) error {
+	plan, err := PlanDeleteNote(vault, params)
 	if err != nil {
 		return err
 	}
-	notePath := filepath.Join(vaultPath, params.NotePath)
 
 	if !params.Force {
-		links, err := obsidian.FindIncomingLinks(vaultPath, params.NotePath)
+		links, err := obsidian.FindIncomingLinks(plan.VaultPath, plan.RelativeNotePath)
 		if err != nil {
 			fmt.Fprintf(os.Stderr, "warning: could not check for incoming links: %v\n", err)
 		} else if len(links) > 0 {
@@ -42,7 +68,7 @@ func DeleteNote(vault obsidian.VaultManager, note obsidian.NoteManager, params D
 		}
 	}
 
-	err = note.Delete(notePath)
+	err = note.Delete(plan.AbsoluteNotePath)
 	if err != nil {
 		return err
 	}
diff --git a/pkg/actions/delete_test.go b/pkg/actions/delete_test.go
index 9f5c696..f613740 100644
--- a/pkg/actions/delete_test.go
+++ b/pkg/actions/delete_test.go
@@ -59,4 +59,11 @@ func TestDeleteNote(t *testing.T) {
 		// Assert
 		assert.Equal(t, note.DeleteErr, err)
 	})
+
+	t.Run("rejects note paths that escape the vault", func(t *testing.T) {
+		err := actions.DeleteNote(&mocks.MockVaultOperator{}, &mocks.MockNoteManager{}, actions.DeleteParams{
+			NotePath: "../escape",
+		})
+		assert.Error(t, err)
+	})
 }
diff --git a/pkg/actions/frontmatter.go b/pkg/actions/frontmatter.go
new file mode 100644
index 0000000..c3eacb9
--- /dev/null
+++ b/pkg/actions/frontmatter.go
@@ -0,0 +1,111 @@
+package actions
+
+import (
+	"errors"
+	"fmt"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/frontmatter"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+)
+
+type FrontmatterParams struct {
+	NoteName string
+	Print    bool
+	Edit     bool
+	Delete   bool
+	Key      string
+	Value    string
+}
+
+// Frontmatter handles viewing and modifying note frontmatter.
+// Based on flags, it will print, edit, or delete frontmatter keys.
+func Frontmatter(vault obsidian.VaultManager, note obsidian.NoteManager, params FrontmatterParams) (string, error) {
+	_, err := vault.DefaultName()
+	if err != nil {
+		return "", err
+	}
+
+	vaultPath, err := vault.Path()
+	if err != nil {
+		return "", err
+	}
+
+	contents, err := note.GetContents(vaultPath, params.NoteName)
+	if err != nil {
+		return "", err
+	}
+
+	// Handle print operation
+	if params.Print {
+		return handlePrint(contents)
+	}
+
+	// Handle edit operation
+	if params.Edit {
+		return handleEdit(note, vaultPath, params.NoteName, contents, params.Key, params.Value)
+	}
+
+	// Handle delete operation
+	if params.Delete {
+		return handleDelete(note, vaultPath, params.NoteName, contents, params.Key)
+	}
+
+	return "", errors.New("no operation specified: use --print, --edit, or --delete")
+}
+
+func handlePrint(contents string) (string, error) {
+	if !frontmatter.HasFrontmatter(contents) {
+		return "", nil // Return empty for notes without frontmatter
+	}
+
+	fm, _, err := frontmatter.Parse(contents)
+	if err != nil {
+		return "", err
+	}
+
+	formatted, err := frontmatter.Format(fm)
+	if err != nil {
+		return "", err
+	}
+
+	return formatted, nil
+}
+
+func handleEdit(note obsidian.NoteManager, vaultPath, noteName, contents, key, value string) (string, error) {
+	if key == "" {
+		return "", errors.New("--key is required for edit operation")
+	}
+	if value == "" {
+		return "", errors.New("--value is required for edit operation")
+	}
+
+	updatedContent, err := frontmatter.SetKey(contents, key, value)
+	if err != nil {
+		return "", err
+	}
+
+	err = note.SetContents(vaultPath, noteName, updatedContent)
+	if err != nil {
+		return "", err
+	}
+
+	return fmt.Sprintf("Updated frontmatter key '%s' in %s", key, noteName), nil
+}
+
+func handleDelete(note obsidian.NoteManager, vaultPath, noteName, contents, key string) (string, error) {
+	if key == "" {
+		return "", errors.New("--key is required for delete operation")
+	}
+
+	updatedContent, err := frontmatter.DeleteKey(contents, key)
+	if err != nil {
+		return "", err
+	}
+
+	err = note.SetContents(vaultPath, noteName, updatedContent)
+	if err != nil {
+		return "", err
+	}
+
+	return fmt.Sprintf("Deleted frontmatter key '%s' from %s", key, noteName), nil
+}
diff --git a/pkg/actions/frontmatter_test.go b/pkg/actions/frontmatter_test.go
new file mode 100644
index 0000000..0d5231f
--- /dev/null
+++ b/pkg/actions/frontmatter_test.go
@@ -0,0 +1,237 @@
+package actions_test
+
+import (
+	"errors"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/mocks"
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestFrontmatter_Print(t *testing.T) {
+	t.Run("Print frontmatter successfully", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test Note\ntags:\n  - a\n  - b\n---\nBody content",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "title: Test Note")
+		assert.Contains(t, output, "tags:")
+	})
+
+	t.Run("Print empty for note without frontmatter", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "Just body content without frontmatter",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.NoError(t, err)
+		assert.Empty(t, output)
+	})
+
+	t.Run("Vault error propagates", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{
+			DefaultNameErr: errors.New("vault error"),
+		}
+		note := mocks.MockNoteManager{}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.Error(t, err)
+	})
+
+	t.Run("Note error propagates", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			GetContentsError: errors.New("note not found"),
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.Error(t, err)
+	})
+}
+
+func TestFrontmatter_Edit(t *testing.T) {
+	t.Run("Edit existing frontmatter key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Old Title\n---\nBody",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+			Value:    "New Title",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Updated frontmatter key 'title'")
+	})
+
+	t.Run("Add new frontmatter key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "author",
+			Value:    "John",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Updated frontmatter key 'author'")
+	})
+
+	t.Run("Create frontmatter when none exists", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "Just body content",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+			Value:    "New Note",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Updated frontmatter key 'title'")
+	})
+
+	t.Run("Edit requires key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Value:    "value",
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "--key is required")
+	})
+
+	t.Run("Edit requires value", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "--value is required")
+	})
+
+	t.Run("Write error propagates", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents:         "---\ntitle: Test\n---\nBody",
+			SetContentsError: errors.New("write error"),
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+			Value:    "New",
+		})
+
+		assert.Error(t, err)
+	})
+}
+
+func TestFrontmatter_Delete(t *testing.T) {
+	t.Run("Delete existing key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\nauthor: John\n---\nBody",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Delete:   true,
+			Key:      "author",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Deleted frontmatter key 'author'")
+	})
+
+	t.Run("Delete requires key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Delete:   true,
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "--key is required")
+	})
+
+	t.Run("Delete from note without frontmatter returns error", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "Just body content",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Delete:   true,
+			Key:      "title",
+		})
+
+		assert.Error(t, err)
+	})
+}
+
+func TestFrontmatter_NoOperation(t *testing.T) {
+	t.Run("Returns error when no operation specified", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "no operation specified")
+	})
+}
diff --git a/pkg/actions/search_content_test.go b/pkg/actions/search_content_test.go
index 30ecc1e..9c9c547 100644
--- a/pkg/actions/search_content_test.go
+++ b/pkg/actions/search_content_test.go
@@ -14,11 +14,12 @@ import (
 // CustomMockNoteForSingleMatch returns exactly one match for editor testing
 type CustomMockNoteForSingleMatch struct{}
 
-func (m *CustomMockNoteForSingleMatch) Delete(string) error { return nil }
-func (m *CustomMockNoteForSingleMatch) Move(string, string) error { return nil }
-func (m *CustomMockNoteForSingleMatch) UpdateLinks(string, string, string) error { return nil }
+func (m *CustomMockNoteForSingleMatch) Delete(string) error                       { return nil }
+func (m *CustomMockNoteForSingleMatch) Move(string, string) error                  { return nil }
+func (m *CustomMockNoteForSingleMatch) UpdateLinks(string, string, string) error   { return nil }
 func (m *CustomMockNoteForSingleMatch) GetContents(string, string) (string, error) { return "", nil }
-func (m *CustomMockNoteForSingleMatch) GetNotesList(string) ([]string, error) { return nil, nil }
+func (m *CustomMockNoteForSingleMatch) SetContents(string, string, string) error   { return nil }
+func (m *CustomMockNoteForSingleMatch) GetNotesList(string) ([]string, error)      { return nil, nil }
 func (m *CustomMockNoteForSingleMatch) SearchNotesWithSnippets(string, string) ([]obsidian.NoteMatch, error) {
 	return []obsidian.NoteMatch{
 		{FilePath: "test-note.md", LineNumber: 5, MatchLine: "test content"},
diff --git a/pkg/actions/target.go b/pkg/actions/target.go
new file mode 100644
index 0000000..77d4a1f
--- /dev/null
+++ b/pkg/actions/target.go
@@ -0,0 +1,94 @@
+package actions
+
+import (
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+)
+
+// AppendToTarget appends content to the note resolved by the given target.
+// If dryRun is true, it returns the computed plan without writing anything.
+func AppendToTarget(vault *obsidian.Vault, targetName string, content string, now time.Time, dryRun bool) (obsidian.TargetPlan, error) {
+	targetName = strings.TrimSpace(targetName)
+	if targetName == "" {
+		return obsidian.TargetPlan{}, errors.New("target name is required")
+	}
+	content = strings.TrimSpace(content)
+	if content == "" {
+		return obsidian.TargetPlan{}, errors.New("no content provided")
+	}
+
+	targets, err := obsidian.LoadTargets()
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+	target, ok := targets[targetName]
+	if !ok {
+		return obsidian.TargetPlan{}, fmt.Errorf("target not found: %s", targetName)
+	}
+
+	effectiveVault := vault
+	if strings.TrimSpace(target.Vault) != "" {
+		effectiveVault = &obsidian.Vault{Name: strings.TrimSpace(target.Vault)}
+	}
+
+	vaultName, err := effectiveVault.DefaultName()
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+	vaultPath, err := effectiveVault.Path()
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+
+	plan, err := obsidian.PlanTargetAppend(vaultPath, targetName, target, now)
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+	plan.VaultName = vaultName
+	plan.VaultPath = vaultPath
+
+	if dryRun {
+		return plan, nil
+	}
+
+	if err := os.MkdirAll(filepath.Dir(plan.AbsoluteNotePath), 0750); err != nil {
+		return obsidian.TargetPlan{}, fmt.Errorf("failed to create note directory: %w", err)
+	}
+
+	var existing []byte
+	mode := os.FileMode(0600)
+	if info, err := os.Stat(plan.AbsoluteNotePath); err == nil {
+		mode = info.Mode()
+		b, err := os.ReadFile(plan.AbsoluteNotePath)
+		if err != nil {
+			return obsidian.TargetPlan{}, fmt.Errorf("failed to read note: %w", err)
+		}
+		existing = b
+	} else if err != nil && !os.IsNotExist(err) {
+		return obsidian.TargetPlan{}, fmt.Errorf("failed to stat note: %w", err)
+	} else {
+		existing = []byte{}
+		if plan.WillApplyTemplate {
+			templateContent, err := os.ReadFile(plan.AbsoluteTemplate)
+			if err != nil {
+				return obsidian.TargetPlan{}, fmt.Errorf("failed to read template: %w", err)
+			}
+			title := filepath.Base(plan.AbsoluteNotePath)
+			templateContent = obsidian.ExpandTemplateVariablesAt(templateContent, title, now)
+			existing = templateContent
+		}
+	}
+
+	next := appendWithSeparator(string(existing), content)
+	if err := os.WriteFile(plan.AbsoluteNotePath, []byte(next), mode); err != nil {
+		return obsidian.TargetPlan{}, fmt.Errorf("failed to write note: %w", err)
+	}
+
+	return plan, nil
+}
diff --git a/pkg/actions/target_test.go b/pkg/actions/target_test.go
new file mode 100644
index 0000000..5096953
--- /dev/null
+++ b/pkg/actions/target_test.go
@@ -0,0 +1,89 @@
+package actions_test
+
+import (
+	"os"
+	"path/filepath"
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestAppendToTarget_DryRunAndWrite(t *testing.T) {
+	originalUserConfigDirectory := config.UserConfigDirectory
+	defer func() { config.UserConfigDirectory = originalUserConfigDirectory }()
+
+	cfgRoot := t.TempDir()
+	config.UserConfigDirectory = func() (string, error) { return cfgRoot, nil }
+
+	cliDir, targetsFile, err := config.TargetsPath()
+	assert.NoError(t, err)
+	assert.NoError(t, os.MkdirAll(cliDir, 0750))
+	assert.NoError(t, os.WriteFile(targetsFile, []byte("inbox:\n  type: file\n  file: Inbox.md\n"), 0600))
+
+	vaultRoot := t.TempDir()
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	originalObsidianConfigFile := obsidian.ObsidianConfigFile
+	defer func() { obsidian.ObsidianConfigFile = originalObsidianConfigFile }()
+	mockCfg := `{"vaults":{"id":{"path":"` + vaultRoot + `/vault"}}}`
+	cfgFile := filepath.Join(t.TempDir(), "obsidian.json")
+	assert.NoError(t, os.WriteFile(cfgFile, []byte(mockCfg), 0644))
+	obsidian.ObsidianConfigFile = func() (string, error) { return cfgFile, nil }
+	v := &obsidian.Vault{Name: "vault"}
+
+	plan, err := actions.AppendToTarget(v, "inbox", "x", now, true)
+	assert.NoError(t, err)
+	assert.True(t, plan.WillCreateFile)
+	_, err = os.Stat(filepath.Join(vaultRoot, "vault", "Inbox.md"))
+	assert.True(t, os.IsNotExist(err))
+
+	plan, err = actions.AppendToTarget(v, "inbox", "x", now, false)
+	assert.NoError(t, err)
+	b, err := os.ReadFile(filepath.Join(vaultRoot, "vault", "Inbox.md"))
+	assert.NoError(t, err)
+	assert.Equal(t, "x\n", string(b))
+}
+
+func TestAppendToTarget_AppliesTemplateOnCreate(t *testing.T) {
+	originalUserConfigDirectory := config.UserConfigDirectory
+	defer func() { config.UserConfigDirectory = originalUserConfigDirectory }()
+
+	cfgRoot := t.TempDir()
+	config.UserConfigDirectory = func() (string, error) { return cfgRoot, nil }
+
+	vaultRoot := t.TempDir()
+
+	originalObsidianConfigFile := obsidian.ObsidianConfigFile
+	defer func() { obsidian.ObsidianConfigFile = originalObsidianConfigFile }()
+	mockCfg := `{"vaults":{"id":{"path":"` + vaultRoot + `/vault"}}}`
+	cfgFile := filepath.Join(t.TempDir(), "obsidian.json")
+	assert.NoError(t, os.WriteFile(cfgFile, []byte(mockCfg), 0644))
+	obsidian.ObsidianConfigFile = func() (string, error) { return cfgFile, nil }
+
+	cliDir, targetsFile, err := config.TargetsPath()
+	assert.NoError(t, err)
+	assert.NoError(t, os.MkdirAll(cliDir, 0750))
+
+	// Template lives in the vault.
+	assert.NoError(t, os.MkdirAll(filepath.Join(vaultRoot, "vault", "Templates"), 0750))
+	assert.NoError(t, os.WriteFile(filepath.Join(vaultRoot, "vault", "Templates", "T.md"), []byte("Title={{title}}\nDate={{date:YYYYMMDDHHmmss}}\n"), 0600))
+
+	assert.NoError(t, os.WriteFile(targetsFile, []byte("t:\n  type: file\n  file: Inbox.md\n  template: Templates/T\n"), 0600))
+
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+	v := &obsidian.Vault{Name: "vault"}
+
+	_, err = actions.AppendToTarget(v, "t", "hello", now, false)
+	assert.NoError(t, err)
+
+	b, err := os.ReadFile(filepath.Join(vaultRoot, "vault", "Inbox.md"))
+	assert.NoError(t, err)
+	s := string(b)
+	assert.Contains(t, s, "Title=Inbox")
+	assert.Contains(t, s, "Date=20240115143052")
+	assert.Contains(t, s, "hello\n")
+}
diff --git a/pkg/config/targets_path.go b/pkg/config/targets_path.go
new file mode 100644
index 0000000..7bd7166
--- /dev/null
+++ b/pkg/config/targets_path.go
@@ -0,0 +1,15 @@
+package config
+
+import "path/filepath"
+
+const ObsidianCLITargetsFile = "targets.yaml"
+
+// TargetsPath returns the directory and absolute file path for targets.yaml.
+func TargetsPath() (cliConfigDir string, targetsFile string, err error) {
+	cliConfigDir, _, err = CliPath()
+	if err != nil {
+		return "", "", err
+	}
+	targetsFile = filepath.Join(cliConfigDir, ObsidianCLITargetsFile)
+	return cliConfigDir, targetsFile, nil
+}
diff --git a/pkg/config/targets_path_test.go b/pkg/config/targets_path_test.go
new file mode 100644
index 0000000..39ebde0
--- /dev/null
+++ b/pkg/config/targets_path_test.go
@@ -0,0 +1,34 @@
+package config_test
+
+import (
+	"errors"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestTargetsPath(t *testing.T) {
+	originalUserConfigDirectory := config.UserConfigDirectory
+	defer func() { config.UserConfigDirectory = originalUserConfigDirectory }()
+
+	t.Run("Returns targets path under obsidian-cli config directory", func(t *testing.T) {
+		config.UserConfigDirectory = func() (string, error) {
+			return "user/config/dir", nil
+		}
+
+		dir, file, err := config.TargetsPath()
+		assert.NoError(t, err)
+		assert.Equal(t, "user/config/dir/obsidian-cli", dir)
+		assert.Equal(t, "user/config/dir/obsidian-cli/targets.yaml", file)
+	})
+
+	t.Run("Returns error when user config directory not found", func(t *testing.T) {
+		config.UserConfigDirectory = func() (string, error) {
+			return "", errors.New(config.UserConfigDirectoryNotFoundErrorMessage)
+		}
+
+		_, _, err := config.TargetsPath()
+		assert.Equal(t, config.UserConfigDirectoryNotFoundErrorMessage, err.Error())
+	})
+}
diff --git a/pkg/frontmatter/frontmatter.go b/pkg/frontmatter/frontmatter.go
new file mode 100644
index 0000000..43a6fc4
--- /dev/null
+++ b/pkg/frontmatter/frontmatter.go
@@ -0,0 +1,145 @@
+package frontmatter
+
+import (
+	"errors"
+	"strings"
+
+	"github.com/adrg/frontmatter"
+	"gopkg.in/yaml.v3"
+)
+
+const (
+	Delimiter              = "---"
+	NoFrontmatterError     = "note does not contain frontmatter"
+	InvalidFrontmatterError = "frontmatter contains invalid YAML"
+)
+
+// Parse extracts and parses frontmatter from note content.
+// Returns the frontmatter as a map, the body content, and any error.
+func Parse(content string) (map[string]interface{}, string, error) {
+	var fm map[string]interface{}
+	rest, err := frontmatter.Parse(strings.NewReader(content), &fm)
+	if err != nil {
+		return nil, "", errors.New(InvalidFrontmatterError)
+	}
+	return fm, string(rest), nil
+}
+
+// Format converts a frontmatter map to a YAML string.
+func Format(fm map[string]interface{}) (string, error) {
+	if fm == nil || len(fm) == 0 {
+		return "", nil
+	}
+	data, err := yaml.Marshal(fm)
+	if err != nil {
+		return "", err
+	}
+	return string(data), nil
+}
+
+// HasFrontmatter checks if content starts with frontmatter delimiters.
+func HasFrontmatter(content string) bool {
+	lines := strings.Split(content, "\n")
+	if len(lines) == 0 {
+		return false
+	}
+	return strings.TrimSpace(lines[0]) == Delimiter
+}
+
+// SetKey updates or adds a key in the frontmatter, returning the full updated content.
+// If no frontmatter exists, it creates new frontmatter with the key.
+func SetKey(content, key, value string) (string, error) {
+	parsedValue := parseValue(value)
+
+	if !HasFrontmatter(content) {
+		// Create new frontmatter
+		fm := map[string]interface{}{key: parsedValue}
+		fmStr, err := yaml.Marshal(fm)
+		if err != nil {
+			return "", err
+		}
+		return Delimiter + "\n" + string(fmStr) + Delimiter + "\n" + content, nil
+	}
+
+	// Parse existing frontmatter
+	fm, body, err := Parse(content)
+	if err != nil {
+		return "", err
+	}
+
+	if fm == nil {
+		fm = make(map[string]interface{})
+	}
+
+	// Update the key
+	fm[key] = parsedValue
+
+	// Reconstruct content
+	fmStr, err := yaml.Marshal(fm)
+	if err != nil {
+		return "", err
+	}
+
+	return Delimiter + "\n" + string(fmStr) + Delimiter + "\n" + body, nil
+}
+
+// DeleteKey removes a key from the frontmatter, returning the full updated content.
+func DeleteKey(content, key string) (string, error) {
+	if !HasFrontmatter(content) {
+		return "", errors.New(NoFrontmatterError)
+	}
+
+	fm, body, err := Parse(content)
+	if err != nil {
+		return "", err
+	}
+
+	if fm == nil {
+		return "", errors.New(NoFrontmatterError)
+	}
+
+	// Delete the key
+	delete(fm, key)
+
+	// If no keys left, return just the body
+	if len(fm) == 0 {
+		return strings.TrimPrefix(body, "\n"), nil
+	}
+
+	// Reconstruct content
+	fmStr, err := yaml.Marshal(fm)
+	if err != nil {
+		return "", err
+	}
+
+	return Delimiter + "\n" + string(fmStr) + Delimiter + "\n" + body, nil
+}
+
+// parseValue attempts to parse the value into appropriate Go types.
+// Supports: booleans, arrays (comma-separated in brackets), strings.
+func parseValue(value string) interface{} {
+	// Try boolean
+	if value == "true" {
+		return true
+	}
+	if value == "false" {
+		return false
+	}
+
+	// Try array (comma-separated in brackets)
+	if strings.HasPrefix(value, "[") && strings.HasSuffix(value, "]") {
+		inner := value[1 : len(value)-1]
+		if inner == "" {
+			return []string{}
+		}
+		parts := strings.Split(inner, ",")
+		result := make([]string, 0, len(parts))
+		for _, p := range parts {
+			result = append(result, strings.TrimSpace(p))
+		}
+		return result
+	}
+
+	// Default to string
+	return value
+}
diff --git a/pkg/frontmatter/frontmatter_test.go b/pkg/frontmatter/frontmatter_test.go
new file mode 100644
index 0000000..ab4155e
--- /dev/null
+++ b/pkg/frontmatter/frontmatter_test.go
@@ -0,0 +1,172 @@
+package frontmatter_test
+
+import (
+	"strings"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/frontmatter"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestParse(t *testing.T) {
+	t.Run("Parse valid frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\ntags:\n  - a\n  - b\n---\nBody content"
+		fm, body, err := frontmatter.Parse(content)
+		assert.NoError(t, err)
+		assert.Equal(t, "Test", fm["title"])
+		assert.Equal(t, "Body content", body)
+	})
+
+	t.Run("Parse empty frontmatter", func(t *testing.T) {
+		content := "---\n---\nBody content"
+		fm, body, err := frontmatter.Parse(content)
+		assert.NoError(t, err)
+		assert.Empty(t, fm)
+		assert.Equal(t, "Body content", body)
+	})
+
+	t.Run("No frontmatter returns empty map", func(t *testing.T) {
+		content := "Just body content"
+		fm, body, err := frontmatter.Parse(content)
+		assert.NoError(t, err)
+		assert.Empty(t, fm)
+		assert.Equal(t, "Just body content", body)
+	})
+
+	t.Run("Invalid YAML returns error", func(t *testing.T) {
+		content := "---\ninvalid: [unclosed\n---\nBody"
+		_, _, err := frontmatter.Parse(content)
+		assert.Error(t, err)
+	})
+}
+
+func TestHasFrontmatter(t *testing.T) {
+	t.Run("Has frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		assert.True(t, frontmatter.HasFrontmatter(content))
+	})
+
+	t.Run("No frontmatter", func(t *testing.T) {
+		content := "Just body content"
+		assert.False(t, frontmatter.HasFrontmatter(content))
+	})
+
+	t.Run("Empty content", func(t *testing.T) {
+		assert.False(t, frontmatter.HasFrontmatter(""))
+	})
+}
+
+func TestFormat(t *testing.T) {
+	t.Run("Format valid map", func(t *testing.T) {
+		fm := map[string]interface{}{
+			"title": "Test",
+		}
+		result, err := frontmatter.Format(fm)
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: Test")
+	})
+
+	t.Run("Format empty map", func(t *testing.T) {
+		result, err := frontmatter.Format(map[string]interface{}{})
+		assert.NoError(t, err)
+		assert.Empty(t, result)
+	})
+
+	t.Run("Format nil map", func(t *testing.T) {
+		result, err := frontmatter.Format(nil)
+		assert.NoError(t, err)
+		assert.Empty(t, result)
+	})
+}
+
+func TestSetKey(t *testing.T) {
+	t.Run("Add key to existing frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		result, err := frontmatter.SetKey(content, "author", "John")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "author: John")
+		assert.Contains(t, result, "title: Test")
+		assert.Contains(t, result, "Body")
+	})
+
+	t.Run("Update existing key", func(t *testing.T) {
+		content := "---\ntitle: Old\n---\nBody"
+		result, err := frontmatter.SetKey(content, "title", "New")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: New")
+		assert.NotContains(t, result, "title: Old")
+	})
+
+	t.Run("Create frontmatter when none exists", func(t *testing.T) {
+		content := "Just body content"
+		result, err := frontmatter.SetKey(content, "title", "New")
+		assert.NoError(t, err)
+		assert.True(t, strings.HasPrefix(result, "---\n"))
+		assert.Contains(t, result, "title: New")
+		assert.Contains(t, result, "Just body content")
+	})
+
+	t.Run("Parse boolean value true", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "draft", "true")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "draft: true")
+	})
+
+	t.Run("Parse boolean value false", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "published", "false")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "published: false")
+	})
+
+	t.Run("Parse array value", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "tags", "[one, two, three]")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "tags:")
+		assert.Contains(t, result, "- one")
+		assert.Contains(t, result, "- two")
+		assert.Contains(t, result, "- three")
+	})
+
+	t.Run("Parse empty array value", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "tags", "[]")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "tags: []")
+	})
+}
+
+func TestDeleteKey(t *testing.T) {
+	t.Run("Delete existing key", func(t *testing.T) {
+		content := "---\ntitle: Test\nauthor: John\n---\nBody"
+		result, err := frontmatter.DeleteKey(content, "author")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: Test")
+		assert.NotContains(t, result, "author")
+		assert.Contains(t, result, "Body")
+	})
+
+	t.Run("Delete last key removes frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		result, err := frontmatter.DeleteKey(content, "title")
+		assert.NoError(t, err)
+		assert.False(t, strings.HasPrefix(result, "---"))
+		assert.Contains(t, result, "Body")
+	})
+
+	t.Run("Delete from no frontmatter returns error", func(t *testing.T) {
+		content := "Just body content"
+		_, err := frontmatter.DeleteKey(content, "title")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "does not contain frontmatter")
+	})
+
+	t.Run("Delete non-existent key succeeds", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		result, err := frontmatter.DeleteKey(content, "nonexistent")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: Test")
+	})
+}
diff --git a/pkg/obsidian/date_format.go b/pkg/obsidian/date_format.go
new file mode 100644
index 0000000..de93737
--- /dev/null
+++ b/pkg/obsidian/date_format.go
@@ -0,0 +1,164 @@
+package obsidian
+
+import (
+	"fmt"
+	"strings"
+	"time"
+)
+
+// ExpandDatePattern expands an Obsidian-style date format string using the provided time.
+//
+// It supports:
+//   - Obsidian/Moment.js-style tokens (curated subset): YYYY, YY, MMMM, MMM, MM, M, DD, D,
+//     dddd, ddd, HH, H, hh, h, mm, m, ss, s, A, a, ZZ, Z, z
+//   - Literal blocks wrapped in square brackets, e.g. YYYY-[ToDo]-MM
+//   - Legacy "token brace" patterns used in this repo, e.g. {YYYY-MM-DD-HHmmss}
+//   - The "zettel" style: YYYYMMDDHHmmss (with or without braces)
+//
+// Unknown characters are treated literally.
+func ExpandDatePattern(pattern string, now time.Time) string {
+	s, _ := FormatDatePattern(pattern, now)
+	return s
+}
+
+// FormatDatePattern is like ExpandDatePattern, but returns an error if the pattern is empty.
+func FormatDatePattern(pattern string, now time.Time) (string, error) {
+	pattern = strings.TrimSpace(pattern)
+	if pattern == "" {
+		return "", fmt.Errorf("pattern is empty")
+	}
+
+	cleaned, literals := stripBracesAndCaptureLiterals(pattern)
+	goFormat := convertObsidianFormatToGo(cleaned)
+	formatted := now.Format(goFormat)
+
+	for placeholder, literal := range literals {
+		formatted = strings.ReplaceAll(formatted, placeholder, literal)
+	}
+
+	return formatted, nil
+}
+
+func stripBracesAndCaptureLiterals(pattern string) (cleaned string, literals map[string]string) {
+	var b strings.Builder
+	literals = map[string]string{}
+
+	inBracketLiteral := false
+	var literal strings.Builder
+	literalIndex := 0
+
+	for _, r := range pattern {
+		switch r {
+		case '[':
+			if inBracketLiteral {
+				literal.WriteRune(r)
+				continue
+			}
+			inBracketLiteral = true
+			literal.Reset()
+		case ']':
+			if !inBracketLiteral {
+				b.WriteRune(r)
+				continue
+			}
+			inBracketLiteral = false
+			placeholder := fmt.Sprintf("\x00LIT%d\x00", literalIndex)
+			literalIndex++
+			literals[placeholder] = literal.String()
+			b.WriteString(placeholder)
+		case '{', '}':
+			if inBracketLiteral {
+				literal.WriteRune(r)
+			}
+			// Treat braces as formatting sugar (legacy patterns) and ignore them.
+		default:
+			if inBracketLiteral {
+				literal.WriteRune(r)
+				continue
+			}
+			b.WriteRune(r)
+		}
+	}
+
+	// If a literal block was opened but never closed, treat it literally (keep the '[').
+	if inBracketLiteral {
+		b.WriteRune('[')
+		b.WriteString(literal.String())
+	}
+
+	return b.String(), literals
+}
+
+func convertObsidianFormatToGo(format string) string {
+	// Curated Obsidian/Moment tokens mapped to Go time format.
+	tokenMap := map[string]string{
+		// Year
+		"YYYY": "2006",
+		"YY":   "06",
+
+		// Month
+		"MMMM": "January",
+		"MMM":  "Jan",
+		"MM":   "01",
+		"M":    "1",
+
+		// Day of month
+		"DD": "02",
+		"D":  "2",
+
+		// Weekday
+		"dddd": "Monday",
+		"ddd":  "Mon",
+
+		// Hour
+		"HH": "15",
+		"H":  "15",
+		"hh": "03",
+		"h":  "3",
+
+		// Minute
+		"mm": "04",
+		"m":  "4",
+
+		// Second
+		"ss": "05",
+		"s":  "5",
+
+		// AM/PM
+		"A": "PM",
+		"a": "pm",
+
+		// Timezone
+		"ZZ": "-0700",
+		"Z":  "-07:00",
+		"z":  "MST",
+	}
+
+	orderedTokens := []string{
+		"YYYY", "MMMM", "dddd",
+		"MMM", "ddd",
+		"YY", "MM", "DD", "HH", "hh", "mm", "ss", "ZZ",
+		"M", "D", "H", "h", "m", "s", "A", "a", "Z", "z",
+	}
+
+	var out strings.Builder
+	i := 0
+	for i < len(format) {
+		matched := false
+		for _, tok := range orderedTokens {
+			if i+len(tok) <= len(format) && format[i:i+len(tok)] == tok {
+				out.WriteString(tokenMap[tok])
+				i += len(tok)
+				matched = true
+				break
+			}
+		}
+		if matched {
+			continue
+		}
+		out.WriteByte(format[i])
+		i++
+	}
+	return out.String()
+}
+
diff --git a/pkg/obsidian/date_format_test.go b/pkg/obsidian/date_format_test.go
new file mode 100644
index 0000000..fdb0b73
--- /dev/null
+++ b/pkg/obsidian/date_format_test.go
@@ -0,0 +1,44 @@
+package obsidian_test
+
+import (
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestExpandDatePattern(t *testing.T) {
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	cases := []struct {
+		name    string
+		pattern string
+		want    string
+	}{
+		{"brace date", "{YYYY-MM-DD}", "2024-01-15"},
+		{"plain date", "YYYY-MM-DD", "2024-01-15"},
+		{"brace datetime minutes", "{YYYY-MM-DD-HHmm}", "2024-01-15-1430"},
+		{"brace datetime seconds", "{YYYY-MM-DD-HHmmss}", "2024-01-15-143052"},
+		{"zettel braced", "{YYYYMMDDHHmmss}", "20240115143052"},
+		{"zettel plain", "YYYYMMDDHHmmss", "20240115143052"},
+		{"weekday", "dddd", "Monday"},
+		{"month name", "MMMM", "January"},
+		{"month abbrev", "MMM", "Jan"},
+		{"bracket literal", "YYYY-[ToDo]-MM", "2024-ToDo-01"},
+		{"bracket literal preserved", "[Mon]-YYYY", "Mon-2024"},
+		{"braces + literal", "{YYYY}-[Mon]-{MM}", "2024-Mon-01"},
+	}
+
+	for _, tc := range cases {
+		t.Run(tc.name, func(t *testing.T) {
+			assert.Equal(t, tc.want, obsidian.ExpandDatePattern(tc.pattern, now))
+		})
+	}
+}
+
+func TestFormatDatePatternErrorsOnEmpty(t *testing.T) {
+	_, err := obsidian.FormatDatePattern("   ", time.Now())
+	assert.Error(t, err)
+}
+
diff --git a/pkg/obsidian/note.go b/pkg/obsidian/note.go
index 3608dd9..f9f5f49 100644
--- a/pkg/obsidian/note.go
+++ b/pkg/obsidian/note.go
@@ -25,6 +25,7 @@ type NoteManager interface {
 	Delete(string) error
 	UpdateLinks(string, string, string) error
 	GetContents(string, string) (string, error)
+	SetContents(string, string, string) error
 	GetNotesList(string) ([]string, error)
 	SearchNotesWithSnippets(string, string) ([]NoteMatch, error)
 }
@@ -101,6 +102,45 @@ func (m *Note) GetContents(vaultPath string, noteName string) (string, error) {
 	return string(content), nil
 }
 
+func (m *Note) SetContents(vaultPath string, noteName string, content string) error {
+	note := AddMdSuffix(noteName)
+
+	var notePath string
+	err := filepath.WalkDir(vaultPath, func(path string, d os.DirEntry, err error) error {
+		if err != nil {
+			return err
+		}
+		if d.IsDir() {
+			return nil
+		}
+
+		// Check for full path match first
+		relPath, err := filepath.Rel(vaultPath, path)
+		if err == nil && relPath == note {
+			notePath = path
+			return filepath.SkipDir
+		}
+
+		// Fall back to basename match for backward compatibility
+		if filepath.Base(path) == note {
+			notePath = path
+			return filepath.SkipDir
+		}
+		return nil
+	})
+
+	if err != nil || notePath == "" {
+		return errors.New(NoteDoesNotExistError)
+	}
+
+	err = os.WriteFile(notePath, []byte(content), 0644)
+	if err != nil {
+		return errors.New(VaultWriteError)
+	}
+
+	return nil
+}
+
 func (m *Note) UpdateLinks(vaultPath string, oldNoteName string, newNoteName string) error {
 	err := filepath.Walk(vaultPath, func(path string, info os.FileInfo, err error) error {
 		if err != nil {
diff --git a/pkg/obsidian/path_safety.go b/pkg/obsidian/path_safety.go
new file mode 100644
index 0000000..50840e9
--- /dev/null
+++ b/pkg/obsidian/path_safety.go
@@ -0,0 +1,37 @@
+package obsidian
+
+import (
+	"fmt"
+	"path/filepath"
+	"strings"
+)
+
+// SafeJoinVaultPath joins a vault root and a relative note path and ensures the result stays within the vault.
+func SafeJoinVaultPath(vaultPath string, relativePath string) (string, error) {
+	if filepath.IsAbs(relativePath) {
+		return "", fmt.Errorf("absolute paths are not allowed: %s", relativePath)
+	}
+	cleaned := filepath.Clean(strings.TrimSpace(relativePath))
+	cleaned = strings.TrimPrefix(cleaned, string(filepath.Separator))
+	cleaned = strings.TrimPrefix(cleaned, "./")
+	if cleaned == "" || cleaned == "." {
+		return "", fmt.Errorf("note path cannot be empty")
+	}
+
+	absVault, err := filepath.Abs(vaultPath)
+	if err != nil {
+		return "", fmt.Errorf("failed to resolve vault path: %w", err)
+	}
+
+	joined := filepath.Join(absVault, filepath.FromSlash(cleaned))
+	absJoined, err := filepath.Abs(joined)
+	if err != nil {
+		return "", fmt.Errorf("failed to resolve note path: %w", err)
+	}
+
+	if absJoined != absVault && !strings.HasPrefix(absJoined, absVault+string(filepath.Separator)) {
+		return "", fmt.Errorf("note path escapes vault: %s", relativePath)
+	}
+
+	return absJoined, nil
+}
diff --git a/pkg/obsidian/path_safety_test.go b/pkg/obsidian/path_safety_test.go
new file mode 100644
index 0000000..45b604b
--- /dev/null
+++ b/pkg/obsidian/path_safety_test.go
@@ -0,0 +1,37 @@
+package obsidian_test
+
+import (
+	"path/filepath"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestSafeJoinVaultPath(t *testing.T) {
+	vault := t.TempDir()
+
+	t.Run("Rejects absolute paths", func(t *testing.T) {
+		_, err := obsidian.SafeJoinVaultPath(vault, string(filepath.Separator)+"abs.md")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "absolute paths are not allowed:")
+	})
+
+	t.Run("Rejects empty paths", func(t *testing.T) {
+		_, err := obsidian.SafeJoinVaultPath(vault, "  ")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "note path cannot be empty")
+	})
+
+	t.Run("Rejects escape paths", func(t *testing.T) {
+		_, err := obsidian.SafeJoinVaultPath(vault, "../escape.md")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "note path escapes vault:")
+	})
+
+	t.Run("Allows normal relative paths", func(t *testing.T) {
+		got, err := obsidian.SafeJoinVaultPath(vault, "Folder/Note.md")
+		assert.NoError(t, err)
+		assert.Contains(t, got, filepath.Join(vault, "Folder", "Note.md"))
+	})
+}
diff --git a/pkg/obsidian/targets.go b/pkg/obsidian/targets.go
new file mode 100644
index 0000000..62a1304
--- /dev/null
+++ b/pkg/obsidian/targets.go
@@ -0,0 +1,298 @@
+package obsidian
+
+import (
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"gopkg.in/yaml.v3"
+)
+
+// Target represents a named capture target.
+// Targets are configured in targets.yaml stored next to preferences.json.
+type Target struct {
+	Type     string `yaml:"type,omitempty"`           // "folder" or "file"
+	Folder   string `yaml:"folder,omitempty"`         // for folder targets
+	Pattern  string `yaml:"pattern,omitempty"`        // for folder targets (Obsidian-style or legacy tokens)
+	Template string `yaml:"template,omitempty"`       // optional template path
+	File     string `yaml:"file,omitempty"`           // for file targets
+	Note     string `yaml:"note,omitempty"`           // legacy/simplified key (treated as file)
+	Vault    string `yaml:"vault,omitempty"`          // optional: override default vault
+	Format   string `yaml:"pattern_format,omitempty"` // "auto" (default), "obsidian", or "tokens"
+}
+
+func (t *Target) UnmarshalYAML(value *yaml.Node) error {
+	switch value.Kind {
+	case yaml.ScalarNode:
+		// Simplified format: id: "Some/Path" → treat as file target.
+		t.Type = "file"
+		t.File = strings.TrimSpace(value.Value)
+		return nil
+	case yaml.MappingNode:
+		type raw Target
+		var r raw
+		if err := value.Decode(&r); err != nil {
+			return err
+		}
+		*t = Target(r)
+		if strings.TrimSpace(t.File) == "" && strings.TrimSpace(t.Note) != "" {
+			t.File = t.Note
+			if strings.TrimSpace(t.Type) == "" {
+				t.Type = "file"
+			}
+		}
+		if strings.TrimSpace(t.Type) == "" {
+			// Infer if possible.
+			if strings.TrimSpace(t.Folder) != "" || strings.TrimSpace(t.Pattern) != "" {
+				t.Type = "folder"
+			} else if strings.TrimSpace(t.File) != "" {
+				t.Type = "file"
+			}
+		}
+		return nil
+	default:
+		return fmt.Errorf("invalid target definition")
+	}
+}
+
+// TargetsConfig is the map of target name to Target.
+type TargetsConfig map[string]Target
+
+var reservedTargetNames = map[string]bool{
+	"add":      true,
+	"remove":   true,
+	"rm":       true,
+	"list":     true,
+	"ls":       true,
+	"edit":     true,
+	"validate": true,
+	"test":     true,
+	"help":     true,
+}
+
+type TargetPlan struct {
+	TargetName        string
+	TargetType        string
+	VaultName         string
+	VaultPath         string
+	RelativeNotePath  string
+	AbsoluteNotePath  string
+	TemplatePath      string
+	AbsoluteTemplate  string
+	WillCreateDirs    bool
+	WillCreateFile    bool
+	WillApplyTemplate bool
+}
+
+func TargetsFile() (string, error) {
+	_, path, err := config.TargetsPath()
+	return path, err
+}
+
+func EnsureTargetsFileExists() (string, error) {
+	dir, path, err := config.TargetsPath()
+	if err != nil {
+		return "", err
+	}
+	if err := os.MkdirAll(dir, 0750); err != nil {
+		return "", err
+	}
+	if _, err := os.Stat(path); err == nil {
+		return path, nil
+	} else if err != nil && !os.IsNotExist(err) {
+		return "", err
+	}
+	if err := os.WriteFile(path, []byte(targetsFileHeader), 0600); err != nil {
+		return "", err
+	}
+	return path, nil
+}
+
+// LoadTargets loads targets from targets.yaml.
+func LoadTargets() (TargetsConfig, error) {
+	path, err := TargetsFile()
+	if err != nil {
+		return nil, err
+	}
+	raw, err := os.ReadFile(path)
+	if err != nil {
+		return nil, err
+	}
+	cfg := TargetsConfig{}
+	if err := yaml.Unmarshal(raw, &cfg); err != nil {
+		return nil, err
+	}
+	return cfg, nil
+}
+
+// SaveTargets saves targets to targets.yaml.
+func SaveTargets(cfg TargetsConfig) error {
+	path, err := EnsureTargetsFileExists()
+	if err != nil {
+		return err
+	}
+	out, err := yaml.Marshal(cfg)
+	if err != nil {
+		return err
+	}
+	content := append([]byte(targetsFileHeader), out...)
+	return os.WriteFile(path, content, 0600)
+}
+
+func ListTargetNames(cfg TargetsConfig) []string {
+	var names []string
+	for name := range cfg {
+		names = append(names, name)
+	}
+	sort.Slice(names, func(i, j int) bool { return strings.ToLower(names[i]) < strings.ToLower(names[j]) })
+	return names
+}
+
+func ValidateTargetName(name string) error {
+	n := strings.TrimSpace(name)
+	if n == "" {
+		return errors.New("target name is required")
+	}
+	if strings.ContainsAny(n, " \t\r\n") {
+		return errors.New("target name cannot contain whitespace")
+	}
+	if reservedTargetNames[strings.ToLower(n)] {
+		return fmt.Errorf("target name '%s' is reserved", n)
+	}
+	return nil
+}
+
+func ValidateTarget(t Target) error {
+	tt := strings.ToLower(strings.TrimSpace(t.Type))
+	switch tt {
+	case "file":
+		if strings.TrimSpace(t.File) == "" {
+			return errors.New("file target requires 'file' field")
+		}
+		return nil
+	case "folder":
+		if strings.TrimSpace(t.Folder) == "" {
+			return errors.New("folder target requires 'folder' field")
+		}
+		if strings.TrimSpace(t.Pattern) == "" {
+			return errors.New("folder target requires 'pattern' field")
+		}
+		return nil
+	default:
+		if tt == "" {
+			return errors.New("target type is required: must be 'folder' or 'file'")
+		}
+		return fmt.Errorf("invalid target type '%s': must be 'folder' or 'file'", t.Type)
+	}
+}
+
+func ResolveTargetNotePath(vaultPath string, target Target, now time.Time) (relative string, absolute string, err error) {
+	if err := ValidateTarget(target); err != nil {
+		return "", "", err
+	}
+
+	switch strings.ToLower(strings.TrimSpace(target.Type)) {
+	case "file":
+		relative = strings.TrimSpace(target.File)
+	case "folder":
+		filename := ExpandDatePattern(strings.TrimSpace(target.Pattern), now)
+		if strings.TrimSpace(filename) == "" {
+			return "", "", errors.New("expanded filename is empty")
+		}
+		relative = filepath.ToSlash(filepath.Join(strings.TrimSpace(target.Folder), filename))
+	}
+
+	absolute, err = SafeJoinVaultPath(vaultPath, filepath.ToSlash(relative))
+	if err != nil {
+		return "", "", err
+	}
+	if !strings.HasSuffix(strings.ToLower(absolute), ".md") {
+		absolute += ".md"
+	}
+	return relative, absolute, nil
+}
+
+func PlanTargetAppend(vaultPath string, targetName string, target Target, now time.Time) (TargetPlan, error) {
+	rel, abs, err := ResolveTargetNotePath(vaultPath, target, now)
+	if err != nil {
+		return TargetPlan{}, err
+	}
+
+	plan := TargetPlan{
+		TargetName:       targetName,
+		TargetType:       strings.ToLower(strings.TrimSpace(target.Type)),
+		RelativeNotePath: rel,
+		AbsoluteNotePath: abs,
+	}
+
+	if _, err := os.Stat(filepath.Dir(abs)); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateDirs = true
+		} else {
+			return TargetPlan{}, err
+		}
+	}
+
+	if _, err := os.Stat(abs); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateFile = true
+		} else {
+			return TargetPlan{}, err
+		}
+	}
+
+	templateRel := strings.TrimSpace(target.Template)
+	if plan.WillCreateFile && templateRel != "" {
+		templateAbs, err := SafeJoinVaultPath(vaultPath, filepath.ToSlash(templateRel))
+		if err != nil {
+			return TargetPlan{}, fmt.Errorf("invalid template path: %w", err)
+		}
+		if !strings.HasSuffix(strings.ToLower(templateAbs), ".md") {
+			templateAbs += ".md"
+		}
+		plan.TemplatePath = templateRel
+		plan.AbsoluteTemplate = templateAbs
+		plan.WillApplyTemplate = true
+	}
+
+	return plan, nil
+}
+
+const targetsFileHeader = `# Obsidian CLI Targets
+#
+# This file defines capture targets for:
+#   obsidian-cli target <id> [text]
+#
+# Targets can be:
+#   - type: file   (append to a fixed note)
+#   - type: folder (append to a dated note determined by folder + pattern)
+#
+# Fields:
+#   type:     "file" or "folder"
+#   file:     (file target) path relative to vault root
+#   folder:   (folder target) folder path relative to vault root
+#   pattern:  (folder target) filename pattern, without ".md"
+#   template: (optional) note path to use as initial content when creating a new file
+#   vault:    (optional) override the default vault
+#
+# Pattern format:
+#   - Supports Obsidian-style tokens like YYYY, MM, DD, HH, mm, ss, ddd, dddd, MMM, MMMM
+#   - Supports [literal] blocks, e.g. YYYY-[log]-MM
+#   - Supports the "zettel" timestamp: YYYYMMDDHHmmss
+#
+# Examples:
+#   inbox:
+#     type: file
+#     file: Inbox.md
+#
+#   hourly-log:
+#     type: folder
+#     folder: Logs
+#     pattern: YYYY-MM-DD_HH
+#
+`
diff --git a/pkg/obsidian/targets_test.go b/pkg/obsidian/targets_test.go
new file mode 100644
index 0000000..1de8093
--- /dev/null
+++ b/pkg/obsidian/targets_test.go
@@ -0,0 +1,63 @@
+package obsidian_test
+
+import (
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+	"gopkg.in/yaml.v3"
+)
+
+func TestValidateTargetName(t *testing.T) {
+	assert.Error(t, obsidian.ValidateTargetName(""))
+	assert.Error(t, obsidian.ValidateTargetName(" "))
+	assert.Error(t, obsidian.ValidateTargetName("has space"))
+	assert.Error(t, obsidian.ValidateTargetName("add"))
+	assert.NoError(t, obsidian.ValidateTargetName("inbox"))
+}
+
+func TestTargetUnmarshalScalarTreatsAsFile(t *testing.T) {
+	var cfg obsidian.TargetsConfig
+	err := yaml.Unmarshal([]byte("inbox: Inbox.md\n"), &cfg)
+	assert.NoError(t, err)
+	tg := cfg["inbox"]
+	assert.Equal(t, "file", tg.Type)
+	assert.Equal(t, "Inbox.md", tg.File)
+}
+
+func TestResolveTargetNotePath(t *testing.T) {
+	vault := t.TempDir()
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	t.Run("file target", func(t *testing.T) {
+		rel, abs, err := obsidian.ResolveTargetNotePath(vault, obsidian.Target{Type: "file", File: "Inbox"}, now)
+		assert.NoError(t, err)
+		assert.Equal(t, "Inbox", rel)
+		assert.Contains(t, abs, "Inbox.md")
+	})
+
+	t.Run("folder target expands pattern", func(t *testing.T) {
+		rel, abs, err := obsidian.ResolveTargetNotePath(vault, obsidian.Target{Type: "folder", Folder: "Daily", Pattern: "YYYY-MM-DD_HH"}, now)
+		assert.NoError(t, err)
+		assert.Equal(t, "Daily/2024-01-15_14", rel)
+		assert.Contains(t, abs, "Daily")
+		assert.Contains(t, abs, "2024-01-15_14.md")
+	})
+
+	t.Run("reject escape", func(t *testing.T) {
+		_, _, err := obsidian.ResolveTargetNotePath(vault, obsidian.Target{Type: "file", File: "../escape.md"}, now)
+		assert.Error(t, err)
+	})
+}
+
+func TestPlanTargetAppend(t *testing.T) {
+	vault := t.TempDir()
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	plan, err := obsidian.PlanTargetAppend(vault, "inbox", obsidian.Target{Type: "file", File: "Inbox.md"}, now)
+	assert.NoError(t, err)
+	assert.Equal(t, "inbox", plan.TargetName)
+	assert.True(t, plan.WillCreateFile)
+	assert.False(t, plan.WillCreateDirs)
+}
diff --git a/pkg/obsidian/template.go b/pkg/obsidian/template.go
new file mode 100644
index 0000000..05b8e38
--- /dev/null
+++ b/pkg/obsidian/template.go
@@ -0,0 +1,61 @@
+package obsidian
+
+import (
+	"regexp"
+	"strings"
+	"time"
+)
+
+// ExpandTemplateVariables expands common Obsidian template variables in content.
+// Supported:
+//   - {{date}} / {{date:FORMAT}}
+//   - {{time}} / {{time:FORMAT}}
+//   - {{title}}
+//
+// FORMAT uses the same curated Obsidian-style tokens as ExpandDatePattern (including [literal] blocks).
+func ExpandTemplateVariables(content []byte, title string) []byte {
+	return ExpandTemplateVariablesAt(content, title, time.Now())
+}
+
+// ExpandTemplateVariablesAt is like ExpandTemplateVariables but uses the provided time.
+func ExpandTemplateVariablesAt(content []byte, title string, now time.Time) []byte {
+	result := string(content)
+	result = strings.ReplaceAll(result, "{{title}}", RemoveMdSuffix(title))
+
+	result = expandTemplateDate(result, now)
+	result = expandTemplateTime(result, now)
+
+	return []byte(result)
+}
+
+func expandTemplateDate(content string, now time.Time) string {
+	// {{date:FORMAT}}
+	re := regexp.MustCompile(`\{\{date:([^}]+)\}\}`)
+	content = re.ReplaceAllStringFunc(content, func(match string) string {
+		format := match[7 : len(match)-2]
+		s, err := FormatDatePattern(format, now)
+		if err != nil {
+			return now.Format("2006-01-02")
+		}
+		return s
+	})
+
+	// {{date}}
+	return strings.ReplaceAll(content, "{{date}}", now.Format("2006-01-02"))
+}
+
+func expandTemplateTime(content string, now time.Time) string {
+	// {{time:FORMAT}}
+	re := regexp.MustCompile(`\{\{time:([^}]+)\}\}`)
+	content = re.ReplaceAllStringFunc(content, func(match string) string {
+		format := match[7 : len(match)-2]
+		s, err := FormatDatePattern(format, now)
+		if err != nil {
+			return now.Format("15:04")
+		}
+		return s
+	})
+
+	// {{time}}
+	return strings.ReplaceAll(content, "{{time}}", now.Format("15:04"))
+}
diff --git a/pkg/obsidian/template_test.go b/pkg/obsidian/template_test.go
new file mode 100644
index 0000000..9e24b0d
--- /dev/null
+++ b/pkg/obsidian/template_test.go
@@ -0,0 +1,20 @@
+package obsidian_test
+
+import (
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestExpandTemplateVariablesAt(t *testing.T) {
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+	in := []byte("{{title}}\n{{date}}\n{{time}}\n{{date:YYYY-[ToDo]-MM}}\n{{time:HH:mm:ss}}\n")
+	out := string(obsidian.ExpandTemplateVariablesAt(in, "Note.md", now))
+	assert.Contains(t, out, "Note\n")
+	assert.Contains(t, out, "2024-01-15\n")
+	assert.Contains(t, out, "14:30\n")
+	assert.Contains(t, out, "2024-ToDo-01\n")
+	assert.Contains(t, out, "14:30:52\n")
+}
```

## Cumulative Diff (vs origin/main)

Compare: origin/main..pr-04d-init

### Commits
```
226e6c8 docs: update README for note picker and targets
39a9c56 feat(target): allow invoking targets by name
f083982 feat: add --ls/--select note picker
a251ea4 feat(append): select target via --select/--ls
b177d5d docs: add versioning note (pr-04d-init)
1f4a6eb docs: refresh README (pr-04d-init)
a6934da docs: note PR #59 compatibility
52f8fdb docs: README document frontmatter command
9eb89f6 feat: add frontmatter command for viewing and editing note metadata
6b4e28b docs: dedupe append section in README
1263511 docs: add acknowledgements for link-update work
783807c docs: add target/remove/test and append time-format examples
dbadbf1 docs: update README for init/append/targets and dry-run
7625bce chore(cli): improve usage/help text
4eb45d1 feat(cli): add dry-run to append/daily/delete
5be92e6 feat(target): improve edit prompt
b41c946 feat(wizard): add back/skip/help prompts
469f685 fix(init): add wizard UI helpers
4787252 feat(append): apply daily note template on create
0bf2948 chore(target): preserve targets.yaml header on save
2aaafd0 feat(cli): add interactive target editor
b5740c7 feat(cli): add target command
aff3c5b feat(target): add targets config and execution
368ef12 feat(obsidian): support obsidian-style date patterns
50c1a11 docs: README document delete confirmation and --force
abe6eb2 docs: acknowledge upstream PR #58 for link updates
382b61c docs: README clarify move link updates
900d9ba fix(delete): use 'del' alias
7bcfa66 docs: refresh README (pr-04b-append)
32b835f docs: README document append command
fea6040 feat(append): add daily note append command
2b849c1 docs: README mention preferences.json and per-vault settings
9071de0 docs: README document delete confirmation and --force
533ca73 feat(delete): warn when note has incoming links
dd03cfb docs: acknowledge upstream PR #58 for link updates
6b619bc docs: README clarify move link updates
21aaedd fix(delete): use 'del' alias
022b22d docs: refresh README (pr-04a-settings)
1f72c77 fix(config): enforce secure perms on preferences
38d6fdc docs: README mention preferences.json and per-vault settings
9a9556a feat(config): add per-vault settings to preferences.json
ceec85e docs: README document delete confirmation and --force
dedd70a feat(delete): warn when note has incoming links
508047d docs: acknowledge upstream PR #58 for link updates
a6b408a docs: README clarify move link updates
bb3ac19 fix(delete): use 'del' alias
eaa2818 docs: refresh README (pr-03-delete)
6a92bec docs: README document delete confirmation and --force
9603bb0 feat(delete): warn when note has incoming links
1335bcd docs: acknowledge upstream PR #58 for link updates
c0e5926 docs: README clarify move link updates
9bbb6fa docs: refresh README (pr-02-links)
3f5cc8a docs: acknowledge upstream PR #58 for link updates
6749716 docs: README clarify move link updates
3545693 fix: update path-based wikilinks and markdown links when moving notes
5e56d0f docs: README mention command-specific help
8bb520e docs: update generated help (pr-01-ux)
63ea5e1 feat(cli): add alias helper command
ee4eee9 docs: refresh README (pr-01-ux)
06d79e8 docs: README mention command-specific help
d9d3fa9 feat(cli): improve help and error handling
```

### Diff Stat (all files)
```
 README.md                                          |  841 +++++-
 cmd/alias.go                                       |  191 ++
 cmd/append.go                                      |  159 ++
 cmd/create.go                                      |   60 +-
 cmd/daily.go                                       |   21 +-
 cmd/delete.go                                      |   77 +-
 cmd/frontmatter.go                                 |   92 +
 cmd/init.go                                        |  734 ++++++
 cmd/move.go                                        |   94 +-
 cmd/note_picker.go                                 |  103 +
 cmd/open.go                                        |   51 +-
 cmd/print.go                                       |   51 +-
 cmd/print_default.go                               |   22 +-
 cmd/root.go                                        |   42 +
 cmd/search.go                                      |   58 +-
 cmd/search_content.go                              |   27 +-
 cmd/set_default.go                                 |   26 +-
 cmd/target.go                                      | 1326 ++++++++++
 cmd/wizard_prompt.go                               |   44 +
 cmd/wizard_ui.go                                   |   28 +
 go.mod                                             |    3 +
 go.sum                                             |    6 +
 mocks/note.go                                      |    9 +
 pkg/actions/append.go                              |  215 ++
 pkg/actions/append_test.go                         |  143 ++
 pkg/actions/daily.go                               |   15 +-
 pkg/actions/delete.go                              |   68 +-
 pkg/actions/delete_test.go                         |    7 +
 pkg/actions/frontmatter.go                         |  111 +
 pkg/actions/frontmatter_test.go                    |  237 ++
 pkg/actions/search_content_test.go                 |    9 +-
 pkg/actions/target.go                              |   94 +
 pkg/actions/target_test.go                         |   89 +
 pkg/config/targets_path.go                         |   15 +
 pkg/config/targets_path_test.go                    |   34 +
 pkg/frontmatter/frontmatter.go                     |  145 ++
 pkg/frontmatter/frontmatter_test.go                |  172 ++
 pkg/obsidian/date_format.go                        |  164 ++
 pkg/obsidian/date_format_test.go                   |   44 +
 pkg/obsidian/note.go                               |   50 +-
 pkg/obsidian/note_test.go                          |   78 +
 pkg/obsidian/path_safety.go                        |   37 +
 pkg/obsidian/path_safety_test.go                   |   37 +
 pkg/obsidian/targets.go                            |  298 +++
 pkg/obsidian/targets_test.go                       |   63 +
 pkg/obsidian/template.go                           |   61 +
 pkg/obsidian/template_test.go                      |   20 +
 pkg/obsidian/utils.go                              |  142 +
 pkg/obsidian/utils_test.go                         |   97 +
 pkg/obsidian/vault.go                              |   18 +-
 pkg/obsidian/vault_default_name.go                 |   99 +-
 pkg/obsidian/vault_default_name_test.go            |   14 +-
 pkg/obsidian/vault_settings_test.go                |   82 +
 vendor/github.com/BurntSushi/toml/.gitignore       |    5 +
 vendor/github.com/BurntSushi/toml/.travis.yml      |   15 +
 vendor/github.com/BurntSushi/toml/COMPATIBLE       |    3 +
 vendor/github.com/BurntSushi/toml/COPYING          |   21 +
 vendor/github.com/BurntSushi/toml/Makefile         |   19 +
 vendor/github.com/BurntSushi/toml/README.md        |  218 ++
 vendor/github.com/BurntSushi/toml/decode.go        |  509 ++++
 vendor/github.com/BurntSushi/toml/decode_meta.go   |  121 +
 vendor/github.com/BurntSushi/toml/doc.go           |   27 +
 vendor/github.com/BurntSushi/toml/encode.go        |  568 ++++
 .../github.com/BurntSushi/toml/encoding_types.go   |   19 +
 .../BurntSushi/toml/encoding_types_1.1.go          |   18 +
 vendor/github.com/BurntSushi/toml/lex.go           |  953 +++++++
 vendor/github.com/BurntSushi/toml/parse.go         |  592 +++++
 vendor/github.com/BurntSushi/toml/session.vim      |    1 +
 vendor/github.com/BurntSushi/toml/type_check.go    |   91 +
 vendor/github.com/BurntSushi/toml/type_fields.go   |  242 ++
 .../github.com/adrg/frontmatter/CODE_OF_CONDUCT.md |   77 +
 vendor/github.com/adrg/frontmatter/CONTRIBUTING.md |  135 +
 vendor/github.com/adrg/frontmatter/LICENSE         |   21 +
 vendor/github.com/adrg/frontmatter/README.md       |  170 ++
 vendor/github.com/adrg/frontmatter/format.go       |   70 +
 vendor/github.com/adrg/frontmatter/frontmatter.go  |   47 +
 vendor/github.com/adrg/frontmatter/parser.go       |  132 +
 vendor/gopkg.in/yaml.v2/.travis.yml                |   16 +
 vendor/gopkg.in/yaml.v2/LICENSE                    |  201 ++
 vendor/gopkg.in/yaml.v2/LICENSE.libyaml            |   31 +
 vendor/gopkg.in/yaml.v2/NOTICE                     |   13 +
 vendor/gopkg.in/yaml.v2/README.md                  |  133 +
 vendor/gopkg.in/yaml.v2/apic.go                    |  740 ++++++
 vendor/gopkg.in/yaml.v2/decode.go                  |  815 ++++++
 vendor/gopkg.in/yaml.v2/emitterc.go                | 1685 ++++++++++++
 vendor/gopkg.in/yaml.v2/encode.go                  |  390 +++
 vendor/gopkg.in/yaml.v2/parserc.go                 | 1095 ++++++++
 vendor/gopkg.in/yaml.v2/readerc.go                 |  412 +++
 vendor/gopkg.in/yaml.v2/resolve.go                 |  258 ++
 vendor/gopkg.in/yaml.v2/scannerc.go                | 2711 ++++++++++++++++++++
 vendor/gopkg.in/yaml.v2/sorter.go                  |  113 +
 vendor/gopkg.in/yaml.v2/writerc.go                 |   26 +
 vendor/gopkg.in/yaml.v2/yaml.go                    |  466 ++++
 vendor/gopkg.in/yaml.v2/yamlh.go                   |  739 ++++++
 vendor/gopkg.in/yaml.v2/yamlprivateh.go            |  173 ++
 vendor/modules.txt                                 |    9 +
 96 files changed, 20690 insertions(+), 133 deletions(-)
```

### Diff Stat (vendor only)
```
 vendor/github.com/BurntSushi/toml/.gitignore       |    5 +
 vendor/github.com/BurntSushi/toml/.travis.yml      |   15 +
 vendor/github.com/BurntSushi/toml/COMPATIBLE       |    3 +
 vendor/github.com/BurntSushi/toml/COPYING          |   21 +
 vendor/github.com/BurntSushi/toml/Makefile         |   19 +
 vendor/github.com/BurntSushi/toml/README.md        |  218 ++
 vendor/github.com/BurntSushi/toml/decode.go        |  509 ++++
 vendor/github.com/BurntSushi/toml/decode_meta.go   |  121 +
 vendor/github.com/BurntSushi/toml/doc.go           |   27 +
 vendor/github.com/BurntSushi/toml/encode.go        |  568 ++++
 .../github.com/BurntSushi/toml/encoding_types.go   |   19 +
 .../BurntSushi/toml/encoding_types_1.1.go          |   18 +
 vendor/github.com/BurntSushi/toml/lex.go           |  953 +++++++
 vendor/github.com/BurntSushi/toml/parse.go         |  592 +++++
 vendor/github.com/BurntSushi/toml/session.vim      |    1 +
 vendor/github.com/BurntSushi/toml/type_check.go    |   91 +
 vendor/github.com/BurntSushi/toml/type_fields.go   |  242 ++
 .../github.com/adrg/frontmatter/CODE_OF_CONDUCT.md |   77 +
 vendor/github.com/adrg/frontmatter/CONTRIBUTING.md |  135 +
 vendor/github.com/adrg/frontmatter/LICENSE         |   21 +
 vendor/github.com/adrg/frontmatter/README.md       |  170 ++
 vendor/github.com/adrg/frontmatter/format.go       |   70 +
 vendor/github.com/adrg/frontmatter/frontmatter.go  |   47 +
 vendor/github.com/adrg/frontmatter/parser.go       |  132 +
 vendor/gopkg.in/yaml.v2/.travis.yml                |   16 +
 vendor/gopkg.in/yaml.v2/LICENSE                    |  201 ++
 vendor/gopkg.in/yaml.v2/LICENSE.libyaml            |   31 +
 vendor/gopkg.in/yaml.v2/NOTICE                     |   13 +
 vendor/gopkg.in/yaml.v2/README.md                  |  133 +
 vendor/gopkg.in/yaml.v2/apic.go                    |  740 ++++++
 vendor/gopkg.in/yaml.v2/decode.go                  |  815 ++++++
 vendor/gopkg.in/yaml.v2/emitterc.go                | 1685 ++++++++++++
 vendor/gopkg.in/yaml.v2/encode.go                  |  390 +++
 vendor/gopkg.in/yaml.v2/parserc.go                 | 1095 ++++++++
 vendor/gopkg.in/yaml.v2/readerc.go                 |  412 +++
 vendor/gopkg.in/yaml.v2/resolve.go                 |  258 ++
 vendor/gopkg.in/yaml.v2/scannerc.go                | 2711 ++++++++++++++++++++
 vendor/gopkg.in/yaml.v2/sorter.go                  |  113 +
 vendor/gopkg.in/yaml.v2/writerc.go                 |   26 +
 vendor/gopkg.in/yaml.v2/yaml.go                    |  466 ++++
 vendor/gopkg.in/yaml.v2/yamlh.go                   |  739 ++++++
 vendor/gopkg.in/yaml.v2/yamlprivateh.go            |  173 ++
 vendor/modules.txt                                 |    9 +
 43 files changed, 14100 insertions(+)
```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index 591f921..6b0d9f2 100644
--- a/README.md
+++ b/README.md
@@ -2,12 +2,54 @@
 
 ---
 
-## ![obsidian-cli Usage](./docs/usage.png)
+## CLI Help (Generated)
+
+```text
+$ obsidian-cli --help
+obsidian-cli - CLI to open, search, move, create, delete and update notes
+
+Usage:
+  obsidian-cli [command]
+
+Available Commands:
+  alias          Generate a shell alias snippet or install a symlink shortcut
+  append         Append text to today's daily note
+  completion     Generate the autocompletion script for the specified shell
+  create         Creates note in vault
+  daily          Creates or opens daily note in vault
+  delete         Delete note in vault
+  help           Help about any command
+  init           Interactive setup wizard
+  move           Move or rename note in vault and update corresponding links
+  open           Opens note in vault by note name
+  print          Print contents of note
+  print-default  Prints default vault name and path
+  search         Fuzzy searches and opens note in vault
+  search-content Search note content for search term
+  set-default    Sets default vault
+  target         Append text to a configured target note
+
+Flags:
+  -h, --help      help for obsidian-cli
+  -v, --version   version for obsidian-cli
+
+Use "obsidian-cli [command] --help" for more information about a command.
+```
 
 ## Description
 
-Obsidian is a powerful and extensible knowledge base application
-that works on top of your local folder of plain text notes. This CLI tool (written in Go) will let you interact with the application using the terminal. You are currently able to open, search, move, create, update and delete notes.
+Obsidian is a powerful and extensible knowledge base application that works on top of your local folder of plain text notes.
+This CLI tool (written in Go) lets you interact with Obsidian from the terminal:
+
+- Open/search/create/move/delete notes
+- Open daily notes
+- Append to daily notes from the CLI
+- Capture into named “targets” configured in `targets.yaml`
+- Guided `init` wizard to set up defaults
+
+## Versioning (Maintainer Note)
+
+This PR keeps `obsidian-cli` at `v0.2.0` to avoid unnecessary merge/rebase churn across a stacked PR chain. If you merge the full chain (or the “just merge it” option), consider doing a single version bump afterward (for example to `v0.3.0`) in a follow-up PR/release.
 
 ---
 
@@ -48,6 +90,77 @@ For full installation instructions, see [Mac and Linux manual](https://yakitrak.
 obsidian-cli --help
 ```
 
+### Quickstart (Recommended)
+
+Run the interactive wizard:
+
+```bash
+obsidian-cli init
+```
+
+The wizard:
+
+- Selects and saves your default vault (`set-default`)
+- Configures per-vault daily note settings (used by `append`)
+- Offers to set up/migrate `targets.yaml`, and optionally add your first target
+
+Most wizard prompts accept:
+
+- `?` for help
+- `back` to go back / cancel
+- `skip` to accept defaults where applicable
+
+<details>
+<summary><code>init</code> command reference (help, flags, examples)</summary>
+
+```text
+$ obsidian-cli init --help
+Interactive setup wizard to configure your default vault, daily note settings, and targets.
+
+Usage:
+  obsidian-cli init [flags]
+
+Examples:
+  obsidian-cli init
+
+Flags:
+  -h, --help   help for init
+```
+
+</details>
+
+### Interactive Workflows
+
+Many commands support “guided” behavior when you omit arguments/flags:
+
+- `init` is a full interactive wizard (and supports a fuzzy finder when choosing a vault).
+- `append` and `target` read content from stdin (piped), or prompt for multi-line input until EOF (Ctrl-D to save, Ctrl-C to cancel).
+- `target add` runs a guided workflow when you omit the `[name]`.
+- `delete` prompts for confirmation if the note has incoming links (use `--force` to skip).
+- `open`, `create`, and `print` can prompt you to pick a note (or type a new one) when you omit the note path.
+- `delete`, `frontmatter`, and `move` support `--ls` / `--select` to pick an existing note interactively.
+
+The fuzzy finder (used by `search`, `search-content`, `open`, `create`, `print`, `frontmatter --ls`, `delete --ls`, `move --ls`, and `target --select`) lets you type to filter, then press Enter to choose a result. Press Esc, Ctrl-C, or Ctrl-D to abort the selection.
+
+In the note picker used by `search`, `open`, and `create`, there’s a “Create new note…” option which lets you choose an existing folder (or create a new folder), then type the note name.
+
+### Command Shortcut (Alias)
+
+If you want a shorter command name (for example `obsi`), you can either:
+
+- Create a shell alias (session-scoped unless you add it to your shell profile):
+
+  ```bash
+  # zsh/bash
+  eval "$(obsidian-cli alias obsi --shell zsh)"
+  ```
+
+- Or install a persistent symlink shortcut (recommended):
+
+  ```bash
+  obsidian-cli alias obsi --symlink --dir "$HOME/.local/bin"
+  ```
+
 ### Editor Flag
 
 The `search`, `search-content`, `create`, and `move` commands support the `--editor` (or `-e`) flag, which opens notes in your default text editor instead of the Obsidian application. This is useful for quick edits or when working in a terminal-only environment.
@@ -83,6 +196,57 @@ obsidian-cli set-default "{vault-name}"
 
 Note: `open` and other commands in `obsidian-cli` use this vault's base directory as the working directory, not the current working directory of your terminal.
 
+`set-default` stores the default vault name in `preferences.json` under your OS user config directory (`os.UserConfigDir()`), at:
+
+- `obsidian-cli/preferences.json`
+
+This preferences file also supports optional per-vault settings under `vault_settings` (for example, daily note configuration). For example:
+
+```json
+{
+  "default_vault_name": "My Vault",
+  "vault_settings": {
+    "My Vault": {
+      "daily_note": {
+        "folder": "Daily",
+        "filename_pattern": "{YYYY-MM-DD}",
+        "template_path": "Templates/Daily.md",
+        "create_if_missing": true
+      }
+    }
+  }
+}
+```
+
+<details>
+<summary><code>set-default</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli set-default --help
+Sets the default vault for all commands.
+
+The vault name must match exactly as it appears in Obsidian.
+Once set, you won't need to specify --vault for each command.
+
+Usage:
+  obsidian-cli set-default <vault> [flags]
+
+Aliases:
+  set-default, sd
+
+Examples:
+  # Set default vault
+  obsidian-cli set-default "My Vault"
+
+  # Verify it worked
+  obsidian-cli print-default
+
+Flags:
+  -h, --help   help for set-default
+```
+
+</details>
+
 ### Print Default Vault
 
 Prints default vault and path. Please set this with `set-default` command if not set.
@@ -95,6 +259,35 @@ obsidian-cli print-default
 obsidian-cli print-default --path-only
 ```
 
+<details>
+<summary><code>print-default</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli print-default --help
+Shows the currently configured default vault.
+
+Use --path-only to output just the path, useful for scripting.
+
+Usage:
+  obsidian-cli print-default [flags]
+
+Aliases:
+  print-default, pd
+
+Examples:
+  # Show default vault info
+  obsidian-cli print-default
+
+  # Get just the path (for scripts)
+  obsidian-cli print-default --path-only
+
+Flags:
+  -h, --help        help for print-default
+      --path-only   print only the vault path
+```
+
+</details>
+
 You can add this to your shell configuration file (like `~/.zshrc`) to quickly navigate to the default vault:
 
 ```bash
@@ -106,22 +299,75 @@ obs_cd() {
 
 Then you can use `obs_cd` to navigate to the default vault directory within your terminal.
 
+### Config Files
+
+`obsidian-cli` reads and writes configuration under your OS user config directory (`os.UserConfigDir()`):
+
+- `obsidian-cli/preferences.json` (default vault name + optional per-vault `vault_settings`)
+- `obsidian-cli/targets.yaml` (capture targets, used by `target`)
+
+It also reads Obsidian’s vault list from:
+
+- `obsidian/obsidian.json` (Obsidian config, used for vault discovery)
+
+Note: when writing `preferences.json`, the CLI attempts to create the config directory with mode `0750` and the file with mode `0600` (confirmed from `os.MkdirAll(…, 0750)` / `os.WriteFile(…, 0600)` in code).
+
 ### Open Note
 
-Open given note name in Obsidian. Note can also be an absolute path from top level of vault.
+Open a note in Obsidian by vault-relative note path (or pick one interactively with `--ls` / `--select`).
 
 ```bash
 # Opens note in obsidian vault
 obsidian-cli open "{note-name}"
 
+# Pick (or create) a note path interactively
+obsidian-cli open --ls
+
 # Opens note in specified obsidian vault
 obsidian-cli open "{note-name}" --vault "{vault-name}"
 
 ```
 
+<details>
+<summary><code>open</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli open --help
+Opens a note in Obsidian by name or path.
+
+The note name can be just the filename or a path relative to the vault root.
+The .md extension is optional.
+
+Usage:
+  obsidian-cli open [note-path] [flags]
+
+Aliases:
+  open, o
+
+Examples:
+  # Open a note by name
+  obsidian-cli open "Meeting Notes"
+
+  # Open a note in a subfolder
+  obsidian-cli open "Projects/my-project"
+
+  # Open in a specific vault
+  obsidian-cli open "Daily" --vault "Work"
+
+Flags:
+  -h, --help           help for open
+      --ls             select a note interactively
+      --select         select a note interactively
+  -v, --vault string   vault name (not required if default is set)
+```
+
+</details>
+
 ### Daily Note
 
-Open daily note in Obsidian. It will create one (using template) if one does not exist.
+Open the daily note in Obsidian (via Obsidian URI).
+
+Note: creation/templates are controlled by Obsidian’s daily note settings/plugins. Use `append` (below) if you want the CLI to write the daily note Markdown file directly.
 
 ```bash
 # Creates / opens daily note in obsidian vault
@@ -130,11 +376,334 @@ obsidian-cli daily
 # Creates / opens daily note in specified obsidian vault
 obsidian-cli daily --vault "{vault-name}"
 
+# Print the Obsidian URI (does not open Obsidian)
+obsidian-cli daily --dry-run
+
+```
+
+### Append to Daily Note
+
+Append text to today’s daily note **by writing the Markdown file directly** using your per-vault settings in `preferences.json` (`daily_note.folder`, `daily_note.filename_pattern`, and optional `daily_note.template_path`).
+
+If you provide no text, content is read from stdin (piped) or entered interactively until EOF (Ctrl-D to save, Ctrl-C to cancel).
+
+Tip: if you already use targets, `append --select` / `append --ls` is a convenience shortcut that selects a target from `targets.yaml` and appends to that target instead of the daily note.
+
+```bash
+# Append a one-liner
+obsidian-cli append "Meeting notes: discussed roadmap"
+
+# Multi-line content interactively (Ctrl-D to save, Ctrl-C to cancel)
+obsidian-cli append
+
+# Select a target, then append to it (shortcut for: obsidian-cli target --select)
+obsidian-cli append --select
+obsidian-cli append --ls "Buy milk"
+
+# Pipe content
+printf "line1\nline2\n" | obsidian-cli append
+
+# Append with timestamp
+obsidian-cli append --timestamp "Started work on feature X"
+
+# Append with timestamp + custom format (Go time format)
+obsidian-cli append --timestamp --time-format "15:04:05" "Did the thing"
+
+# Preview which file would be written (does not write)
+obsidian-cli append --dry-run "hello"
+
+# Append in a specific vault
+obsidian-cli append --vault "{vault-name}" "Daily standup notes"
+```
+
+<details>
+<summary><code>append</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli append --help
+Appends text to today's daily note.
+
+This command writes to a daily note path derived from your per-vault settings
+in preferences.json (daily_note.folder and daily_note.filename_pattern).
+
+If you prefer, you can use --select/--ls to pick a target from targets.yaml and append to that note instead.
+
+If no text argument is provided, content is read from stdin (piped) or entered
+interactively until EOF.
+
+Usage:
+  obsidian-cli append [text] [flags]
+
+Aliases:
+  append, a
+
+Examples:
+  # Append a one-liner
+  obsidian-cli append "Meeting notes: discussed roadmap"
+
+  # Append multi-line content interactively (Ctrl-D to save)
+  obsidian-cli append
+
+  # Pick a target interactively, then enter content
+  obsidian-cli append --select
+
+  # Append with timestamp
+  obsidian-cli append --timestamp "Started work on feature X"
+
+  # Append in a specific vault
+  obsidian-cli append --vault "Work" "Daily standup notes"
+
+Flags:
+      --dry-run              preview which note would be written without writing
+  -h, --help                 help for append
+      --ls                   select a target interactively (targets.yaml)
+      --select               select a target interactively (targets.yaml)
+      --time-format string   custom timestamp format (Go time format, default: 15:04)
+  -t, --timestamp            prepend a timestamp to the content
+  -v, --vault string         vault name (not required if default is set)
+```
+
+</details>
+
+### Target (Quick Capture)
+
+Targets let you define named shortcuts for capturing into specific notes.
+
+Targets are configured in `targets.yaml` (stored next to `preferences.json`), and can point at:
+
+- A fixed file path (always append to the same note)
+- A folder + filename pattern (append to a dated note based on the current time)
+
+Common workflows:
+
+```bash
+# Guided target creation workflow
+obsidian-cli target add
+
+# Short alias for `target`
+obsidian-cli t inbox "Buy milk"
+
+# You can also invoke a target name directly (the CLI routes it to `target <name> ...`)
+obsidian-cli inbox "Buy milk"
+
+# Capture a one-liner to a target
+obsidian-cli target inbox "Buy milk"
+
+# Multi-line content (Ctrl-D to save, Ctrl-C to cancel)
+obsidian-cli target inbox
+
+# Pick a target interactively, then enter content
+obsidian-cli target --select
+
+# Alias for --select
+obsidian-cli target --ls
+
+# Preview which file would be used (does not write)
+obsidian-cli target inbox --dry-run
+
+# Preview resolved paths for one or all targets
+obsidian-cli target test inbox
+obsidian-cli target test
+
+# List targets
+obsidian-cli target list
+
+# Remove a target
+obsidian-cli target remove inbox
+obsidian-cli target rm inbox
+
+# Edit targets (choose CLI mode or open targets.yaml in your editor)
+obsidian-cli target edit
+
+# Run a target using a specific vault (unless the target has its own vault override)
+obsidian-cli target --vault "{vault-name}" inbox "hello"
+```
+
+Minimal `targets.yaml` examples:
+
+```yaml
+# Fixed-file target
+inbox:
+  type: file
+  file: Inbox.md
+
+# Folder + pattern target
+log:
+  type: folder
+  folder: Log
+  pattern: YYYY-MM-DD
+
+# Folder target with a template and per-target vault override
+worklog:
+  type: folder
+  folder: Log
+  pattern: YYYY-MM-DD
+  template: Templates/Daily
+  vault: Work
+```
+
+Notes:
+
+- A simplified scalar form is also accepted and can be migrated by `init` / `target edit`:
+  - `inbox: Inbox.md`
+- The legacy `note` key is treated as `file` (file target).
+- Target names cannot contain whitespace, and some names are reserved:
+  - `add`, `remove`, `rm`, `list`, `ls`, `edit`, `validate`, `test`, `help`
+
+<details>
+<summary><code>target</code> command reference (help, flags, subcommands)</summary>
+
+```text
+$ obsidian-cli target --help
+Appends text to a note configured in targets.yaml.
+
+Targets can point at:
+  - a fixed file path (always append to the same note)
+  - a folder + filename pattern (append to a dated note based on the current time)
+
+If no text is provided, content is read from stdin (piped) or entered interactively until EOF.
+
+Usage:
+  obsidian-cli target [id] [text] [flags]
+  obsidian-cli target [command]
+
+Aliases:
+  target, t
+
+Examples:
+  # Append a one-liner to a target
+  obsidian-cli target inbox "Buy milk"
+
+  # Multi-line content (Ctrl-D to save, Ctrl-C to cancel)
+  obsidian-cli target inbox
+
+  # Pick a target interactively, then enter content
+  obsidian-cli target --select
+
+  # Preview which file would be used
+  obsidian-cli target inbox --dry-run
+
+Available Commands:
+  add         Add a new target
+  edit        Edit targets in CLI or open targets.yaml
+  list        List configured targets
+  remove      Remove a target
+  test        Preview the resolved path for a target
+
+Flags:
+      --dry-run        preview the resolved target path without writing
+  -h, --help           help for target
+      --ls             select a target interactively
+      --select         select a target interactively
+  -v, --vault string   vault name (not required if default is set)
+
+Use "obsidian-cli target [command] --help" for more information about a command.
+```
+
+</details>
+
+<details>
+<summary><code>target</code> subcommand references (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli target add --help
+Add a new capture target.
+
+Run without a name to start a guided workflow.
+
+Usage:
+  obsidian-cli target add [name] [flags]
+
+Examples:
+  # Guided workflow
+  obsidian-cli target add
+
+  # Add a fixed-file target
+  obsidian-cli target add inbox
+
+  # Add a folder+pattern target
+  obsidian-cli target add log
+
+Flags:
+  -h, --help   help for add
+```
+
+```text
+$ obsidian-cli target remove --help
+Remove a target
+
+Usage:
+  obsidian-cli target remove [name] [flags]
+
+Aliases:
+  remove, rm
+
+Flags:
+  -h, --help   help for remove
+```
+
+```text
+$ obsidian-cli target list --help
+List configured targets
+
+Usage:
+  obsidian-cli target list [flags]
+
+Flags:
+  -h, --help   help for list
+```
+
+```text
+$ obsidian-cli target edit --help
+Edit targets in CLI or open targets.yaml
+
+Usage:
+  obsidian-cli target edit [flags]
+
+Flags:
+  -h, --help   help for edit
+```
+
+```text
+$ obsidian-cli target test --help
+Shows which file would be created or appended to for the given target.
+
+If no name is provided, previews all targets.
+
+Usage:
+  obsidian-cli target test [name] [flags]
+
+Flags:
+  -h, --help   help for test
+```
+
+</details>
+
+### Date Patterns and Template Variables
+
+Date patterns (used by daily note filename patterns and folder targets) support Obsidian-style tokens, legacy `{brace}` patterns, and `[literal]` blocks:
+
+- Tokens (curated subset): `YYYY`, `YY`, `MMMM`, `MMM`, `MM`, `M`, `DD`, `D`, `dddd`, `ddd`, `HH`, `H`, `hh`, `h`, `mm`, `m`, `ss`, `s`, `A`, `a`, `ZZ`, `Z`, `z`
+- Legacy brace patterns: `{YYYY-MM-DD}` and `{YYYY-MM-DD-HHmmss}` (braces are ignored)
+- Zettel timestamp: `YYYYMMDDHHmmss` (with or without braces)
+- Literal blocks: wrap text in `[brackets]`, e.g. `YYYY-[log]-MM`
+
+Templates (used when `append` or `target` creates a note that doesn’t exist yet) support:
+
+- `{{title}}`
+- `{{date}}` / `{{date:FORMAT}}`
+- `{{time}}` / `{{time:FORMAT}}`
+
+Example template snippet:
+
+```text
+Title={{title}}
+Created={{date:YYYY-MM-DD}} {{time:HH:mm}}
 ```
 
 ### Search Note
 
-Starts a fuzzy search displaying notes in the terminal from the vault. You can hit enter on a note to open that in Obsidian.
+Starts a fuzzy search displaying notes in the terminal from the vault. Press Enter on a note to open it in Obsidian (or choose “Create new note…” to pick/create a folder and type a new note name).
 
 ```bash
 # Searches in default obsidian vault
@@ -166,12 +735,15 @@ obsidian-cli search-content "search term" --editor
 
 ### Print Note
 
-Prints the contents of given note name or path in Obsidian.
+Prints the contents of a note to stdout (useful for piping to other commands).
 
 ```bash
 # Prints note in default vault
 obsidian-cli print "{note-name}"
 
+# Pick a note interactively
+obsidian-cli print --ls
+
 # Prints note by path in default vault
 obsidian-cli print "{note-path}"
 
@@ -180,15 +752,58 @@ obsidian-cli print "{note-name}" --vault "{vault-name}"
 
 ```
 
+<details>
+<summary><code>print</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli print --help
+Prints the contents of a note to stdout.
+
+Useful for piping note contents to other commands, or quickly viewing
+a note without opening Obsidian.
+
+Usage:
+  obsidian-cli print [note-path] [flags]
+
+Aliases:
+  print, p
+
+Examples:
+  # Print a note
+  obsidian-cli print "Meeting Notes"
+
+  # Print note in subfolder
+  obsidian-cli print "Projects/readme"
+
+  # Pipe to grep
+  obsidian-cli print "Todo" | grep "TODO"
+
+  # Copy to clipboard (macOS)
+  obsidian-cli print "Template" | pbcopy
+
+Flags:
+  -h, --help           help for print
+      --ls             select a note interactively
+      --select         select a note interactively
+  -v, --vault string   vault name
+```
+
+</details>
+
 ### Create / Update Note
 
-Creates note (can also be a path with name) in vault. By default, if the note exists, it will create another note but passing `--overwrite` or `--append` can be used to edit the named note.
+Creates a note (can be a path from the top level of the vault). By default, if the note exists, it will create another note; passing `--overwrite` or `--append` changes that behavior.
+
+Note: `--editor` only applies when `--open` is also provided.
 
 ```bash
-# Creates empty note in default obsidian and opens it
+# Creates empty note in default obsidian (does not open unless --open is used)
 obsidian-cli create "{note-name}"
 
-# Creates empty note in given obsidian and opens it
+# Pick (or create) a note path interactively
+obsidian-cli create --ls
+
+# Creates empty note in given obsidian
 obsidian-cli create "{note-name}"  --vault "{vault-name}"
 
 # Creates note in default obsidian with content
@@ -208,40 +823,236 @@ obsidian-cli create "{note-name}" --content "abcde" --open --editor
 
 ```
 
+<details>
+<summary><code>create</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli create --help
+Creates a new note in your Obsidian vault.
+
+By default, if the note already exists, Obsidian will create a new note
+with a numeric suffix. Use --append to add to an existing note, or
+--overwrite to replace its contents.
+
+Usage:
+  obsidian-cli create [note-path] [flags]
+
+Aliases:
+  create, c
+
+Examples:
+  # Create an empty note
+  obsidian-cli create "New Note"
+
+  # Create with content
+  obsidian-cli create "Ideas" --content "My brilliant idea"
+
+  # Append to existing note
+  obsidian-cli create "Log" --content "Entry" --append
+
+  # Create and open in Obsidian
+  obsidian-cli create "Draft" --open
+
+  # Create and open in $EDITOR
+  obsidian-cli create "Draft" --open --editor
+
+Flags:
+  -a, --append           append to note
+  -c, --content string   text to add to note
+  -e, --editor           open in editor instead of Obsidian (requires --open flag)
+  -h, --help             help for create
+      --ls               select a note interactively
+      --open             open created note
+  -o, --overwrite        overwrite note
+      --select           select a note interactively
+  -v, --vault string     vault name
+```
+
+</details>
+
 ### Move / Rename Note
 
-Moves a given note(path from top level of vault) with new name given (top level of vault). If given same path but different name then its treated as a rename. All links inside vault are updated to match new name.
+Moves a note to a new path (or renames it) and updates links inside the vault to match.
+
+Note: `--editor` only applies when `--open` is also provided.
 
 ```bash
 # Renames a note in default obsidian
 obsidian-cli move "{current-note-path}" "{new-note-path}"
 
+# Pick the note to move interactively
+obsidian-cli move --ls "{new-note-path}"
+
 # Renames a note and given obsidian
 obsidian-cli move "{current-note-path}" "{new-note-path}" --vault "{vault-name}"
 
 # Renames a note in default obsidian and opens it
 obsidian-cli move "{current-note-path}" "{new-note-path}" --open
 
-# Renames a note and opens it in your default editor
-obsidian-cli move "{current-note-path}" "{new-note-path}" --open --editor
+	# Renames a note and opens it in your default editor
+	obsidian-cli move "{current-note-path}" "{new-note-path}" --open --editor
+```
+
+<details>
+<summary><code>move</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli move --help
+Moves or renames a note and updates all links pointing to it.
+
+This command safely renames notes by also updating any [[wikilinks]]
+or [markdown](links) that reference the moved note.
+
+Usage:
+  obsidian-cli move [from-note-path] [to-note-path] [flags]
+
+Aliases:
+  move, m
+
+Examples:
+  # Rename a note
+  obsidian-cli move "Old Name" "New Name"
+
+  # Move to a different folder
+  obsidian-cli move "Inbox/note" "Projects/note"
+
+  # Move and open the result
+  obsidian-cli move "temp" "Archive/temp" --open
+
+Flags:
+  -e, --editor         open in editor instead of Obsidian (requires --open flag)
+  -h, --help           help for move
+      --ls             select the note to move interactively
+  -o, --open           open new note
+      --select         select the note to move interactively
+  -v, --vault string   vault name
 ```
 
+</details>
+
+### Frontmatter
+
+View or edit a note’s YAML frontmatter.
+
+```bash
+# Print frontmatter
+obsidian-cli frontmatter "My Note" --print
+
+# Edit a key
+obsidian-cli frontmatter "My Note" --edit --key "status" --value "done"
+
+# Delete a key
+obsidian-cli frontmatter "My Note" --delete --key "draft"
+
+# Pick a note interactively
+obsidian-cli frontmatter --ls
+```
+
+<details>
+<summary><code>frontmatter</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli frontmatter --help
+View or modify YAML frontmatter in a note.
+
+Use --print to display frontmatter, --edit to modify a key,
+or --delete to remove a key.
+
+Examples:
+  obsidian-cli frontmatter "My Note" --print
+  obsidian-cli frontmatter "My Note" --edit --key "status" --value "done"
+  obsidian-cli frontmatter "My Note" --delete --key "draft"
+
+Usage:
+  obsidian-cli frontmatter [note] [flags]
+
+Aliases:
+  frontmatter, fm
+
+Flags:
+  -d, --delete         delete a frontmatter key
+  -e, --edit           edit a frontmatter key
+  -h, --help           help for frontmatter
+  -k, --key string     key to edit or delete
+      --ls             select a note interactively
+  -p, --print          print frontmatter
+      --select         select a note interactively
+      --value string   value to set (required for --edit)
+  -v, --vault string   vault name
+```
+
+</details>
+
 ### Delete Note
 
 Deletes a given note (path from top level of vault).
 
+If other notes link to the note, `delete` prints the incoming links and prompts for confirmation. The default is **No** (press Enter to cancel).
+
+Use `--force` (`-f`) to skip confirmation (recommended for scripts). Alias: `delete, del`. Heads up: `daily` uses alias `d`, so `delete` uses `del` to avoid ambiguity.
+
 ```bash
-# Renames a note in default obsidian
+# Delete a note in default obsidian vault
 obsidian-cli delete "{note-path}"
 
-# Renames a note in given obsidian
+# Pick a note interactively (existing notes only)
+obsidian-cli delete --ls
+
+# Delete a note in given obsidian vault
 obsidian-cli delete "{note-path}" --vault "{vault-name}"
+
+# Force delete without prompt
+obsidian-cli delete "{note-path}" --force
+
+# Preview which file would be deleted (does not delete)
+obsidian-cli delete --dry-run "{note-path}"
 ```
 
+<details>
+<summary><code>delete</code> command reference (help, flags, aliases)</summary>
+
+```text
+$ obsidian-cli delete --help
+Delete a note from the vault.
+
+If other notes link to the note, you'll be prompted to confirm.
+Use --force to skip confirmation (recommended for scripts).
+
+Usage:
+  obsidian-cli delete [note-path] [flags]
+
+Aliases:
+  delete, del
+
+Examples:
+  # Delete a note (prompts if linked)
+  obsidian-cli delete "old-note"
+
+  # Force delete without prompt
+  obsidian-cli delete "temp" --force
+
+  # Delete from specific vault
+  obsidian-cli delete "note" --vault "Archive"
+
+Flags:
+      --dry-run        preview which file would be deleted without deleting it
+  -f, --force          skip confirmation if the note has incoming links
+  -h, --help           help for delete
+      --ls             select a note interactively
+      --select         select a note interactively
+  -v, --vault string   vault name
+```
+
+</details>
+
 ## Contribution
 
 Fork the project, add your feature or fix and submit a pull request. You can also open an [issue](https://github.com/yakitrak/obsidian-cli/issues/new/choose) to report a bug or request a feature.
 
+## Acknowledgements
+
+The note move/rename link-update improvements proposed in this repo build on the approach from Logan McDuffie’s fix for issue #44 (`tidalstudio/fix/issue-44-link-updates`).
+
 ## License
 
 Available under [MIT License](./LICENSE)
diff --git a/cmd/alias.go b/cmd/alias.go
new file mode 100644
index 0000000..5b26cd9
--- /dev/null
+++ b/cmd/alias.go
@@ -0,0 +1,191 @@
+package cmd
+
+import (
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"runtime"
+	"strings"
+
+	"github.com/spf13/cobra"
+)
+
+type aliasShell string
+
+const (
+	aliasShellBash       aliasShell = "bash"
+	aliasShellZsh        aliasShell = "zsh"
+	aliasShellFish       aliasShell = "fish"
+	aliasShellPowerShell aliasShell = "powershell"
+	aliasShellCmd        aliasShell = "cmd"
+)
+
+var aliasCmdName string
+var aliasCmdShell string
+var aliasCmdPrint bool
+var aliasCmdSymlink bool
+var aliasCmdSymlinkDir string
+var aliasCmdForce bool
+var aliasCmdDryRun bool
+
+var aliasCmd = &cobra.Command{
+	Use:   "alias [name]",
+	Short: "Generate a shell alias snippet or install a symlink shortcut",
+	Long: `Aliases are typically a shell feature. This command helps by generating an alias snippet you can add to your shell profile,
+or by creating a symlink (e.g. ~/.local/bin/obsi -> obsidian-cli) so you can run the CLI with a shorter name.`,
+	Args: cobra.MaximumNArgs(1),
+	Example: `  # Print an alias snippet (paste into your shell profile, or eval it)
+  obsidian-cli alias obsi --shell zsh
+  eval "$(obsidian-cli alias obsi --shell zsh)"
+
+  # Install a symlink shortcut (recommended for a persistent shortcut)
+  obsidian-cli alias obsi --symlink --dir "$HOME/.local/bin"`,
+	RunE: func(cmd *cobra.Command, args []string) error {
+		if len(args) == 1 && aliasCmdName == "" {
+			aliasCmdName = args[0]
+		}
+		if aliasCmdName == "" {
+			return errors.New("alias name is required (provide [name] or --name)")
+		}
+
+		if !isValidAliasName(aliasCmdName) {
+			return fmt.Errorf("invalid alias name %q (use letters/numbers/underscore/dash; must start with a letter)", aliasCmdName)
+		}
+
+		shell := normalizeShell(aliasCmdShell, os.Getenv("SHELL"))
+
+		if aliasCmdSymlink {
+			if aliasCmdSymlinkDir == "" {
+				return errors.New("--dir is required when using --symlink")
+			}
+			if err := installSymlinkAlias(aliasCmdName, aliasCmdSymlinkDir, aliasCmdForce, aliasCmdDryRun); err != nil {
+				return err
+			}
+			if !aliasCmdPrint {
+				return nil
+			}
+		}
+
+		if aliasCmdPrint {
+			fmt.Print(renderAliasSnippet(aliasCmdName, shell))
+		}
+		return nil
+	},
+}
+
+func init() {
+	aliasCmd.Flags().StringVar(&aliasCmdName, "name", "", "alias name (e.g. obsi)")
+	aliasCmd.Flags().StringVar(&aliasCmdShell, "shell", "", "shell for snippet output: bash, zsh, fish, powershell, cmd (default: detect from $SHELL)")
+	aliasCmd.Flags().BoolVar(&aliasCmdPrint, "print", true, "print the alias snippet to stdout")
+
+	aliasCmd.Flags().BoolVar(&aliasCmdSymlink, "symlink", false, "install a symlink shortcut in --dir pointing to this executable")
+	aliasCmd.Flags().StringVar(&aliasCmdSymlinkDir, "dir", filepath.Join(os.Getenv("HOME"), ".local", "bin"), "directory to place the symlink (used with --symlink)")
+	aliasCmd.Flags().BoolVar(&aliasCmdForce, "force", false, "overwrite an existing file at the symlink path")
+	aliasCmd.Flags().BoolVar(&aliasCmdDryRun, "dry-run", false, "show what would be done without writing anything")
+
+	rootCmd.AddCommand(aliasCmd)
+}
+
+func isValidAliasName(name string) bool {
+	if name == "" {
+		return false
+	}
+	first := name[0]
+	if !((first >= 'A' && first <= 'Z') || (first >= 'a' && first <= 'z')) {
+		return false
+	}
+	for i := 0; i < len(name); i++ {
+		c := name[i]
+		isLetter := (c >= 'A' && c <= 'Z') || (c >= 'a' && c <= 'z')
+		isDigit := c >= '0' && c <= '9'
+		isOK := isLetter || isDigit || c == '_' || c == '-'
+		if !isOK {
+			return false
+		}
+	}
+	return true
+}
+
+func normalizeShell(flag string, envShell string) aliasShell {
+	if flag != "" {
+		return aliasShell(strings.ToLower(strings.TrimSpace(flag)))
+	}
+	base := strings.ToLower(filepath.Base(envShell))
+	switch base {
+	case "bash":
+		return aliasShellBash
+	case "zsh":
+		return aliasShellZsh
+	case "fish":
+		return aliasShellFish
+	case "pwsh", "powershell":
+		return aliasShellPowerShell
+	case "cmd", "cmd.exe":
+		return aliasShellCmd
+	default:
+		return aliasShellZsh
+	}
+}
+
+func renderAliasSnippet(name string, shell aliasShell) string {
+	switch shell {
+	case aliasShellFish:
+		return fmt.Sprintf("alias %s 'obsidian-cli'\n", name)
+	case aliasShellPowerShell:
+		return fmt.Sprintf("Set-Alias -Name %s -Value obsidian-cli\n", name)
+	case aliasShellCmd:
+		return fmt.Sprintf("doskey %s=obsidian-cli $*\n", name)
+	case aliasShellBash, aliasShellZsh:
+		fallthrough
+	default:
+		return fmt.Sprintf("alias %s=\"obsidian-cli\"\n", name)
+	}
+}
+
+func installSymlinkAlias(name string, dir string, force bool, dryRun bool) error {
+	exe, err := os.Executable()
+	if err != nil {
+		return err
+	}
+	exe, err = filepath.EvalSymlinks(exe)
+	if err != nil {
+		return err
+	}
+
+	if err := os.MkdirAll(dir, 0o755); err != nil {
+		return err
+	}
+
+	linkName := name
+	if runtime.GOOS == "windows" && !strings.HasSuffix(strings.ToLower(linkName), ".exe") {
+		linkName += ".exe"
+	}
+	dst := filepath.Join(dir, linkName)
+
+	if _, err := os.Lstat(dst); err == nil {
+		if !force {
+			return fmt.Errorf("refusing to overwrite existing path: %s (use --force)", dst)
+		}
+		if dryRun {
+			fmt.Printf("Would remove existing path: %s\n", dst)
+		} else if err := os.Remove(dst); err != nil {
+			return err
+		}
+	} else if err != nil && !errors.Is(err, os.ErrNotExist) {
+		return err
+	}
+
+	if dryRun {
+		fmt.Printf("Would create symlink: %s -> %s\n", dst, exe)
+		return nil
+	}
+	if err := os.Symlink(exe, dst); err != nil {
+		if runtime.GOOS == "windows" {
+			return fmt.Errorf("failed to create symlink %s -> %s: %w (Windows may require admin/developer mode)", dst, exe, err)
+		}
+		return err
+	}
+	return nil
+}
+
diff --git a/cmd/append.go b/cmd/append.go
new file mode 100644
index 0000000..17b3c03
--- /dev/null
+++ b/cmd/append.go
@@ -0,0 +1,159 @@
+package cmd
+
+import (
+	"errors"
+	"fmt"
+	"io"
+	"os"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/spf13/cobra"
+)
+
+var (
+	appendTimestamp bool
+	appendTimeFmt   string
+	appendDryRun    bool
+	appendSelect    bool
+)
+
+var appendCmd = &cobra.Command{
+	Use:     "append [text]",
+	Aliases: []string{"a"},
+	Short:   "Append text to today's daily note (or select a target)",
+	Long: `Appends text to today's daily note.
+
+This command writes to a daily note path derived from your per-vault settings
+in preferences.json (daily_note.folder and daily_note.filename_pattern).
+
+If you prefer, you can use --select/--ls to pick a target from targets.yaml and append to that note instead.
+
+If no text argument is provided, content is read from stdin (piped) or entered
+interactively until EOF.`,
+	Example: `  # Append a one-liner
+  obsidian-cli append "Meeting notes: discussed roadmap"
+
+  # Append multi-line content interactively (Ctrl-D to save)
+  obsidian-cli append
+
+  # Pick a target interactively, then enter content
+  obsidian-cli append --select
+
+  # Append with timestamp
+  obsidian-cli append --timestamp "Started work on feature X"
+
+  # Append in a specific vault
+  obsidian-cli append --vault "Work" "Daily standup notes"`,
+	Args: cobra.ArbitraryArgs,
+	RunE: func(cmd *cobra.Command, args []string) error {
+		vault := obsidian.Vault{Name: vaultName}
+
+		if appendSelect {
+			targetName, err := pickTargetName()
+			if err != nil {
+				return err
+			}
+			if strings.TrimSpace(targetName) == "" {
+				return errors.New("no target selected")
+			}
+
+			content := strings.TrimSpace(strings.Join(args, " "))
+			if content == "" {
+				stat, err := os.Stdin.Stat()
+				if err == nil && (stat.Mode()&os.ModeCharDevice) == 0 {
+					b, err := io.ReadAll(os.Stdin)
+					if err != nil {
+						return err
+					}
+					content = strings.TrimSpace(string(b))
+				} else if !appendDryRun {
+					content, err = actions.PromptForContentIfEmpty(content)
+					if err != nil {
+						return err
+					}
+				}
+			}
+
+			if appendTimestamp {
+				format := appendTimeFmt
+				if strings.TrimSpace(format) == "" {
+					format = "15:04"
+				}
+				content = fmt.Sprintf("- %s %s", time.Now().Format(format), content)
+			}
+
+			if appendDryRun {
+				return printTargetPlan(&vault, targetName)
+			}
+
+			plan, err := actions.AppendToTarget(&vault, targetName, content, time.Now(), false)
+			if err != nil {
+				return err
+			}
+			fmt.Printf("Wrote to: %s\n", plan.AbsoluteNotePath)
+			return nil
+		}
+
+		content := strings.TrimSpace(strings.Join(args, " "))
+		if content == "" {
+			stat, err := os.Stdin.Stat()
+			if err == nil && (stat.Mode()&os.ModeCharDevice) == 0 {
+				b, err := io.ReadAll(os.Stdin)
+				if err != nil {
+					return err
+				}
+				content = strings.TrimSpace(string(b))
+			} else if !appendDryRun {
+				var err error
+				content, err = actions.PromptForContentIfEmpty(content)
+				if err != nil {
+					return err
+				}
+			}
+		}
+
+		if appendTimestamp {
+			format := appendTimeFmt
+			if strings.TrimSpace(format) == "" {
+				format = "15:04"
+			}
+			content = fmt.Sprintf("- %s %s", time.Now().Format(format), content)
+		}
+
+		if appendDryRun {
+			plan, err := actions.PlanDailyAppend(&vault, time.Now())
+			if err != nil {
+				return err
+			}
+			fmt.Println("Append dry-run:")
+			fmt.Printf("  vault: %s\n", plan.VaultName)
+			fmt.Printf("  note: %s\n", plan.AbsoluteNotePath)
+			fmt.Printf("  create_dirs: %t\n", plan.WillCreateDirs)
+			fmt.Printf("  create_file: %t\n", plan.WillCreateFile)
+			if plan.WillApplyTemplate {
+				fmt.Printf("  template: %s\n", plan.TemplateAbs)
+			}
+			if strings.TrimSpace(content) == "" {
+				fmt.Println("  content: (none supplied; would prompt interactively without --dry-run)")
+			} else {
+				fmt.Printf("  append_bytes: %d\n", len([]byte(content)))
+			}
+			return nil
+		}
+
+		return actions.AppendToDailyNote(&vault, content)
+	},
+}
+
+func init() {
+	appendCmd.Flags().BoolVar(&appendSelect, "ls", false, "select a target interactively (targets.yaml)")
+	appendCmd.Flags().BoolVar(&appendSelect, "select", false, "select a target interactively (targets.yaml)")
+	appendCmd.Flags().BoolVarP(&appendTimestamp, "timestamp", "t", false, "prepend a timestamp to the content")
+	appendCmd.Flags().StringVar(&appendTimeFmt, "time-format", "", "custom timestamp format (Go time format, default: 15:04)")
+	appendCmd.Flags().BoolVar(&appendDryRun, "dry-run", false, "preview which note would be written without writing")
+	appendCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
+	rootCmd.AddCommand(appendCmd)
+}
diff --git a/cmd/create.go b/cmd/create.go
index 338dcc9..8cad0c3 100644
--- a/cmd/create.go
+++ b/cmd/create.go
@@ -1,27 +1,68 @@
 package cmd
 
 import (
+	"errors"
+	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
-	"log"
 )
 
 var shouldAppend bool
 var shouldOverwrite bool
 var content string
+var createSelect bool
 var createNoteCmd = &cobra.Command{
-	Use:     "create",
+	Use:     "create [note-path]",
 	Aliases: []string{"c"},
 	Short:   "Creates note in vault",
-	Args:    cobra.ExactArgs(1),
-	Run: func(cmd *cobra.Command, args []string) {
+	Long: `Creates a new note in your Obsidian vault.
+
+By default, if the note already exists, Obsidian will create a new note
+with a numeric suffix. Use --append to add to an existing note, or
+--overwrite to replace its contents.`,
+	Example: `  # Create an empty note
+  obsidian-cli create "New Note"
+
+  # Create with content
+  obsidian-cli create "Ideas" --content "My brilliant idea"
+
+  # Append to existing note
+  obsidian-cli create "Log" --content "Entry" --append
+
+  # Create and open in Obsidian
+  obsidian-cli create "Draft" --open
+
+  # Create and open in $EDITOR
+  obsidian-cli create "Draft" --open --editor`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
-		noteName := args[0]
+		noteName := ""
+		if len(args) > 0 && !createSelect {
+			noteName = args[0]
+		} else {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickNotePathOrNew(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+		if noteName == "" {
+			return errors.New("no note selected")
+		}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
-			log.Fatalf("Failed to parse --editor flag: %v", err)
+			return fmt.Errorf("failed to parse --editor flag: %w", err)
 		}
 		params := actions.CreateParams{
 			NoteName:        noteName,
@@ -31,15 +72,14 @@ var createNoteCmd = &cobra.Command{
 			ShouldOpen:      shouldOpen,
 			UseEditor:       useEditor,
 		}
-		err = actions.CreateNote(&vault, &uri, params)
-		if err != nil {
-			log.Fatal(err)
-		}
+		return actions.CreateNote(&vault, &uri, params)
 	},
 }
 
 func init() {
 	createNoteCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	createNoteCmd.Flags().BoolVar(&createSelect, "ls", false, "select a note interactively")
+	createNoteCmd.Flags().BoolVar(&createSelect, "select", false, "select a note interactively")
 	createNoteCmd.Flags().BoolVarP(&shouldOpen, "open", "", false, "open created note")
 	createNoteCmd.Flags().StringVarP(&content, "content", "c", "", "text to add to note")
 	createNoteCmd.Flags().BoolVarP(&shouldAppend, "append", "a", false, "append to note")
diff --git a/cmd/daily.go b/cmd/daily.go
index 4105495..8dafc58 100644
--- a/cmd/daily.go
+++ b/cmd/daily.go
@@ -1,29 +1,40 @@
 package cmd
 
 import (
-	"log"
+	"fmt"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
 )
 
+var dailyDryRun bool
+
 var DailyCmd = &cobra.Command{
 	Use:     "daily",
 	Aliases: []string{"d"},
 	Short:   "Creates or opens daily note in vault",
 	Args:    cobra.ExactArgs(0),
-	Run: func(cmd *cobra.Command, args []string) {
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
-		err := actions.DailyNote(&vault, &uri)
-		if err != nil {
-			log.Fatal(err)
+
+		if dailyDryRun {
+			u, err := actions.PlanDailyNote(&vault, &uri)
+			if err != nil {
+				return err
+			}
+			fmt.Println("Daily dry-run:")
+			fmt.Println(u)
+			return nil
 		}
+
+		return actions.DailyNote(&vault, &uri)
 	},
 }
 
 func init() {
+	DailyCmd.Flags().BoolVar(&dailyDryRun, "dry-run", false, "print the Obsidian URI without opening it")
 	DailyCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
 	rootCmd.AddCommand(DailyCmd)
 }
diff --git a/cmd/delete.go b/cmd/delete.go
index f29f691..b486c2f 100644
--- a/cmd/delete.go
+++ b/cmd/delete.go
@@ -1,32 +1,87 @@
 package cmd
 
 import (
+	"errors"
+	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
 
+var deleteForce bool
+var deleteDryRun bool
+var deleteSelect bool
+
 var deleteCmd = &cobra.Command{
-	Use:     "delete",
-	Aliases: []string{"d"},
+	Use:     "delete [note-path]",
+	Aliases: []string{"del"},
 	Short:   "Delete note in vault",
-	Args:    cobra.ExactArgs(1),
-	Run: func(cmd *cobra.Command, args []string) {
+	Long: `Delete a note from the vault.
+
+If other notes link to the note, you'll be prompted to confirm.
+Use --force to skip confirmation (recommended for scripts).`,
+	Example: `  # Delete a note (prompts if linked)
+  obsidian-cli delete "old-note"
+
+  # Force delete without prompt
+  obsidian-cli delete "temp" --force
+
+  # Delete from specific vault
+  obsidian-cli delete "note" --vault "Archive"`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		note := obsidian.Note{}
-		notePath := args[0]
-		params := actions.DeleteParams{NotePath: notePath}
-		err := actions.DeleteNote(&vault, &note, params)
-		if err != nil {
-			log.Fatal(err)
+		notePath := ""
+
+		if len(args) > 0 && !deleteSelect {
+			notePath = args[0]
+		} else {
+			if !deleteSelect {
+				return errors.New("note path required (or use --ls)")
+			}
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			notePath = selected
+		}
+		if notePath == "" {
+			return errors.New("no note selected")
+		}
+		params := actions.DeleteParams{
+			NotePath: notePath,
+			Force:    deleteForce,
+		}
+
+		if deleteDryRun {
+			plan, err := actions.PlanDeleteNote(&vault, params)
+			if err != nil {
+				return err
+			}
+			fmt.Println("Delete dry-run:")
+			fmt.Printf("  vault: %s\n", plan.VaultName)
+			fmt.Printf("  path: %s\n", plan.AbsoluteNotePath)
+			return nil
 		}
+		return actions.DeleteNote(&vault, &note, params)
 	},
 }
 
 func init() {
-	deleteCmd.Flags().BoolVarP(&shouldOpen, "open", "o", false, "open new note")
+	deleteCmd.Flags().BoolVar(&deleteDryRun, "dry-run", false, "preview which file would be deleted without deleting it")
 	deleteCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	deleteCmd.Flags().BoolVarP(&deleteForce, "force", "f", false, "skip confirmation if the note has incoming links")
+	deleteCmd.Flags().BoolVar(&deleteSelect, "ls", false, "select a note interactively")
+	deleteCmd.Flags().BoolVar(&deleteSelect, "select", false, "select a note interactively")
 	rootCmd.AddCommand(deleteCmd)
 }
diff --git a/cmd/frontmatter.go b/cmd/frontmatter.go
new file mode 100644
index 0000000..e7a611d
--- /dev/null
+++ b/cmd/frontmatter.go
@@ -0,0 +1,92 @@
+package cmd
+
+import (
+	"errors"
+	"fmt"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/spf13/cobra"
+)
+
+var fmPrint bool
+var fmEdit bool
+var fmDelete bool
+var fmKey string
+var fmValue string
+var fmSelect bool
+
+var frontmatterCmd = &cobra.Command{
+	Use:     "frontmatter [note]",
+	Aliases: []string{"fm"},
+	Short:   "View or modify note frontmatter",
+	Long: `View or modify YAML frontmatter in a note.
+
+Use --print to display frontmatter, --edit to modify a key,
+or --delete to remove a key.
+
+Examples:
+  obsidian-cli frontmatter "My Note" --print
+  obsidian-cli frontmatter "My Note" --edit --key "status" --value "done"
+  obsidian-cli frontmatter "My Note" --delete --key "draft"`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		vault := obsidian.Vault{Name: vaultName}
+		note := obsidian.Note{}
+		noteName := ""
+
+		if len(args) > 0 && !fmSelect {
+			noteName = args[0]
+		} else {
+			if !fmSelect {
+				return errors.New("note path required (or use --ls)")
+			}
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+		if noteName == "" {
+			return errors.New("no note selected")
+		}
+
+		params := actions.FrontmatterParams{
+			NoteName: noteName,
+			Print:    fmPrint,
+			Edit:     fmEdit,
+			Delete:   fmDelete,
+			Key:      fmKey,
+			Value:    fmValue,
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, params)
+		if err != nil {
+			return err
+		}
+
+		if output != "" {
+			fmt.Print(output)
+		}
+		return nil
+	},
+}
+
+func init() {
+	frontmatterCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	frontmatterCmd.Flags().BoolVar(&fmSelect, "ls", false, "select a note interactively")
+	frontmatterCmd.Flags().BoolVar(&fmSelect, "select", false, "select a note interactively")
+	frontmatterCmd.Flags().BoolVarP(&fmPrint, "print", "p", false, "print frontmatter")
+	frontmatterCmd.Flags().BoolVarP(&fmEdit, "edit", "e", false, "edit a frontmatter key")
+	frontmatterCmd.Flags().BoolVarP(&fmDelete, "delete", "d", false, "delete a frontmatter key")
+	frontmatterCmd.Flags().StringVarP(&fmKey, "key", "k", "", "key to edit or delete")
+	frontmatterCmd.Flags().StringVar(&fmValue, "value", "", "value to set (required for --edit)")
+	rootCmd.AddCommand(frontmatterCmd)
+}
diff --git a/cmd/init.go b/cmd/init.go
new file mode 100644
index 0000000..f7324fe
--- /dev/null
+++ b/cmd/init.go
@@ -0,0 +1,734 @@
+package cmd
+
+import (
+	"bufio"
+	"bytes"
+	"encoding/json"
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/ktr0731/go-fuzzyfinder"
+	"github.com/spf13/cobra"
+	"gopkg.in/yaml.v3"
+)
+
+var initCmd = &cobra.Command{
+	Use:     "init",
+	Short:   "Interactive setup wizard",
+	Long:    "Interactive setup wizard to configure your default vault, daily note settings, and targets.",
+	Args:    cobra.NoArgs,
+	Example: "  obsidian-cli init",
+	RunE: func(cmd *cobra.Command, args []string) error {
+		in := bufio.NewReader(os.Stdin)
+
+		fmt.Println()
+		fmt.Println(DoubleLine(DefaultBorderWidth))
+		fmt.Println("                            OBSIDIAN-CLI SETUP WIZARD                        ")
+		fmt.Println(DoubleLine(DefaultBorderWidth))
+		fmt.Println()
+
+		return runInitWizard(in)
+	},
+}
+
+func init() {
+	rootCmd.AddCommand(initCmd)
+}
+
+type discoveredVault struct {
+	Name string
+	Path string
+	ID   string
+}
+
+func runInitWizard(in *bufio.Reader) error {
+	var vaultName string
+	var vaultPath string
+
+	var settings obsidian.VaultSettings
+	var daily obsidian.DailyNoteSettings
+
+vaultSelection:
+	// Step 1: vault selection
+	for {
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 1: Default vault")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("Type '?' for help, or 'back' to cancel.")
+		fmt.Println()
+
+		vaults, err := discoverVaults()
+		if err != nil || len(vaults) == 0 {
+			if err != nil {
+				fmt.Printf("Could not discover vaults: %v\n", err)
+			} else {
+				fmt.Println("No vaults discovered.")
+			}
+			fmt.Println("Enter the vault name (usually the folder name), or type '?' for help.")
+			name, action, err := promptLine(in, "> ")
+			if err != nil {
+				return err
+			}
+			switch action {
+			case actionBack:
+				return errors.New("cancelled")
+			case actionHelp:
+				printVaultHelp()
+				continue
+			default:
+			}
+			name = strings.TrimSpace(name)
+			if name == "" {
+				fmt.Println("Vault name is required.")
+				continue
+			}
+			vault := obsidian.Vault{Name: name}
+			path, err := vault.Path()
+			if err != nil {
+				fmt.Printf("Vault not found: %v\n", err)
+				fmt.Println("Tip: ensure Obsidian has created an obsidian.json with your vaults.")
+				continue
+			}
+			vaultName = name
+			vaultPath = path
+			break
+		}
+
+		fmt.Println("Discovered vaults:")
+		for i, v := range vaults {
+			fmt.Printf("  %d) %s\n", i+1, v.Name)
+			fmt.Printf("     %s\n", v.Path)
+		}
+		fmt.Println()
+		fmt.Println("Choose a vault:")
+		fmt.Println("  - Enter a number")
+		fmt.Println("  - Type the exact name")
+		fmt.Println("  - Type 'select' to choose with fuzzy finder")
+
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		switch action {
+		case actionBack:
+			return errors.New("cancelled")
+		case actionSkip:
+			fmt.Println("Skip is not available here. Choose a vault or type 'back' to cancel.")
+			continue
+		case actionHelp:
+			printVaultHelp()
+			continue
+		default:
+		}
+
+		line = strings.TrimSpace(line)
+		if strings.EqualFold(line, "select") {
+			name, path, err := pickVaultFuzzy(vaults)
+			if err != nil {
+				fmt.Printf("Selection cancelled: %v\n", err)
+				continue
+			}
+			vaultName = name
+			vaultPath = path
+			break
+		}
+
+		if n, ok := parseNumber(line); ok {
+			if n < 1 || n > len(vaults) {
+				fmt.Printf("Invalid selection: enter 1-%d\n", len(vaults))
+				continue
+			}
+			vaultName = vaults[n-1].Name
+			vaultPath = vaults[n-1].Path
+			break
+		}
+
+		if name, path, ok := matchVaultName(vaults, line); ok {
+			vaultName = name
+			vaultPath = path
+			break
+		}
+
+		fmt.Printf("Vault '%s' not found. Try again.\n", line)
+	}
+
+	// Confirm and write default vault name
+	for {
+		fmt.Println()
+		fmt.Printf("Set default vault to: %s\n", vaultName)
+		fmt.Println("This writes to preferences.json in your user config directory.")
+		fmt.Println("Continue? (y/N)  Type 'back' to re-select vault.")
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			goto vaultSelection
+		}
+		if action == actionHelp {
+			fmt.Println("This sets which vault is used when --vault is not provided.")
+			continue
+		}
+		if action == actionSkip {
+			fmt.Println("Skip is not available here. Answer 'y' to write preferences.json, or type 'back' to re-select.")
+			continue
+		}
+		if !isYes(line) {
+			return errors.New("cancelled")
+		}
+
+		v := obsidian.Vault{}
+		if err := v.SetDefaultName(vaultName); err != nil {
+			fmt.Printf("Failed to write preferences.json: %v\n", err)
+			fmt.Println("Tip: check permissions on your user config directory.")
+			continue
+		}
+		break
+	}
+
+	vault := obsidian.Vault{Name: vaultName}
+
+dailySettings:
+	// Step 2: daily folder
+	settings, _ = vault.Settings()
+	daily = settings.DailyNote
+
+	folder, err := promptDailyFolder(in, vaultPath, daily.Folder)
+	if err != nil {
+		if errors.Is(err, errBack) {
+			goto vaultSelection
+		}
+		return err
+	}
+	daily.Folder = folder
+
+	// Step 3: pattern
+	pattern, err := promptDailyPattern(in, daily.FilenamePattern)
+	if err != nil {
+		if errors.Is(err, errBack) {
+			goto dailySettings
+		}
+		return err
+	}
+	daily.FilenamePattern = pattern
+
+	// Step 4: template
+	templatePath, err := promptDailyTemplate(in, vaultPath, daily.TemplatePath)
+	if err != nil {
+		if errors.Is(err, errBack) {
+			goto dailySettings
+		}
+		return err
+	}
+	daily.TemplatePath = templatePath
+
+	// Step 5: save daily settings
+	daily.CreateIfMissing = true
+	settings.DailyNote = daily
+
+	fmt.Println()
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Println("  Step 2: Daily note settings summary")
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Printf("  folder:   %s\n", daily.Folder)
+	fmt.Printf("  pattern:  %s\n", daily.FilenamePattern)
+	fmt.Printf("  example:  %s.md\n", obsidian.ExpandDatePattern(daily.FilenamePattern, time.Now()))
+	if strings.TrimSpace(daily.TemplatePath) == "" {
+		fmt.Println("  template: (none)")
+	} else {
+		fmt.Printf("  template: %s\n", daily.TemplatePath)
+	}
+	fmt.Println()
+	fmt.Println("Save these daily note settings? (y/N)  Type 'back' to edit.")
+
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			goto dailySettings
+		}
+		if action == actionHelp {
+			printDailyHelp()
+			continue
+		}
+		if !isYes(line) {
+			return errors.New("cancelled")
+		}
+		if err := vault.SetSettings(settings); err != nil {
+			return err
+		}
+		break
+	}
+
+targetsStep:
+	// Step 6: targets migration / setup
+	if err := runTargetsMigration(in); err != nil {
+		if errors.Is(err, errBack) {
+			goto dailySettings
+		}
+		return err
+	}
+
+	// Step 7: optionally add a first target
+	if err := runInitAddTargets(in, vaultPath); err != nil {
+		if errors.Is(err, errBack) {
+			goto targetsStep
+		}
+		return err
+	}
+
+	// Summary
+	fmt.Println()
+	fmt.Println(DoubleLine(DefaultBorderWidth))
+	fmt.Println("                                  SETUP COMPLETE                              ")
+	fmt.Println(DoubleLine(DefaultBorderWidth))
+	fmt.Println()
+	fmt.Printf("Default vault: %s\n", vaultName)
+	fmt.Printf("Vault path:    %s\n", vaultPath)
+	fmt.Println()
+	fmt.Println("Next steps:")
+	fmt.Println("  - Append to today's daily note:")
+	fmt.Println("      obsidian-cli append \"some text\"")
+	fmt.Println("      obsidian-cli a \"some text\"")
+	fmt.Println("  - Append multi-line content (Ctrl-D to save, Ctrl-C to cancel):")
+	fmt.Println("      obsidian-cli append")
+	fmt.Println("  - Capture to a target:")
+	fmt.Println("      obsidian-cli target --select")
+	fmt.Println("      obsidian-cli target inbox \"some text\"")
+	return nil
+}
+
+func parseNumber(s string) (int, bool) {
+	var n int
+	_, err := fmt.Sscanf(strings.TrimSpace(s), "%d", &n)
+	return n, err == nil
+}
+
+func discoverVaults() ([]discoveredVault, error) {
+	obsidianConfigFile, err := config.ObsidianFile()
+	if err != nil {
+		return nil, err
+	}
+	content, err := os.ReadFile(obsidianConfigFile)
+	if err != nil {
+		return nil, err
+	}
+	var cfg struct {
+		Vaults map[string]struct {
+			Path string `json:"path"`
+		} `json:"vaults"`
+	}
+	if err := json.Unmarshal(content, &cfg); err != nil {
+		return nil, err
+	}
+	if len(cfg.Vaults) == 0 {
+		return nil, errors.New("no vaults found in obsidian.json")
+	}
+	var out []discoveredVault
+	for id, v := range cfg.Vaults {
+		p := v.Path
+		if strings.TrimSpace(p) == "" {
+			continue
+		}
+		base := filepath.Base(filepath.Clean(p))
+		if base == "." || base == string(filepath.Separator) || base == "" {
+			continue
+		}
+		out = append(out, discoveredVault{Name: base, Path: p, ID: id})
+	}
+	sort.Slice(out, func(i, j int) bool { return strings.ToLower(out[i].Name) < strings.ToLower(out[j].Name) })
+	return out, nil
+}
+
+func matchVaultName(vaults []discoveredVault, input string) (string, string, bool) {
+	in := strings.TrimSpace(input)
+	if in == "" {
+		return "", "", false
+	}
+	for _, v := range vaults {
+		if strings.EqualFold(v.Name, in) || (v.ID != "" && strings.EqualFold(v.ID, in)) {
+			return v.Name, v.Path, true
+		}
+	}
+	return "", "", false
+}
+
+func pickVaultFuzzy(vaults []discoveredVault) (string, string, error) {
+	items := make([]string, 0, len(vaults))
+	for _, v := range vaults {
+		items = append(items, fmt.Sprintf("%s  (%s)", v.Name, v.Path))
+	}
+	idx, err := fuzzyfinder.Find(items, func(i int) string { return items[i] })
+	if err != nil {
+		return "", "", err
+	}
+	return vaults[idx].Name, vaults[idx].Path, nil
+}
+
+func promptDailyFolder(in *bufio.Reader, vaultPath string, existing string) (string, error) {
+	const defaultDailyFolder = "Daily"
+	for {
+		fmt.Println()
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 2: Daily note folder")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		if strings.TrimSpace(existing) != "" {
+			fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+		}
+		fmt.Println("Enter a folder path relative to the vault, or type 'ls' to browse.")
+		fmt.Println("Type 'skip' to accept a default, '?' for help, or 'back' to go back.")
+
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			fmt.Printf("Using default: %s\n", defaultDailyFolder)
+			return defaultDailyFolder, nil
+		}
+		if action == actionHelp {
+			printDailyHelp()
+			continue
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		if strings.EqualFold(line, "ls") {
+			p, err := pickOrCreateFolderPath(vaultPath)
+			if err != nil {
+				fmt.Printf("Selection error: %v\n", err)
+				continue
+			}
+			return p, nil
+		}
+		if strings.TrimSpace(line) == "" {
+			fmt.Println("Folder is required.")
+			continue
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid folder path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptDailyPattern(in *bufio.Reader, existing string) (string, error) {
+	const defaultDailyPattern = "YYYY-MM-DD"
+	for {
+		fmt.Println()
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 3: Daily note filename pattern")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		if strings.TrimSpace(existing) != "" {
+			fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+		}
+		if strings.TrimSpace(existing) == "" {
+			fmt.Printf("Tip: type 'skip' to accept the default (%s).\n", defaultDailyPattern)
+		}
+
+		pattern, err := promptForTargetPattern(in, existing)
+		if err != nil {
+			if errors.Is(err, errBack) {
+				return "", errBack
+			}
+			return "", err
+		}
+		if strings.TrimSpace(pattern) == "" {
+			fmt.Println("Pattern is required.")
+			continue
+		}
+		return pattern, nil
+	}
+}
+
+func promptDailyTemplate(in *bufio.Reader, vaultPath string, existing string) (string, error) {
+	for {
+		fmt.Println()
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		fmt.Println("  Step 4: Daily note template (optional)")
+		fmt.Println(SingleLine(DefaultBorderWidth))
+		if strings.TrimSpace(existing) != "" {
+			fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+		}
+		fmt.Println("Press Enter for none, type a path, or type 'ls' to browse.")
+		fmt.Println("Type 'skip' to skip this step, '?' for help, or 'back' to go back.")
+
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		switch action {
+		case actionBack:
+			return "", errBack
+		case actionSkip:
+			return existing, nil
+		case actionHelp:
+			printTemplateHelp()
+			continue
+		default:
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		if line == "" {
+			return "", nil
+		}
+		if strings.EqualFold(line, "ls") {
+			p, err := pickOrCreateNotePath(vaultPath)
+			if err != nil {
+				fmt.Printf("Selection error: %v\n", err)
+				continue
+			}
+			return p, nil
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func runTargetsMigration(in *bufio.Reader) error {
+	fmt.Println()
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Println("  Step 5: Targets (optional)")
+	fmt.Println(SingleLine(DefaultBorderWidth))
+	fmt.Println("Type 'skip' to skip, 'back' to go back, '?' for help.")
+
+	_, targetsFile, err := config.TargetsPath()
+	if err != nil {
+		return err
+	}
+	raw, err := os.ReadFile(targetsFile)
+	if err != nil {
+		if os.IsNotExist(err) {
+			fmt.Println("No targets.yaml found.")
+			fmt.Println("Create it now? (y/N)  Type 'skip' to skip, 'back' to go back.")
+			for {
+				line, action, err := promptLine(in, "> ")
+				if err != nil {
+					return err
+				}
+				if action == actionBack {
+					return errBack
+				}
+				if action == actionSkip {
+					return nil
+				}
+				if action == actionHelp {
+					printTargetsHelp()
+					continue
+				}
+				if isYes(line) {
+					path, err := obsidian.EnsureTargetsFileExists()
+					if err != nil {
+						return err
+					}
+					fmt.Printf("Created: %s\n", path)
+				}
+				return nil
+			}
+		}
+		return err
+	}
+
+	scalarNames, err := detectScalarTargets(raw)
+	if err != nil {
+		fmt.Printf("Could not parse targets.yaml: %v\n", err)
+		fmt.Println("Options:")
+		fmt.Println("  - Run: obsidian-cli target edit")
+		fmt.Println("  - Or open it now in your editor")
+		fmt.Println("Open in editor now? (y/N)  Type 'back' to go back, 'skip' to skip.")
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			return errBack
+		}
+		if action == actionSkip {
+			return nil
+		}
+		if isYes(line) {
+			return obsidian.OpenInEditor(targetsFile)
+		}
+		return nil
+	}
+	if len(scalarNames) == 0 {
+		fmt.Println("targets.yaml looks OK (no simplified scalar targets detected).")
+		return nil
+	}
+
+	fmt.Printf("Found %d simplified targets (e.g. inbox: Inbox.md).\n", len(scalarNames))
+	fmt.Println("Migrate them to the canonical format (type/file fields)?")
+	fmt.Println("This will rewrite targets.yaml and may remove custom comments (a standard header will be added).")
+	fmt.Println("Continue? (y/N)  Type 'skip' to skip, 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			return errBack
+		}
+		if action == actionSkip {
+			return nil
+		}
+		if action == actionHelp {
+			printTargetsHelp()
+			continue
+		}
+		if !isYes(line) {
+			return nil
+		}
+		break
+	}
+
+	cfg, err := obsidian.LoadTargets()
+	if err != nil {
+		return err
+	}
+
+	before := bytes.TrimSpace(raw)
+	if err := obsidian.SaveTargets(cfg); err != nil {
+		return err
+	}
+	afterRaw, _ := os.ReadFile(targetsFile)
+	after := bytes.TrimSpace(afterRaw)
+	if bytes.Equal(before, after) {
+		fmt.Println("No changes made.")
+	} else {
+		fmt.Println("Migrated targets.yaml.")
+	}
+	return nil
+}
+
+func detectScalarTargets(raw []byte) ([]string, error) {
+	var node yaml.Node
+	if err := yaml.Unmarshal(raw, &node); err != nil {
+		return nil, err
+	}
+	if len(node.Content) == 0 {
+		return nil, nil
+	}
+	root := node.Content[0]
+	if root.Kind != yaml.MappingNode {
+		return nil, nil
+	}
+
+	var names []string
+	for i := 0; i+1 < len(root.Content); i += 2 {
+		k := root.Content[i]
+		v := root.Content[i+1]
+		if k.Kind == yaml.ScalarNode && v.Kind == yaml.ScalarNode {
+			n := strings.TrimSpace(k.Value)
+			if n != "" {
+				names = append(names, n)
+			}
+		}
+	}
+	sort.Strings(names)
+	return names, nil
+}
+
+func runInitAddTargets(in *bufio.Reader, vaultPath string) error {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return err
+	}
+
+	fmt.Println()
+	fmt.Println("Add a target now? (y/N)  Type 'skip' to skip, 'back' to go back, '?' for help.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if action == actionBack {
+			return errBack
+		}
+		if action == actionSkip {
+			return nil
+		}
+		if action == actionHelp {
+			printTargetsHelp()
+			continue
+		}
+		if !isYes(line) {
+			return nil
+		}
+		break
+	}
+
+	for {
+		name, target, err := runTargetAddWizard(in, vaultPath, "", cfg)
+		if err != nil {
+			fmt.Printf("Error: %v\n", err)
+		} else {
+			cfg[name] = target
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Saved target: %s\n", name)
+		}
+
+		fmt.Println("Add another? (y/N)")
+		line, _, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		if !isYes(line) {
+			break
+		}
+	}
+	return nil
+}
+
+func printVaultHelp() {
+	fmt.Println()
+	fmt.Println("Vault help:")
+	fmt.Println("- Vault name is usually the folder name of your vault path.")
+	fmt.Println("- obsidian-cli reads vaults from Obsidian's obsidian.json in your user config directory.")
+	fmt.Println("- If no vaults are discovered, open Obsidian once and ensure at least one vault is configured.")
+}
+
+func printDailyHelp() {
+	fmt.Println()
+	fmt.Println("Daily note help:")
+	fmt.Println("- Folder is a path relative to the vault root (example: Daily).")
+	fmt.Println("- Pattern controls the daily note filename and supports Obsidian-style tokens like YYYY, MM, DD, HH, mm, ss.")
+	fmt.Println("- [literal] blocks are preserved, e.g. YYYY-[log]-MM.")
+}
+
+func printTemplateHelp() {
+	fmt.Println()
+	fmt.Println("Template help:")
+	fmt.Println("- Template is a note path relative to the vault root (example: Templates/Daily).")
+	fmt.Println("- When a daily note is created for the first time, template content is copied and variables like {{date}}, {{time}}, {{title}} are expanded.")
+}
+
+func printTargetsHelp() {
+	fmt.Println()
+	fmt.Println("Targets help:")
+	fmt.Println("- Targets let you capture quickly to a configured note or dated note pattern.")
+	fmt.Println("- Use: obsidian-cli target add    (guided workflow)")
+	fmt.Println("- Use: obsidian-cli target --select")
+	fmt.Println("- targets.yaml is stored next to preferences.json.")
+}
diff --git a/cmd/move.go b/cmd/move.go
index 114dc62..6a6ffb6 100644
--- a/cmd/move.go
+++ b/cmd/move.go
@@ -1,28 +1,98 @@
 package cmd
 
 import (
+	"errors"
+	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
 
 var shouldOpen bool
+var moveSelect bool
 var moveCmd = &cobra.Command{
-	Use:     "move",
+	Use:     "move [from-note-path] [to-note-path]",
 	Aliases: []string{"m"},
-	Short:   "Move or rename note in vault and updated corresponding links",
-	Args:    cobra.ExactArgs(2),
-	Run: func(cmd *cobra.Command, args []string) {
-		currentName := args[0]
-		newName := args[1]
+	Short:   "Move or rename note in vault and update corresponding links",
+	Long: `Moves or renames a note and updates all links pointing to it.
+
+This command safely renames notes by also updating any [[wikilinks]]
+or [markdown](links) that reference the moved note.`,
+	Example: `  # Rename a note
+  obsidian-cli move "Old Name" "New Name"
+
+  # Move to a different folder
+  obsidian-cli move "Inbox/note" "Projects/note"
+
+  # Move and open the result
+  obsidian-cli move "temp" "Archive/temp" --open`,
+	Args: cobra.MaximumNArgs(2),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		note := obsidian.Note{}
 		uri := obsidian.Uri{}
+		currentName := ""
+		newName := ""
+
+		var vaultPath string
+		needVaultPath := moveSelect || len(args) < 2
+		if needVaultPath {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			p, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			vaultPath = p
+		}
+
+		switch len(args) {
+		case 2:
+			currentName = args[0]
+			newName = args[1]
+		case 1:
+			if moveSelect {
+				newName = args[0]
+				selected, err := pickExistingNotePath(vaultPath)
+				if err != nil {
+					return err
+				}
+				currentName = selected
+			} else {
+				currentName = args[0]
+				selected, err := promptNewNotePath(vaultPath)
+				if err != nil {
+					return err
+				}
+				newName = selected
+			}
+		case 0:
+			if !moveSelect {
+				return errors.New("from-note-path required (or use --ls)")
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			currentName = selected
+			selected, err = promptNewNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			newName = selected
+		default:
+			return errors.New("expected 0, 1, or 2 arguments")
+		}
+
+		if currentName == "" || newName == "" {
+			return errors.New("both from-note-path and to-note-path are required")
+		}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
-			log.Fatalf("Failed to parse --editor flag: %v", err)
+			return fmt.Errorf("failed to parse --editor flag: %w", err)
 		}
 		params := actions.MoveParams{
 			CurrentNoteName: currentName,
@@ -30,17 +100,15 @@ var moveCmd = &cobra.Command{
 			ShouldOpen:      shouldOpen,
 			UseEditor:       useEditor,
 		}
-		err = actions.MoveNote(&vault, &note, &uri, params)
-		if err != nil {
-			log.Fatal(err)
-		}
-
+		return actions.MoveNote(&vault, &note, &uri, params)
 	},
 }
 
 func init() {
 	moveCmd.Flags().BoolVarP(&shouldOpen, "open", "o", false, "open new note")
 	moveCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	moveCmd.Flags().BoolVar(&moveSelect, "ls", false, "select the note to move interactively")
+	moveCmd.Flags().BoolVar(&moveSelect, "select", false, "select the note to move interactively")
 	moveCmd.Flags().BoolP("editor", "e", false, "open in editor instead of Obsidian (requires --open flag)")
 	rootCmd.AddCommand(moveCmd)
 }
diff --git a/cmd/note_picker.go b/cmd/note_picker.go
new file mode 100644
index 0000000..2613389
--- /dev/null
+++ b/cmd/note_picker.go
@@ -0,0 +1,103 @@
+package cmd
+
+import (
+	"bufio"
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/ktr0731/go-fuzzyfinder"
+)
+
+const (
+	notePickerNewNote   = "(Create new note...)"
+	notePickerVaultRoot = "(Vault root)"
+)
+
+func pickExistingNotePath(vaultPath string) (string, error) {
+	notes, err := listVaultNotes(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	if len(notes) == 0 {
+		return "", errors.New("no notes found in vault")
+	}
+	sort.Strings(notes)
+
+	idx, err := fuzzyfinder.Find(notes, func(i int) string { return notes[i] })
+	if err != nil {
+		return "", err
+	}
+	return notes[idx], nil
+}
+
+func pickNotePathOrNew(vaultPath string) (string, error) {
+	notes, err := listVaultNotes(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	notes = append(notes, notePickerNewNote)
+	sort.Strings(notes)
+
+	idx, err := fuzzyfinder.Find(notes, func(i int) string { return notes[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := notes[idx]
+	if choice != notePickerNewNote {
+		return choice, nil
+	}
+	return promptNewNotePath(vaultPath)
+}
+
+func promptNewNotePath(vaultPath string) (string, error) {
+	folders, err := listVaultFolders(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	folders = append(folders, notePickerVaultRoot, "(Create new folder...)")
+	sort.Strings(folders)
+
+	idx, err := fuzzyfinder.Find(folders, func(i int) string { return folders[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := folders[idx]
+	switch choice {
+	case "(Create new folder...)":
+		created, err := promptCreateFolder(vaultPath)
+		if err != nil {
+			return "", err
+		}
+		choice = created
+	case notePickerVaultRoot:
+		choice = ""
+	}
+
+	in := bufio.NewReader(os.Stdin)
+	fmt.Println("Enter new note name (relative to the selected folder; .md optional):")
+	fmt.Print("> ")
+	name, err := in.ReadString('\n')
+	if err != nil {
+		return "", err
+	}
+	name = strings.TrimSpace(name)
+	if name == "" {
+		return "", errors.New("note name is required")
+	}
+	name = strings.TrimSuffix(name, ".md")
+
+	rel := strings.TrimSpace(filepath.ToSlash(filepath.Join(choice, name)))
+	if rel == "" {
+		return "", errors.New("note path is required")
+	}
+	if _, err := obsidian.SafeJoinVaultPath(vaultPath, rel); err != nil {
+		return "", err
+	}
+	return rel, nil
+}
+
diff --git a/cmd/open.go b/cmd/open.go
index 9a10bff..61bca0f 100644
--- a/cmd/open.go
+++ b/cmd/open.go
@@ -1,7 +1,7 @@
 package cmd
 
 import (
-	"log"
+	"errors"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
@@ -9,24 +9,57 @@ import (
 )
 
 var vaultName string
+var openSelect bool
 var OpenVaultCmd = &cobra.Command{
-	Use:     "open",
+	Use:     "open [note-path]",
 	Aliases: []string{"o"},
 	Short:   "Opens note in vault by note name",
-	Args:    cobra.ExactArgs(1),
-	Run: func(cmd *cobra.Command, args []string) {
+	Long: `Opens a note in Obsidian by name or path.
+
+The note name can be just the filename or a path relative to the vault root.
+The .md extension is optional.`,
+	Example: `  # Open a note by name
+  obsidian-cli open "Meeting Notes"
+
+  # Open a note in a subfolder
+  obsidian-cli open "Projects/my-project"
+
+  # Open in a specific vault
+  obsidian-cli open "Daily" --vault "Work"`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
-		noteName := args[0]
-		params := actions.OpenParams{NoteName: noteName}
-		err := actions.OpenNote(&vault, &uri, params)
-		if err != nil {
-			log.Fatal(err)
+		noteName := ""
+
+		if len(args) > 0 && !openSelect {
+			noteName = args[0]
+		} else {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickNotePathOrNew(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+
+		if noteName == "" {
+			return errors.New("no note selected")
 		}
+		params := actions.OpenParams{NoteName: noteName}
+		return actions.OpenNote(&vault, &uri, params)
 	},
 }
 
 func init() {
 	OpenVaultCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
+	OpenVaultCmd.Flags().BoolVar(&openSelect, "ls", false, "select a note interactively")
+	OpenVaultCmd.Flags().BoolVar(&openSelect, "select", false, "select a note interactively")
 	rootCmd.AddCommand(OpenVaultCmd)
 }
diff --git a/cmd/print.go b/cmd/print.go
index 4b40d90..a638b38 100644
--- a/cmd/print.go
+++ b/cmd/print.go
@@ -1,36 +1,75 @@
 package cmd
 
 import (
+	"errors"
 	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
 
 var shouldRenderMarkdown bool
+var printSelect bool
 var printCmd = &cobra.Command{
-	Use:     "print",
+	Use:     "print [note-path]",
 	Aliases: []string{"p"},
 	Short:   "Print contents of note",
-	Args:    cobra.ExactArgs(1),
-	Run: func(cmd *cobra.Command, args []string) {
-		noteName := args[0]
+	Long: `Prints the contents of a note to stdout.
+
+Useful for piping note contents to other commands, or quickly viewing
+a note without opening Obsidian.`,
+	Example: `  # Print a note
+  obsidian-cli print "Meeting Notes"
+
+  # Print note in subfolder
+  obsidian-cli print "Projects/readme"
+
+  # Pipe to grep
+  obsidian-cli print "Todo" | grep "TODO"
+
+  # Copy to clipboard (macOS)
+  obsidian-cli print "Template" | pbcopy`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
+		noteName := ""
+		if len(args) > 0 && !printSelect {
+			noteName = args[0]
+		} else {
+			if _, err := vault.DefaultName(); err != nil {
+				return err
+			}
+			vaultPath, err := vault.Path()
+			if err != nil {
+				return err
+			}
+			selected, err := pickExistingNotePath(vaultPath)
+			if err != nil {
+				return err
+			}
+			noteName = selected
+		}
+		if noteName == "" {
+			return errors.New("no note selected")
+		}
 		note := obsidian.Note{}
 		params := actions.PrintParams{
 			NoteName: noteName,
 		}
 		contents, err := actions.PrintNote(&vault, &note, params)
 		if err != nil {
-			log.Fatal(err)
+			return err
 		}
 		fmt.Println(contents)
+		return nil
 	},
 }
 
 func init() {
 	printCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	printCmd.Flags().BoolVar(&printSelect, "ls", false, "select a note interactively")
+	printCmd.Flags().BoolVar(&printSelect, "select", false, "select a note interactively")
 	rootCmd.AddCommand(printCmd)
 }
diff --git a/cmd/print_default.go b/cmd/print_default.go
index bee464d..6b96a70 100644
--- a/cmd/print_default.go
+++ b/cmd/print_default.go
@@ -2,7 +2,6 @@ package cmd
 
 import (
 	"fmt"
-	"log"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
@@ -12,26 +11,35 @@ var printPathOnly bool
 var printDefaultCmd = &cobra.Command{
 	Use:     "print-default",
 	Aliases: []string{"pd"},
-	Short:   "prints default vault name and path",
-	Args:    cobra.ExactArgs(0),
-	Run: func(cmd *cobra.Command, args []string) {
+	Short:   "Prints default vault name and path",
+	Long: `Shows the currently configured default vault.
+
+Use --path-only to output just the path, useful for scripting.`,
+	Example: `  # Show default vault info
+  obsidian-cli print-default
+
+  # Get just the path (for scripts)
+  obsidian-cli print-default --path-only`,
+	Args: cobra.ExactArgs(0),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{}
 		name, err := vault.DefaultName()
 		if err != nil {
-			log.Fatal(err)
+			return err
 		}
 		path, err := vault.Path()
 		if err != nil {
-			log.Fatal(err)
+			return err
 		}
 
 		if printPathOnly {
 			fmt.Print(path)
-			return
+			return nil
 		}
 
 		fmt.Println("Default vault name: ", name)
 		fmt.Println("Default vault path: ", path)
+		return nil
 	},
 }
 
diff --git a/cmd/root.go b/cmd/root.go
index 3aed97e..20fd5ee 100644
--- a/cmd/root.go
+++ b/cmd/root.go
@@ -3,7 +3,9 @@ package cmd
 import (
 	"fmt"
 	"os"
+	"strings"
 
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
 )
 
@@ -15,8 +17,48 @@ var rootCmd = &cobra.Command{
 }
 
 func Execute() {
+	maybeRewriteArgsForTargetNames()
 	if err := rootCmd.Execute(); err != nil {
 		fmt.Fprintf(os.Stderr, "Whoops. There was an error while executing your CLI '%s'", err)
 		os.Exit(1)
 	}
 }
+
+func maybeRewriteArgsForTargetNames() {
+	args := os.Args[1:]
+	if len(args) == 0 {
+		return
+	}
+	first := strings.TrimSpace(args[0])
+	if first == "" || strings.HasPrefix(first, "-") {
+		return
+	}
+	if isKnownRootCommand(first) {
+		return
+	}
+
+	cfg, err := obsidian.LoadTargets()
+	if err != nil {
+		return
+	}
+	if _, ok := cfg[first]; !ok {
+		return
+	}
+
+	// Treat: obsidian-cli <targetName> ... as: obsidian-cli target <targetName> ...
+	rootCmd.SetArgs(append([]string{"target"}, args...))
+}
+
+func isKnownRootCommand(name string) bool {
+	for _, c := range rootCmd.Commands() {
+		if c.Name() == name {
+			return true
+		}
+		for _, a := range c.Aliases {
+			if a == name {
+				return true
+			}
+		}
+	}
+	return false
+}
diff --git a/cmd/search.go b/cmd/search.go
index 47740f5..c6c4836 100644
--- a/cmd/search.go
+++ b/cmd/search.go
@@ -1,9 +1,11 @@
 package cmd
 
 import (
+	"fmt"
+	"strings"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
@@ -12,20 +14,58 @@ var searchCmd = &cobra.Command{
 	Use:     "search",
 	Aliases: []string{"s"},
 	Short:   "Fuzzy searches and opens note in vault",
-	Args:    cobra.NoArgs,
-	Run: func(cmd *cobra.Command, args []string) {
+	Long: `Opens an interactive fuzzy finder to search and open notes.
+
+Type to filter notes by filename. Press Enter to open the selected
+note in Obsidian, or use --editor to open in your $EDITOR.`,
+	Example: `  # Interactive search
+  obsidian-cli search
+
+  # Search and open in editor
+  obsidian-cli search --editor
+
+  # Search in specific vault
+  obsidian-cli search --vault "Work"`,
+	Args: cobra.NoArgs,
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
-		note := obsidian.Note{}
-		uri := obsidian.Uri{}
-		fuzzyFinder := obsidian.FuzzyFinder{}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
-			log.Fatalf("failed to retrieve 'editor' flag: %v", err)
+			return fmt.Errorf("failed to retrieve 'editor' flag: %w", err)
+		}
+
+		if _, err := vault.DefaultName(); err != nil {
+			return err
 		}
-		err = actions.SearchNotes(&vault, &note, &uri, &fuzzyFinder, useEditor)
+		vaultPath, err := vault.Path()
 		if err != nil {
-			log.Fatal(err)
+			return err
 		}
+
+		notePath, err := pickNotePathOrNew(vaultPath)
+		if err != nil {
+			return err
+		}
+		if strings.TrimSpace(notePath) == "" {
+			return fmt.Errorf("no note selected")
+		}
+
+		if useEditor {
+			fmt.Printf("Opening note: %s\n", notePath)
+			rel := notePath
+			if !strings.HasSuffix(strings.ToLower(rel), ".md") {
+				rel += ".md"
+			}
+			abs, err := obsidian.SafeJoinVaultPath(vaultPath, rel)
+			if err != nil {
+				return err
+			}
+			return obsidian.OpenInEditor(abs)
+		}
+
+		uri := obsidian.Uri{}
+		params := actions.OpenParams{NoteName: notePath}
+		return actions.OpenNote(&vault, &uri, params)
 	},
 }
 
diff --git a/cmd/search_content.go b/cmd/search_content.go
index dd7b218..fe6340d 100644
--- a/cmd/search_content.go
+++ b/cmd/search_content.go
@@ -1,7 +1,7 @@
 package cmd
 
 import (
-	"log"
+	"fmt"
 
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
@@ -10,11 +10,23 @@ import (
 )
 
 var searchContentCmd = &cobra.Command{
-	Use:     "search-content [search term]",
-	Short:   "Search node content for search term",
+	Use:   "search-content <search-term>",
+	Short: "Search note content for search term",
+	Long: `Searches the contents of all notes for a term.
+
+Displays matching notes with line numbers and snippets. If multiple
+matches are found, opens a fuzzy finder to select which note to open.`,
+	Example: `  # Search for a term
+  obsidian-cli search-content "TODO"
+
+  # Search and open in editor
+  obsidian-cli search-content "bug" --editor
+
+  # Search in specific vault
+  obsidian-cli search-content "project" --vault "Work"`,
 	Args:    cobra.ExactArgs(1),
 	Aliases: []string{"sc"},
-	Run: func(cmd *cobra.Command, args []string) {
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		note := obsidian.Note{}
 		uri := obsidian.Uri{}
@@ -23,12 +35,9 @@ var searchContentCmd = &cobra.Command{
 		searchTerm := args[0]
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
-			log.Fatalf("Failed to parse 'editor' flag: %v", err)
-		}
-		err = actions.SearchNotesContent(&vault, &note, &uri, &fuzzyFinder, searchTerm, useEditor)
-		if err != nil {
-			log.Fatal(err)
+			return fmt.Errorf("failed to parse 'editor' flag: %w", err)
 		}
+		return actions.SearchNotesContent(&vault, &note, &uri, &fuzzyFinder, searchTerm, useEditor)
 	},
 }
 
diff --git a/cmd/set_default.go b/cmd/set_default.go
index f2cb992..75a7c85 100644
--- a/cmd/set_default.go
+++ b/cmd/set_default.go
@@ -2,30 +2,38 @@ package cmd
 
 import (
 	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
-	"log"
 )
 
 var setDefaultCmd = &cobra.Command{
-	Use:     "set-default",
+	Use:     "set-default <vault-name>",
 	Aliases: []string{"sd"},
 	Short:   "Sets default vault",
-	Args:    cobra.ExactArgs(1),
-	Run: func(cmd *cobra.Command, args []string) {
+	Long: `Sets the default vault for all commands.
+
+The vault name must match exactly as it appears in Obsidian.
+Once set, you won't need to specify --vault for each command.`,
+	Example: `  # Set default vault
+  obsidian-cli set-default "My Vault"
+
+  # Verify it worked
+  obsidian-cli print-default`,
+	Args: cobra.ExactArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		name := args[0]
 		v := obsidian.Vault{Name: name}
-		err := v.SetDefaultName(name)
-		if err != nil {
-			log.Fatal(err)
+		if err := v.SetDefaultName(name); err != nil {
+			return err
 		}
 		path, err := v.Path()
 		if err != nil {
-			log.Fatal(err)
+			return err
 		}
 		fmt.Println("Default vault set to: ", name)
 		fmt.Println("Default vault path set to: ", path)
-
+		return nil
 	},
 }
 
diff --git a/cmd/target.go b/cmd/target.go
new file mode 100644
index 0000000..720e19e
--- /dev/null
+++ b/cmd/target.go
@@ -0,0 +1,1326 @@
+package cmd
+
+import (
+	"bufio"
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/ktr0731/go-fuzzyfinder"
+	"github.com/spf13/cobra"
+)
+
+var (
+	targetSelect bool
+	targetDryRun bool
+)
+
+var targetCmd = &cobra.Command{
+	Use:     "target [id] [text]",
+	Aliases: []string{"t"},
+	Short: "Append text to a configured target note",
+	Long: `Appends text to a note configured in targets.yaml.
+
+Targets can point at:
+  - a fixed file path (always append to the same note)
+  - a folder + filename pattern (append to a dated note based on the current time)
+
+If no text is provided, content is read from stdin (piped) or entered interactively until EOF.`,
+	Example: `  # Append a one-liner to a target
+  obsidian-cli target inbox "Buy milk"
+
+  # Multi-line content (Ctrl-D to save, Ctrl-C to cancel)
+  obsidian-cli target inbox
+
+  # Pick a target interactively, then enter content
+  obsidian-cli target --select
+
+  # Preview which file would be used
+  obsidian-cli target inbox --dry-run`,
+	Args: cobra.ArbitraryArgs,
+	RunE: func(cmd *cobra.Command, args []string) error {
+		vault := obsidian.Vault{Name: vaultName}
+
+		if len(args) == 0 || targetSelect {
+			name, err := pickTargetName()
+			if err != nil {
+				return err
+			}
+			if strings.TrimSpace(name) == "" {
+				return errors.New("no target selected")
+			}
+
+			if targetDryRun {
+				return printTargetPlan(&vault, name)
+			}
+
+			content, err := actions.PromptForContentIfEmpty("")
+			if err != nil {
+				return err
+			}
+			plan, err := actions.AppendToTarget(&vault, name, content, time.Now(), false)
+			if err != nil {
+				return err
+			}
+			fmt.Printf("Wrote to: %s\n", plan.AbsoluteNotePath)
+			return nil
+		}
+
+		name := strings.TrimSpace(args[0])
+		content := strings.TrimSpace(strings.Join(args[1:], " "))
+
+		if targetDryRun {
+			return printTargetPlan(&vault, name)
+		}
+
+		var err error
+		content, err = actions.PromptForContentIfEmpty(content)
+		if err != nil {
+			return err
+		}
+
+		plan, err := actions.AppendToTarget(&vault, name, content, time.Now(), false)
+		if err != nil {
+			return err
+		}
+		fmt.Printf("Wrote to: %s\n", plan.AbsoluteNotePath)
+		return nil
+	},
+}
+
+var targetListCmd = &cobra.Command{
+	Use:   "list",
+	Short: "List configured targets",
+	RunE: func(cmd *cobra.Command, args []string) error {
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+		names := obsidian.ListTargetNames(cfg)
+		if len(names) == 0 {
+			fmt.Println("No targets configured.")
+			fmt.Println("Run: obsidian-cli target add")
+			return nil
+		}
+		for _, n := range names {
+			t := cfg[n]
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+				fmt.Printf("- %s (folder: %s, pattern: %s)\n", n, t.Folder, t.Pattern)
+			} else {
+				fmt.Printf("- %s (file: %s)\n", n, firstNonEmpty(t.File, t.Note))
+			}
+		}
+		return nil
+	},
+}
+
+var targetAddCmd = &cobra.Command{
+	Use:   "add [name]",
+	Short: "Add a new target",
+	Long: `Add a new capture target.
+
+Run without a name to start a guided workflow.`,
+	Example: `  # Guided workflow
+  obsidian-cli target add
+
+  # Add a fixed-file target
+  obsidian-cli target add inbox
+
+  # Add a folder+pattern target
+  obsidian-cli target add log`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		in := bufio.NewReader(os.Stdin)
+
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+
+		var name string
+		if len(args) == 1 {
+			name = strings.TrimSpace(args[0])
+		}
+
+		vault := obsidian.Vault{Name: vaultName}
+		vaultPath, err := vault.Path()
+		if err != nil {
+			return err
+		}
+
+		name, target, err := runTargetAddWizard(in, vaultPath, name, cfg)
+		if err != nil {
+			return err
+		}
+
+		cfg[name] = target
+		if err := obsidian.SaveTargets(cfg); err != nil {
+			return err
+		}
+
+		fmt.Printf("Saved target: %s\n", name)
+		return nil
+	},
+}
+
+var targetRemoveCmd = &cobra.Command{
+	Use:     "remove [name]",
+	Aliases: []string{"rm"},
+	Short:   "Remove a target",
+	Args:    cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+
+		var name string
+		if len(args) == 1 {
+			name = strings.TrimSpace(args[0])
+		} else {
+			name, err = pickTargetNameFromConfig(cfg)
+			if err != nil {
+				return err
+			}
+		}
+		if strings.TrimSpace(name) == "" {
+			return errors.New("target name is required")
+		}
+
+		if _, ok := cfg[name]; !ok {
+			return fmt.Errorf("target not found: %s", name)
+		}
+		delete(cfg, name)
+		if err := obsidian.SaveTargets(cfg); err != nil {
+			return err
+		}
+		fmt.Printf("Removed target: %s\n", name)
+		return nil
+	},
+}
+
+var targetTestCmd = &cobra.Command{
+	Use:   "test [name]",
+	Short: "Preview the resolved path for a target",
+	Long: `Shows which file would be created or appended to for the given target.
+
+If no name is provided, previews all targets.`,
+	Args: cobra.MaximumNArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
+		vault := obsidian.Vault{Name: vaultName}
+		cfg, err := loadTargetsOrEmpty()
+		if err != nil {
+			return err
+		}
+		names := obsidian.ListTargetNames(cfg)
+		if len(names) == 0 {
+			return errors.New("no targets configured")
+		}
+
+		if len(args) == 1 {
+			name := strings.TrimSpace(args[0])
+			return printTargetPlan(&vault, name)
+		}
+
+		// Preview all targets.
+		for _, name := range names {
+			fmt.Printf("%s:\n", name)
+			if err := printTargetPlan(&vault, name); err != nil {
+				fmt.Printf("  error: %v\n", err)
+			}
+		}
+		return nil
+	},
+}
+
+var targetEditCmd = &cobra.Command{
+	Use:   "edit",
+	Short: "Edit targets in CLI or open targets.yaml",
+	RunE: func(cmd *cobra.Command, args []string) error {
+		in := bufio.NewReader(os.Stdin)
+
+		path, err := obsidian.EnsureTargetsFileExists()
+		if err != nil {
+			return err
+		}
+
+		for {
+			fmt.Println("Edit targets:")
+			fmt.Println("  1) Stay in CLI (recommended)")
+			fmt.Println("  2) Open in editor")
+			fmt.Println("Press Enter for CLI, type '?' for help, or 'back' to cancel.")
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return err
+			}
+			switch action {
+			case actionBack:
+				return nil
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Edit help:")
+				fmt.Printf("- CLI mode provides guided add/edit/remove/test/list workflows.\n")
+				fmt.Printf("- Editor mode opens: %s\n", path)
+				fmt.Println()
+				continue
+			case actionSkip:
+				return runTargetEditor(in)
+			default:
+			}
+			switch strings.ToLower(strings.TrimSpace(line)) {
+			case "", "1", "cli":
+				return runTargetEditor(in)
+			case "2", "editor":
+				return obsidian.OpenInEditor(path)
+			default:
+				fmt.Println("Invalid selection. Enter 1 or 2, or type '?' for help.")
+			}
+		}
+	},
+}
+
+func init() {
+	targetCmd.Flags().BoolVar(&targetSelect, "ls", false, "select a target interactively")
+	targetCmd.Flags().BoolVar(&targetSelect, "select", false, "select a target interactively")
+	targetCmd.Flags().BoolVar(&targetDryRun, "dry-run", false, "preview the resolved target path without writing")
+	targetCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
+
+	targetCmd.AddCommand(targetListCmd)
+	targetCmd.AddCommand(targetAddCmd)
+	targetCmd.AddCommand(targetRemoveCmd)
+	targetCmd.AddCommand(targetTestCmd)
+	targetCmd.AddCommand(targetEditCmd)
+
+	rootCmd.AddCommand(targetCmd)
+}
+
+func loadTargetsOrEmpty() (obsidian.TargetsConfig, error) {
+	_, err := obsidian.EnsureTargetsFileExists()
+	if err != nil {
+		return nil, err
+	}
+	cfg, err := obsidian.LoadTargets()
+	if err != nil {
+		return nil, err
+	}
+	return cfg, nil
+}
+
+func pickTargetName() (string, error) {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return "", err
+	}
+	return pickTargetNameFromConfig(cfg)
+}
+
+func pickTargetNameFromConfig(cfg obsidian.TargetsConfig) (string, error) {
+	names := obsidian.ListTargetNames(cfg)
+	if len(names) == 0 {
+		return "", errors.New("no targets configured (run: obsidian-cli target add)")
+	}
+
+	idx, err := fuzzyfinder.Find(names, func(i int) string {
+		return names[i]
+	})
+	if err != nil {
+		return "", err
+	}
+	return names[idx], nil
+}
+
+func printTargetPlan(vault *obsidian.Vault, name string) error {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return err
+	}
+	target, ok := cfg[name]
+	if !ok {
+		return fmt.Errorf("target not found: %s", name)
+	}
+
+	effectiveVault := vault
+	if strings.TrimSpace(target.Vault) != "" {
+		effectiveVault = &obsidian.Vault{Name: strings.TrimSpace(target.Vault)}
+	}
+
+	vaultName, err := effectiveVault.DefaultName()
+	if err != nil {
+		return err
+	}
+	vaultPath, err := effectiveVault.Path()
+	if err != nil {
+		return err
+	}
+
+	plan, err := obsidian.PlanTargetAppend(vaultPath, name, target, time.Now())
+	if err != nil {
+		return err
+	}
+
+	fmt.Printf("  vault: %s\n", vaultName)
+	fmt.Printf("  note: %s\n", plan.AbsoluteNotePath)
+	fmt.Printf("  create_dirs: %t\n", plan.WillCreateDirs)
+	fmt.Printf("  create_file: %t\n", plan.WillCreateFile)
+	if plan.WillApplyTemplate {
+		fmt.Printf("  template: %s\n", plan.AbsoluteTemplate)
+	}
+	return nil
+}
+
+func runTargetAddWizard(in *bufio.Reader, vaultPath string, existingName string, existing obsidian.TargetsConfig) (string, obsidian.Target, error) {
+	step := 0
+	var name string
+	var t obsidian.Target
+
+	for {
+		switch step {
+		case 0:
+			if strings.TrimSpace(existingName) != "" {
+				name = existingName
+			} else {
+				fmt.Println("Target name")
+				fmt.Println("Type '?' for help, or 'back' to cancel.")
+				line, action, err := promptLine(in, "> ")
+				if err != nil {
+					return "", obsidian.Target{}, err
+				}
+				switch action {
+				case actionBack:
+					return "", obsidian.Target{}, errors.New("cancelled")
+				case actionHelp:
+					fmt.Println()
+					fmt.Println("Target name help:")
+					fmt.Println("- Names cannot contain whitespace.")
+					fmt.Println("- Reserved names: add, rm, ls, edit, validate, test, help.")
+					fmt.Println("- Examples: inbox, todo, log")
+					fmt.Println()
+					continue
+				case actionSkip:
+					fmt.Println("Target name is required (skip is not available here).")
+					continue
+				default:
+				}
+				if err := obsidian.ValidateTargetName(line); err != nil {
+					fmt.Printf("Invalid name: %v\n", err)
+					continue
+				}
+				if _, ok := existing[line]; ok {
+					fmt.Println("A target with that name already exists.")
+					continue
+				}
+				name = line
+			}
+			step = 1
+		case 1:
+			fmt.Println("Target type:")
+			fmt.Println("  1) file   (append to one fixed note)")
+			fmt.Println("  2) folder (append to a dated note based on a pattern)")
+			fmt.Println("Type a number, 'skip' for file (recommended), '?' for help, or 'back' to change the name.")
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return "", obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				step = 0
+				existingName = ""
+				continue
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Type help:")
+				fmt.Println("- file: always appends to the same note")
+				fmt.Println("- folder: appends to a note path derived from folder + date pattern")
+				fmt.Println()
+				continue
+			case actionSkip:
+				t.Type = "file"
+				step = 2
+				continue
+			default:
+			}
+			switch line {
+			case "1":
+				t.Type = "file"
+				step = 2
+			case "2":
+				t.Type = "folder"
+				step = 2
+			default:
+				fmt.Println("Invalid selection.")
+			}
+		case 2:
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "file" {
+				p, err := promptForTargetFile(in, vaultPath, "", name+".md")
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return "", obsidian.Target{}, err
+				}
+				t.File = p
+				step = 4
+			} else {
+				folder, err := promptForTargetFolder(in, vaultPath, "", name)
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return "", obsidian.Target{}, err
+				}
+				t.Folder = folder
+				step = 3
+			}
+		case 3:
+			pattern, err := promptForTargetPattern(in, "")
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 2
+					continue
+				}
+				return "", obsidian.Target{}, err
+			}
+			t.Pattern = pattern
+			step = 4
+		case 4:
+			template, err := promptForTemplatePath(in, vaultPath, t.Template)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+						step = 3
+					} else {
+						step = 2
+					}
+					continue
+				}
+				return "", obsidian.Target{}, err
+			}
+			t.Template = template
+			step = 5
+		case 5:
+			vaultOverride, err := promptForVaultOverride(in, t.Vault)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 4
+					continue
+				}
+				return "", obsidian.Target{}, err
+			}
+			t.Vault = vaultOverride
+			step = 6
+		case 6:
+			fmt.Println()
+			fmt.Println("Save this target? (y/N)  Type 'back' to edit, '?' for help, or 'skip' to cancel.")
+			fmt.Printf("  name: %s\n", name)
+			fmt.Printf("  type: %s\n", t.Type)
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+				fmt.Printf("  folder: %s\n", t.Folder)
+				fmt.Printf("  pattern: %s\n", t.Pattern)
+				fmt.Printf("  example: %s\n", obsidian.ExpandDatePattern(t.Pattern, time.Now()))
+			} else {
+				fmt.Printf("  file: %s\n", t.File)
+			}
+			if strings.TrimSpace(t.Template) != "" {
+				fmt.Printf("  template: %s\n", t.Template)
+			}
+			if strings.TrimSpace(t.Vault) != "" {
+				fmt.Printf("  vault: %s\n", t.Vault)
+			}
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return "", obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				step = 5
+				continue
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Save help:")
+				fmt.Println("- Answer 'y' to save to targets.yaml.")
+				fmt.Println("- Answer 'n' or press Enter to cancel without saving.")
+				fmt.Println()
+				continue
+			case actionSkip:
+				return "", obsidian.Target{}, errors.New("cancelled")
+			default:
+			}
+			if isYes(line) {
+				return name, t, nil
+			}
+			return "", obsidian.Target{}, errors.New("cancelled")
+		default:
+			return "", obsidian.Target{}, errors.New("invalid wizard state")
+		}
+	}
+}
+
+var errBack = errors.New("back")
+
+func promptForTargetFile(in *bufio.Reader, vaultPath string, existing string, defaultPath string) (string, error) {
+	fmt.Println("Target file path (relative to vault).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	if strings.TrimSpace(existing) == "" && strings.TrimSpace(defaultPath) != "" {
+		fmt.Printf("Default: %s\n", defaultPath)
+	}
+	fmt.Println("Type a path, 'ls' to browse, 'skip' to accept default/keep current, '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionHelp {
+			fmt.Println()
+			fmt.Println("File help:")
+			fmt.Println("- Path is relative to the vault root (example: Inbox/Capture.md).")
+			fmt.Println("- A .md extension is optional.")
+			fmt.Println()
+			continue
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultPath) == "" {
+				fmt.Println("No default available; enter a file path.")
+				continue
+			}
+			fmt.Printf("Using default: %s\n", defaultPath)
+			return defaultPath, nil
+		}
+		if line == "" {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultPath) != "" {
+				fmt.Printf("Using default: %s\n", defaultPath)
+				return defaultPath, nil
+			}
+			fmt.Println("File path is required.")
+			continue
+		}
+		if strings.EqualFold(line, "ls") {
+			return pickOrCreateNotePath(vaultPath)
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptForTargetFolder(in *bufio.Reader, vaultPath string, existing string, defaultFolder string) (string, error) {
+	fmt.Println("Target folder path (relative to vault).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	if strings.TrimSpace(existing) == "" && strings.TrimSpace(defaultFolder) != "" {
+		fmt.Printf("Default: %s\n", defaultFolder)
+	}
+	fmt.Println("Type a folder path, 'ls' to browse, 'skip' to accept default/keep current, '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionHelp {
+			fmt.Println()
+			fmt.Println("Folder help:")
+			fmt.Println("- Path is relative to the vault root (example: Inbox).")
+			fmt.Println("- A date-based filename will be appended using your selected pattern.")
+			fmt.Println()
+			continue
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultFolder) == "" {
+				fmt.Println("No default available; enter a folder path.")
+				continue
+			}
+			fmt.Printf("Using default: %s\n", defaultFolder)
+			return defaultFolder, nil
+		}
+		if line == "" {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			if strings.TrimSpace(defaultFolder) != "" {
+				fmt.Printf("Using default: %s\n", defaultFolder)
+				return defaultFolder, nil
+			}
+			fmt.Println("Folder path is required.")
+			continue
+		}
+		if strings.EqualFold(line, "ls") {
+			return pickOrCreateFolderPath(vaultPath)
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptForVaultOverride(in *bufio.Reader, existing string) (string, error) {
+	fmt.Println("Vault override (optional).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	fmt.Println("Press Enter for default vault, type a vault name, or type 'skip' to keep current.")
+	fmt.Println("Type '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		switch action {
+		case actionBack:
+			return "", errBack
+		case actionHelp:
+			fmt.Println()
+			fmt.Println("Vault override help:")
+			fmt.Println("- Leave empty to use the default vault.")
+			fmt.Println("- Set to a vault name (as it appears in Obsidian) to route this target elsewhere.")
+			fmt.Println()
+			continue
+		case actionSkip:
+			return existing, nil
+		default:
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		return line, nil
+	}
+}
+
+func promptForTemplatePath(in *bufio.Reader, vaultPath string, existing string) (string, error) {
+	fmt.Println("Template note path (optional, relative to vault).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	fmt.Println("Press Enter for none, type a path, type 'ls' to browse, or type 'skip' to keep current.")
+	fmt.Println("Type '?' for help, or 'back' to go back.")
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		switch action {
+		case actionBack:
+			return "", errBack
+		case actionHelp:
+			fmt.Println()
+			fmt.Println("Template help:")
+			fmt.Println("- Applied only when creating a new note for the first time.")
+			fmt.Println("- Supports variables like {{date}}, {{time}}, and {{title}}.")
+			fmt.Println()
+			continue
+		case actionSkip:
+			return existing, nil
+		default:
+		}
+		if line == "" && strings.TrimSpace(existing) != "" {
+			return existing, nil
+		}
+		if line == "" {
+			return "", nil
+		}
+		if strings.EqualFold(line, "ls") {
+			path, err := pickOrCreateNotePath(vaultPath)
+			if err != nil {
+				return "", err
+			}
+			return path, nil
+		}
+		if _, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line)); err != nil {
+			fmt.Printf("Invalid path: %v\n", err)
+			continue
+		}
+		return line, nil
+	}
+}
+
+func promptForTargetPattern(in *bufio.Reader, existing string) (string, error) {
+	now := time.Now()
+	const defaultPattern = "YYYY-MM-DD"
+	options := []struct {
+		label   string
+		pattern string
+	}{
+		{"Daily (YYYY-MM-DD)", "YYYY-MM-DD"},
+		{"Hourly (YYYY-MM-DD_HH)", "YYYY-MM-DD_HH"},
+		{"Minute (YYYY-MM-DD_HHmm)", "YYYY-MM-DD_HHmm"},
+		{"Second (YYYY-MM-DD_HHmmss)", "YYYY-MM-DD_HHmmss"},
+		{"Zettel (YYYYMMDDHHmmss)", "YYYYMMDDHHmmss"},
+		{"Daily + weekday (YYYY-MM-DD_ddd)", "YYYY-MM-DD_ddd"},
+		{"Daily + weekday full (YYYY-MM-DD_dddd)", "YYYY-MM-DD_dddd"},
+		{"Month name (YYYY-MMMM-DD)", "YYYY-MMMM-DD"},
+		{"Month abbrev (YYYY-MMM-DD)", "YYYY-MMM-DD"},
+		{"AM/PM (YYYY-MM-DD_A)", "YYYY-MM-DD_A"},
+		{"Literal blocks (YYYY-[log]-MM)", "YYYY-[log]-MM"},
+		{"Custom pattern...", ""},
+	}
+
+	fmt.Println("Filename pattern (controls when a new file is created).")
+	if strings.TrimSpace(existing) != "" {
+		fmt.Printf("Current: %s (press Enter to keep)\n", existing)
+	}
+	fmt.Printf("Type a number, type a pattern directly, 'skip' for default (%s), '?' for help, or 'back' to go back.\n", defaultPattern)
+	for i, o := range options {
+		ex := o.pattern
+		if ex != "" {
+			ex = obsidian.ExpandDatePattern(o.pattern, now)
+			ex = ex + ".md"
+		}
+		if o.pattern == "" {
+			fmt.Printf("  %d) %s\n", i+1, o.label)
+		} else {
+			fmt.Printf("  %d) %s -> %s\n", i+1, o.label, ex)
+		}
+	}
+
+	for {
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return "", err
+		}
+		if action == actionBack {
+			return "", errBack
+		}
+		if action == actionHelp {
+			printPatternHelp()
+			continue
+		}
+		if action == actionSkip {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			return defaultPattern, nil
+		}
+		if line == "" {
+			if strings.TrimSpace(existing) != "" {
+				return existing, nil
+			}
+			return defaultPattern, nil
+		}
+		// allow direct entry of a pattern
+		if line != "" && (strings.ContainsAny(line, "YMDHmsd") || strings.Contains(line, "{") || strings.Contains(line, "[") || strings.Contains(line, "]")) && !isDigits(line) {
+			return line, nil
+		}
+		n, convErr := parseChoice(line, len(options))
+		if convErr != nil {
+			fmt.Println(convErr.Error())
+			continue
+		}
+		chosen := options[n-1]
+		if chosen.pattern == "" {
+			fmt.Println("Enter custom pattern (Obsidian-style tokens; supports [literal] blocks).")
+			fmt.Println("Examples: YYYY-MM-DD_HHmm, YYYYMMDDHHmmss, YYYY-[ToDo]-MM")
+			fmt.Printf("Type 'skip' for default (%s), '?' for help, or 'back' to go back.\n", defaultPattern)
+			for {
+				custom, customAction, err := promptLine(in, "> ")
+				if err != nil {
+					return "", err
+				}
+				switch customAction {
+				case actionBack:
+					goto continuePatternMenu
+				case actionHelp:
+					printPatternHelp()
+					continue
+				case actionSkip:
+					if strings.TrimSpace(existing) != "" {
+						return existing, nil
+					}
+					return defaultPattern, nil
+				default:
+				}
+				custom = strings.TrimSpace(custom)
+				if strings.TrimSpace(custom) == "" {
+					fmt.Println("Pattern is required.")
+					continue
+				}
+				return custom, nil
+			}
+		}
+		return chosen.pattern, nil
+
+	continuePatternMenu:
+		continue
+	}
+}
+
+func printPatternHelp() {
+	fmt.Println()
+	fmt.Println("Pattern help:")
+	fmt.Println("- Tokens (curated subset): YYYY, YY, MM, M, DD, D, HH, H, mm, m, ss, s, ddd, dddd, MMM, MMMM, A, a")
+	fmt.Println("- Zettel timestamp: YYYYMMDDHHmmss")
+	fmt.Println("- Literal blocks: wrap text in [brackets], e.g. YYYY-[log]-MM")
+	fmt.Println("- Examples:")
+	fmt.Println("    YYYY-MM-DD")
+	fmt.Println("    YYYY-MM-DD_HH")
+	fmt.Println("    YYYY-MM-DD_HHmmss")
+	fmt.Println("    YYYYMMDDHHmmss")
+	fmt.Println()
+}
+
+func parseChoice(line string, max int) (int, error) {
+	if line == "" {
+		return 0, errors.New("enter a selection")
+	}
+	var n int
+	_, err := fmt.Sscanf(line, "%d", &n)
+	if err != nil || n < 1 || n > max {
+		return 0, fmt.Errorf("invalid selection: enter 1-%d", max)
+	}
+	return n, nil
+}
+
+func isDigits(s string) bool {
+	for _, r := range s {
+		if r < '0' || r > '9' {
+			return false
+		}
+	}
+	return s != ""
+}
+
+func pickOrCreateFolderPath(vaultPath string) (string, error) {
+	dirs, err := listVaultFolders(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	dirs = append(dirs, "(Create new folder...)")
+	sort.Strings(dirs)
+
+	idx, err := fuzzyfinder.Find(dirs, func(i int) string { return dirs[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := dirs[idx]
+	if choice != "(Create new folder...)" {
+		return choice, nil
+	}
+	return promptCreateFolder(vaultPath)
+}
+
+func pickOrCreateNotePath(vaultPath string) (string, error) {
+	files, err := listVaultNotes(vaultPath)
+	if err != nil {
+		return "", err
+	}
+	files = append(files, "(Create new note...)")
+	sort.Strings(files)
+
+	idx, err := fuzzyfinder.Find(files, func(i int) string { return files[i] })
+	if err != nil {
+		return "", err
+	}
+	choice := files[idx]
+	if choice != "(Create new note...)" {
+		return choice, nil
+	}
+	return promptCreateNote(vaultPath)
+}
+
+func listVaultFolders(vaultPath string) ([]string, error) {
+	var out []string
+	err := filepath.WalkDir(vaultPath, func(path string, d os.DirEntry, err error) error {
+		if err != nil {
+			return err
+		}
+		if !d.IsDir() {
+			return nil
+		}
+		if path == vaultPath {
+			return nil
+		}
+		base := filepath.Base(path)
+		if strings.HasPrefix(base, ".") {
+			return filepath.SkipDir
+		}
+		rel, err := filepath.Rel(vaultPath, path)
+		if err != nil {
+			return err
+		}
+		out = append(out, filepath.ToSlash(rel))
+		return nil
+	})
+	return out, err
+}
+
+func listVaultNotes(vaultPath string) ([]string, error) {
+	var out []string
+	err := filepath.WalkDir(vaultPath, func(path string, d os.DirEntry, err error) error {
+		if err != nil {
+			return err
+		}
+		base := filepath.Base(path)
+		if d.IsDir() {
+			if strings.HasPrefix(base, ".") {
+				return filepath.SkipDir
+			}
+			return nil
+		}
+		if strings.HasPrefix(base, ".") {
+			return nil
+		}
+		if strings.ToLower(filepath.Ext(base)) != ".md" {
+			return nil
+		}
+		rel, err := filepath.Rel(vaultPath, path)
+		if err != nil {
+			return err
+		}
+		out = append(out, filepath.ToSlash(strings.TrimSuffix(rel, ".md")))
+		return nil
+	})
+	return out, err
+}
+
+func promptCreateFolder(vaultPath string) (string, error) {
+	in := bufio.NewReader(os.Stdin)
+	fmt.Println("Enter folder path to create (relative to vault):")
+	fmt.Print("> ")
+	line, err := in.ReadString('\n')
+	if err != nil {
+		return "", err
+	}
+	line = strings.TrimSpace(line)
+	if line == "" {
+		return "", errors.New("folder path is required")
+	}
+	abs, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line))
+	if err != nil {
+		return "", err
+	}
+	if err := os.MkdirAll(abs, 0750); err != nil {
+		return "", err
+	}
+	return line, nil
+}
+
+func promptCreateNote(vaultPath string) (string, error) {
+	in := bufio.NewReader(os.Stdin)
+	fmt.Println("Enter note path to create (relative to vault):")
+	fmt.Print("> ")
+	line, err := in.ReadString('\n')
+	if err != nil {
+		return "", err
+	}
+	line = strings.TrimSpace(line)
+	if line == "" {
+		return "", errors.New("note path is required")
+	}
+	abs, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(line))
+	if err != nil {
+		return "", err
+	}
+	if !strings.HasSuffix(strings.ToLower(abs), ".md") {
+		abs += ".md"
+	}
+	if err := os.MkdirAll(filepath.Dir(abs), 0750); err != nil {
+		return "", err
+	}
+	if err := os.WriteFile(abs, []byte{}, 0600); err != nil {
+		return "", err
+	}
+	return line, nil
+}
+
+func firstNonEmpty(values ...string) string {
+	for _, v := range values {
+		if strings.TrimSpace(v) != "" {
+			return v
+		}
+	}
+	return ""
+}
+
+func runTargetEditor(in *bufio.Reader) error {
+	cfg, err := loadTargetsOrEmpty()
+	if err != nil {
+		return err
+	}
+
+	vault := obsidian.Vault{Name: vaultName}
+	vaultPath, err := vault.Path()
+	if err != nil {
+		return err
+	}
+
+	for {
+		fmt.Println()
+		fmt.Println("Target editor:")
+		fmt.Println("  1) Add target")
+		fmt.Println("  2) Edit target")
+		fmt.Println("  3) Remove target")
+		fmt.Println("  4) Test target")
+		fmt.Println("  5) List targets")
+		fmt.Println("  6) Back")
+		fmt.Println("Type '?' for help, or 'back'/'skip' to exit.")
+		line, action, err := promptLine(in, "> ")
+		if err != nil {
+			return err
+		}
+		switch action {
+		case actionBack, actionSkip:
+			return nil
+		case actionHelp:
+			fmt.Println()
+			fmt.Println("Editor help:")
+			fmt.Println("- Add/Edit prompts support 'back', 'skip', and '?' to help you continue without restarting.")
+			fmt.Println()
+			continue
+		default:
+		}
+		switch line {
+		case "1":
+			name, target, err := runTargetAddWizard(in, vaultPath, "", cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			cfg[name] = target
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Saved target: %s\n", name)
+		case "2":
+			name, err := pickTargetNameFromConfig(cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			current := cfg[name]
+			updated, err := runTargetEditWizard(in, vaultPath, name, current)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			cfg[name] = updated
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Updated target: %s\n", name)
+		case "3":
+			name, err := pickTargetNameFromConfig(cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			delete(cfg, name)
+			if err := obsidian.SaveTargets(cfg); err != nil {
+				return err
+			}
+			fmt.Printf("Removed target: %s\n", name)
+		case "4":
+			name, err := pickTargetNameFromConfig(cfg)
+			if err != nil {
+				fmt.Printf("Error: %v\n", err)
+				continue
+			}
+			if err := printTargetPlan(&vault, name); err != nil {
+				fmt.Printf("Error: %v\n", err)
+			}
+		case "5":
+			names := obsidian.ListTargetNames(cfg)
+			if len(names) == 0 {
+				fmt.Println("No targets configured.")
+				continue
+			}
+			for _, n := range names {
+				t := cfg[n]
+				if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+					fmt.Printf("- %s (folder: %s, pattern: %s)\n", n, t.Folder, t.Pattern)
+				} else {
+					fmt.Printf("- %s (file: %s)\n", n, firstNonEmpty(t.File, t.Note))
+				}
+			}
+		case "6", "back":
+			return nil
+		default:
+			fmt.Println("Invalid selection.")
+		}
+	}
+}
+
+func runTargetEditWizard(in *bufio.Reader, vaultPath string, name string, current obsidian.Target) (obsidian.Target, error) {
+	step := 0
+	t := current
+
+	for {
+		switch step {
+		case 0:
+			fmt.Printf("Editing target: %s\n", name)
+			step = 1
+		case 1:
+			fmt.Println("Target type:")
+			fmt.Printf("Current: %s (press Enter to keep)\n", strings.TrimSpace(t.Type))
+			fmt.Println("  1) file   (append to one fixed note)")
+			fmt.Println("  2) folder (append to a dated note based on a pattern)")
+			fmt.Println("Type a number, press Enter to keep current, 'skip' to keep current, '?' for help, or 'back' to cancel.")
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				return obsidian.Target{}, errors.New("cancelled")
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Type help:")
+				fmt.Println("- file: always appends to the same note")
+				fmt.Println("- folder: appends to a note path derived from folder + date pattern")
+				fmt.Println()
+				continue
+			case actionSkip:
+				step = 2
+				continue
+			default:
+			}
+			if line == "" && strings.TrimSpace(t.Type) != "" {
+				step = 2
+				continue
+			}
+			switch line {
+			case "1":
+				t.Type = "file"
+				step = 2
+			case "2":
+				t.Type = "folder"
+				step = 2
+			default:
+				fmt.Println("Invalid selection.")
+			}
+		case 2:
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "file" {
+				p, err := promptForTargetFile(in, vaultPath, firstNonEmpty(t.File, t.Note), name+".md")
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return obsidian.Target{}, err
+				}
+				t.File = p
+				t.Note = ""
+				t.Folder = ""
+				t.Pattern = ""
+				step = 4
+			} else {
+				folder, err := promptForTargetFolder(in, vaultPath, t.Folder, name)
+				if err != nil {
+					if errors.Is(err, errBack) {
+						step = 1
+						continue
+					}
+					return obsidian.Target{}, err
+				}
+				t.Folder = folder
+				step = 3
+			}
+		case 3:
+			pattern, err := promptForTargetPattern(in, t.Pattern)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 2
+					continue
+				}
+				return obsidian.Target{}, err
+			}
+			t.Pattern = pattern
+			step = 4
+		case 4:
+			template, err := promptForTemplatePath(in, vaultPath, t.Template)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+						step = 3
+					} else {
+						step = 2
+					}
+					continue
+				}
+				return obsidian.Target{}, err
+			}
+			t.Template = template
+			step = 5
+		case 5:
+			vaultOverride, err := promptForVaultOverride(in, t.Vault)
+			if err != nil {
+				if errors.Is(err, errBack) {
+					step = 4
+					continue
+				}
+				return obsidian.Target{}, err
+			}
+			t.Vault = vaultOverride
+			step = 6
+		case 6:
+			fmt.Println()
+			fmt.Println("Save changes? (y/N)  Type 'back' to edit, '?' for help, or 'skip' to cancel.")
+			fmt.Printf("  name: %s\n", name)
+			fmt.Printf("  type: %s\n", t.Type)
+			if strings.ToLower(strings.TrimSpace(t.Type)) == "folder" {
+				fmt.Printf("  folder: %s\n", t.Folder)
+				fmt.Printf("  pattern: %s\n", t.Pattern)
+				fmt.Printf("  example: %s\n", obsidian.ExpandDatePattern(t.Pattern, time.Now()))
+			} else {
+				fmt.Printf("  file: %s\n", t.File)
+			}
+			if strings.TrimSpace(t.Template) != "" {
+				fmt.Printf("  template: %s\n", t.Template)
+			}
+			if strings.TrimSpace(t.Vault) != "" {
+				fmt.Printf("  vault: %s\n", t.Vault)
+			}
+			line, action, err := promptLine(in, "> ")
+			if err != nil {
+				return obsidian.Target{}, err
+			}
+			switch action {
+			case actionBack:
+				step = 5
+				continue
+			case actionHelp:
+				fmt.Println()
+				fmt.Println("Save help:")
+				fmt.Println("- Answer 'y' to save to targets.yaml.")
+				fmt.Println("- Answer 'n' or press Enter to cancel without saving.")
+				fmt.Println()
+				continue
+			case actionSkip:
+				return obsidian.Target{}, errors.New("cancelled")
+			default:
+			}
+			if isYes(line) {
+				return t, nil
+			}
+			return obsidian.Target{}, errors.New("cancelled")
+		default:
+			return obsidian.Target{}, errors.New("invalid wizard state")
+		}
+	}
+}
diff --git a/cmd/wizard_prompt.go b/cmd/wizard_prompt.go
new file mode 100644
index 0000000..e55327b
--- /dev/null
+++ b/cmd/wizard_prompt.go
@@ -0,0 +1,44 @@
+package cmd
+
+import (
+	"bufio"
+	"fmt"
+	"strings"
+)
+
+type promptAction int
+
+const (
+	actionNone promptAction = iota
+	actionBack
+	actionSkip
+	actionHelp
+)
+
+func promptLine(in *bufio.Reader, prompt string) (string, promptAction, error) {
+	fmt.Print(prompt)
+	line, err := in.ReadString('\n')
+	if err != nil {
+		return "", actionNone, err
+	}
+	line = strings.TrimSpace(line)
+	switch strings.ToLower(line) {
+	case "back":
+		return "", actionBack, nil
+	case "skip":
+		return "", actionSkip, nil
+	case "?":
+		return "", actionHelp, nil
+	default:
+		return line, actionNone, nil
+	}
+}
+
+func isYes(s string) bool {
+	switch strings.ToLower(strings.TrimSpace(s)) {
+	case "y", "yes":
+		return true
+	default:
+		return false
+	}
+}
diff --git a/cmd/wizard_ui.go b/cmd/wizard_ui.go
new file mode 100644
index 0000000..90aded0
--- /dev/null
+++ b/cmd/wizard_ui.go
@@ -0,0 +1,28 @@
+package cmd
+
+import "strings"
+
+// wizard_ui.go defines shared constants and functions for interactive wizard UI styling.
+// This ensures consistent border styling across all wizard prompts.
+
+// DefaultBorderWidth is the standard width for wizard borders
+const DefaultBorderWidth = 76
+
+// DoubleLine returns a double-line border of the specified width.
+// Used for major section headers (wizard start, completion).
+func DoubleLine(width int) string {
+	if width < 0 {
+		width = 0
+	}
+	return strings.Repeat("═", width)
+}
+
+// SingleLine returns a single-line border of the specified width.
+// Used for step headers within a wizard.
+func SingleLine(width int) string {
+	if width < 0 {
+		width = 0
+	}
+	return strings.Repeat("─", width)
+}
+
diff --git a/go.mod b/go.mod
index 656a5b7..a7631c0 100644
--- a/go.mod
+++ b/go.mod
@@ -10,6 +10,8 @@ require (
 )
 
 require (
+	github.com/BurntSushi/toml v0.3.1 // indirect
+	github.com/adrg/frontmatter v0.2.0 // indirect
 	github.com/davecgh/go-spew v1.1.1 // indirect
 	github.com/gdamore/encoding v1.0.1 // indirect
 	github.com/gdamore/tcell/v2 v2.7.4 // indirect
@@ -25,5 +27,6 @@ require (
 	golang.org/x/sys v0.28.0 // indirect
 	golang.org/x/term v0.27.0 // indirect
 	golang.org/x/text v0.21.0 // indirect
+	gopkg.in/yaml.v2 v2.3.0 // indirect
 	gopkg.in/yaml.v3 v3.0.1 // indirect
 )
diff --git a/go.sum b/go.sum
index 2339b50..294ac6d 100644
--- a/go.sum
+++ b/go.sum
@@ -1,3 +1,7 @@
+github.com/BurntSushi/toml v0.3.1 h1:WXkYYl6Yr3qBf1K79EBnL4mak0OimBfB0XUf9Vl28OQ=
+github.com/BurntSushi/toml v0.3.1/go.mod h1:xHWCNGjB5oqiDr8zfno3MHue2Ht5sIBksp03qcyfWMU=
+github.com/adrg/frontmatter v0.2.0 h1:/DgnNe82o03riBd1S+ZDjd43wAmC6W35q67NHeLkPd4=
+github.com/adrg/frontmatter v0.2.0/go.mod h1:93rQCj3z3ZlwyxxpQioRKC1wDLto4aXHrbqIsnH9wmE=
 github.com/cpuguy83/go-md2man/v2 v2.0.4/go.mod h1:tgQtvFlXSQOSOSIRvRPT7W67SCa46tRHOmNcaadrF8o=
 github.com/davecgh/go-spew v1.1.0/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
 github.com/davecgh/go-spew v1.1.1 h1:vj9j/u1bqnvCEfJOwUhtlOARqs3+rkHYY13jYWTU97c=
@@ -90,6 +94,8 @@ golang.org/x/tools v0.6.0/go.mod h1:Xwgl3UAJ/d3gWutnCtw505GrjyAbvKui8lOU390QaIU=
 golang.org/x/xerrors v0.0.0-20190717185122-a985d3407aa7/go.mod h1:I/5z698sn9Ka8TeJc9MKroUUfqBBauWjQqLJ2OPfmY0=
 gopkg.in/check.v1 v0.0.0-20161208181325-20d25e280405 h1:yhCVgyC4o1eVCa2tZl7eS0r+SDo693bJlVdllGtEeKM=
 gopkg.in/check.v1 v0.0.0-20161208181325-20d25e280405/go.mod h1:Co6ibVJAznAaIkqp8huTwlJQCZ016jof/cbN4VW5Yz0=
+gopkg.in/yaml.v2 v2.3.0 h1:clyUAQHOM3G0M3f5vQj7LuJrETvjVot3Z5el9nffUtU=
+gopkg.in/yaml.v2 v2.3.0/go.mod h1:hI93XBmqTisBFMUTm0b8Fm+jr3Dg1NNxqwp+5A1VGuI=
 gopkg.in/yaml.v3 v3.0.0-20200313102051-9f266ea9e77c/go.mod h1:K4uyk7z7BCEPqu6E+C64Yfv1cQ7kz7rIZviUmN+EgEM=
 gopkg.in/yaml.v3 v3.0.1 h1:fxVm/GzAzEWqLHuvctI91KS9hhNmmWOoWu0XTYJS7CA=
 gopkg.in/yaml.v3 v3.0.1/go.mod h1:K4uyk7z7BCEPqu6E+C64Yfv1cQ7kz7rIZviUmN+EgEM=
diff --git a/mocks/note.go b/mocks/note.go
index efb9295..407e788 100644
--- a/mocks/note.go
+++ b/mocks/note.go
@@ -7,7 +7,9 @@ type MockNoteManager struct {
 	MoveErr          error
 	UpdateLinksError error
 	GetContentsError error
+	SetContentsError error
 	NoMatches        bool
+	Contents         string
 }
 
 func (m *MockNoteManager) Delete(string) error {
@@ -23,9 +25,16 @@ func (m *MockNoteManager) UpdateLinks(string, string, string) error {
 }
 
 func (m *MockNoteManager) GetContents(string, string) (string, error) {
+	if m.Contents != "" {
+		return m.Contents, m.GetContentsError
+	}
 	return "example contents", m.GetContentsError
 }
 
+func (m *MockNoteManager) SetContents(string, string, string) error {
+	return m.SetContentsError
+}
+
 func (m *MockNoteManager) GetNotesList(string) ([]string, error) {
 	return []string{"note1", "note2", "note3"}, m.GetContentsError
 }
diff --git a/pkg/actions/append.go b/pkg/actions/append.go
new file mode 100644
index 0000000..0293eee
--- /dev/null
+++ b/pkg/actions/append.go
@@ -0,0 +1,215 @@
+package actions
+
+import (
+	"bufio"
+	"errors"
+	"fmt"
+	"io"
+	"os"
+	"path/filepath"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+)
+
+type DailyAppendPlan struct {
+	VaultName         string
+	VaultPath         string
+	Folder            string
+	Pattern           string
+	Filename          string
+	RelativeNotePath  string
+	AbsoluteNotePath  string
+	WillCreateDirs    bool
+	WillCreateFile    bool
+	TemplateRel       string
+	TemplateAbs       string
+	WillApplyTemplate bool
+}
+
+func PlanDailyAppend(vault *obsidian.Vault, now time.Time) (DailyAppendPlan, error) {
+	vaultName, err := vault.DefaultName()
+	if err != nil {
+		return DailyAppendPlan{}, err
+	}
+
+	vaultPath, err := vault.Path()
+	if err != nil {
+		return DailyAppendPlan{}, err
+	}
+
+	settings, err := vault.Settings()
+	if err != nil {
+		return DailyAppendPlan{}, err
+	}
+
+	folder := strings.TrimSpace(settings.DailyNote.Folder)
+	if folder == "" {
+		return DailyAppendPlan{}, errors.New("daily note is not configured (missing daily_note.folder in preferences.json)")
+	}
+
+	pattern := strings.TrimSpace(settings.DailyNote.FilenamePattern)
+	if pattern == "" {
+		pattern = "{YYYY-MM-DD}"
+	}
+	filename, err := obsidian.FormatDatePattern(pattern, now)
+	if err != nil {
+		return DailyAppendPlan{}, err
+	}
+
+	relNotePath := filepath.ToSlash(filepath.Join(folder, filename))
+	abs, err := safeJoinVaultPath(vaultPath, relNotePath)
+	if err != nil {
+		return DailyAppendPlan{}, err
+	}
+	if !strings.HasSuffix(strings.ToLower(abs), ".md") {
+		abs += ".md"
+	}
+
+	plan := DailyAppendPlan{
+		VaultName:        vaultName,
+		VaultPath:        vaultPath,
+		Folder:           folder,
+		Pattern:          pattern,
+		Filename:         filename,
+		RelativeNotePath: relNotePath,
+		AbsoluteNotePath: abs,
+	}
+
+	if _, err := os.Stat(filepath.Dir(abs)); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateDirs = true
+		} else {
+			return DailyAppendPlan{}, err
+		}
+	}
+
+	if _, err := os.Stat(abs); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateFile = true
+		} else {
+			return DailyAppendPlan{}, err
+		}
+	}
+
+	templateRel := strings.TrimSpace(settings.DailyNote.TemplatePath)
+	if templateRel != "" && plan.WillCreateFile {
+		templateAbs, err := obsidian.SafeJoinVaultPath(vaultPath, filepath.ToSlash(templateRel))
+		if err != nil {
+			return DailyAppendPlan{}, fmt.Errorf("invalid template path: %w", err)
+		}
+		if !strings.HasSuffix(strings.ToLower(templateAbs), ".md") {
+			templateAbs += ".md"
+		}
+		if _, err := os.Stat(templateAbs); err != nil {
+			return DailyAppendPlan{}, fmt.Errorf("failed to read template: %w", err)
+		}
+
+		plan.TemplateRel = templateRel
+		plan.TemplateAbs = templateAbs
+		plan.WillApplyTemplate = true
+	}
+
+	return plan, nil
+}
+
+// AppendToDailyNote appends content to today's daily note, using per-vault settings.
+func AppendToDailyNote(vault *obsidian.Vault, content string) error {
+	content = strings.TrimSpace(content)
+	if content == "" {
+		return errors.New("no content provided")
+	}
+
+	now := time.Now()
+
+	plan, err := PlanDailyAppend(vault, now)
+	if err != nil {
+		return err
+	}
+	abs := plan.AbsoluteNotePath
+
+	if err := os.MkdirAll(filepath.Dir(abs), 0750); err != nil {
+		return fmt.Errorf("failed to create note directory: %w", err)
+	}
+
+	mode := os.FileMode(0600)
+	if info, err := os.Stat(abs); err == nil {
+		mode = info.Mode()
+	}
+
+	existing, err := os.ReadFile(abs)
+	if err != nil {
+		if !os.IsNotExist(err) {
+			return fmt.Errorf("failed to read note: %w", err)
+		}
+		existing = []byte{}
+
+		if plan.WillApplyTemplate {
+			b, err := os.ReadFile(plan.TemplateAbs)
+			if err != nil {
+				return fmt.Errorf("failed to read template: %w", err)
+			}
+			title := filepath.Base(abs)
+			b = obsidian.ExpandTemplateVariablesAt(b, title, now)
+			existing = b
+		}
+	}
+
+	next := appendWithSeparator(string(existing), content)
+	return os.WriteFile(abs, []byte(next), mode)
+}
+
+// PromptForContentIfEmpty returns content if non-empty, otherwise reads stdin (piped) or prompts
+// for multi-line input until EOF.
+func PromptForContentIfEmpty(content string) (string, error) {
+	content = strings.TrimSpace(content)
+	if content != "" {
+		return content, nil
+	}
+
+	stat, err := os.Stdin.Stat()
+	if err == nil && (stat.Mode()&os.ModeCharDevice) == 0 {
+		b, err := io.ReadAll(os.Stdin)
+		if err != nil {
+			return "", err
+		}
+		s := strings.TrimSpace(string(b))
+		if s == "" {
+			return "", errors.New("no content provided")
+		}
+		return s, nil
+	}
+
+	fmt.Println("Enter text to append (Ctrl-D to save, Ctrl-C to cancel):")
+	in := bufio.NewScanner(os.Stdin)
+	var lines []string
+	for in.Scan() {
+		lines = append(lines, in.Text())
+	}
+	if err := in.Err(); err != nil {
+		return "", err
+	}
+	s := strings.TrimSpace(strings.Join(lines, "\n"))
+	if s == "" {
+		return "", errors.New("no content provided")
+	}
+	return s, nil
+}
+
+func appendWithSeparator(existing string, addition string) string {
+	existing = strings.TrimRight(existing, "\n")
+	addition = strings.TrimSpace(addition)
+	if addition == "" {
+		return existing + "\n"
+	}
+	if existing == "" {
+		return addition + "\n"
+	}
+	return existing + "\n\n" + addition + "\n"
+}
+
+// safeJoinVaultPath joins a vault root and a relative note path and ensures the result stays within the vault.
+func safeJoinVaultPath(vaultPath string, relativePath string) (string, error) {
+	return obsidian.SafeJoinVaultPath(vaultPath, relativePath)
+}
diff --git a/pkg/actions/append_test.go b/pkg/actions/append_test.go
new file mode 100644
index 0000000..5ee4280
--- /dev/null
+++ b/pkg/actions/append_test.go
@@ -0,0 +1,143 @@
+package actions
+
+import (
+	"encoding/json"
+	"os"
+	"path/filepath"
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/mocks"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestAppendToDailyNoteErrorsWhenNotConfigured(t *testing.T) {
+	withTempVaultAndConfig(t, func(vault *obsidian.Vault, _ string) {
+		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
+			DailyNote: obsidian.DailyNoteSettings{
+				Folder:          "",
+				FilenamePattern: "{YYYY-MM-DD}",
+			},
+		}))
+
+		err := AppendToDailyNote(vault, "hello")
+		assert.Error(t, err)
+	})
+}
+
+func TestAppendToDailyNoteCreatesAndAppends(t *testing.T) {
+	withTempVaultAndConfig(t, func(vault *obsidian.Vault, vaultPath string) {
+		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
+			DailyNote: obsidian.DailyNoteSettings{
+				Folder:          "Daily",
+				FilenamePattern: "{YYYY-MM-DD}",
+			},
+		}))
+
+		assert.NoError(t, AppendToDailyNote(vault, "first line"))
+		assert.NoError(t, AppendToDailyNote(vault, "second line"))
+
+		filename := time.Now().Format("2006-01-02") + ".md"
+		noteAbs := filepath.Join(vaultPath, "Daily", filename)
+		b, err := os.ReadFile(noteAbs)
+		assert.NoError(t, err)
+		assert.Contains(t, string(b), "first line\n\nsecond line\n")
+	})
+}
+
+func TestAppendToDailyNoteAppliesTemplateOnCreate(t *testing.T) {
+	withTempVaultAndConfig(t, func(vault *obsidian.Vault, vaultPath string) {
+		now := time.Now()
+
+		assert.NoError(t, os.MkdirAll(filepath.Join(vaultPath, "Templates"), 0750))
+		assert.NoError(t, os.WriteFile(filepath.Join(vaultPath, "Templates", "T.md"), []byte("Title={{title}}\nDate={{date:YYYYMMDDHHmmss}}\n"), 0600))
+
+		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
+			DailyNote: obsidian.DailyNoteSettings{
+				Folder:          "Daily",
+				FilenamePattern: "{YYYY-MM-DD}",
+				TemplatePath:    "Templates/T",
+			},
+		}))
+
+		assert.NoError(t, AppendToDailyNote(vault, "hello"))
+
+		filename := now.Format("2006-01-02") + ".md"
+		noteAbs := filepath.Join(vaultPath, "Daily", filename)
+		b, err := os.ReadFile(noteAbs)
+		assert.NoError(t, err)
+		s := string(b)
+		assert.Contains(t, s, "Title="+now.Format("2006-01-02")+"\n") // title is the note filename
+		assert.Contains(t, s, "Date=")
+		assert.Contains(t, s, "hello\n")
+	})
+}
+
+func TestAppendToDailyNoteRejectsEscapePaths(t *testing.T) {
+	withTempVaultAndConfig(t, func(vault *obsidian.Vault, _ string) {
+		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
+			DailyNote: obsidian.DailyNoteSettings{
+				Folder:          "../escape",
+				FilenamePattern: "{YYYY-MM-DD}",
+			},
+		}))
+
+		err := AppendToDailyNote(vault, "hello")
+		assert.Error(t, err)
+	})
+}
+
+func TestPlanDailyAppendSupportsTimeBasedPatterns(t *testing.T) {
+	withTempVaultAndConfig(t, func(vault *obsidian.Vault, _ string) {
+		now := time.Date(2025, 12, 25, 17, 31, 45, 0, time.UTC)
+		assert.NoError(t, vault.SetSettings(obsidian.VaultSettings{
+			DailyNote: obsidian.DailyNoteSettings{
+				Folder:          "Daily",
+				FilenamePattern: "YYYY-MM-DD_HHmmss",
+			},
+		}))
+
+		plan, err := PlanDailyAppend(vault, now)
+		assert.NoError(t, err)
+		assert.Equal(t, "2025-12-25_173145", plan.Filename)
+	})
+}
+
+func withTempVaultAndConfig(t *testing.T, fn func(vault *obsidian.Vault, vaultPath string)) {
+	t.Helper()
+
+	originalObsidianConfigFile := obsidian.ObsidianConfigFile
+	t.Cleanup(func() { obsidian.ObsidianConfigFile = originalObsidianConfigFile })
+	mockObsidianConfigFile := mocks.CreateMockObsidianConfigFile(t)
+	obsidian.ObsidianConfigFile = func() (string, error) {
+		return mockObsidianConfigFile, nil
+	}
+
+	originalCliConfigPath := obsidian.CliConfigPath
+	t.Cleanup(func() { obsidian.CliConfigPath = originalCliConfigPath })
+	mockCliConfigDir, mockCliConfigFile := mocks.CreateMockCliConfigDirectories(t)
+	obsidian.CliConfigPath = func() (string, string, error) {
+		return mockCliConfigDir, mockCliConfigFile, nil
+	}
+
+	vaultName := "Example Vault"
+	vaultPath := filepath.Join(t.TempDir(), vaultName)
+	assert.NoError(t, os.MkdirAll(vaultPath, 0750))
+
+	obsConfig := obsidian.ObsidianVaultConfig{
+		Vaults: map[string]struct {
+			Path string `json:"path"`
+		}{
+			vaultName: {Path: vaultPath},
+		},
+	}
+	obsBytes, err := json.Marshal(obsConfig)
+	assert.NoError(t, err)
+	assert.NoError(t, os.WriteFile(mockObsidianConfigFile, obsBytes, 0600))
+
+	vault := obsidian.Vault{}
+	assert.NoError(t, vault.SetDefaultName(vaultName))
+
+	fn(&vault, vaultPath)
+}
diff --git a/pkg/actions/daily.go b/pkg/actions/daily.go
index d45914e..1b3f3b9 100644
--- a/pkg/actions/daily.go
+++ b/pkg/actions/daily.go
@@ -4,15 +4,22 @@ import (
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 )
 
-func DailyNote(vault obsidian.VaultManager, uri obsidian.UriManager) error {
+func PlanDailyNote(vault obsidian.VaultManager, uri obsidian.UriManager) (string, error) {
 	vaultName, err := vault.DefaultName()
 	if err != nil {
-		return err
+		return "", err
 	}
 
-	obsidianUri := uri.Construct(OnsDailyUrl, map[string]string{
+	return uri.Construct(OnsDailyUrl, map[string]string{
 		"vault": vaultName,
-	})
+	}), nil
+}
+
+func DailyNote(vault obsidian.VaultManager, uri obsidian.UriManager) error {
+	obsidianUri, err := PlanDailyNote(vault, uri)
+	if err != nil {
+		return err
+	}
 
 	err = uri.Execute(obsidianUri)
 	if err != nil {
diff --git a/pkg/actions/delete.go b/pkg/actions/delete.go
index 7fe630c..d079f89 100644
--- a/pkg/actions/delete.go
+++ b/pkg/actions/delete.go
@@ -1,29 +1,87 @@
 package actions
 
 import (
+	"bufio"
+	"fmt"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"os"
 	"path/filepath"
+	"strings"
 )
 
 type DeleteParams struct {
 	NotePath string
+	Force    bool
 }
 
-func DeleteNote(vault obsidian.VaultManager, note obsidian.NoteManager, params DeleteParams) error {
-	_, err := vault.DefaultName()
+type DeletePlan struct {
+	VaultName        string
+	VaultPath        string
+	RelativeNotePath string
+	AbsoluteNotePath string
+}
+
+func PlanDeleteNote(vault obsidian.VaultManager, params DeleteParams) (DeletePlan, error) {
+	vaultName, err := vault.DefaultName()
 	if err != nil {
-		return err
+		return DeletePlan{}, err
 	}
 
 	vaultPath, err := vault.Path()
+	if err != nil {
+		return DeletePlan{}, err
+	}
+
+	rel := filepath.ToSlash(params.NotePath)
+	abs, err := obsidian.SafeJoinVaultPath(vaultPath, rel)
+	if err != nil {
+		return DeletePlan{}, err
+	}
+
+	return DeletePlan{
+		VaultName:        vaultName,
+		VaultPath:        vaultPath,
+		RelativeNotePath: rel,
+		AbsoluteNotePath: abs,
+	}, nil
+}
+
+func DeleteNote(vault obsidian.VaultManager, note obsidian.NoteManager, params DeleteParams) error {
+	plan, err := PlanDeleteNote(vault, params)
 	if err != nil {
 		return err
 	}
-	notePath := filepath.Join(vaultPath, params.NotePath)
 
-	err = note.Delete(notePath)
+	if !params.Force {
+		links, err := obsidian.FindIncomingLinks(plan.VaultPath, plan.RelativeNotePath)
+		if err != nil {
+			fmt.Fprintf(os.Stderr, "warning: could not check for incoming links: %v\n", err)
+		} else if len(links) > 0 {
+			fmt.Printf("This note is linked from %d other note(s):\n", len(links))
+			for _, link := range links {
+				fmt.Printf("  - %s\n", link.SourcePath)
+			}
+			if !confirmDelete() {
+				fmt.Println("Delete cancelled.")
+				return nil
+			}
+		}
+	}
+
+	err = note.Delete(plan.AbsoluteNotePath)
 	if err != nil {
 		return err
 	}
 	return nil
 }
+
+func confirmDelete() bool {
+	reader := bufio.NewReader(os.Stdin)
+	fmt.Print("Delete anyway? (y/N): ")
+	response, err := reader.ReadString('\n')
+	if err != nil {
+		return false
+	}
+	response = strings.TrimSpace(strings.ToLower(response))
+	return response == "y" || response == "yes"
+}
diff --git a/pkg/actions/delete_test.go b/pkg/actions/delete_test.go
index 9f5c696..f613740 100644
--- a/pkg/actions/delete_test.go
+++ b/pkg/actions/delete_test.go
@@ -59,4 +59,11 @@ func TestDeleteNote(t *testing.T) {
 		// Assert
 		assert.Equal(t, note.DeleteErr, err)
 	})
+
+	t.Run("rejects note paths that escape the vault", func(t *testing.T) {
+		err := actions.DeleteNote(&mocks.MockVaultOperator{}, &mocks.MockNoteManager{}, actions.DeleteParams{
+			NotePath: "../escape",
+		})
+		assert.Error(t, err)
+	})
 }
diff --git a/pkg/actions/frontmatter.go b/pkg/actions/frontmatter.go
new file mode 100644
index 0000000..c3eacb9
--- /dev/null
+++ b/pkg/actions/frontmatter.go
@@ -0,0 +1,111 @@
+package actions
+
+import (
+	"errors"
+	"fmt"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/frontmatter"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+)
+
+type FrontmatterParams struct {
+	NoteName string
+	Print    bool
+	Edit     bool
+	Delete   bool
+	Key      string
+	Value    string
+}
+
+// Frontmatter handles viewing and modifying note frontmatter.
+// Based on flags, it will print, edit, or delete frontmatter keys.
+func Frontmatter(vault obsidian.VaultManager, note obsidian.NoteManager, params FrontmatterParams) (string, error) {
+	_, err := vault.DefaultName()
+	if err != nil {
+		return "", err
+	}
+
+	vaultPath, err := vault.Path()
+	if err != nil {
+		return "", err
+	}
+
+	contents, err := note.GetContents(vaultPath, params.NoteName)
+	if err != nil {
+		return "", err
+	}
+
+	// Handle print operation
+	if params.Print {
+		return handlePrint(contents)
+	}
+
+	// Handle edit operation
+	if params.Edit {
+		return handleEdit(note, vaultPath, params.NoteName, contents, params.Key, params.Value)
+	}
+
+	// Handle delete operation
+	if params.Delete {
+		return handleDelete(note, vaultPath, params.NoteName, contents, params.Key)
+	}
+
+	return "", errors.New("no operation specified: use --print, --edit, or --delete")
+}
+
+func handlePrint(contents string) (string, error) {
+	if !frontmatter.HasFrontmatter(contents) {
+		return "", nil // Return empty for notes without frontmatter
+	}
+
+	fm, _, err := frontmatter.Parse(contents)
+	if err != nil {
+		return "", err
+	}
+
+	formatted, err := frontmatter.Format(fm)
+	if err != nil {
+		return "", err
+	}
+
+	return formatted, nil
+}
+
+func handleEdit(note obsidian.NoteManager, vaultPath, noteName, contents, key, value string) (string, error) {
+	if key == "" {
+		return "", errors.New("--key is required for edit operation")
+	}
+	if value == "" {
+		return "", errors.New("--value is required for edit operation")
+	}
+
+	updatedContent, err := frontmatter.SetKey(contents, key, value)
+	if err != nil {
+		return "", err
+	}
+
+	err = note.SetContents(vaultPath, noteName, updatedContent)
+	if err != nil {
+		return "", err
+	}
+
+	return fmt.Sprintf("Updated frontmatter key '%s' in %s", key, noteName), nil
+}
+
+func handleDelete(note obsidian.NoteManager, vaultPath, noteName, contents, key string) (string, error) {
+	if key == "" {
+		return "", errors.New("--key is required for delete operation")
+	}
+
+	updatedContent, err := frontmatter.DeleteKey(contents, key)
+	if err != nil {
+		return "", err
+	}
+
+	err = note.SetContents(vaultPath, noteName, updatedContent)
+	if err != nil {
+		return "", err
+	}
+
+	return fmt.Sprintf("Deleted frontmatter key '%s' from %s", key, noteName), nil
+}
diff --git a/pkg/actions/frontmatter_test.go b/pkg/actions/frontmatter_test.go
new file mode 100644
index 0000000..0d5231f
--- /dev/null
+++ b/pkg/actions/frontmatter_test.go
@@ -0,0 +1,237 @@
+package actions_test
+
+import (
+	"errors"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/mocks"
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestFrontmatter_Print(t *testing.T) {
+	t.Run("Print frontmatter successfully", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test Note\ntags:\n  - a\n  - b\n---\nBody content",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "title: Test Note")
+		assert.Contains(t, output, "tags:")
+	})
+
+	t.Run("Print empty for note without frontmatter", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "Just body content without frontmatter",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.NoError(t, err)
+		assert.Empty(t, output)
+	})
+
+	t.Run("Vault error propagates", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{
+			DefaultNameErr: errors.New("vault error"),
+		}
+		note := mocks.MockNoteManager{}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.Error(t, err)
+	})
+
+	t.Run("Note error propagates", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			GetContentsError: errors.New("note not found"),
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Print:    true,
+		})
+
+		assert.Error(t, err)
+	})
+}
+
+func TestFrontmatter_Edit(t *testing.T) {
+	t.Run("Edit existing frontmatter key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Old Title\n---\nBody",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+			Value:    "New Title",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Updated frontmatter key 'title'")
+	})
+
+	t.Run("Add new frontmatter key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "author",
+			Value:    "John",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Updated frontmatter key 'author'")
+	})
+
+	t.Run("Create frontmatter when none exists", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "Just body content",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+			Value:    "New Note",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Updated frontmatter key 'title'")
+	})
+
+	t.Run("Edit requires key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Value:    "value",
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "--key is required")
+	})
+
+	t.Run("Edit requires value", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "--value is required")
+	})
+
+	t.Run("Write error propagates", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents:         "---\ntitle: Test\n---\nBody",
+			SetContentsError: errors.New("write error"),
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Edit:     true,
+			Key:      "title",
+			Value:    "New",
+		})
+
+		assert.Error(t, err)
+	})
+}
+
+func TestFrontmatter_Delete(t *testing.T) {
+	t.Run("Delete existing key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\nauthor: John\n---\nBody",
+		}
+
+		output, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Delete:   true,
+			Key:      "author",
+		})
+
+		assert.NoError(t, err)
+		assert.Contains(t, output, "Deleted frontmatter key 'author'")
+	})
+
+	t.Run("Delete requires key", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Delete:   true,
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "--key is required")
+	})
+
+	t.Run("Delete from note without frontmatter returns error", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "Just body content",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+			Delete:   true,
+			Key:      "title",
+		})
+
+		assert.Error(t, err)
+	})
+}
+
+func TestFrontmatter_NoOperation(t *testing.T) {
+	t.Run("Returns error when no operation specified", func(t *testing.T) {
+		vault := mocks.MockVaultOperator{Name: "myVault"}
+		note := mocks.MockNoteManager{
+			Contents: "---\ntitle: Test\n---\nBody",
+		}
+
+		_, err := actions.Frontmatter(&vault, &note, actions.FrontmatterParams{
+			NoteName: "test-note",
+		})
+
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "no operation specified")
+	})
+}
diff --git a/pkg/actions/search_content_test.go b/pkg/actions/search_content_test.go
index 30ecc1e..9c9c547 100644
--- a/pkg/actions/search_content_test.go
+++ b/pkg/actions/search_content_test.go
@@ -14,11 +14,12 @@ import (
 // CustomMockNoteForSingleMatch returns exactly one match for editor testing
 type CustomMockNoteForSingleMatch struct{}
 
-func (m *CustomMockNoteForSingleMatch) Delete(string) error { return nil }
-func (m *CustomMockNoteForSingleMatch) Move(string, string) error { return nil }
-func (m *CustomMockNoteForSingleMatch) UpdateLinks(string, string, string) error { return nil }
+func (m *CustomMockNoteForSingleMatch) Delete(string) error                       { return nil }
+func (m *CustomMockNoteForSingleMatch) Move(string, string) error                  { return nil }
+func (m *CustomMockNoteForSingleMatch) UpdateLinks(string, string, string) error   { return nil }
 func (m *CustomMockNoteForSingleMatch) GetContents(string, string) (string, error) { return "", nil }
-func (m *CustomMockNoteForSingleMatch) GetNotesList(string) ([]string, error) { return nil, nil }
+func (m *CustomMockNoteForSingleMatch) SetContents(string, string, string) error   { return nil }
+func (m *CustomMockNoteForSingleMatch) GetNotesList(string) ([]string, error)      { return nil, nil }
 func (m *CustomMockNoteForSingleMatch) SearchNotesWithSnippets(string, string) ([]obsidian.NoteMatch, error) {
 	return []obsidian.NoteMatch{
 		{FilePath: "test-note.md", LineNumber: 5, MatchLine: "test content"},
diff --git a/pkg/actions/target.go b/pkg/actions/target.go
new file mode 100644
index 0000000..77d4a1f
--- /dev/null
+++ b/pkg/actions/target.go
@@ -0,0 +1,94 @@
+package actions
+
+import (
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+)
+
+// AppendToTarget appends content to the note resolved by the given target.
+// If dryRun is true, it returns the computed plan without writing anything.
+func AppendToTarget(vault *obsidian.Vault, targetName string, content string, now time.Time, dryRun bool) (obsidian.TargetPlan, error) {
+	targetName = strings.TrimSpace(targetName)
+	if targetName == "" {
+		return obsidian.TargetPlan{}, errors.New("target name is required")
+	}
+	content = strings.TrimSpace(content)
+	if content == "" {
+		return obsidian.TargetPlan{}, errors.New("no content provided")
+	}
+
+	targets, err := obsidian.LoadTargets()
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+	target, ok := targets[targetName]
+	if !ok {
+		return obsidian.TargetPlan{}, fmt.Errorf("target not found: %s", targetName)
+	}
+
+	effectiveVault := vault
+	if strings.TrimSpace(target.Vault) != "" {
+		effectiveVault = &obsidian.Vault{Name: strings.TrimSpace(target.Vault)}
+	}
+
+	vaultName, err := effectiveVault.DefaultName()
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+	vaultPath, err := effectiveVault.Path()
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+
+	plan, err := obsidian.PlanTargetAppend(vaultPath, targetName, target, now)
+	if err != nil {
+		return obsidian.TargetPlan{}, err
+	}
+	plan.VaultName = vaultName
+	plan.VaultPath = vaultPath
+
+	if dryRun {
+		return plan, nil
+	}
+
+	if err := os.MkdirAll(filepath.Dir(plan.AbsoluteNotePath), 0750); err != nil {
+		return obsidian.TargetPlan{}, fmt.Errorf("failed to create note directory: %w", err)
+	}
+
+	var existing []byte
+	mode := os.FileMode(0600)
+	if info, err := os.Stat(plan.AbsoluteNotePath); err == nil {
+		mode = info.Mode()
+		b, err := os.ReadFile(plan.AbsoluteNotePath)
+		if err != nil {
+			return obsidian.TargetPlan{}, fmt.Errorf("failed to read note: %w", err)
+		}
+		existing = b
+	} else if err != nil && !os.IsNotExist(err) {
+		return obsidian.TargetPlan{}, fmt.Errorf("failed to stat note: %w", err)
+	} else {
+		existing = []byte{}
+		if plan.WillApplyTemplate {
+			templateContent, err := os.ReadFile(plan.AbsoluteTemplate)
+			if err != nil {
+				return obsidian.TargetPlan{}, fmt.Errorf("failed to read template: %w", err)
+			}
+			title := filepath.Base(plan.AbsoluteNotePath)
+			templateContent = obsidian.ExpandTemplateVariablesAt(templateContent, title, now)
+			existing = templateContent
+		}
+	}
+
+	next := appendWithSeparator(string(existing), content)
+	if err := os.WriteFile(plan.AbsoluteNotePath, []byte(next), mode); err != nil {
+		return obsidian.TargetPlan{}, fmt.Errorf("failed to write note: %w", err)
+	}
+
+	return plan, nil
+}
diff --git a/pkg/actions/target_test.go b/pkg/actions/target_test.go
new file mode 100644
index 0000000..5096953
--- /dev/null
+++ b/pkg/actions/target_test.go
@@ -0,0 +1,89 @@
+package actions_test
+
+import (
+	"os"
+	"path/filepath"
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/actions"
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestAppendToTarget_DryRunAndWrite(t *testing.T) {
+	originalUserConfigDirectory := config.UserConfigDirectory
+	defer func() { config.UserConfigDirectory = originalUserConfigDirectory }()
+
+	cfgRoot := t.TempDir()
+	config.UserConfigDirectory = func() (string, error) { return cfgRoot, nil }
+
+	cliDir, targetsFile, err := config.TargetsPath()
+	assert.NoError(t, err)
+	assert.NoError(t, os.MkdirAll(cliDir, 0750))
+	assert.NoError(t, os.WriteFile(targetsFile, []byte("inbox:\n  type: file\n  file: Inbox.md\n"), 0600))
+
+	vaultRoot := t.TempDir()
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	originalObsidianConfigFile := obsidian.ObsidianConfigFile
+	defer func() { obsidian.ObsidianConfigFile = originalObsidianConfigFile }()
+	mockCfg := `{"vaults":{"id":{"path":"` + vaultRoot + `/vault"}}}`
+	cfgFile := filepath.Join(t.TempDir(), "obsidian.json")
+	assert.NoError(t, os.WriteFile(cfgFile, []byte(mockCfg), 0644))
+	obsidian.ObsidianConfigFile = func() (string, error) { return cfgFile, nil }
+	v := &obsidian.Vault{Name: "vault"}
+
+	plan, err := actions.AppendToTarget(v, "inbox", "x", now, true)
+	assert.NoError(t, err)
+	assert.True(t, plan.WillCreateFile)
+	_, err = os.Stat(filepath.Join(vaultRoot, "vault", "Inbox.md"))
+	assert.True(t, os.IsNotExist(err))
+
+	plan, err = actions.AppendToTarget(v, "inbox", "x", now, false)
+	assert.NoError(t, err)
+	b, err := os.ReadFile(filepath.Join(vaultRoot, "vault", "Inbox.md"))
+	assert.NoError(t, err)
+	assert.Equal(t, "x\n", string(b))
+}
+
+func TestAppendToTarget_AppliesTemplateOnCreate(t *testing.T) {
+	originalUserConfigDirectory := config.UserConfigDirectory
+	defer func() { config.UserConfigDirectory = originalUserConfigDirectory }()
+
+	cfgRoot := t.TempDir()
+	config.UserConfigDirectory = func() (string, error) { return cfgRoot, nil }
+
+	vaultRoot := t.TempDir()
+
+	originalObsidianConfigFile := obsidian.ObsidianConfigFile
+	defer func() { obsidian.ObsidianConfigFile = originalObsidianConfigFile }()
+	mockCfg := `{"vaults":{"id":{"path":"` + vaultRoot + `/vault"}}}`
+	cfgFile := filepath.Join(t.TempDir(), "obsidian.json")
+	assert.NoError(t, os.WriteFile(cfgFile, []byte(mockCfg), 0644))
+	obsidian.ObsidianConfigFile = func() (string, error) { return cfgFile, nil }
+
+	cliDir, targetsFile, err := config.TargetsPath()
+	assert.NoError(t, err)
+	assert.NoError(t, os.MkdirAll(cliDir, 0750))
+
+	// Template lives in the vault.
+	assert.NoError(t, os.MkdirAll(filepath.Join(vaultRoot, "vault", "Templates"), 0750))
+	assert.NoError(t, os.WriteFile(filepath.Join(vaultRoot, "vault", "Templates", "T.md"), []byte("Title={{title}}\nDate={{date:YYYYMMDDHHmmss}}\n"), 0600))
+
+	assert.NoError(t, os.WriteFile(targetsFile, []byte("t:\n  type: file\n  file: Inbox.md\n  template: Templates/T\n"), 0600))
+
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+	v := &obsidian.Vault{Name: "vault"}
+
+	_, err = actions.AppendToTarget(v, "t", "hello", now, false)
+	assert.NoError(t, err)
+
+	b, err := os.ReadFile(filepath.Join(vaultRoot, "vault", "Inbox.md"))
+	assert.NoError(t, err)
+	s := string(b)
+	assert.Contains(t, s, "Title=Inbox")
+	assert.Contains(t, s, "Date=20240115143052")
+	assert.Contains(t, s, "hello\n")
+}
diff --git a/pkg/config/targets_path.go b/pkg/config/targets_path.go
new file mode 100644
index 0000000..7bd7166
--- /dev/null
+++ b/pkg/config/targets_path.go
@@ -0,0 +1,15 @@
+package config
+
+import "path/filepath"
+
+const ObsidianCLITargetsFile = "targets.yaml"
+
+// TargetsPath returns the directory and absolute file path for targets.yaml.
+func TargetsPath() (cliConfigDir string, targetsFile string, err error) {
+	cliConfigDir, _, err = CliPath()
+	if err != nil {
+		return "", "", err
+	}
+	targetsFile = filepath.Join(cliConfigDir, ObsidianCLITargetsFile)
+	return cliConfigDir, targetsFile, nil
+}
diff --git a/pkg/config/targets_path_test.go b/pkg/config/targets_path_test.go
new file mode 100644
index 0000000..39ebde0
--- /dev/null
+++ b/pkg/config/targets_path_test.go
@@ -0,0 +1,34 @@
+package config_test
+
+import (
+	"errors"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestTargetsPath(t *testing.T) {
+	originalUserConfigDirectory := config.UserConfigDirectory
+	defer func() { config.UserConfigDirectory = originalUserConfigDirectory }()
+
+	t.Run("Returns targets path under obsidian-cli config directory", func(t *testing.T) {
+		config.UserConfigDirectory = func() (string, error) {
+			return "user/config/dir", nil
+		}
+
+		dir, file, err := config.TargetsPath()
+		assert.NoError(t, err)
+		assert.Equal(t, "user/config/dir/obsidian-cli", dir)
+		assert.Equal(t, "user/config/dir/obsidian-cli/targets.yaml", file)
+	})
+
+	t.Run("Returns error when user config directory not found", func(t *testing.T) {
+		config.UserConfigDirectory = func() (string, error) {
+			return "", errors.New(config.UserConfigDirectoryNotFoundErrorMessage)
+		}
+
+		_, _, err := config.TargetsPath()
+		assert.Equal(t, config.UserConfigDirectoryNotFoundErrorMessage, err.Error())
+	})
+}
diff --git a/pkg/frontmatter/frontmatter.go b/pkg/frontmatter/frontmatter.go
new file mode 100644
index 0000000..43a6fc4
--- /dev/null
+++ b/pkg/frontmatter/frontmatter.go
@@ -0,0 +1,145 @@
+package frontmatter
+
+import (
+	"errors"
+	"strings"
+
+	"github.com/adrg/frontmatter"
+	"gopkg.in/yaml.v3"
+)
+
+const (
+	Delimiter              = "---"
+	NoFrontmatterError     = "note does not contain frontmatter"
+	InvalidFrontmatterError = "frontmatter contains invalid YAML"
+)
+
+// Parse extracts and parses frontmatter from note content.
+// Returns the frontmatter as a map, the body content, and any error.
+func Parse(content string) (map[string]interface{}, string, error) {
+	var fm map[string]interface{}
+	rest, err := frontmatter.Parse(strings.NewReader(content), &fm)
+	if err != nil {
+		return nil, "", errors.New(InvalidFrontmatterError)
+	}
+	return fm, string(rest), nil
+}
+
+// Format converts a frontmatter map to a YAML string.
+func Format(fm map[string]interface{}) (string, error) {
+	if fm == nil || len(fm) == 0 {
+		return "", nil
+	}
+	data, err := yaml.Marshal(fm)
+	if err != nil {
+		return "", err
+	}
+	return string(data), nil
+}
+
+// HasFrontmatter checks if content starts with frontmatter delimiters.
+func HasFrontmatter(content string) bool {
+	lines := strings.Split(content, "\n")
+	if len(lines) == 0 {
+		return false
+	}
+	return strings.TrimSpace(lines[0]) == Delimiter
+}
+
+// SetKey updates or adds a key in the frontmatter, returning the full updated content.
+// If no frontmatter exists, it creates new frontmatter with the key.
+func SetKey(content, key, value string) (string, error) {
+	parsedValue := parseValue(value)
+
+	if !HasFrontmatter(content) {
+		// Create new frontmatter
+		fm := map[string]interface{}{key: parsedValue}
+		fmStr, err := yaml.Marshal(fm)
+		if err != nil {
+			return "", err
+		}
+		return Delimiter + "\n" + string(fmStr) + Delimiter + "\n" + content, nil
+	}
+
+	// Parse existing frontmatter
+	fm, body, err := Parse(content)
+	if err != nil {
+		return "", err
+	}
+
+	if fm == nil {
+		fm = make(map[string]interface{})
+	}
+
+	// Update the key
+	fm[key] = parsedValue
+
+	// Reconstruct content
+	fmStr, err := yaml.Marshal(fm)
+	if err != nil {
+		return "", err
+	}
+
+	return Delimiter + "\n" + string(fmStr) + Delimiter + "\n" + body, nil
+}
+
+// DeleteKey removes a key from the frontmatter, returning the full updated content.
+func DeleteKey(content, key string) (string, error) {
+	if !HasFrontmatter(content) {
+		return "", errors.New(NoFrontmatterError)
+	}
+
+	fm, body, err := Parse(content)
+	if err != nil {
+		return "", err
+	}
+
+	if fm == nil {
+		return "", errors.New(NoFrontmatterError)
+	}
+
+	// Delete the key
+	delete(fm, key)
+
+	// If no keys left, return just the body
+	if len(fm) == 0 {
+		return strings.TrimPrefix(body, "\n"), nil
+	}
+
+	// Reconstruct content
+	fmStr, err := yaml.Marshal(fm)
+	if err != nil {
+		return "", err
+	}
+
+	return Delimiter + "\n" + string(fmStr) + Delimiter + "\n" + body, nil
+}
+
+// parseValue attempts to parse the value into appropriate Go types.
+// Supports: booleans, arrays (comma-separated in brackets), strings.
+func parseValue(value string) interface{} {
+	// Try boolean
+	if value == "true" {
+		return true
+	}
+	if value == "false" {
+		return false
+	}
+
+	// Try array (comma-separated in brackets)
+	if strings.HasPrefix(value, "[") && strings.HasSuffix(value, "]") {
+		inner := value[1 : len(value)-1]
+		if inner == "" {
+			return []string{}
+		}
+		parts := strings.Split(inner, ",")
+		result := make([]string, 0, len(parts))
+		for _, p := range parts {
+			result = append(result, strings.TrimSpace(p))
+		}
+		return result
+	}
+
+	// Default to string
+	return value
+}
diff --git a/pkg/frontmatter/frontmatter_test.go b/pkg/frontmatter/frontmatter_test.go
new file mode 100644
index 0000000..ab4155e
--- /dev/null
+++ b/pkg/frontmatter/frontmatter_test.go
@@ -0,0 +1,172 @@
+package frontmatter_test
+
+import (
+	"strings"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/frontmatter"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestParse(t *testing.T) {
+	t.Run("Parse valid frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\ntags:\n  - a\n  - b\n---\nBody content"
+		fm, body, err := frontmatter.Parse(content)
+		assert.NoError(t, err)
+		assert.Equal(t, "Test", fm["title"])
+		assert.Equal(t, "Body content", body)
+	})
+
+	t.Run("Parse empty frontmatter", func(t *testing.T) {
+		content := "---\n---\nBody content"
+		fm, body, err := frontmatter.Parse(content)
+		assert.NoError(t, err)
+		assert.Empty(t, fm)
+		assert.Equal(t, "Body content", body)
+	})
+
+	t.Run("No frontmatter returns empty map", func(t *testing.T) {
+		content := "Just body content"
+		fm, body, err := frontmatter.Parse(content)
+		assert.NoError(t, err)
+		assert.Empty(t, fm)
+		assert.Equal(t, "Just body content", body)
+	})
+
+	t.Run("Invalid YAML returns error", func(t *testing.T) {
+		content := "---\ninvalid: [unclosed\n---\nBody"
+		_, _, err := frontmatter.Parse(content)
+		assert.Error(t, err)
+	})
+}
+
+func TestHasFrontmatter(t *testing.T) {
+	t.Run("Has frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		assert.True(t, frontmatter.HasFrontmatter(content))
+	})
+
+	t.Run("No frontmatter", func(t *testing.T) {
+		content := "Just body content"
+		assert.False(t, frontmatter.HasFrontmatter(content))
+	})
+
+	t.Run("Empty content", func(t *testing.T) {
+		assert.False(t, frontmatter.HasFrontmatter(""))
+	})
+}
+
+func TestFormat(t *testing.T) {
+	t.Run("Format valid map", func(t *testing.T) {
+		fm := map[string]interface{}{
+			"title": "Test",
+		}
+		result, err := frontmatter.Format(fm)
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: Test")
+	})
+
+	t.Run("Format empty map", func(t *testing.T) {
+		result, err := frontmatter.Format(map[string]interface{}{})
+		assert.NoError(t, err)
+		assert.Empty(t, result)
+	})
+
+	t.Run("Format nil map", func(t *testing.T) {
+		result, err := frontmatter.Format(nil)
+		assert.NoError(t, err)
+		assert.Empty(t, result)
+	})
+}
+
+func TestSetKey(t *testing.T) {
+	t.Run("Add key to existing frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		result, err := frontmatter.SetKey(content, "author", "John")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "author: John")
+		assert.Contains(t, result, "title: Test")
+		assert.Contains(t, result, "Body")
+	})
+
+	t.Run("Update existing key", func(t *testing.T) {
+		content := "---\ntitle: Old\n---\nBody"
+		result, err := frontmatter.SetKey(content, "title", "New")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: New")
+		assert.NotContains(t, result, "title: Old")
+	})
+
+	t.Run("Create frontmatter when none exists", func(t *testing.T) {
+		content := "Just body content"
+		result, err := frontmatter.SetKey(content, "title", "New")
+		assert.NoError(t, err)
+		assert.True(t, strings.HasPrefix(result, "---\n"))
+		assert.Contains(t, result, "title: New")
+		assert.Contains(t, result, "Just body content")
+	})
+
+	t.Run("Parse boolean value true", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "draft", "true")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "draft: true")
+	})
+
+	t.Run("Parse boolean value false", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "published", "false")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "published: false")
+	})
+
+	t.Run("Parse array value", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "tags", "[one, two, three]")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "tags:")
+		assert.Contains(t, result, "- one")
+		assert.Contains(t, result, "- two")
+		assert.Contains(t, result, "- three")
+	})
+
+	t.Run("Parse empty array value", func(t *testing.T) {
+		content := "---\n---\nBody"
+		result, err := frontmatter.SetKey(content, "tags", "[]")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "tags: []")
+	})
+}
+
+func TestDeleteKey(t *testing.T) {
+	t.Run("Delete existing key", func(t *testing.T) {
+		content := "---\ntitle: Test\nauthor: John\n---\nBody"
+		result, err := frontmatter.DeleteKey(content, "author")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: Test")
+		assert.NotContains(t, result, "author")
+		assert.Contains(t, result, "Body")
+	})
+
+	t.Run("Delete last key removes frontmatter", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		result, err := frontmatter.DeleteKey(content, "title")
+		assert.NoError(t, err)
+		assert.False(t, strings.HasPrefix(result, "---"))
+		assert.Contains(t, result, "Body")
+	})
+
+	t.Run("Delete from no frontmatter returns error", func(t *testing.T) {
+		content := "Just body content"
+		_, err := frontmatter.DeleteKey(content, "title")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "does not contain frontmatter")
+	})
+
+	t.Run("Delete non-existent key succeeds", func(t *testing.T) {
+		content := "---\ntitle: Test\n---\nBody"
+		result, err := frontmatter.DeleteKey(content, "nonexistent")
+		assert.NoError(t, err)
+		assert.Contains(t, result, "title: Test")
+	})
+}
diff --git a/pkg/obsidian/date_format.go b/pkg/obsidian/date_format.go
new file mode 100644
index 0000000..de93737
--- /dev/null
+++ b/pkg/obsidian/date_format.go
@@ -0,0 +1,164 @@
+package obsidian
+
+import (
+	"fmt"
+	"strings"
+	"time"
+)
+
+// ExpandDatePattern expands an Obsidian-style date format string using the provided time.
+//
+// It supports:
+//   - Obsidian/Moment.js-style tokens (curated subset): YYYY, YY, MMMM, MMM, MM, M, DD, D,
+//     dddd, ddd, HH, H, hh, h, mm, m, ss, s, A, a, ZZ, Z, z
+//   - Literal blocks wrapped in square brackets, e.g. YYYY-[ToDo]-MM
+//   - Legacy "token brace" patterns used in this repo, e.g. {YYYY-MM-DD-HHmmss}
+//   - The "zettel" style: YYYYMMDDHHmmss (with or without braces)
+//
+// Unknown characters are treated literally.
+func ExpandDatePattern(pattern string, now time.Time) string {
+	s, _ := FormatDatePattern(pattern, now)
+	return s
+}
+
+// FormatDatePattern is like ExpandDatePattern, but returns an error if the pattern is empty.
+func FormatDatePattern(pattern string, now time.Time) (string, error) {
+	pattern = strings.TrimSpace(pattern)
+	if pattern == "" {
+		return "", fmt.Errorf("pattern is empty")
+	}
+
+	cleaned, literals := stripBracesAndCaptureLiterals(pattern)
+	goFormat := convertObsidianFormatToGo(cleaned)
+	formatted := now.Format(goFormat)
+
+	for placeholder, literal := range literals {
+		formatted = strings.ReplaceAll(formatted, placeholder, literal)
+	}
+
+	return formatted, nil
+}
+
+func stripBracesAndCaptureLiterals(pattern string) (cleaned string, literals map[string]string) {
+	var b strings.Builder
+	literals = map[string]string{}
+
+	inBracketLiteral := false
+	var literal strings.Builder
+	literalIndex := 0
+
+	for _, r := range pattern {
+		switch r {
+		case '[':
+			if inBracketLiteral {
+				literal.WriteRune(r)
+				continue
+			}
+			inBracketLiteral = true
+			literal.Reset()
+		case ']':
+			if !inBracketLiteral {
+				b.WriteRune(r)
+				continue
+			}
+			inBracketLiteral = false
+			placeholder := fmt.Sprintf("\x00LIT%d\x00", literalIndex)
+			literalIndex++
+			literals[placeholder] = literal.String()
+			b.WriteString(placeholder)
+		case '{', '}':
+			if inBracketLiteral {
+				literal.WriteRune(r)
+			}
+			// Treat braces as formatting sugar (legacy patterns) and ignore them.
+		default:
+			if inBracketLiteral {
+				literal.WriteRune(r)
+				continue
+			}
+			b.WriteRune(r)
+		}
+	}
+
+	// If a literal block was opened but never closed, treat it literally (keep the '[').
+	if inBracketLiteral {
+		b.WriteRune('[')
+		b.WriteString(literal.String())
+	}
+
+	return b.String(), literals
+}
+
+func convertObsidianFormatToGo(format string) string {
+	// Curated Obsidian/Moment tokens mapped to Go time format.
+	tokenMap := map[string]string{
+		// Year
+		"YYYY": "2006",
+		"YY":   "06",
+
+		// Month
+		"MMMM": "January",
+		"MMM":  "Jan",
+		"MM":   "01",
+		"M":    "1",
+
+		// Day of month
+		"DD": "02",
+		"D":  "2",
+
+		// Weekday
+		"dddd": "Monday",
+		"ddd":  "Mon",
+
+		// Hour
+		"HH": "15",
+		"H":  "15",
+		"hh": "03",
+		"h":  "3",
+
+		// Minute
+		"mm": "04",
+		"m":  "4",
+
+		// Second
+		"ss": "05",
+		"s":  "5",
+
+		// AM/PM
+		"A": "PM",
+		"a": "pm",
+
+		// Timezone
+		"ZZ": "-0700",
+		"Z":  "-07:00",
+		"z":  "MST",
+	}
+
+	orderedTokens := []string{
+		"YYYY", "MMMM", "dddd",
+		"MMM", "ddd",
+		"YY", "MM", "DD", "HH", "hh", "mm", "ss", "ZZ",
+		"M", "D", "H", "h", "m", "s", "A", "a", "Z", "z",
+	}
+
+	var out strings.Builder
+	i := 0
+	for i < len(format) {
+		matched := false
+		for _, tok := range orderedTokens {
+			if i+len(tok) <= len(format) && format[i:i+len(tok)] == tok {
+				out.WriteString(tokenMap[tok])
+				i += len(tok)
+				matched = true
+				break
+			}
+		}
+		if matched {
+			continue
+		}
+		out.WriteByte(format[i])
+		i++
+	}
+	return out.String()
+}
+
diff --git a/pkg/obsidian/date_format_test.go b/pkg/obsidian/date_format_test.go
new file mode 100644
index 0000000..fdb0b73
--- /dev/null
+++ b/pkg/obsidian/date_format_test.go
@@ -0,0 +1,44 @@
+package obsidian_test
+
+import (
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestExpandDatePattern(t *testing.T) {
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	cases := []struct {
+		name    string
+		pattern string
+		want    string
+	}{
+		{"brace date", "{YYYY-MM-DD}", "2024-01-15"},
+		{"plain date", "YYYY-MM-DD", "2024-01-15"},
+		{"brace datetime minutes", "{YYYY-MM-DD-HHmm}", "2024-01-15-1430"},
+		{"brace datetime seconds", "{YYYY-MM-DD-HHmmss}", "2024-01-15-143052"},
+		{"zettel braced", "{YYYYMMDDHHmmss}", "20240115143052"},
+		{"zettel plain", "YYYYMMDDHHmmss", "20240115143052"},
+		{"weekday", "dddd", "Monday"},
+		{"month name", "MMMM", "January"},
+		{"month abbrev", "MMM", "Jan"},
+		{"bracket literal", "YYYY-[ToDo]-MM", "2024-ToDo-01"},
+		{"bracket literal preserved", "[Mon]-YYYY", "Mon-2024"},
+		{"braces + literal", "{YYYY}-[Mon]-{MM}", "2024-Mon-01"},
+	}
+
+	for _, tc := range cases {
+		t.Run(tc.name, func(t *testing.T) {
+			assert.Equal(t, tc.want, obsidian.ExpandDatePattern(tc.pattern, now))
+		})
+	}
+}
+
+func TestFormatDatePatternErrorsOnEmpty(t *testing.T) {
+	_, err := obsidian.FormatDatePattern("   ", time.Now())
+	assert.Error(t, err)
+}
+
diff --git a/pkg/obsidian/note.go b/pkg/obsidian/note.go
index 286c06d..f9f5f49 100644
--- a/pkg/obsidian/note.go
+++ b/pkg/obsidian/note.go
@@ -25,6 +25,7 @@ type NoteManager interface {
 	Delete(string) error
 	UpdateLinks(string, string, string) error
 	GetContents(string, string) (string, error)
+	SetContents(string, string, string) error
 	GetNotesList(string) ([]string, error)
 	SearchNotesWithSnippets(string, string) ([]NoteMatch, error)
 }
@@ -101,6 +102,45 @@ func (m *Note) GetContents(vaultPath string, noteName string) (string, error) {
 	return string(content), nil
 }
 
+func (m *Note) SetContents(vaultPath string, noteName string, content string) error {
+	note := AddMdSuffix(noteName)
+
+	var notePath string
+	err := filepath.WalkDir(vaultPath, func(path string, d os.DirEntry, err error) error {
+		if err != nil {
+			return err
+		}
+		if d.IsDir() {
+			return nil
+		}
+
+		// Check for full path match first
+		relPath, err := filepath.Rel(vaultPath, path)
+		if err == nil && relPath == note {
+			notePath = path
+			return filepath.SkipDir
+		}
+
+		// Fall back to basename match for backward compatibility
+		if filepath.Base(path) == note {
+			notePath = path
+			return filepath.SkipDir
+		}
+		return nil
+	})
+
+	if err != nil || notePath == "" {
+		return errors.New(NoteDoesNotExistError)
+	}
+
+	err = os.WriteFile(notePath, []byte(content), 0644)
+	if err != nil {
+		return errors.New(VaultWriteError)
+	}
+
+	return nil
+}
+
 func (m *Note) UpdateLinks(vaultPath string, oldNoteName string, newNoteName string) error {
 	err := filepath.Walk(vaultPath, func(path string, info os.FileInfo, err error) error {
 		if err != nil {
@@ -116,14 +156,8 @@ func (m *Note) UpdateLinks(vaultPath string, oldNoteName string, newNoteName str
 			return errors.New(VaultReadError)
 		}
 
-		oldNoteLinkTexts := GenerateNoteLinkTexts(oldNoteName)
-		newNoteLinkTexts := GenerateNoteLinkTexts(newNoteName)
-
-		updatedContent := ReplaceContent(originalContent, map[string]string{
-			oldNoteLinkTexts[0]: newNoteLinkTexts[0],
-			oldNoteLinkTexts[1]: newNoteLinkTexts[1],
-			oldNoteLinkTexts[2]: newNoteLinkTexts[2],
-		})
+		replacements := GenerateLinkReplacements(oldNoteName, newNoteName)
+		updatedContent := ReplaceContent(originalContent, replacements)
 
 		if bytes.Equal(originalContent, updatedContent) {
 			return nil
diff --git a/pkg/obsidian/note_test.go b/pkg/obsidian/note_test.go
index c5e6d0c..e599bcb 100644
--- a/pkg/obsidian/note_test.go
+++ b/pkg/obsidian/note_test.go
@@ -283,6 +283,84 @@ func TestUpdateNoteLinks(t *testing.T) {
 	})
 }
 
+func TestUpdateNoteLinks_PathBasedWikilinks(t *testing.T) {
+	t.Run("Update path-based wikilinks", func(t *testing.T) {
+		tmpDir := t.TempDir()
+
+		// Create a file with path-based wikilinks
+		content := []byte("Link to [[folder/oldNote]] and [[folder/oldNote#section]] and [[folder/oldNote|alias]]")
+		testFile := filepath.Join(tmpDir, "test.md")
+		err := os.WriteFile(testFile, content, 0644)
+		assert.NoError(t, err)
+
+		noteManager := obsidian.Note{}
+		err = noteManager.UpdateLinks(tmpDir, "folder/oldNote", "folder/newNote")
+		assert.NoError(t, err)
+
+		newContent, _ := os.ReadFile(testFile)
+		contentStr := string(newContent)
+		assert.Contains(t, contentStr, "[[folder/newNote]]")
+		assert.Contains(t, contentStr, "[[folder/newNote#section]]")
+		assert.Contains(t, contentStr, "[[folder/newNote|alias]]")
+	})
+}
+
+func TestUpdateNoteLinks_MarkdownLinks(t *testing.T) {
+	t.Run("Update markdown links", func(t *testing.T) {
+		tmpDir := t.TempDir()
+
+		// Create a file with markdown links
+		content := []byte("Link [here](folder/oldNote.md) and [also](./folder/oldNote.md) and [no ext](folder/oldNote)")
+		testFile := filepath.Join(tmpDir, "test.md")
+		err := os.WriteFile(testFile, content, 0644)
+		assert.NoError(t, err)
+
+		noteManager := obsidian.Note{}
+		err = noteManager.UpdateLinks(tmpDir, "folder/oldNote", "folder/newNote")
+		assert.NoError(t, err)
+
+		newContent, _ := os.ReadFile(testFile)
+		contentStr := string(newContent)
+		assert.Contains(t, contentStr, "(folder/newNote.md)")
+		assert.Contains(t, contentStr, "(./folder/newNote.md)")
+		assert.Contains(t, contentStr, "(folder/newNote)")
+	})
+}
+
+func TestUpdateNoteLinks_MixedFormats(t *testing.T) {
+	t.Run("Update mixed link formats in same file", func(t *testing.T) {
+		tmpDir := t.TempDir()
+
+		// Create a file with both wikilinks and markdown links
+		content := []byte(`# Mixed Links
+- Wikilink: [[folder/oldNote]]
+- Wikilink with alias: [[folder/oldNote|click here]]
+- Wikilink with section: [[folder/oldNote#intro]]
+- Simple wikilink: [[oldNote]]
+- Markdown link: [text](folder/oldNote.md)
+- Relative markdown: [text](./folder/oldNote.md)
+`)
+		testFile := filepath.Join(tmpDir, "test.md")
+		err := os.WriteFile(testFile, content, 0644)
+		assert.NoError(t, err)
+
+		noteManager := obsidian.Note{}
+		err = noteManager.UpdateLinks(tmpDir, "folder/oldNote", "newFolder/newNote")
+		assert.NoError(t, err)
+
+		newContent, _ := os.ReadFile(testFile)
+		contentStr := string(newContent)
+
+		// All links should be updated
+		assert.Contains(t, contentStr, "[[newFolder/newNote]]")
+		assert.Contains(t, contentStr, "[[newFolder/newNote|click here]]")
+		assert.Contains(t, contentStr, "[[newFolder/newNote#intro]]")
+		assert.Contains(t, contentStr, "[[newNote]]")
+		assert.Contains(t, contentStr, "(newFolder/newNote.md)")
+		assert.Contains(t, contentStr, "(./newFolder/newNote.md)")
+	})
+}
+
 func TestUpdateLinks_PreservesTimestamps(t *testing.T) {
 	t.Run("Only writes files with actual link changes", func(t *testing.T) {
 		// Arrange
diff --git a/pkg/obsidian/path_safety.go b/pkg/obsidian/path_safety.go
new file mode 100644
index 0000000..50840e9
--- /dev/null
+++ b/pkg/obsidian/path_safety.go
@@ -0,0 +1,37 @@
+package obsidian
+
+import (
+	"fmt"
+	"path/filepath"
+	"strings"
+)
+
+// SafeJoinVaultPath joins a vault root and a relative note path and ensures the result stays within the vault.
+func SafeJoinVaultPath(vaultPath string, relativePath string) (string, error) {
+	if filepath.IsAbs(relativePath) {
+		return "", fmt.Errorf("absolute paths are not allowed: %s", relativePath)
+	}
+	cleaned := filepath.Clean(strings.TrimSpace(relativePath))
+	cleaned = strings.TrimPrefix(cleaned, string(filepath.Separator))
+	cleaned = strings.TrimPrefix(cleaned, "./")
+	if cleaned == "" || cleaned == "." {
+		return "", fmt.Errorf("note path cannot be empty")
+	}
+
+	absVault, err := filepath.Abs(vaultPath)
+	if err != nil {
+		return "", fmt.Errorf("failed to resolve vault path: %w", err)
+	}
+
+	joined := filepath.Join(absVault, filepath.FromSlash(cleaned))
+	absJoined, err := filepath.Abs(joined)
+	if err != nil {
+		return "", fmt.Errorf("failed to resolve note path: %w", err)
+	}
+
+	if absJoined != absVault && !strings.HasPrefix(absJoined, absVault+string(filepath.Separator)) {
+		return "", fmt.Errorf("note path escapes vault: %s", relativePath)
+	}
+
+	return absJoined, nil
+}
diff --git a/pkg/obsidian/path_safety_test.go b/pkg/obsidian/path_safety_test.go
new file mode 100644
index 0000000..45b604b
--- /dev/null
+++ b/pkg/obsidian/path_safety_test.go
@@ -0,0 +1,37 @@
+package obsidian_test
+
+import (
+	"path/filepath"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestSafeJoinVaultPath(t *testing.T) {
+	vault := t.TempDir()
+
+	t.Run("Rejects absolute paths", func(t *testing.T) {
+		_, err := obsidian.SafeJoinVaultPath(vault, string(filepath.Separator)+"abs.md")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "absolute paths are not allowed:")
+	})
+
+	t.Run("Rejects empty paths", func(t *testing.T) {
+		_, err := obsidian.SafeJoinVaultPath(vault, "  ")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "note path cannot be empty")
+	})
+
+	t.Run("Rejects escape paths", func(t *testing.T) {
+		_, err := obsidian.SafeJoinVaultPath(vault, "../escape.md")
+		assert.Error(t, err)
+		assert.Contains(t, err.Error(), "note path escapes vault:")
+	})
+
+	t.Run("Allows normal relative paths", func(t *testing.T) {
+		got, err := obsidian.SafeJoinVaultPath(vault, "Folder/Note.md")
+		assert.NoError(t, err)
+		assert.Contains(t, got, filepath.Join(vault, "Folder", "Note.md"))
+	})
+}
diff --git a/pkg/obsidian/targets.go b/pkg/obsidian/targets.go
new file mode 100644
index 0000000..62a1304
--- /dev/null
+++ b/pkg/obsidian/targets.go
@@ -0,0 +1,298 @@
+package obsidian
+
+import (
+	"errors"
+	"fmt"
+	"os"
+	"path/filepath"
+	"sort"
+	"strings"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/config"
+	"gopkg.in/yaml.v3"
+)
+
+// Target represents a named capture target.
+// Targets are configured in targets.yaml stored next to preferences.json.
+type Target struct {
+	Type     string `yaml:"type,omitempty"`           // "folder" or "file"
+	Folder   string `yaml:"folder,omitempty"`         // for folder targets
+	Pattern  string `yaml:"pattern,omitempty"`        // for folder targets (Obsidian-style or legacy tokens)
+	Template string `yaml:"template,omitempty"`       // optional template path
+	File     string `yaml:"file,omitempty"`           // for file targets
+	Note     string `yaml:"note,omitempty"`           // legacy/simplified key (treated as file)
+	Vault    string `yaml:"vault,omitempty"`          // optional: override default vault
+	Format   string `yaml:"pattern_format,omitempty"` // "auto" (default), "obsidian", or "tokens"
+}
+
+func (t *Target) UnmarshalYAML(value *yaml.Node) error {
+	switch value.Kind {
+	case yaml.ScalarNode:
+		// Simplified format: id: "Some/Path" → treat as file target.
+		t.Type = "file"
+		t.File = strings.TrimSpace(value.Value)
+		return nil
+	case yaml.MappingNode:
+		type raw Target
+		var r raw
+		if err := value.Decode(&r); err != nil {
+			return err
+		}
+		*t = Target(r)
+		if strings.TrimSpace(t.File) == "" && strings.TrimSpace(t.Note) != "" {
+			t.File = t.Note
+			if strings.TrimSpace(t.Type) == "" {
+				t.Type = "file"
+			}
+		}
+		if strings.TrimSpace(t.Type) == "" {
+			// Infer if possible.
+			if strings.TrimSpace(t.Folder) != "" || strings.TrimSpace(t.Pattern) != "" {
+				t.Type = "folder"
+			} else if strings.TrimSpace(t.File) != "" {
+				t.Type = "file"
+			}
+		}
+		return nil
+	default:
+		return fmt.Errorf("invalid target definition")
+	}
+}
+
+// TargetsConfig is the map of target name to Target.
+type TargetsConfig map[string]Target
+
+var reservedTargetNames = map[string]bool{
+	"add":      true,
+	"remove":   true,
+	"rm":       true,
+	"list":     true,
+	"ls":       true,
+	"edit":     true,
+	"validate": true,
+	"test":     true,
+	"help":     true,
+}
+
+type TargetPlan struct {
+	TargetName        string
+	TargetType        string
+	VaultName         string
+	VaultPath         string
+	RelativeNotePath  string
+	AbsoluteNotePath  string
+	TemplatePath      string
+	AbsoluteTemplate  string
+	WillCreateDirs    bool
+	WillCreateFile    bool
+	WillApplyTemplate bool
+}
+
+func TargetsFile() (string, error) {
+	_, path, err := config.TargetsPath()
+	return path, err
+}
+
+func EnsureTargetsFileExists() (string, error) {
+	dir, path, err := config.TargetsPath()
+	if err != nil {
+		return "", err
+	}
+	if err := os.MkdirAll(dir, 0750); err != nil {
+		return "", err
+	}
+	if _, err := os.Stat(path); err == nil {
+		return path, nil
+	} else if err != nil && !os.IsNotExist(err) {
+		return "", err
+	}
+	if err := os.WriteFile(path, []byte(targetsFileHeader), 0600); err != nil {
+		return "", err
+	}
+	return path, nil
+}
+
+// LoadTargets loads targets from targets.yaml.
+func LoadTargets() (TargetsConfig, error) {
+	path, err := TargetsFile()
+	if err != nil {
+		return nil, err
+	}
+	raw, err := os.ReadFile(path)
+	if err != nil {
+		return nil, err
+	}
+	cfg := TargetsConfig{}
+	if err := yaml.Unmarshal(raw, &cfg); err != nil {
+		return nil, err
+	}
+	return cfg, nil
+}
+
+// SaveTargets saves targets to targets.yaml.
+func SaveTargets(cfg TargetsConfig) error {
+	path, err := EnsureTargetsFileExists()
+	if err != nil {
+		return err
+	}
+	out, err := yaml.Marshal(cfg)
+	if err != nil {
+		return err
+	}
+	content := append([]byte(targetsFileHeader), out...)
+	return os.WriteFile(path, content, 0600)
+}
+
+func ListTargetNames(cfg TargetsConfig) []string {
+	var names []string
+	for name := range cfg {
+		names = append(names, name)
+	}
+	sort.Slice(names, func(i, j int) bool { return strings.ToLower(names[i]) < strings.ToLower(names[j]) })
+	return names
+}
+
+func ValidateTargetName(name string) error {
+	n := strings.TrimSpace(name)
+	if n == "" {
+		return errors.New("target name is required")
+	}
+	if strings.ContainsAny(n, " \t\r\n") {
+		return errors.New("target name cannot contain whitespace")
+	}
+	if reservedTargetNames[strings.ToLower(n)] {
+		return fmt.Errorf("target name '%s' is reserved", n)
+	}
+	return nil
+}
+
+func ValidateTarget(t Target) error {
+	tt := strings.ToLower(strings.TrimSpace(t.Type))
+	switch tt {
+	case "file":
+		if strings.TrimSpace(t.File) == "" {
+			return errors.New("file target requires 'file' field")
+		}
+		return nil
+	case "folder":
+		if strings.TrimSpace(t.Folder) == "" {
+			return errors.New("folder target requires 'folder' field")
+		}
+		if strings.TrimSpace(t.Pattern) == "" {
+			return errors.New("folder target requires 'pattern' field")
+		}
+		return nil
+	default:
+		if tt == "" {
+			return errors.New("target type is required: must be 'folder' or 'file'")
+		}
+		return fmt.Errorf("invalid target type '%s': must be 'folder' or 'file'", t.Type)
+	}
+}
+
+func ResolveTargetNotePath(vaultPath string, target Target, now time.Time) (relative string, absolute string, err error) {
+	if err := ValidateTarget(target); err != nil {
+		return "", "", err
+	}
+
+	switch strings.ToLower(strings.TrimSpace(target.Type)) {
+	case "file":
+		relative = strings.TrimSpace(target.File)
+	case "folder":
+		filename := ExpandDatePattern(strings.TrimSpace(target.Pattern), now)
+		if strings.TrimSpace(filename) == "" {
+			return "", "", errors.New("expanded filename is empty")
+		}
+		relative = filepath.ToSlash(filepath.Join(strings.TrimSpace(target.Folder), filename))
+	}
+
+	absolute, err = SafeJoinVaultPath(vaultPath, filepath.ToSlash(relative))
+	if err != nil {
+		return "", "", err
+	}
+	if !strings.HasSuffix(strings.ToLower(absolute), ".md") {
+		absolute += ".md"
+	}
+	return relative, absolute, nil
+}
+
+func PlanTargetAppend(vaultPath string, targetName string, target Target, now time.Time) (TargetPlan, error) {
+	rel, abs, err := ResolveTargetNotePath(vaultPath, target, now)
+	if err != nil {
+		return TargetPlan{}, err
+	}
+
+	plan := TargetPlan{
+		TargetName:       targetName,
+		TargetType:       strings.ToLower(strings.TrimSpace(target.Type)),
+		RelativeNotePath: rel,
+		AbsoluteNotePath: abs,
+	}
+
+	if _, err := os.Stat(filepath.Dir(abs)); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateDirs = true
+		} else {
+			return TargetPlan{}, err
+		}
+	}
+
+	if _, err := os.Stat(abs); err != nil {
+		if os.IsNotExist(err) {
+			plan.WillCreateFile = true
+		} else {
+			return TargetPlan{}, err
+		}
+	}
+
+	templateRel := strings.TrimSpace(target.Template)
+	if plan.WillCreateFile && templateRel != "" {
+		templateAbs, err := SafeJoinVaultPath(vaultPath, filepath.ToSlash(templateRel))
+		if err != nil {
+			return TargetPlan{}, fmt.Errorf("invalid template path: %w", err)
+		}
+		if !strings.HasSuffix(strings.ToLower(templateAbs), ".md") {
+			templateAbs += ".md"
+		}
+		plan.TemplatePath = templateRel
+		plan.AbsoluteTemplate = templateAbs
+		plan.WillApplyTemplate = true
+	}
+
+	return plan, nil
+}
+
+const targetsFileHeader = `# Obsidian CLI Targets
+#
+# This file defines capture targets for:
+#   obsidian-cli target <id> [text]
+#
+# Targets can be:
+#   - type: file   (append to a fixed note)
+#   - type: folder (append to a dated note determined by folder + pattern)
+#
+# Fields:
+#   type:     "file" or "folder"
+#   file:     (file target) path relative to vault root
+#   folder:   (folder target) folder path relative to vault root
+#   pattern:  (folder target) filename pattern, without ".md"
+#   template: (optional) note path to use as initial content when creating a new file
+#   vault:    (optional) override the default vault
+#
+# Pattern format:
+#   - Supports Obsidian-style tokens like YYYY, MM, DD, HH, mm, ss, ddd, dddd, MMM, MMMM
+#   - Supports [literal] blocks, e.g. YYYY-[log]-MM
+#   - Supports the "zettel" timestamp: YYYYMMDDHHmmss
+#
+# Examples:
+#   inbox:
+#     type: file
+#     file: Inbox.md
+#
+#   hourly-log:
+#     type: folder
+#     folder: Logs
+#     pattern: YYYY-MM-DD_HH
+#
+`
diff --git a/pkg/obsidian/targets_test.go b/pkg/obsidian/targets_test.go
new file mode 100644
index 0000000..1de8093
--- /dev/null
+++ b/pkg/obsidian/targets_test.go
@@ -0,0 +1,63 @@
+package obsidian_test
+
+import (
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+	"gopkg.in/yaml.v3"
+)
+
+func TestValidateTargetName(t *testing.T) {
+	assert.Error(t, obsidian.ValidateTargetName(""))
+	assert.Error(t, obsidian.ValidateTargetName(" "))
+	assert.Error(t, obsidian.ValidateTargetName("has space"))
+	assert.Error(t, obsidian.ValidateTargetName("add"))
+	assert.NoError(t, obsidian.ValidateTargetName("inbox"))
+}
+
+func TestTargetUnmarshalScalarTreatsAsFile(t *testing.T) {
+	var cfg obsidian.TargetsConfig
+	err := yaml.Unmarshal([]byte("inbox: Inbox.md\n"), &cfg)
+	assert.NoError(t, err)
+	tg := cfg["inbox"]
+	assert.Equal(t, "file", tg.Type)
+	assert.Equal(t, "Inbox.md", tg.File)
+}
+
+func TestResolveTargetNotePath(t *testing.T) {
+	vault := t.TempDir()
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	t.Run("file target", func(t *testing.T) {
+		rel, abs, err := obsidian.ResolveTargetNotePath(vault, obsidian.Target{Type: "file", File: "Inbox"}, now)
+		assert.NoError(t, err)
+		assert.Equal(t, "Inbox", rel)
+		assert.Contains(t, abs, "Inbox.md")
+	})
+
+	t.Run("folder target expands pattern", func(t *testing.T) {
+		rel, abs, err := obsidian.ResolveTargetNotePath(vault, obsidian.Target{Type: "folder", Folder: "Daily", Pattern: "YYYY-MM-DD_HH"}, now)
+		assert.NoError(t, err)
+		assert.Equal(t, "Daily/2024-01-15_14", rel)
+		assert.Contains(t, abs, "Daily")
+		assert.Contains(t, abs, "2024-01-15_14.md")
+	})
+
+	t.Run("reject escape", func(t *testing.T) {
+		_, _, err := obsidian.ResolveTargetNotePath(vault, obsidian.Target{Type: "file", File: "../escape.md"}, now)
+		assert.Error(t, err)
+	})
+}
+
+func TestPlanTargetAppend(t *testing.T) {
+	vault := t.TempDir()
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+
+	plan, err := obsidian.PlanTargetAppend(vault, "inbox", obsidian.Target{Type: "file", File: "Inbox.md"}, now)
+	assert.NoError(t, err)
+	assert.Equal(t, "inbox", plan.TargetName)
+	assert.True(t, plan.WillCreateFile)
+	assert.False(t, plan.WillCreateDirs)
+}
diff --git a/pkg/obsidian/template.go b/pkg/obsidian/template.go
new file mode 100644
index 0000000..05b8e38
--- /dev/null
+++ b/pkg/obsidian/template.go
@@ -0,0 +1,61 @@
+package obsidian
+
+import (
+	"regexp"
+	"strings"
+	"time"
+)
+
+// ExpandTemplateVariables expands common Obsidian template variables in content.
+// Supported:
+//   - {{date}} / {{date:FORMAT}}
+//   - {{time}} / {{time:FORMAT}}
+//   - {{title}}
+//
+// FORMAT uses the same curated Obsidian-style tokens as ExpandDatePattern (including [literal] blocks).
+func ExpandTemplateVariables(content []byte, title string) []byte {
+	return ExpandTemplateVariablesAt(content, title, time.Now())
+}
+
+// ExpandTemplateVariablesAt is like ExpandTemplateVariables but uses the provided time.
+func ExpandTemplateVariablesAt(content []byte, title string, now time.Time) []byte {
+	result := string(content)
+	result = strings.ReplaceAll(result, "{{title}}", RemoveMdSuffix(title))
+
+	result = expandTemplateDate(result, now)
+	result = expandTemplateTime(result, now)
+
+	return []byte(result)
+}
+
+func expandTemplateDate(content string, now time.Time) string {
+	// {{date:FORMAT}}
+	re := regexp.MustCompile(`\{\{date:([^}]+)\}\}`)
+	content = re.ReplaceAllStringFunc(content, func(match string) string {
+		format := match[7 : len(match)-2]
+		s, err := FormatDatePattern(format, now)
+		if err != nil {
+			return now.Format("2006-01-02")
+		}
+		return s
+	})
+
+	// {{date}}
+	return strings.ReplaceAll(content, "{{date}}", now.Format("2006-01-02"))
+}
+
+func expandTemplateTime(content string, now time.Time) string {
+	// {{time:FORMAT}}
+	re := regexp.MustCompile(`\{\{time:([^}]+)\}\}`)
+	content = re.ReplaceAllStringFunc(content, func(match string) string {
+		format := match[7 : len(match)-2]
+		s, err := FormatDatePattern(format, now)
+		if err != nil {
+			return now.Format("15:04")
+		}
+		return s
+	})
+
+	// {{time}}
+	return strings.ReplaceAll(content, "{{time}}", now.Format("15:04"))
+}
diff --git a/pkg/obsidian/template_test.go b/pkg/obsidian/template_test.go
new file mode 100644
index 0000000..9e24b0d
--- /dev/null
+++ b/pkg/obsidian/template_test.go
@@ -0,0 +1,20 @@
+package obsidian_test
+
+import (
+	"testing"
+	"time"
+
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestExpandTemplateVariablesAt(t *testing.T) {
+	now := time.Date(2024, 1, 15, 14, 30, 52, 0, time.UTC)
+	in := []byte("{{title}}\n{{date}}\n{{time}}\n{{date:YYYY-[ToDo]-MM}}\n{{time:HH:mm:ss}}\n")
+	out := string(obsidian.ExpandTemplateVariablesAt(in, "Note.md", now))
+	assert.Contains(t, out, "Note\n")
+	assert.Contains(t, out, "2024-01-15\n")
+	assert.Contains(t, out, "14:30\n")
+	assert.Contains(t, out, "2024-ToDo-01\n")
+	assert.Contains(t, out, "14:30:52\n")
+}
diff --git a/pkg/obsidian/utils.go b/pkg/obsidian/utils.go
index 76e3077..d792aa6 100644
--- a/pkg/obsidian/utils.go
+++ b/pkg/obsidian/utils.go
@@ -23,6 +23,12 @@ func RemoveMdSuffix(str string) string {
 	return str
 }
 
+// normalizePathSeparators converts backslashes to forward slashes for cross-platform consistency.
+// Obsidian uses forward slashes in links regardless of OS.
+func normalizePathSeparators(path string) string {
+	return strings.ReplaceAll(path, "\\", "/")
+}
+
 func GenerateNoteLinkTexts(noteName string) [3]string {
 	var noteLinkTexts [3]string
 	noteName = filepath.Base(noteName)
@@ -33,6 +39,53 @@ func GenerateNoteLinkTexts(noteName string) [3]string {
 	return noteLinkTexts
 }
 
+// GenerateLinkReplacements creates all replacement patterns for updating links when moving a note.
+// This handles:
+// - Simple wikilinks: [[note]], [[note|alias]], [[note#heading]]
+// - Path-based wikilinks: [[folder/note]], [[folder/note|alias]], [[folder/note#heading]]
+// - Markdown links: [text](folder/note.md), [text](./folder/note.md)
+func GenerateLinkReplacements(oldNotePath, newNotePath string) map[string]string {
+	replacements := make(map[string]string)
+
+	// Normalize paths to forward slashes for consistent matching
+	oldNormalized := normalizePathSeparators(oldNotePath)
+	newNormalized := normalizePathSeparators(newNotePath)
+
+	// Get basename without .md extension
+	oldBase := RemoveMdSuffix(filepath.Base(oldNotePath))
+	newBase := RemoveMdSuffix(filepath.Base(newNotePath))
+
+	// Get full path without .md extension
+	oldPathNoExt := RemoveMdSuffix(oldNormalized)
+	newPathNoExt := RemoveMdSuffix(newNormalized)
+
+	// 1. Simple wikilinks (basename only) - for backward compatibility
+	replacements["[["+oldBase+"]]"] = "[[" + newBase + "]]"
+	replacements["[["+oldBase+"|"] = "[[" + newBase + "|"
+	replacements["[["+oldBase+"#"] = "[[" + newBase + "#"
+
+	// 2. Path-based wikilinks (only if path differs from basename)
+	if oldPathNoExt != oldBase {
+		replacements["[["+oldPathNoExt+"]]"] = "[[" + newPathNoExt + "]]"
+		replacements["[["+oldPathNoExt+"|"] = "[[" + newPathNoExt + "|"
+		replacements["[["+oldPathNoExt+"#"] = "[[" + newPathNoExt + "#"
+	}
+
+	// 3. Markdown links (various formats)
+	oldMd := AddMdSuffix(oldNormalized)
+	newMd := AddMdSuffix(newNormalized)
+
+	// Standard markdown link: [text](folder/note.md)
+	replacements["]("+oldMd+")"] = "](" + newMd + ")"
+	replacements["]("+oldPathNoExt+")"] = "](" + newPathNoExt + ")"
+
+	// Relative markdown link: [text](./folder/note.md)
+	replacements["](./"+oldMd+")"] = "](./" + newMd + ")"
+	replacements["](./"+oldPathNoExt+")"] = "](./" + newPathNoExt + ")"
+
+	return replacements
+}
+
 func ReplaceContent(content []byte, replacements map[string]string) []byte {
 	for o, n := range replacements {
 		content = bytes.ReplaceAll(content, []byte(o), []byte(n))
@@ -40,6 +93,95 @@ func ReplaceContent(content []byte, replacements map[string]string) []byte {
 	return content
 }
 
+type IncomingLink struct {
+	SourcePath string
+	LinkText   string
+}
+
+func FindIncomingLinks(vaultPath string, targetNotePath string) ([]IncomingLink, error) {
+	var links []IncomingLink
+
+	patterns := generateLinkPatterns(targetNotePath)
+	target := AddMdSuffix(normalizePathSeparators(targetNotePath))
+
+	err := filepath.Walk(vaultPath, func(path string, info os.FileInfo, err error) error {
+		if err != nil {
+			return nil
+		}
+
+		if info.IsDir() {
+			// Avoid descending into hidden directories (.obsidian, .git, etc.).
+			if strings.HasPrefix(info.Name(), ".") {
+				return filepath.SkipDir
+			}
+			return nil
+		}
+
+		if ShouldSkipDirectoryOrFile(info) {
+			return nil
+		}
+
+		relPath, err := filepath.Rel(vaultPath, path)
+		if err != nil {
+			return nil
+		}
+		relPath = normalizePathSeparators(relPath)
+
+		if relPath == target {
+			return nil
+		}
+
+		content, err := os.ReadFile(path)
+		if err != nil {
+			return nil
+		}
+
+		contentStr := string(content)
+		for _, pattern := range patterns {
+			if strings.Contains(contentStr, pattern) {
+				links = append(links, IncomingLink{
+					SourcePath: relPath,
+					LinkText:   pattern,
+				})
+				break
+			}
+		}
+
+		return nil
+	})
+	if err != nil {
+		return nil, fmt.Errorf("failed to scan vault: %w", err)
+	}
+
+	return links, nil
+}
+
+func generateLinkPatterns(notePath string) []string {
+	var patterns []string
+
+	normalized := normalizePathSeparators(notePath)
+	basename := RemoveMdSuffix(filepath.Base(notePath))
+	pathNoExt := RemoveMdSuffix(normalized)
+
+	patterns = append(patterns, "[["+basename+"]]")
+	patterns = append(patterns, "[["+basename+"|")
+	patterns = append(patterns, "[["+basename+"#")
+
+	if pathNoExt != basename {
+		patterns = append(patterns, "[["+pathNoExt+"]]")
+		patterns = append(patterns, "[["+pathNoExt+"|")
+		patterns = append(patterns, "[["+pathNoExt+"#")
+	}
+
+	withMd := AddMdSuffix(normalized)
+	patterns = append(patterns, "]("+withMd+")")
+	patterns = append(patterns, "]("+pathNoExt+")")
+	patterns = append(patterns, "](./"+withMd+")")
+	patterns = append(patterns, "](./"+pathNoExt+")")
+
+	return patterns
+}
+
 func ShouldSkipDirectoryOrFile(info os.FileInfo) bool {
 	isDirectory := info.IsDir()
 	isHidden := info.Name()[0] == '.'
diff --git a/pkg/obsidian/utils_test.go b/pkg/obsidian/utils_test.go
index 1e6ac4f..4f92060 100644
--- a/pkg/obsidian/utils_test.go
+++ b/pkg/obsidian/utils_test.go
@@ -71,6 +71,73 @@ func TestGenerateNoteLinkTexts(t *testing.T) {
 	}
 }
 
+func TestGenerateLinkReplacements(t *testing.T) {
+	t.Run("Simple note rename", func(t *testing.T) {
+		replacements := obsidian.GenerateLinkReplacements("oldNote", "newNote")
+
+		// Should have wikilink patterns
+		assert.Equal(t, "[[newNote]]", replacements["[[oldNote]]"])
+		assert.Equal(t, "[[newNote|", replacements["[[oldNote|"])
+		assert.Equal(t, "[[newNote#", replacements["[[oldNote#"])
+
+		// Should have markdown link patterns
+		assert.Equal(t, "](newNote.md)", replacements["](oldNote.md)"])
+		assert.Equal(t, "](newNote)", replacements["](oldNote)"])
+	})
+
+	t.Run("Note with path", func(t *testing.T) {
+		replacements := obsidian.GenerateLinkReplacements("folder/oldNote", "folder/newNote")
+
+		// Simple wikilinks (basename)
+		assert.Equal(t, "[[newNote]]", replacements["[[oldNote]]"])
+		assert.Equal(t, "[[newNote|", replacements["[[oldNote|"])
+		assert.Equal(t, "[[newNote#", replacements["[[oldNote#"])
+
+		// Path-based wikilinks
+		assert.Equal(t, "[[folder/newNote]]", replacements["[[folder/oldNote]]"])
+		assert.Equal(t, "[[folder/newNote|", replacements["[[folder/oldNote|"])
+		assert.Equal(t, "[[folder/newNote#", replacements["[[folder/oldNote#"])
+
+		// Markdown links
+		assert.Equal(t, "](folder/newNote.md)", replacements["](folder/oldNote.md)"])
+		assert.Equal(t, "](folder/newNote)", replacements["](folder/oldNote)"])
+
+		// Relative markdown links
+		assert.Equal(t, "](./folder/newNote.md)", replacements["](./folder/oldNote.md)"])
+	})
+
+	t.Run("Move to different folder", func(t *testing.T) {
+		replacements := obsidian.GenerateLinkReplacements("folder1/note", "folder2/note")
+
+		// Basename stays same
+		assert.Equal(t, "[[note]]", replacements["[[note]]"])
+
+		// Path-based wikilinks update
+		assert.Equal(t, "[[folder2/note]]", replacements["[[folder1/note]]"])
+
+		// Markdown links update
+		assert.Equal(t, "](folder2/note.md)", replacements["](folder1/note.md)"])
+	})
+
+	t.Run("Note with .md extension", func(t *testing.T) {
+		replacements := obsidian.GenerateLinkReplacements("folder/note.md", "folder/renamed.md")
+
+		// Wikilinks don't include .md
+		assert.Equal(t, "[[renamed]]", replacements["[[note]]"])
+		assert.Equal(t, "[[folder/renamed]]", replacements["[[folder/note]]"])
+
+		// Markdown links with .md
+		assert.Equal(t, "](folder/renamed.md)", replacements["](folder/note.md)"])
+	})
+
+	t.Run("Nested path", func(t *testing.T) {
+		replacements := obsidian.GenerateLinkReplacements("a/b/c/note", "x/y/note")
+
+		assert.Equal(t, "[[x/y/note]]", replacements["[[a/b/c/note]]"])
+		assert.Equal(t, "](x/y/note.md)", replacements["](a/b/c/note.md)"])
+	})
+}
+
 func TestReplaceContent(t *testing.T) {
 	tests := []struct {
 		testName     string
@@ -195,3 +262,33 @@ func TestOpenInEditor(t *testing.T) {
 		}
 	})
 }
+
+func TestFindIncomingLinks(t *testing.T) {
+	vaultPath := t.TempDir()
+
+	targetNote := filepath.Join(vaultPath, "target.md")
+	linkingNote := filepath.Join(vaultPath, "linking.md")
+	nonLinkingNote := filepath.Join(vaultPath, "other.md")
+	aliasLinkNote := filepath.Join(vaultPath, "alias.md")
+
+	assert.NoError(t, os.WriteFile(targetNote, []byte("# Target\nThis is the target note."), 0644))
+	assert.NoError(t, os.WriteFile(linkingNote, []byte("# Linking\nSee [[target]] for more info."), 0644))
+	assert.NoError(t, os.WriteFile(nonLinkingNote, []byte("# Other\nNo links here."), 0644))
+	assert.NoError(t, os.WriteFile(aliasLinkNote, []byte("# Alias\nCheck [[target|the target]] out."), 0644))
+
+	links, err := obsidian.FindIncomingLinks(vaultPath, "target")
+	assert.NoError(t, err)
+	assert.Len(t, links, 2)
+
+	sourceFiles := make(map[string]bool)
+	for _, link := range links {
+		sourceFiles[link.SourcePath] = true
+	}
+	assert.True(t, sourceFiles["linking.md"])
+	assert.True(t, sourceFiles["alias.md"])
+	assert.False(t, sourceFiles["target.md"])
+
+	links, err = obsidian.FindIncomingLinks(vaultPath, "other")
+	assert.NoError(t, err)
+	assert.Len(t, links, 0)
+}
diff --git a/pkg/obsidian/vault.go b/pkg/obsidian/vault.go
index cace38c..ce5f545 100644
--- a/pkg/obsidian/vault.go
+++ b/pkg/obsidian/vault.go
@@ -1,7 +1,23 @@
 package obsidian
 
+// CliConfig represents the obsidian-cli configuration stored in preferences.json.
+// It contains the default vault name and optional per-vault settings.
 type CliConfig struct {
-	DefaultVaultName string `json:"default_vault_name"`
+	DefaultVaultName string                   `json:"default_vault_name"`
+	VaultSettings    map[string]VaultSettings `json:"vault_settings,omitempty"`
+}
+
+// VaultSettings contains per-vault configuration options.
+type VaultSettings struct {
+	DailyNote DailyNoteSettings `json:"daily_note,omitempty"`
+}
+
+// DailyNoteSettings configures how daily notes are created and managed.
+type DailyNoteSettings struct {
+	Folder          string `json:"folder,omitempty"`            // Folder path relative to vault root
+	FilenamePattern string `json:"filename_pattern,omitempty"`  // Pattern with {YYYY-MM-DD} placeholder
+	TemplatePath    string `json:"template_path,omitempty"`     // Optional template file path
+	CreateIfMissing bool   `json:"create_if_missing,omitempty"` // Auto-create notes if they don't exist
 }
 
 type ObsidianVaultConfig struct {
diff --git a/pkg/obsidian/vault_default_name.go b/pkg/obsidian/vault_default_name.go
index e5e829b..3ee515a 100644
--- a/pkg/obsidian/vault_default_name.go
+++ b/pkg/obsidian/vault_default_name.go
@@ -44,8 +44,12 @@ func (v *Vault) DefaultName() (string, error) {
 }
 
 func (v *Vault) SetDefaultName(name string) error {
-	// marshal obsidian name to json
-	cliConfig := CliConfig{DefaultVaultName: name}
+	cliConfig, err := readCliConfig()
+	if err != nil {
+		return err
+	}
+	cliConfig.DefaultVaultName = name
+
 	jsonContent, err := JsonMarshal(cliConfig)
 	if err != nil {
 		return errors.New(ObsidianCLIConfigGenerateJSONError)
@@ -58,18 +62,105 @@ func (v *Vault) SetDefaultName(name string) error {
 	}
 
 	// create directory
-	err = os.MkdirAll(obsConfigDir, os.ModePerm)
+	err = os.MkdirAll(obsConfigDir, 0750)
 	if err != nil {
 		return errors.New(ObsidianCLIConfigDirWriteEror)
 	}
+	if err := os.Chmod(obsConfigDir, 0750); err != nil {
+		return errors.New(ObsidianCLIConfigDirWriteEror)
+	}
 
 	// create and write file
-	err = os.WriteFile(obsConfigFile, jsonContent, 0644)
+	err = os.WriteFile(obsConfigFile, jsonContent, 0600)
 	if err != nil {
 		return errors.New(ObsidianCLIConfigWriteError)
 	}
+	if err := os.Chmod(obsConfigFile, 0600); err != nil {
+		return errors.New(ObsidianCLIConfigWriteError)
+	}
 
 	v.Name = name
 
 	return nil
 }
+
+// Settings returns the VaultSettings for the current default vault.
+// Returns empty settings if none are configured (not an error).
+func (v *Vault) Settings() (VaultSettings, error) {
+	name, err := v.DefaultName()
+	if err != nil {
+		return VaultSettings{}, err
+	}
+
+	cfg, err := readCliConfig()
+	if err != nil {
+		return VaultSettings{}, err
+	}
+
+	if cfg.VaultSettings == nil {
+		return VaultSettings{}, nil
+	}
+	settings, ok := cfg.VaultSettings[name]
+	if !ok {
+		return VaultSettings{}, nil
+	}
+	return settings, nil
+}
+
+// SetSettings saves the VaultSettings for the current default vault.
+func (v *Vault) SetSettings(settings VaultSettings) error {
+	name, err := v.DefaultName()
+	if err != nil {
+		return err
+	}
+
+	cfg, err := readCliConfig()
+	if err != nil {
+		return err
+	}
+	if cfg.VaultSettings == nil {
+		cfg.VaultSettings = map[string]VaultSettings{}
+	}
+	cfg.VaultSettings[name] = settings
+
+	jsonContent, err := JsonMarshal(cfg)
+	if err != nil {
+		return errors.New(ObsidianCLIConfigGenerateJSONError)
+	}
+
+	obsConfigDir, obsConfigFile, err := CliConfigPath()
+	if err != nil {
+		return err
+	}
+	if err := os.MkdirAll(obsConfigDir, 0750); err != nil {
+		return errors.New(ObsidianCLIConfigDirWriteEror)
+	}
+	if err := os.Chmod(obsConfigDir, 0750); err != nil {
+		return errors.New(ObsidianCLIConfigDirWriteEror)
+	}
+	if err := os.WriteFile(obsConfigFile, jsonContent, 0600); err != nil {
+		return errors.New(ObsidianCLIConfigWriteError)
+	}
+	if err := os.Chmod(obsConfigFile, 0600); err != nil {
+		return errors.New(ObsidianCLIConfigWriteError)
+	}
+	return nil
+}
+
+// readCliConfig reads the CLI configuration from the preferences file.
+// Returns an empty config (not an error) if the file doesn't exist yet.
+func readCliConfig() (CliConfig, error) {
+	_, cliConfigFile, err := CliConfigPath()
+	if err != nil {
+		return CliConfig{}, err
+	}
+	content, err := os.ReadFile(cliConfigFile)
+	if err != nil {
+		return CliConfig{}, nil
+	}
+	cliConfig := CliConfig{}
+	if err := json.Unmarshal(content, &cliConfig); err != nil {
+		return CliConfig{}, errors.New(ObsidianCLIConfigParseError)
+	}
+	return cliConfig, nil
+}
diff --git a/pkg/obsidian/vault_default_name_test.go b/pkg/obsidian/vault_default_name_test.go
index a5b4099..546e234 100644
--- a/pkg/obsidian/vault_default_name_test.go
+++ b/pkg/obsidian/vault_default_name_test.go
@@ -132,13 +132,17 @@ func TestVaultSetDefaultName(t *testing.T) {
 	})
 
 	t.Run("Error in json marshal", func(t *testing.T) {
+		mockCliConfigDir, mockCliConfigFile := mocks.CreateMockCliConfigDirectories(t)
+		obsidian.CliConfigPath = func() (string, string, error) {
+			return mockCliConfigDir, mockCliConfigFile, nil
+		}
+
 		// Temporarily override the JsonMarshal function
 		originalJsonMarshal := obsidian.JsonMarshal
 		defer func() { obsidian.JsonMarshal = originalJsonMarshal }()
 		obsidian.JsonMarshal = func(v interface{}) ([]byte, error) {
 			return nil, errors.New("json marshal error")
 		}
-		// Arrange
 		vault := obsidian.Vault{}
 		// Act
 		err := vault.SetDefaultName("invalid json")
@@ -160,11 +164,13 @@ func TestVaultSetDefaultName(t *testing.T) {
 
 	t.Run("Error in writing to default vault config file", func(t *testing.T) {
 		// Arrange
-		mockCliConfigDir, _ := mocks.CreateMockCliConfigDirectories(t)
+		mockCliConfigDir, mockCliConfigFile := mocks.CreateMockCliConfigDirectories(t)
 		obsidian.CliConfigPath = func() (string, string, error) {
-			return mockCliConfigDir + "/unwrittable", mockCliConfigDir + "unwrittable/preferences.json", nil
+			return mockCliConfigDir, mockCliConfigFile, nil
 		}
-		err := os.Mkdir(mockCliConfigDir+"/unwrittable", 0444)
+		// Create a directory at the file path so os.WriteFile fails deterministically.
+		err := os.Mkdir(mockCliConfigFile, 0755)
+		assert.NoError(t, err)
 		vault := obsidian.Vault{}
 		// Act
 		err = vault.SetDefaultName("vault-name")
diff --git a/pkg/obsidian/vault_settings_test.go b/pkg/obsidian/vault_settings_test.go
new file mode 100644
index 0000000..f6dd5ff
--- /dev/null
+++ b/pkg/obsidian/vault_settings_test.go
@@ -0,0 +1,82 @@
+package obsidian_test
+
+import (
+	"encoding/json"
+	"os"
+	"path/filepath"
+	"testing"
+
+	"github.com/Yakitrak/obsidian-cli/mocks"
+	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
+	"github.com/stretchr/testify/assert"
+)
+
+func TestSetDefaultNamePreservesVaultSettings(t *testing.T) {
+	mockCliConfigDir, mockCliConfigFile := mocks.CreateMockCliConfigDirectories(t)
+	originalCliConfigPath := obsidian.CliConfigPath
+	t.Cleanup(func() { obsidian.CliConfigPath = originalCliConfigPath })
+	obsidian.CliConfigPath = func() (string, string, error) {
+		return mockCliConfigDir, mockCliConfigFile, nil
+	}
+
+	initial := obsidian.CliConfig{
+		DefaultVaultName: "Old Vault",
+		VaultSettings: map[string]obsidian.VaultSettings{
+			"Old Vault": {
+				DailyNote: obsidian.DailyNoteSettings{
+					Folder:          "Daily",
+					FilenamePattern: "{YYYY-MM-DD}",
+					TemplatePath:    "Templates/Daily.md",
+					CreateIfMissing: true,
+				},
+			},
+		},
+	}
+	b, err := json.Marshal(initial)
+	assert.NoError(t, err)
+	assert.NoError(t, os.MkdirAll(filepath.Dir(mockCliConfigFile), os.ModePerm))
+	assert.NoError(t, os.WriteFile(mockCliConfigFile, b, 0644))
+
+	vault := obsidian.Vault{}
+	assert.NoError(t, vault.SetDefaultName("New Vault"))
+
+	updatedBytes, err := os.ReadFile(mockCliConfigFile)
+	assert.NoError(t, err)
+	var updated obsidian.CliConfig
+	assert.NoError(t, json.Unmarshal(updatedBytes, &updated))
+	assert.Equal(t, "New Vault", updated.DefaultVaultName)
+	assert.NotNil(t, updated.VaultSettings)
+	assert.Contains(t, updated.VaultSettings, "Old Vault")
+	assert.Equal(t, initial.VaultSettings["Old Vault"].DailyNote.Folder, updated.VaultSettings["Old Vault"].DailyNote.Folder)
+}
+
+func TestSetSettingsCreatesVaultSettingsEntry(t *testing.T) {
+	mockCliConfigDir, mockCliConfigFile := mocks.CreateMockCliConfigDirectories(t)
+	originalCliConfigPath := obsidian.CliConfigPath
+	t.Cleanup(func() { obsidian.CliConfigPath = originalCliConfigPath })
+	obsidian.CliConfigPath = func() (string, string, error) {
+		return mockCliConfigDir, mockCliConfigFile, nil
+	}
+
+	vault := obsidian.Vault{}
+	assert.NoError(t, vault.SetDefaultName("Example Vault"))
+
+	settings := obsidian.VaultSettings{
+		DailyNote: obsidian.DailyNoteSettings{
+			Folder:          "Daily",
+			FilenamePattern: "{YYYY-MM-DD}",
+			TemplatePath:    "Templates/Daily.md",
+			CreateIfMissing: true,
+		},
+	}
+	assert.NoError(t, vault.SetSettings(settings))
+
+	updatedBytes, err := os.ReadFile(mockCliConfigFile)
+	assert.NoError(t, err)
+	var updated obsidian.CliConfig
+	assert.NoError(t, json.Unmarshal(updatedBytes, &updated))
+	assert.Equal(t, "Example Vault", updated.DefaultVaultName)
+	assert.NotNil(t, updated.VaultSettings)
+	assert.Contains(t, updated.VaultSettings, "Example Vault")
+	assert.Equal(t, "Daily", updated.VaultSettings["Example Vault"].DailyNote.Folder)
+}
```
