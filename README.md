# NeoVim Configuration

This repository is my personal NeoVim setup, built around `lazy.nvim` and tuned for fast navigation, LSP-driven editing, and Git-heavy development.

> This config is tailored to my workflow, so some defaults and keymaps may not match yours exactly.

## Requirements

- NeoVim **0.11** or newer (uses `vim.lsp.config` and `vim.diagnostic.jump`)
- Git
- [ripgrep](https://github.com/BurntSushi/ripgrep) — used by Telescope live grep, `grug-far.nvim`, and `blink-ripgrep`
- A [Nerd Font](https://www.nerdfonts.com/) (the completion menu uses the `mono` variant)
- Formatters used by `conform.nvim` available on your `PATH` (see [Formatting](#formatting)); they can be installed via `:Mason`

## Installation

1. Clone this repository into your NeoVim config directory:

    ```sh
    git clone git@github.com:khenzy-d/nvim_config.git ~/.config/nvim
    ```

2. Launch NeoVim. `lazy.nvim` bootstraps itself and installs all plugins; `mason-lspconfig.nvim` installs the configured language servers.
3. Run `:checkhealth` and `:Lazy sync` if anything is missing or outdated.

## Project structure

```
.
├── init.lua                     # Entry point: disables netrw, loads config and plugin customizations
└── lua
    ├── config
    │   ├── lazy.lua             # lazy.nvim bootstrap, leader keys, plugin spec imports
    │   ├── settings.lua         # Editor options and colorscheme
    │   └── keybinds.lua         # Key mappings
    ├── plugins
    │   └── plugins.lua          # Plugin specs
    ├── themes
    │   └── themes.lua           # Colorscheme plugin specs
    └── customize                # Per-plugin setup
        ├── conform-setup.lua    # Formatters per filetype
        ├── diffview-setup.lua
        ├── mason-setup.lua      # Per-server LSP settings
        ├── mini-setup.lua
        ├── oil-setup.lua
        └── telescope-setup.lua
```

## Plugins

### Git

- `tpope/vim-fugitive` — Git commands from inside NeoVim
- `sindrets/diffview.nvim` — diff and file history views (icons disabled)
- `lewis6991/gitsigns.nvim` — git signs, inline blame, and buffer diffs

### Editor

- `HiPhish/rainbow-delimiters.nvim` — colored bracket pairs
- `saghen/blink.indent` — indent guides with highlighted active scope
- `MagicDuck/grug-far.nvim` — project-wide search and replace

### Mini.nvim

- `nvim-mini/mini.pairs` — auto-pairs
- `nvim-mini/mini.comment` — comment toggling
- `nvim-mini/mini.surround` — surround operations
- `nvim-mini/mini.statusline` — statusline
- `nvim-mini/mini.tabline` — tabline
- `nvim-mini/mini.icons` — icons (used by Oil)

### Completion and snippets

- `saghen/blink.cmp` — completion engine (Rust fuzzy matcher preferred)
- `rafamadriz/friendly-snippets` — snippet library
- `mikavilpas/blink-ripgrep.nvim` — ripgrep-powered completion source

Completion sources: `lsp`, `path`, `snippets`, `buffer`, `ripgrep`. Uses the `default` keymap preset, plus `<C-e>` to show snippets only.

### LSP

- `mason-org/mason.nvim` — tool installer
- `mason-org/mason-lspconfig.nvim` — installs and enables LSP servers
- `neovim/nvim-lspconfig` — server configurations

Servers installed automatically:

- `bashls`
- `docker_compose_language_service`
- `gopls` (with `gofumpt`)
- `jsonls`
- `lua_ls`
- `pylsp` (pycodestyle: max line length 120, `W391` ignored)
- `roslyn_ls`
- `tombi` (TOML)
- `ts_ls`
- `yamlls`

Per-server settings live in `lua/customize/mason-setup.lua`.

### Navigation and file browsing

- `nvim-telescope/telescope.nvim` — fuzzy finding for files, grep, diagnostics, and git
- `nvim-lua/plenary.nvim` — Telescope dependency
- `stevearc/oil.nvim` — buffer-based file explorer (shows hidden files, permissions, size, and mtime columns); replaces netrw, which is disabled

### Formatting

`stevearc/conform.nvim` is configured in `lua/customize/conform-setup.lua`:

| Filetype                                                                  | Formatter                    |
| ------------------------------------------------------------------------- | ---------------------------- |
| all                                                                       | `trim_whitespace`, `trim_newlines` |
| lua                                                                       | `stylua` (4 spaces)          |
| markdown, json, yaml, html, css, javascript(react), typescript(react)     | `prettierd`, else `prettier` |
| python                                                                    | `black`                      |
| go                                                                        | `gofmt`                      |
| sh                                                                        | `shfmt`                      |
| toml                                                                      | `tombi`                      |
| xml, xaml                                                                 | `xmlformatter`               |
| nginx                                                                     | `nginxfmt`                   |

### Themes

Colorschemes are defined in `lua/themes/themes.lua`. The default is `ayu-mirage` (set in `lua/config/settings.lua`) with transparent backgrounds.

Available themes:

- `shatur/neovim-ayu` (default, transparent overrides)
- `catppuccin/nvim` (transparent)
- `d-bastian/gruber-dark.nvim` (transparent)
- `Mofiqul/adwaita.nvim`
- `projekt0n/github-nvim-theme`
- `tanvirtin/monokai.nvim`
- `ellisonleao/gruvbox.nvim`
- `p00f/alabaster.nvim`
- `Mofiqul/dracula.nvim`
- `olimorris/onedarkpro.nvim`

To change the theme, edit the `vim.cmd.colorscheme(...)` line in `lua/config/settings.lua`.

## Editor settings

Defined in `lua/config/settings.lua` and `lua/config/lazy.lua`:

- leader key: `,` — local leader: `.`
- system clipboard via `unnamedplus`
- absolute + relative line numbers, sign column always shown
- 4-space indentation, tabs expanded to spaces
- smart case-insensitive, incremental, highlighted search
- indent-based folding, all folds open by default
- no line wrapping, `cursorline` enabled, `colorcolumn` at 160
- `scrolloff` / `sidescrolloff` set to 8
- splits open below/right
- persistent undo; swap and backup files disabled
- `termguicolors` and mouse support enabled
- `zsh` used as the shell when available

## Keybindings

Defined in `lua/config/keybinds.lua`.

### General

| Key          | Mode     | Action                          |
| ------------ | -------- | ------------------------------- |
| `<Esc>`      | terminal | Exit terminal mode              |
| `<leader>fm` | normal   | Format buffer via LSP           |
| `<leader>fc` | normal   | Format buffer via `conform.nvim` |
| `<leader>sr` | normal   | Search and replace (`grug-far`) |
| `<leader>cp` | normal   | Copy current file path          |
| `<leader>o`  | normal   | Open Oil                        |
| `J` / `K`    | visual   | Move selected lines down / up   |

### Buffers, tabs, and quickfix

| Key          | Action          |
| ------------ | --------------- |
| `<leader>b`  | Next buffer     |
| `<leader>t`  | New tab         |
| `<leader>tq` | Close tab       |
| `<leader>tn` | Next tab        |
| `<leader>tp` | Previous tab    |
| `<leader>qo` | Open quickfix   |
| `<leader>qc` | Close quickfix  |

### Windows and movement

| Key                              | Action                          |
| -------------------------------- | ------------------------------- |
| `<C-d>` / `<C-u>`                | Half-page down / up, centered   |
| `<C-h>` `<C-j>` `<C-k>` `<C-l>`  | Move to left/lower/upper/right window |

### Diagnostics

| Key          | Action                   |
| ------------ | ------------------------ |
| `<leader>do` | Open diagnostic float    |
| `<leader>dp` | Previous diagnostic      |
| `<leader>dn` | Next diagnostic          |

### Telescope

| Key          | Action                  |
| ------------ | ----------------------- |
| `<leader>ff` | Find files              |
| `<leader>fg` | Find git files          |
| `<leader>fl` | Live grep               |
| `<leader>fw` | Find word under cursor  |
| `<leader>fb` | Find buffers            |
| `<leader>fd` | Find diagnostics        |
| `<leader>gd` | Go to definition        |
| `<leader>gc` | Git commits             |

### Git

| Key          | Action                              |
| ------------ | ----------------------------------- |
| `<leader>dv` | Open Diffview                       |
| `<leader>fh` | File history (repository)           |
| `<leader>df` | Current file history                |
| `<leader>tb` | Toggle current line blame           |
| `<leader>tg` | Full blame for current buffer       |
| `<leader>hd` | Diff current buffer                 |

## Customizing

- Add or remove plugins: `lua/plugins/plugins.lua`
- Add colorschemes: `lua/themes/themes.lua`
- Plugin-specific setup: `lua/customize/*.lua` (remember to `require` new files in `init.lua`)
- Options: `lua/config/settings.lua`
- Keymaps: `lua/config/keybinds.lua`
