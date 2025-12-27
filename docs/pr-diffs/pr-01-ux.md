# pr-01-ux

Baseline: origin/main (1b06c32570b17684ba08b958f86368577e87fd20)
Previous: origin/main (1b06c32570b17684ba08b958f86368577e87fd20)
This PR: pr-01-ux (ee4eee9d1b43bc8e4ce87e2368e67398579a2d99)

## Incremental Diff (vs previous in stack)

Compare: origin/main..pr-01-ux

### Commits
```
ee4eee9 docs: refresh README (pr-01-ux)
06d79e8 docs: README mention command-specific help
d9d3fa9 feat(cli): improve help and error handling
```

### Diff Stat (all files)
```
 README.md             | 30 +++++++++++++++++++++++++++++-
 cmd/create.go         | 35 ++++++++++++++++++++++++++---------
 cmd/move.go           | 31 ++++++++++++++++++++-----------
 cmd/open.go           | 25 ++++++++++++++++---------
 cmd/print.go          | 26 +++++++++++++++++++++-----
 cmd/print_default.go  | 22 +++++++++++++++-------
 cmd/search.go         | 26 ++++++++++++++++++--------
 cmd/search_content.go | 27 ++++++++++++++++++---------
 cmd/set_default.go    | 26 +++++++++++++++++---------
 9 files changed, 180 insertions(+), 68 deletions(-)
```

### Diff Stat (vendor only)
```
```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index 591f921..3711fca 100644
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
 
```

## Cumulative Diff (vs origin/main)

Compare: origin/main..pr-01-ux

### Commits
```
ee4eee9 docs: refresh README (pr-01-ux)
06d79e8 docs: README mention command-specific help
d9d3fa9 feat(cli): improve help and error handling
```

### Diff Stat (all files)
```
 README.md             | 30 +++++++++++++++++++++++++++++-
 cmd/create.go         | 35 ++++++++++++++++++++++++++---------
 cmd/move.go           | 31 ++++++++++++++++++++-----------
 cmd/open.go           | 25 ++++++++++++++++---------
 cmd/print.go          | 26 +++++++++++++++++++++-----
 cmd/print_default.go  | 22 +++++++++++++++-------
 cmd/search.go         | 26 ++++++++++++++++++--------
 cmd/search_content.go | 27 ++++++++++++++++++---------
 cmd/set_default.go    | 26 +++++++++++++++++---------
 9 files changed, 180 insertions(+), 68 deletions(-)
```

### Diff Stat (vendor only)
```
```

### Patch (excluding vendor/)
```diff
diff --git a/README.md b/README.md
index 591f921..3711fca 100644
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
 
```
