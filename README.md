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

## Setup

Add a key binding to your `~/.tmux.conf` file to launch the file picker in a
popup window over your current terminal session.

```tmux
# ~/.tmux.conf

bind C-f display-popup -E "tmux-file-picker"

# Use -g flag to show paths relative to git root
bind C-g display-popup -E "tmux-file-picker -g"

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

## Lore

https://github.com/Aider-AI/aider/issues/2156#issuecomment-2439919513

## License

MIT
