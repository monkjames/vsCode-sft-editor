# SWG STF Editor for VS Code

A Visual Studio Code extension for editing Star Wars Galaxies String Table Files (.stf). Provides an Excel-like two-column editor for managing game string translations and text content.

## About

STF (String Table File) is a binary format used by Star Wars Galaxies to store localized strings. Each entry consists of:
- **Key (ID)**: A unique ASCII identifier used to reference the string in code
- **Value**: The actual text content (supports Unicode via UTF-16LE encoding)

This extension replaces the need for external hex editors or command-line tools by providing a native VS Code editing experience with validation, search, and full undo/redo support.

## Features

| Feature | Description |
|---------|-------------|
| Visual Editor | Two-column table with editable Key and Value fields |
| Unique Key Validation | Real-time duplicate detection with error highlighting |
| Search & Filter | Filter entries by key or value text |
| Add/Delete Rows | Toolbar buttons and keyboard shortcuts |
| Auto-resize | Value cells expand automatically for multi-line content |
| Undo/Redo | Full integration with VS Code's edit history |
| Theme Support | Automatically matches your VS Code color theme |

## Installation

### From Source

1. Clone the repository:
   ```bash
   git clone git@github.com:monkjames/vsCode-sft-editor.git
   cd vsCode-sft-editor
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Compile TypeScript:
   ```bash
   npm run compile
   ```

4. Install in VS Code (choose one method):

   **Method A - Development Mode:**
   - Open the extension folder in VS Code
   - Press `F5` to launch Extension Development Host
   - Open any `.stf` file to test

   **Method B - Package and Install:**
   ```bash
   npm install -g @vscode/vsce
   vsce package
   ```
   Then in VS Code: `Extensions` → `...` menu → `Install from VSIX...` → select the generated `.vsix` file

### From VSIX (Pre-built)

1. Download the `.vsix` file from Releases
2. In VS Code: `Extensions` → `...` menu → `Install from VSIX...`
3. Select the downloaded file

## Usage

### Opening STF Files

Simply open any `.stf` file in VS Code. The extension automatically activates and displays the visual editor.

### Editing Entries

- **Edit Key**: Click on any key cell and type. Keys must be unique - duplicates are highlighted in red.
- **Edit Value**: Click on any value cell and type. Cells auto-expand for multi-line content.

### Toolbar Actions

| Button | Action |
|--------|--------|
| **+ Add** | Adds a new entry at the bottom with a unique default key |
| **Delete** | Removes the currently selected row |
| **Search box** | Filters visible entries by key or value |

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+N` | Add new entry |
| `Ctrl+Delete` | Delete selected entry |
| `Ctrl+S` | Save file |
| `Ctrl+Z` | Undo |
| `Ctrl+Shift+Z` | Redo |

### Workflow Example

1. Open `string/en/item_n.stf` in VS Code
2. Use the search box to find entries containing "sword"
3. Click on a value cell to edit the item name
4. Press `Ctrl+S` to save changes back to the binary STF format

## File Format Reference

STF files use a binary format (little-endian):

```
Header:
  - Magic: 0xABCD (2 bytes)
  - Padding: 2 bytes
  - Version: 1 byte
  - NextUID: 4 bytes
  - NumStrings: 4 bytes

Value Section (repeated NumStrings times):
  - Index: 4 bytes
  - Key: 4 bytes (0xFFFFFFFF)
  - StringLen: 4 bytes
  - Value: StringLen * 2 bytes (UTF-16LE)

ID Section (repeated NumStrings times):
  - Index: 4 bytes
  - IDLen: 4 bytes
  - ID: IDLen bytes (ASCII)
```

## Development

### Project Structure

```
vscode-stf-editor/
├── src/
│   ├── extension.ts         # Extension entry point
│   ├── stfEditorProvider.ts # Custom editor + webview UI
│   └── stfParser.ts         # Binary STF parser/serializer
├── package.json             # Extension manifest
└── tsconfig.json            # TypeScript configuration
```

### Building

```bash
npm run compile    # One-time build
npm run watch      # Watch mode for development
```

### Debugging

1. Open the extension folder in VS Code
2. Press `F5` to launch the Extension Development Host
3. Set breakpoints in the TypeScript source files

## License

MIT

## Acknowledgments

- SWGEmu community for STF format documentation
- Built for the Infinity server project
