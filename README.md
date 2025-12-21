# tmux-file-picker

Pop up fzf in tmux to quickly insert file paths, perfect for AI coding
assistants like Claude Code.

https://github.com/user-attachments/assets/df04f352-0c33-4987-bd23-caa19ee019a4

## Installation

1.  Download the script:
    ```sh
    curl -O https://raw.githubusercontent.com/raine/tmux-file-picker/main/tmux-file-picker
    ```
2.  Make it executable:
    ```sh
    chmod +x tmux-file-picker
    ```
3.  Move it to a directory in your `PATH`:
    ```sh
    mv tmux-file-picker ~/.local/bin/
    ```

## Dependencies

- [fzf](https://github.com/junegunn/fzf): For the fuzzy-finding interface.
- [fd](https://github.com/sharkdp/fd): For file discovery.
- [bat](https://github.com/sharkdp/bat) (optional): For syntax-highlighted file
  previews.
- [zoxide](https://github.com/ajeetdsouza/zoxide) (optional): For the `--zoxide`
  flag to select directories from your frecent list.
- [tree](https://gitlab.com/OldManProgrammer/unix-tree) (optional): For better
  directory previews when using `--zoxide`.

**macOS users:** `grealpath` is required when using `--git-root` or passing a
directory argument. Install it via Homebrew:

```sh
brew install coreutils
```

## Setup

Add a key binding to your `~/.tmux.conf` to launch the file picker. When
triggered, it opens an fzf interface in a popup overlay, letting you select
files without leaving your current session.

Choose from the examples below based on your workflow:

```tmux
# ~/.tmux.conf

bind C-f display-popup -E "tmux-file-picker"

# Use -g flag to show paths relative to git root
bind C-g display-popup -E "tmux-file-picker -g"

# Use --directories to select directories instead of files
bind C-d display-popup -E "tmux-file-picker --directories"

# Use --zoxide to select from your frecent directories first
bind C-z display-popup -E "tmux-file-picker --zoxide"

# Use --zoxide with --dir-only to just insert the directory path
bind C-v display-popup -E "tmux-file-picker --zoxide --dir-only"

# Combine --zoxide with --git-root
bind C-x display-popup -E "tmux-file-picker --zoxide --git-root"

# Popup dimensions can be configured like this:
# bind C-f display-popup -h 60% -w 60% -E "tmux-file-picker"
```

Reload your tmux configuration for the changes to take effect:
`tmux source-file ~/.tmux.conf`

## Usage

1.  While typing a message to your AI assistant, press your configured tmux key
    binding (`Ctrl-f` in the example above).
2.  An `fzf` window will pop up, listing all files in the current directory.
3.  Type a few characters to filter the list and find your file.
4.  Press `Enter` to select one file, or `Tab` to select multiple files.
5.  The selected file path(s) will be pasted directly at your cursor position.

### Searching a specific directory

You can pass a path as an argument to search a specific directory instead of the
current pane directory:

```tmux
# Search in a specific directory
bind C-d display-popup -E "tmux-file-picker ~/my-project"

# Combine with --git-root flag
bind C-y display-popup -E "tmux-file-picker --git-root ~/my-project"
```

Both absolute and relative paths are supported. Relative paths are resolved
relative to the current pane directory.

**Note:** If your path contains spaces, it must be quoted inside the command
string:

```tmux
# Example with a quoted path containing spaces
bind C-d display-popup -E "tmux-file-picker '~/my projects/project a'"
```

### Using zoxide for directory selection

The `--zoxide` flag lets you interactively select a directory from your
[zoxide](https://github.com/ajeetdsouza/zoxide) frecent directories before
picking files:

```bash
tmux-file-picker --zoxide
# First: fzf shows your zoxide directories
# Second: fzf shows files from the selected directory
# Returns absolute paths (e.g., /home/user/project/src/file.ts)
```

This is particularly useful when you frequently switch between different
projects. When using `--zoxide`, the script returns absolute paths instead of
relative paths, making it easier to reference files from any directory.

#### Directory-only mode

Use `--dir-only` with `--zoxide` to insert just the directory path without the
file selection step:

```bash
tmux-file-picker --zoxide --dir-only
# Only shows directory selection, skips file picking
# Useful for quickly inserting project paths
```

### Selecting directories

Use `--directories` (or `-d`) to select directories instead of files:

```bash
tmux-file-picker --directories
tmux-file-picker -d

# Can be combined with other flags
tmux-file-picker -d --git-root
```

The preview shows a `tree` view of each directory (falls back to `ls` if tree is
not installed).

### Customizing fd flags

You can customize the file discovery behavior by setting the
`TMUX_FILE_PICKER_FD_FLAGS` environment variable. The default is
`-H --type f --exclude .git` (show hidden files, only files, exclude .git
directory).

## Lore

https://github.com/Aider-AI/aider/issues/2156#issuecomment-2439919513

## Related projects

- [workmux](https://github.com/raine/workmux) — Git worktrees + tmux windows for parallel AI agent workflows
- [consult-llm-mcp](https://github.com/raine/consult-llm-mcp) — MCP server that lets Claude Code consult stronger AI models (o3, Gemini, GPT-5.1 Codex)
- [claude-history](https://github.com/raine/claude-history) — Search and view Claude Code conversation history with fzf
- [tmux-inspect](https://github.com/raine/tmux-inspect) — Inspect objects in tmux popups using jless

## License

MIT
