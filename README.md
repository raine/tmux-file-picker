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

**macOS users:** The script uses `grealpath` for path resolution. Install it via
Homebrew:

```sh
brew install coreutils
```

## Setup

Add a key binding to your `~/.tmux.conf` file to launch the file picker in a
popup window over your current terminal session.

```tmux
# ~/.tmux.conf

bind C-f display-popup -E "tmux-file-picker"

# Use -g flag to show paths relative to git root
bind C-g display-popup -E "tmux-file-picker -g"

# Use --zoxide to select from your frecent directories first
bind C-z display-popup -E "tmux-file-picker --zoxide"

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
```

This is particularly useful when you frequently switch between different
projects. The `--zoxide` flag can be combined with `--git-root` to show file
paths relative to the git repository root.

### Customizing fd flags

You can customize the file discovery behavior by setting the
`TMUX_FILE_PICKER_FD_FLAGS` environment variable. The default is
`-H --type f --exclude .git` (show hidden files, only files, exclude .git
directory).

## Lore

https://github.com/Aider-AI/aider/issues/2156#issuecomment-2439919513

## See also

- https://github.com/raine/consult-llm-mcp
- https://github.com/raine/tmux-inspect

## License

MIT
