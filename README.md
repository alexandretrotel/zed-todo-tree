# Todo Tree - Zed Extension

A Zed editor extension that finds and displays TODO-style comments in your codebase using slash commands in the Assistant.

This extension is maintained as a separate repository at [zed-todo-tree](https://github.com/alexandretrotel/zed-todo-tree) and included as a git submodule in the main [todo-tree](https://github.com/alexandretrotel/todo-tree) repository.

Check the original repository for more information.

## Features

### Slash Commands

Use these commands in the Zed Assistant to interact with TODOs in your project:

| Command | Description |
|---------|-------------|
| `/todos` | List all TODO-style comments in the project |
| `/todos [TAG]` | Filter by specific tag (e.g., `/todos BUG`) |
| `/todos-stats` | Show statistics about TODO items |

### Syntax Highlighting

The extension provides syntax highlighting for TODO-style comments directly in your code editor. Tags like `TODO`, `FIXME`, `BUG`, `HACK`, `NOTE`, and others are highlighted with distinct colors, making them easy to spot while browsing your code.

This feature uses Tree-sitter for parsing and is based on the [zed-comment](https://github.com/thedadams/zed-comment) extension's highlighting structure, with additional support for TODO-specific tags.

#### Customizing Colors

You can override the default highlight colors in your Zed theme. Refer to the [zed-comment](https://github.com/thedadams/zed-comment) extension documentation for details on customizing theme colors for syntax highlighting.

## Prerequisites

This extension requires the `todo-tree` CLI to be installed on your system.

### Installation

Install using Cargo:

```bash
cargo install todo-tree
```

After installation, verify it works:

```bash
tt --version
# or
todo-tree --version
```

## Extension Installation

### From Zed Extensions

1. Open Zed
2. Go to Extensions (`Cmd+Shift+X` on macOS)
3. Search for "Todo Tree"
4. Click Install

### As a Dev Extension

For development or testing:

1. Clone the repository:
   ```bash
   git clone https://github.com/alexandretrotel/zed-todo-tree.git
   cd zed-todo-tree
   ```

2. In Zed, open the command palette (`Cmd+Shift+P`)
3. Run "zed: install dev extension"
4. Select the cloned directory

## Usage

1. Open a project in Zed
2. Open the Assistant panel (`Cmd+?` or through the command palette)
3. Type `/todos` and press Enter
4. The Assistant will display all TODO items found in your project

### Filtering by Tag

Type `/todos` followed by a space, and you'll see autocomplete suggestions for tags:

- `/todos BUG` - Show only BUG comments
- `/todos FIXME` - Show only FIXME comments
- `/todos TODO` - Show only TODO comments

### Viewing Statistics

Use `/todos-stats` to see a statistical breakdown:

- Total number of TODO items
- Files with TODOs vs. files scanned
- Average items per file
- Breakdown by tag with percentages
- Priority level reference

## Required Capabilities

The Todo Tree Zed extension requires certain capabilities that you may need to accept when first using it. Zed extensions operate under a [capability system](https://zed.dev/docs/extensions/capabilities) for security.

When prompted, you may need to grant the following permissions:

| Capability | Purpose |
|------------|---------|
| `process:exec` | Execute `todo-tree` or `tt` CLI for slash commands |

You can manage these permissions in your Zed settings under `granted_extension_capabilities`. Add the following to your `settings.json`:

```json
{
  "granted_extension_capabilities": [
    {
      "kind": "process:exec",
      "command": "tt",
      "args": ["**"]
    },
    {
      "kind": "process:exec",
      "command": "todo-tree",
      "args": ["**"]
    },
  ]
}
```

If you restrict these capabilities, the extension may not function properly.

## How It Works

The extension uses the `todo-tree` CLI under the hood:

1. When you invoke a slash command, the extension finds `tt` or `todo-tree` in your PATH
2. It runs `tt scan --json` on your project
3. The JSON output is parsed and formatted as Markdown for the Assistant

The CLI respects `.gitignore` rules and supports many programming languages and comment styles.

## Troubleshooting

### "todo-tree CLI not found"

Make sure the CLI is installed and available in your PATH:

```bash
which tt
# or
which todo-tree
```

If installed via Cargo, ensure `~/.cargo/bin` is in your PATH.

### No TODOs Found

- Check that your files use standard comment syntax (`//`, `#`, `/* */`, etc.)
- Verify the tags are in uppercase (case-insensitive matching is enabled by default)
- Ensure the files aren't being ignored by `.gitignore`

## Acknowledgments

- Tree-sitter grammar for comment parsing from [tree-sitter-comment](https://github.com/stsewd/tree-sitter-comment)
- Syntax highlighting based on [zed-comment](https://github.com/thedadams/zed-comment) - we re-used the same `highlights.scm` structure with additional tags support. For details about overriding theme colors for syntax highlighting, refer to the `zed-comment` extension.

## Contributing

Contributions are welcome! Please see the [zed-todo-tree repository](https://github.com/alexandretrotel/zed-todo-tree) for contribution guidelines.

## License

MIT License - see [LICENSE](https://github.com/alexandretrotel/todo-tree/blob/main/LICENSE) for details.
