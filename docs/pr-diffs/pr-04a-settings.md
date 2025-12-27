# pr-04a-settings

Baseline: origin/main (1b06c32570b17684ba08b958f86368577e87fd20)
Previous: pr-03-delete (c20affdfe1aad365e123839c2fc8c1d299cf4786)
This PR: pr-04a-settings (90073194af6ef5d8a39abbeee45a2c2dd70e0257)

## Incremental Diff (vs previous in stack)

Compare: pr-03-delete..pr-04a-settings

### Commits
```
9007319 fix(delete): use 'del' alias
e05d883 docs: refresh README (pr-04a-settings)
424ac98 fix(config): enforce secure perms on preferences
f7cb7ea docs: README mention preferences.json and per-vault settings
4f2a0a8 feat(config): add per-vault settings to preferences.json
```

### Diff Stat (all files)
```
 README.md                               | 92 ++++++++++++++++++++++++++++++
 pkg/obsidian/vault.go                   | 18 +++++-
 pkg/obsidian/vault_default_name.go      | 99 +++++++++++++++++++++++++++++++--
 pkg/obsidian/vault_default_name_test.go | 14 +++--
 pkg/obsidian/vault_settings_test.go     | 82 +++++++++++++++++++++++++++
 5 files changed, 296 insertions(+), 9 deletions(-)
```

### Diff Stat (vendor only)
```
```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index e684a37..3faa817 100644
--- a/README.md
+++ b/README.md
@@ -111,6 +111,57 @@ obsidian-cli set-default "{vault-name}"
 
 Note: `open` and other commands in `obsidian-cli` use this vault's base directory as the working directory, not the current working directory of your terminal.
 
