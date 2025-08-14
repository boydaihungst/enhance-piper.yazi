# enhance-piper.yazi
Piper is plugin to run any shell command as a previewer.
And this enhance-piper is a wrapper to cache piper commands's output to ram for better performance.
Next time you scrolling or re-render previewer it won't run the shell command again.

## Installation

> [!IMPORTANT]
> - Minimum version: yazi v25.5.31.
> - Requires piper.yazi.

```sh
ya pkg add yazi-rs/plugins:piper
ya pkg add boydaihungst/enhance-piper
```

## Usage

Enhance-piper is a wrapper for piper.yazi previewer - you can pass any shell command to `enhance-piper` and it will use the command's output as the preview content, and the output of the command will be cached in ram for faster previewer scrolling or re-render previewer.

It accepts a string parameter, which is the shell command to be executed, for example:

```toml
# ~/.config/yazi/yazi.toml
[[plugin.prepend_previewers]]
name = "*"
run  = 'enhance-piper -- echo "$1"'
```

This will set `enhance-piper` as the previewer for all file types and use enhance `$1` (file path) as the preview content.

## Variables

Available variables for command behind `--`:

- `$w`: the width of the preview area.
- `$h`: the height of the preview area.
- `$1`: the path to the file being previewed.

Available arguments for `enhance-piper` command itself:

- `--cache-limit`: the maximum number of cached command's output, default is 100 entries.

## Examples

Here are some configuration examples:

### Preview CSV with [`bat`](https://github.com/sharkdp/bat)

```toml
[[plugin.prepend_previewers]]
name = "*.csv"
run  = 'enhance-piper -- bat -p --color=always "$1"'
```

Note that certain distributions might use a different name for `bat`, like Debian and Ubuntu uses `batcat` instead, so please adjust accordingly.

### Preview Markdown with [`glow`](https://github.com/charmbracelet/glow)

```toml
[[plugin.prepend_previewers]]
name = "*.md"
run  = 'enhance-piper -- CLICOLOR_FORCE=1 glow -w=$w -s=dark "$1"'
```

Note that there's [a bug in Glow v2.0](https://github.com/charmbracelet/glow/issues/440#issuecomment-2307992634) that causes slight color differences between tty and non-tty environments.

## License

This plugin is MIT-licensed. For more information check the [LICENSE](LICENSE) file.
