# stdin-piper.yazi

Pipe any shell command as a previewer, pre-slice for better performance.

## Installation

> [!IMPORTANT]
> Minimum version: yazi v25.5.31.
>
> Requires awk:
> Linux by default (GNU)
> macOS (BSD version)
> Windows (Git Bash/MSYS2 awk)

```sh
ya pkg add boydaihungst/stdin-piper.yazi
```

## Usage

Stdin-piper is a general-purpose previewer - you can pass any shell command to `stdin-piper` and it will use the command's output as the preview content.

It accepts a string parameter, which is the shell command to be executed, the shell command should only read data from stdin (like `glow -` instead of `glow "$1"`), for example:

```toml
# ~/.config/yazi/yazi.toml
[[plugin.prepend_previewers]]
name = "*"
run  = 'stdin-piper -- "$HOME/.config/yazi/plugins/stdin-piper.yazi/assets/big_file_slice.sh" "$1" ${start} ${end} | CLICOLOR_FORCE=1 glow -w=$w -s=dark -'
# On windows, use `$APPDATA` instead of `$HOME/.config`
# run  = 'stdin-piper -- $APPDATA/yazi/plugins/stdin-piper.yazi/assets/big_file_slice.sh "$1" ${start} ${end} | CLICOLOR_FORCE=1 glow -w=$w -s=dark -'
```

This will set `stdin-piper` as the previewer for all file types and use stdin `-` as the preview content.

## Variables

Available variables:

- `$w`: the width of the preview area.
- `$h`: the height of the preview area.
- `$1`: the path to the file being previewed.
- `$start`: start line number to be previewed.
- `$end`: end line number to to be previewed.

## Examples

Here are some configuration examples:

### Preview CSV with [`bat`](https://github.com/sharkdp/bat)

```toml
[[plugin.prepend_previewers]]
name = "*.csv"
run  = 'stdin-piper -- "$HOME/.config/yazi/plugins/stdin-piper.yazi/assets/big_file_slice.sh" "$1" ${start} ${end} | bat -p --color=always -'
```

Note that certain distributions might use a different name for `bat`, like Debian and Ubuntu uses `batcat` instead, so please adjust accordingly.

### Preview Markdown with [`glow`](https://github.com/charmbracelet/glow)

```toml
[[plugin.prepend_previewers]]
name = "*.md"
run  = 'stdin-piper -- "$HOME/.config/yazi/plugins/stdin-piper.yazi/assets/big_file_slice.sh" "$1" ${start} ${end} | CLICOLOR_FORCE=1 glow -w=$w -s=dark -'
```

Note that there's [a bug in Glow v2.0](https://github.com/charmbracelet/glow/issues/440#issuecomment-2307992634) that causes slight color differences between tty and non-tty environments.

## License

This plugin is MIT-licensed. For more information check the [LICENSE](LICENSE) file.