+`set-default` stores the default vault name in `preferences.json` under your OS user config directory (`os.UserConfigDir()`), at:
+
+- `obsidian-cli/preferences.json`
+
+PR04a also introduces optional per-vault settings under `vault_settings` (keyed by vault name). For example:
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
@@ -123,6 +174,35 @@ obsidian-cli print-default
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
@@ -134,6 +214,18 @@ obs_cd() {
 
 Then you can use `obs_cd` to navigate to the default vault directory within your terminal.
 
+### Config Files
+
+`obsidian-cli` reads and writes configuration under your OS user config directory (`os.UserConfigDir()`):
+
+- `obsidian-cli/preferences.json` (default vault name + optional per-vault `vault_settings`)
+
+It also reads Obsidian’s vault list from:
+
+- `obsidian/obsidian.json` (Obsidian config, used for vault discovery)
+
+Note: when writing `preferences.json`, the CLI attempts to create the config directory with mode `0750` and the file with mode `0600` (confirmed from `os.MkdirAll(…, 0750)` / `os.WriteFile(…, 0600)` in code).
+
 ### Open Note
 
 Open given note name in Obsidian. Note can also be an absolute path from top level of vault.
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

## Cumulative Diff (vs origin/main)

Compare: origin/main..pr-04a-settings

### Commits
```
9007319 fix(delete): use 'del' alias
e05d883 docs: refresh README (pr-04a-settings)
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
 README.md                               | 171 +++++++++++++++++++++++++++++++-
 cmd/create.go                           |  35 +++++--
 cmd/delete.go                           |  33 ++++--
 cmd/move.go                             |  31 ++++--
 cmd/open.go                             |  25 +++--
 cmd/print.go                            |  26 ++++-
 cmd/print_default.go                    |  22 ++--
 cmd/search.go                           |  26 +++--
 cmd/search_content.go                   |  27 +++--
 cmd/set_default.go                      |  26 +++--
 pkg/actions/delete.go                   |  32 ++++++
 pkg/obsidian/note.go                    |  10 +-
 pkg/obsidian/note_test.go               |  78 +++++++++++++++
 pkg/obsidian/utils.go                   | 142 ++++++++++++++++++++++++++
 pkg/obsidian/utils_test.go              |  97 ++++++++++++++++++
 pkg/obsidian/vault.go                   |  18 +++-
 pkg/obsidian/vault_default_name.go      |  99 +++++++++++++++++-
 pkg/obsidian/vault_default_name_test.go |  14 ++-
 pkg/obsidian/vault_settings_test.go     |  82 +++++++++++++++
 19 files changed, 896 insertions(+), 98 deletions(-)
```

### Diff Stat (vendor only)
```
```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index 591f921..3faa817 100644
--- a/README.md
+++ b/README.md
@@ -2,7 +2,35 @@
 
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
+  completion     Generate the autocompletion script for the specified shell
+  create         Creates note in vault
+  daily          Creates or opens daily note in vault
+  delete         Delete note in vault
+  help           Help about any command
+  move           Move or rename note in vault and update corresponding links
+  open           Opens note in vault by note name
+  print          Print contents of note
+  print-default  Prints default vault name and path
+  search         Fuzzy searches and opens note in vault
+  search-content Search note content for search term
+  set-default    Sets default vault
+
+Flags:
+  -h, --help      help for obsidian-cli
+  -v, --version   version for obsidian-cli
+
+Use "obsidian-cli [command] --help" for more information about a command.
+```
 
 ## Description
 
@@ -83,6 +111,57 @@ obsidian-cli set-default "{vault-name}"
 
 Note: `open` and other commands in `obsidian-cli` use this vault's base directory as the working directory, not the current working directory of your terminal.
 
+`set-default` stores the default vault name in `preferences.json` under your OS user config directory (`os.UserConfigDir()`), at:
+
+- `obsidian-cli/preferences.json`
+
+PR04a also introduces optional per-vault settings under `vault_settings` (keyed by vault name). For example:
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
@@ -95,6 +174,35 @@ obsidian-cli print-default
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
@@ -106,6 +214,18 @@ obs_cd() {
 
 Then you can use `obs_cd` to navigate to the default vault directory within your terminal.
 
+### Config Files
+
+`obsidian-cli` reads and writes configuration under your OS user config directory (`os.UserConfigDir()`):
+
+- `obsidian-cli/preferences.json` (default vault name + optional per-vault `vault_settings`)
+
+It also reads Obsidian’s vault list from:
+
+- `obsidian/obsidian.json` (Obsidian config, used for vault discovery)
+
+Note: when writing `preferences.json`, the CLI attempts to create the config directory with mode `0750` and the file with mode `0600` (confirmed from `os.MkdirAll(…, 0750)` / `os.WriteFile(…, 0600)` in code).
+
 ### Open Note
 
 Open given note name in Obsidian. Note can also be an absolute path from top level of vault.
@@ -121,7 +241,9 @@ obsidian-cli open "{note-name}" --vault "{vault-name}"
 
 ### Daily Note
 
-Open daily note in Obsidian. It will create one (using template) if one does not exist.
+Open the daily note in Obsidian (via Obsidian URI).
+
+Note: creation/templates are controlled by Obsidian’s daily note settings/plugins.
 
 ```bash
 # Creates / opens daily note in obsidian vault
@@ -230,14 +352,55 @@ obsidian-cli move "{current-note-path}" "{new-note-path}" --open --editor
 
 Deletes a given note (path from top level of vault).
 
+If other notes link to the note, `delete` prints the incoming links and prompts for confirmation. The default is **No** (press Enter to cancel).
+
+Use `--force` (`-f`) to skip confirmation (recommended for scripts). Alias: `delete, del`. Heads up: `daily` uses alias `d`, so `delete` uses `del` to avoid ambiguity.
+
 ```bash
-# Renames a note in default obsidian
+# Delete a note in default obsidian
 obsidian-cli delete "{note-path}"
 
-# Renames a note in given obsidian
+# Force delete without prompt
+obsidian-cli delete "{note-path}" --force
+
+# Delete a note in given obsidian
 obsidian-cli delete "{note-path}" --vault "{vault-name}"
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
+  obsidian-cli delete <note> [flags]
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
+  -f, --force          skip confirmation if the note has incoming links
+  -h, --help           help for delete
+  -v, --vault string   vault name
+```
+
+</details>
+
 ## Contribution
 
 Fork the project, add your feature or fix and submit a pull request. You can also open an [issue](https://github.com/yakitrak/obsidian-cli/issues/new/choose) to report a bug or request a feature.
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
index f29f691..6a0b63e 100644
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
-	Aliases: []string{"d"},
+	Use:     "delete <note>",
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
