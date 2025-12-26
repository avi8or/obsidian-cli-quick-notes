# pr-04b-append

Baseline: origin/main (1b06c32570b17684ba08b958f86368577e87fd20)
Previous: pr-04a-settings (424ac9815db5626cea2c3c9c9726e01eda8a1539)
This PR: pr-04b-append (ffbec389ad6215a9aab006322e121deadde449c7)

## Incremental Diff (vs previous in stack)

Compare: pr-04a-settings..pr-04b-append

### Commits
```
ffbec38 docs: README document append command
bb67cc9 feat(append): add daily note append command
```

### Diff Stat (all files)
```
 README.md                  |  22 +++++++
 cmd/append.go              |  68 +++++++++++++++++++
 pkg/actions/append.go      | 158 +++++++++++++++++++++++++++++++++++++++++++++
 pkg/actions/append_test.go |  99 ++++++++++++++++++++++++++++
 4 files changed, 347 insertions(+)
```

### Diff Stat (vendor only)
```

```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index f59fd15..f578e5f 100644
--- a/README.md
+++ b/README.md
@@ -158,6 +158,28 @@ obsidian-cli daily --vault "{vault-name}"
 
 ```
 
+### Append to Daily Note
+
+Appends text to today's daily note.
+
+This command writes to a daily note path derived from your per-vault settings in `obsidian-cli/preferences.json` (`daily_note.folder` and `daily_note.filename_pattern`). If `daily_note.folder` is not set for the vault, the command will error.
+
+If no text argument is provided, content is read from stdin (piped) or entered interactively until EOF.
+
+```bash
+# Append a one-liner
+obsidian-cli append "Meeting notes: discussed roadmap"
+
+# Append from stdin
+echo "Line 1\nLine 2" | obsidian-cli append
+
+# Append with timestamp
+obsidian-cli append --timestamp "Started work on feature X"
+
+# Append in a specific vault
+obsidian-cli append --vault "{vault-name}" "Daily standup notes"
+```
+
 ### Search Note
 
 Starts a fuzzy search displaying notes in the terminal from the vault. You can hit enter on a note to open that in Obsidian.
diff --git a/cmd/append.go b/cmd/append.go
new file mode 100644
index 0000000..cf40752
--- /dev/null
+++ b/cmd/append.go
@@ -0,0 +1,68 @@
+package cmd
+
+import (
+	"fmt"
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
+)
+
+var appendCmd = &cobra.Command{
+	Use:     "append [text]",
+	Aliases: []string{"a"},
+	Short:   "Append text to today's daily note",
+	Long: `Appends text to today's daily note.
+
+This command writes to a daily note path derived from your per-vault settings
+in preferences.json (daily_note.folder and daily_note.filename_pattern).
+
+If no text argument is provided, content is read from stdin (piped) or entered
+interactively until EOF.`,
+	Example: `  # Append a one-liner
+  obsidian-cli append "Meeting notes: discussed roadmap"
+
+  # Append multi-line content interactively (Ctrl-D to save)
+  obsidian-cli append
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
+		content := strings.TrimSpace(strings.Join(args, " "))
+		var err error
+		content, err = actions.PromptForContentIfEmpty(content)
+		if err != nil {
+			return err
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
+		return actions.AppendToDailyNote(&vault, content)
+	},
+}
+
+func init() {
+	appendCmd.Flags().BoolVarP(&appendTimestamp, "timestamp", "t", false, "prepend a timestamp to the content")
+	appendCmd.Flags().StringVar(&appendTimeFmt, "time-format", "", "custom timestamp format (Go time format, default: 15:04)")
+	appendCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
+	rootCmd.AddCommand(appendCmd)
+}
diff --git a/pkg/actions/append.go b/pkg/actions/append.go
new file mode 100644
index 0000000..fc1427e
--- /dev/null
+++ b/pkg/actions/append.go
@@ -0,0 +1,158 @@
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
+// AppendToDailyNote appends content to today's daily note, using per-vault settings.
+func AppendToDailyNote(vault *obsidian.Vault, content string) error {
+	content = strings.TrimSpace(content)
+	if content == "" {
+		return errors.New("no content provided")
+	}
+
+	vaultPath, err := vault.Path()
+	if err != nil {
+		return err
+	}
+
+	settings, err := vault.Settings()
+	if err != nil {
+		return err
+	}
+
+	folder := strings.TrimSpace(settings.DailyNote.Folder)
+	if folder == "" {
+		return errors.New("daily note is not configured (missing daily_note.folder in preferences.json)")
+	}
+
+	pattern := strings.TrimSpace(settings.DailyNote.FilenamePattern)
+	if pattern == "" {
+		pattern = "{YYYY-MM-DD}"
+	}
+
+	filename := expandDailyFilename(pattern, time.Now())
+	relNotePath := filepath.ToSlash(filepath.Join(folder, filename))
+	abs, err := safeJoinVaultPath(vaultPath, relNotePath)
+	if err != nil {
+		return err
+	}
+	if !strings.HasSuffix(strings.ToLower(abs), ".md") {
+		abs += ".md"
+	}
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
+func expandDailyFilename(pattern string, now time.Time) string {
+	out := pattern
+	out = strings.ReplaceAll(out, "{YYYY-MM-DD}", now.Format("2006-01-02"))
+	out = strings.ReplaceAll(out, "YYYY-MM-DD", now.Format("2006-01-02"))
+	return out
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
diff --git a/pkg/actions/append_test.go b/pkg/actions/append_test.go
new file mode 100644
index 0000000..52de227
--- /dev/null
+++ b/pkg/actions/append_test.go
@@ -0,0 +1,99 @@
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
```

### Full Diff Command (includes vendor/)
```
git diff --no-color pr-04a-settings..pr-04b-append
```

## Cumulative Diff (vs origin/main)

Compare: origin/main..pr-04b-append

### Commits
```
ffbec38 docs: README document append command
bb67cc9 feat(append): add daily note append command
424ac98 fix(config): enforce secure perms on preferences
f7cb7ea docs: README mention preferences.json and per-vault settings
4f2a0a8 feat(config): add per-vault settings to preferences.json
e4d1520 docs: README document delete confirmation and --force
d3a471d feat(delete): warn when note has incoming links
efb95c4 docs: acknowledge upstream PR #58 for link updates
d06731f docs: README clarify move link updates
6233ae2 fix: update path-based wikilinks and markdown links when moving notes
06d79e8 docs: README mention command-specific help
d9d3fa9 feat(cli): improve help and error handling
```

### Diff Stat (all files)
```
 README.md                               |  68 +++++++++++++-
 cmd/append.go                           |  68 ++++++++++++++
 cmd/create.go                           |  35 +++++--
 cmd/delete.go                           |  31 +++++--
 cmd/move.go                             |  31 ++++---
 cmd/open.go                             |  25 +++--
 cmd/print.go                            |  26 +++++-
 cmd/print_default.go                    |  22 +++--
 cmd/search.go                           |  26 ++++--
 cmd/search_content.go                   |  27 ++++--
 cmd/set_default.go                      |  26 ++++--
 pkg/actions/append.go                   | 158 ++++++++++++++++++++++++++++++++
 pkg/actions/append_test.go              |  99 ++++++++++++++++++++
 pkg/actions/delete.go                   |  32 +++++++
 pkg/obsidian/note.go                    |  10 +-
 pkg/obsidian/note_test.go               |  78 ++++++++++++++++
 pkg/obsidian/utils.go                   | 142 ++++++++++++++++++++++++++++
 pkg/obsidian/utils_test.go              |  97 ++++++++++++++++++++
 pkg/obsidian/vault.go                   |  18 +++-
 pkg/obsidian/vault_default_name.go      |  99 +++++++++++++++++++-
 pkg/obsidian/vault_default_name_test.go |  14 ++-
 pkg/obsidian/vault_settings_test.go     |  82 +++++++++++++++++
 22 files changed, 1118 insertions(+), 96 deletions(-)
```

### Diff Stat (vendor only)
```

```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index 591f921..f578e5f 100644
--- a/README.md
+++ b/README.md
@@ -48,6 +48,12 @@ For full installation instructions, see [Mac and Linux manual](https://yakitrak.
 obsidian-cli --help
 ```
 
+For detailed help (including examples) for a specific command:
+
+```bash
+obsidian-cli <command> --help
+```
+
 ### Editor Flag
 
 The `search`, `search-content`, `create`, and `move` commands support the `--editor` (or `-e`) flag, which opens notes in your default text editor instead of the Obsidian application. This is useful for quick edits or when working in a terminal-only environment.
@@ -83,6 +89,26 @@ obsidian-cli set-default "{vault-name}"
 
 Note: `open` and other commands in `obsidian-cli` use this vault's base directory as the working directory, not the current working directory of your terminal.
 
+The default vault is stored in `obsidian-cli/preferences.json` under your OS user config directory (`os.UserConfigDir()`).
+
+This preferences file also supports optional per-vault settings under `vault_settings` (for example, daily note configuration):
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
 ### Print Default Vault
 
 Prints default vault and path. Please set this with `set-default` command if not set.
@@ -132,6 +158,28 @@ obsidian-cli daily --vault "{vault-name}"
 
 ```
 
+### Append to Daily Note
+
+Appends text to today's daily note.
+
+This command writes to a daily note path derived from your per-vault settings in `obsidian-cli/preferences.json` (`daily_note.folder` and `daily_note.filename_pattern`). If `daily_note.folder` is not set for the vault, the command will error.
+
+If no text argument is provided, content is read from stdin (piped) or entered interactively until EOF.
+
+```bash
+# Append a one-liner
+obsidian-cli append "Meeting notes: discussed roadmap"
+
+# Append from stdin
+echo "Line 1\nLine 2" | obsidian-cli append
+
+# Append with timestamp
+obsidian-cli append --timestamp "Started work on feature X"
+
+# Append in a specific vault
+obsidian-cli append --vault "{vault-name}" "Daily standup notes"
+```
+
 ### Search Note
 
 Starts a fuzzy search displaying notes in the terminal from the vault. You can hit enter on a note to open that in Obsidian.
@@ -210,7 +258,12 @@ obsidian-cli create "{note-name}" --content "abcde" --open --editor
 
 ### Move / Rename Note
 
-Moves a given note(path from top level of vault) with new name given (top level of vault). If given same path but different name then its treated as a rename. All links inside vault are updated to match new name.
+Moves a given note (path from top level of vault) to a new path. If given the same path but a different name, it's treated as a rename.
+
+When moving/renaming, `obsidian-cli` updates links inside your vault to match the new location, including:
+
+- Wikilinks: `[[note]]`, `[[folder/note]]`, `[[folder/note|alias]]`, `[[folder/note#heading]]`
+- Markdown links: `[text](folder/note.md)`, `[text](./folder/note.md)`, and the same forms without the `.md` extension
 
 ```bash
 # Renames a note in default obsidian
@@ -230,11 +283,16 @@ obsidian-cli move "{current-note-path}" "{new-note-path}" --open --editor
 
 Deletes a given note (path from top level of vault).
 
+If other notes link to the note, you'll be prompted to confirm. Use `--force` (or `-f`) to skip confirmation.
+
 ```bash
-# Renames a note in default obsidian
+# Delete a note in the default vault
 obsidian-cli delete "{note-path}"
 
-# Renames a note in given obsidian
+# Force delete without prompt (recommended for scripts)
+obsidian-cli delete "{note-path}" --force
+
+# Delete a note in a specific vault
 obsidian-cli delete "{note-path}" --vault "{vault-name}"
 ```
 
@@ -242,6 +300,10 @@ obsidian-cli delete "{note-path}" --vault "{vault-name}"
 
 Fork the project, add your feature or fix and submit a pull request. You can also open an [issue](https://github.com/yakitrak/obsidian-cli/issues/new/choose) to report a bug or request a feature.
 
+## Acknowledgements
+
+- Link-update support for path-based wikilinks and markdown links builds on upstream PR #58: https://github.com/Yakitrak/obsidian-cli/pull/58
+
 ## License
 
 Available under [MIT License](./LICENSE)
diff --git a/cmd/append.go b/cmd/append.go
new file mode 100644
index 0000000..cf40752
--- /dev/null
+++ b/cmd/append.go
@@ -0,0 +1,68 @@
+package cmd
+
+import (
+	"fmt"
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
+)
+
+var appendCmd = &cobra.Command{
+	Use:     "append [text]",
+	Aliases: []string{"a"},
+	Short:   "Append text to today's daily note",
+	Long: `Appends text to today's daily note.
+
+This command writes to a daily note path derived from your per-vault settings
+in preferences.json (daily_note.folder and daily_note.filename_pattern).
+
+If no text argument is provided, content is read from stdin (piped) or entered
+interactively until EOF.`,
+	Example: `  # Append a one-liner
+  obsidian-cli append "Meeting notes: discussed roadmap"
+
+  # Append multi-line content interactively (Ctrl-D to save)
+  obsidian-cli append
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
+		content := strings.TrimSpace(strings.Join(args, " "))
+		var err error
+		content, err = actions.PromptForContentIfEmpty(content)
+		if err != nil {
+			return err
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
+		return actions.AppendToDailyNote(&vault, content)
+	},
+}
+
+func init() {
+	appendCmd.Flags().BoolVarP(&appendTimestamp, "timestamp", "t", false, "prepend a timestamp to the content")
+	appendCmd.Flags().StringVar(&appendTimeFmt, "time-format", "", "custom timestamp format (Go time format, default: 15:04)")
+	appendCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name (not required if default is set)")
+	rootCmd.AddCommand(appendCmd)
+}
diff --git a/cmd/create.go b/cmd/create.go
index 338dcc9..f283518 100644
--- a/cmd/create.go
+++ b/cmd/create.go
@@ -1,27 +1,47 @@
 package cmd
 
 import (
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
 var createNoteCmd = &cobra.Command{
-	Use:     "create",
+	Use:     "create <note>",
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
+	Args: cobra.ExactArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
 		noteName := args[0]
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
-			log.Fatalf("Failed to parse --editor flag: %v", err)
+			return fmt.Errorf("failed to parse --editor flag: %w", err)
 		}
 		params := actions.CreateParams{
 			NoteName:        noteName,
@@ -31,10 +51,7 @@ var createNoteCmd = &cobra.Command{
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
 
diff --git a/cmd/delete.go b/cmd/delete.go
index f29f691..541c08e 100644
--- a/cmd/delete.go
+++ b/cmd/delete.go
@@ -3,30 +3,43 @@ package cmd
 import (
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
 
+var deleteForce bool
+
 var deleteCmd = &cobra.Command{
-	Use:     "delete",
+	Use:     "delete <note>",
 	Aliases: []string{"d"},
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
+	Args: cobra.ExactArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		note := obsidian.Note{}
 		notePath := args[0]
-		params := actions.DeleteParams{NotePath: notePath}
-		err := actions.DeleteNote(&vault, &note, params)
-		if err != nil {
-			log.Fatal(err)
+		params := actions.DeleteParams{
+			NotePath: notePath,
+			Force:    deleteForce,
 		}
+		return actions.DeleteNote(&vault, &note, params)
 	},
 }
 
 func init() {
-	deleteCmd.Flags().BoolVarP(&shouldOpen, "open", "o", false, "open new note")
 	deleteCmd.Flags().StringVarP(&vaultName, "vault", "v", "", "vault name")
+	deleteCmd.Flags().BoolVarP(&deleteForce, "force", "f", false, "skip confirmation if the note has incoming links")
 	rootCmd.AddCommand(deleteCmd)
 }
diff --git a/cmd/move.go b/cmd/move.go
index 114dc62..8e23fa5 100644
--- a/cmd/move.go
+++ b/cmd/move.go
@@ -1,20 +1,33 @@
 package cmd
 
 import (
+	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
 
 var shouldOpen bool
 var moveCmd = &cobra.Command{
-	Use:     "move",
+	Use:     "move <source> <destination>",
 	Aliases: []string{"m"},
-	Short:   "Move or rename note in vault and updated corresponding links",
-	Args:    cobra.ExactArgs(2),
-	Run: func(cmd *cobra.Command, args []string) {
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
+	Args: cobra.ExactArgs(2),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		currentName := args[0]
 		newName := args[1]
 		vault := obsidian.Vault{Name: vaultName}
@@ -22,7 +35,7 @@ var moveCmd = &cobra.Command{
 		uri := obsidian.Uri{}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
-			log.Fatalf("Failed to parse --editor flag: %v", err)
+			return fmt.Errorf("failed to parse --editor flag: %w", err)
 		}
 		params := actions.MoveParams{
 			CurrentNoteName: currentName,
@@ -30,11 +43,7 @@ var moveCmd = &cobra.Command{
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
 
diff --git a/cmd/open.go b/cmd/open.go
index 9a10bff..d4ac325 100644
--- a/cmd/open.go
+++ b/cmd/open.go
@@ -1,8 +1,6 @@
 package cmd
 
 import (
-	"log"
-
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
 	"github.com/spf13/cobra"
@@ -10,19 +8,28 @@ import (
 
 var vaultName string
 var OpenVaultCmd = &cobra.Command{
-	Use:     "open",
+	Use:     "open <note>",
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
+	Args: cobra.ExactArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		vault := obsidian.Vault{Name: vaultName}
 		uri := obsidian.Uri{}
 		noteName := args[0]
 		params := actions.OpenParams{NoteName: noteName}
-		err := actions.OpenNote(&vault, &uri, params)
-		if err != nil {
-			log.Fatal(err)
-		}
+		return actions.OpenNote(&vault, &uri, params)
 	},
 }
 
diff --git a/cmd/print.go b/cmd/print.go
index 4b40d90..652d5e1 100644
--- a/cmd/print.go
+++ b/cmd/print.go
@@ -2,20 +2,35 @@ package cmd
 
 import (
 	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
 
 var shouldRenderMarkdown bool
 var printCmd = &cobra.Command{
-	Use:     "print",
+	Use:     "print <note>",
 	Aliases: []string{"p"},
 	Short:   "Print contents of note",
-	Args:    cobra.ExactArgs(1),
-	Run: func(cmd *cobra.Command, args []string) {
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
+	Args: cobra.ExactArgs(1),
+	RunE: func(cmd *cobra.Command, args []string) error {
 		noteName := args[0]
 		vault := obsidian.Vault{Name: vaultName}
 		note := obsidian.Note{}
@@ -24,9 +39,10 @@ var printCmd = &cobra.Command{
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
 
diff --git a/cmd/search.go b/cmd/search.go
index 47740f5..571b243 100644
--- a/cmd/search.go
+++ b/cmd/search.go
@@ -1,9 +1,10 @@
 package cmd
 
 import (
+	"fmt"
+
 	"github.com/Yakitrak/obsidian-cli/pkg/actions"
 	"github.com/Yakitrak/obsidian-cli/pkg/obsidian"
-	"log"
 
 	"github.com/spf13/cobra"
 )
@@ -12,20 +13,29 @@ var searchCmd = &cobra.Command{
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
 		note := obsidian.Note{}
 		uri := obsidian.Uri{}
 		fuzzyFinder := obsidian.FuzzyFinder{}
 		useEditor, err := cmd.Flags().GetBool("editor")
 		if err != nil {
-			log.Fatalf("failed to retrieve 'editor' flag: %v", err)
-		}
-		err = actions.SearchNotes(&vault, &note, &uri, &fuzzyFinder, useEditor)
-		if err != nil {
-			log.Fatal(err)
+			return fmt.Errorf("failed to retrieve 'editor' flag: %w", err)
 		}
+		return actions.SearchNotes(&vault, &note, &uri, &fuzzyFinder, useEditor)
 	},
 }
 
diff --git a/cmd/search_content.go b/cmd/search_content.go
index dd7b218..6cfd2d1 100644
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
+	Use:   "search-content <term>",
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
index f2cb992..0c855f6 100644
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
+	Use:     "set-default <vault>",
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
 
diff --git a/pkg/actions/append.go b/pkg/actions/append.go
new file mode 100644
index 0000000..fc1427e
--- /dev/null
+++ b/pkg/actions/append.go
@@ -0,0 +1,158 @@
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
+// AppendToDailyNote appends content to today's daily note, using per-vault settings.
+func AppendToDailyNote(vault *obsidian.Vault, content string) error {
+	content = strings.TrimSpace(content)
+	if content == "" {
+		return errors.New("no content provided")
+	}
+
+	vaultPath, err := vault.Path()
+	if err != nil {
+		return err
+	}
+
+	settings, err := vault.Settings()
+	if err != nil {
+		return err
+	}
+
+	folder := strings.TrimSpace(settings.DailyNote.Folder)
+	if folder == "" {
+		return errors.New("daily note is not configured (missing daily_note.folder in preferences.json)")
+	}
+
+	pattern := strings.TrimSpace(settings.DailyNote.FilenamePattern)
+	if pattern == "" {
+		pattern = "{YYYY-MM-DD}"
+	}
+
+	filename := expandDailyFilename(pattern, time.Now())
+	relNotePath := filepath.ToSlash(filepath.Join(folder, filename))
+	abs, err := safeJoinVaultPath(vaultPath, relNotePath)
+	if err != nil {
+		return err
+	}
+	if !strings.HasSuffix(strings.ToLower(abs), ".md") {
+		abs += ".md"
+	}
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
+func expandDailyFilename(pattern string, now time.Time) string {
+	out := pattern
+	out = strings.ReplaceAll(out, "{YYYY-MM-DD}", now.Format("2006-01-02"))
+	out = strings.ReplaceAll(out, "YYYY-MM-DD", now.Format("2006-01-02"))
+	return out
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
diff --git a/pkg/actions/append_test.go b/pkg/actions/append_test.go
new file mode 100644
index 0000000..52de227
--- /dev/null
+++ b/pkg/actions/append_test.go
@@ -0,0 +1,99 @@
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
diff --git a/pkg/actions/delete.go b/pkg/actions/delete.go
index 7fe630c..1b606bf 100644
--- a/pkg/actions/delete.go
+++ b/pkg/actions/delete.go
@@ -1,12 +1,17 @@
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
 
 func DeleteNote(vault obsidian.VaultManager, note obsidian.NoteManager, params DeleteParams) error {
@@ -21,9 +26,36 @@ func DeleteNote(vault obsidian.VaultManager, note obsidian.NoteManager, params D
 	}
 	notePath := filepath.Join(vaultPath, params.NotePath)
 
+	if !params.Force {
+		links, err := obsidian.FindIncomingLinks(vaultPath, params.NotePath)
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
 	err = note.Delete(notePath)
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
diff --git a/pkg/obsidian/note.go b/pkg/obsidian/note.go
index 286c06d..3608dd9 100644
--- a/pkg/obsidian/note.go
+++ b/pkg/obsidian/note.go
@@ -116,14 +116,8 @@ func (m *Note) UpdateLinks(vaultPath string, oldNoteName string, newNoteName str
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

### Full Diff Command (includes vendor/)
```
git diff --no-color origin/main..pr-04b-append
```
