# Fuzzy find and open most recently used files in vis

Use [fzf](https://github.com/junegunn/fzf) to open most recently used files in [vis](https://github.com/martanne/vis).

## Usage

In `vis`:
- `:fzfmru`
- `:fzfmru-last-used`

While in `fzf`:
- `<Enter>` to open the selected file in current window
- `<C-d>` to delete the selected file from the MRU list
- `<C-s>` to open the selected file in a horizontal split
- `<C-v>` to open the selected file in a vertical split

## Configuration

In `visrc.lua`:

```lua
plugin_vis_mru = require('plugins/fzf-mru')

-- Path to the fzf executable (default: "fzf")
plugin_vis_mru.fzfmru_path = "fzf"

-- Arguments passed to fzf (default: "")
plugin_vis_mru.fzfmru_args = "--delimiter / --nth -1" -- Search only by file names

-- File path to file history (default: "$HOME/.mru")
plugin_vis_mru.fzfmru_filepath = os.getenv("HOME") .. "/.config/vis/mru.txt"

-- The number of most recently used files kept in history (default: 20)
plugin_vis_mru.fzfmru_history = 10

-- Mapping configuration example
vis.events.subscribe(vis.events.INIT, function()
	vis:command('map! normal <Space>h :fzfmru<Enter>')
	vis:command('map! normal <Tab> :fzfmru-last-used<Enter>')
end)
```

Complex example for `plugin_vis_mru.fzfmru_args`:

```lua
my_fzf_args = string.gsub([[
    --bind=$my_fzf_key_bindings \
    --color fg:242,bg:236,hl:65,fg+:15,bg+:239,hl+:108 \
    --color info:108,prompt:109,spinner:108,pointer:168,marker:168 \
    --delimiter / --nth -1 \
    --height=70% \
    --inline-info \
    --no-mouse \
    --preview-window=up:70% \
    --preview="(
        bat --style=changes,grid,numbers --color=always {} ||
        highlight -O ansi -l {} ||
        coderay {} ||
        rougify {} ||
        cat {}
    ) 2> /dev/null | head -1000"
]],
'%$([%w_]+)', {
    my_fzf_key_bindings=table.concat({
        "alt-j:preview-down",
        "alt-k:preview-up",
        "ctrl-f:preview-page-down",
        "ctrl-b:preview-page-up",
        "?:toggle-preview",
        "alt-w:toggle-preview-wrap",
        "ctrl-z:clear-screen"
    }, ",")
})

-- Arguments passed to fzf (default: "")
plugin_vis_mru.fzfmru_args = my_fzf_args
```

## Inspired by

- [vis-fzf-open](https://github.com/guillaumecherel/vis-fzf-open/)
- [vis-cursors](https://github.com/erf/vis-cursors)
