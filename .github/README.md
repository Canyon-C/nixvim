<div align="center">
    <img src="assets/neovim-flake-logo-work.svg" alt="neovim-flake Logo"  width="200">
</div>

# Nixvim config

My Neovim config using nixvim.

> [!NOTE]  
> The colorscheme in these screenshots is [paradise](https://github.com/paradise-theme/paradise)

<img src="assets/1.png" alt="nvim">

<details>
    <summary>More!</summary>
    <img src="assets/4.png" alt="nvim">
    <img src="assets/2.png" alt="nvim">
    <img src="assets/3.png" alt="nvim">
</details>

## Configuring

To start configuring, just add or modify the nix files in `./config`.
If you add a new configuration file, remember to add it to the
[`config/default.nix`](../config/default.nix) file

### Current plugins

> [!WARNING]
> Some of them might be disabled, this is every plugins defined and configured in the repo.

<details>
    <summary>List of plugins</summary>

- **[colorscheme/](../config/plug/colorscheme):** Theme configuration. Current one is [paradise](https://github.com/paradise-theme/paradise)
- **[completion/](../config/plug/completion)**
  - **[avante](../config/plug/completion/avante.nix):** Cursor AI at home
  - **[copilot-cmp](../config/plug/completion/copilot-cmp.nix):** Completion support for GitHub copilot
  - **[lspkind](../config/plug/completion/lspkind.nix):** vscode-like pictograms for neovim lsp completion items
  - **[nvim-cmp](../config/plug/completion/cmp.nix):** Completion plugin for nvim + emoji support
  - **[schemastore.nvim](../config/plug/completion/schemastore.nix):** Schemastore integration
- **[git/](../config/plug/git)**
  - **[gitlinker](../config/plug/git/gitlinker.nix):** Generate shareable file permalinks
  - **[gitblame](../config/plug/git/gitblame.nix):** inline git blame
  - **[gitsigns](../config/plug/git/gitsigns.nix):** Git integration for buffers (replaced by mini.diff + gitblame)
- **[lsp/](../config/plug/lsp)**
  - **[conform](../config/plug/lsp/conform.nix):** Formatter plugin
  - **[lsp](../config/plug/lsp/lsp.nix):** LSP configs
  - **[lspsaga](../config/plug/lsp/lspsaga.nix):** Cool LSP features
  - **[none-ls](../config/plug/lsp/none-ls.nix):** null-ls replacement. Use nvim as LSP
  - **[trouble](../config/plug/lsp/trouble.nix):** Pretty interface for working with LSP
- **[snacks](../config/plug/snacks)**
  - set of utilities
- **[snippet/](../config/plug/snippet)**
  - **[luasnip](../config/plug/snippet/luasnip.nix):** Snippet engine in lua
- **[statusline/](../config/plug/statusline)**
  - **[lualine](../config/plug/statusline/lualine.nix):** Status line for neovim
- **[treesitter/](../config/plug/treesitter)**
  - **[treesitter-context](../config/plug/treesitter/treesitter-context.nix):** Show code context
  - **[treesitter-textobject](../config/plug/treesitter/treesitter-textobject.nix):** Allow cool text manipulation thanks to TS
  - **[treesitter](../config/plug/treesitter/treesitter.nix):** Parser generator tool to build a syntax tree of the current buffer
- **[ui/](../config/plug/ui)**
  - **[bufferline](../config/plug/ui/bufferline.nix):** VSCode like line for buffers -> replaced by mini.tabline
  - **[dressing](../config/plug/ui/dressing.nix):** Better vim ui interfaces
  - **[noice](../config/plug/ui/noice.nix):** Better nvim UI
  - **[nvim-notify](../config/plug/ui/nvim-notify.nix):** Notification manager
  - **[smart-splits](../config/plug/ui/smart-splits.nix):** Better split managment
  - **[telescope](../config/plug/ui/telescope.nix):** Best plugin ever ?
- **[utils/](../config/plug/utils)**
  - **[comment](../config/plug/utils/comment.nix):** Quickly toggle comments
  - **[comment-box](../config/plug/utils/comment-box.nix):** Comments utilities
  - **[markview](../config/plug/utils/markview.nix):** Yet another markdown previewer for neovim
  - **[mini](../config/plug/utils/mini.nix):** Cool neovim utilities, currently using ai, notify, surround, diff, tabline, trailspace, icons, indentscope and pairs
  - **[nvim-colorizer](../config/plug/utils/nvim-colorizer.nix):** Preview colors in neovim
  - **[obsidian](../config/plug/utils/obsidian.nix):** Obsidian integration for nvim
  - **[oil](../config/plug/utils/oil.nix):** Navigate in your working folder with a buffer
  - **[spectre](../config/plug/utils/spectre.nix):** Search and replace
  - **[ufo](../config/plug/utils/ufo.nix):** Folding plugin
  - **[undotree](../config/plug/utils/undotree.nix):** Undo history visualizer

</details>

## Testing your new configuration

To test your configuration simply run the following command

```
nix run .
```

If you have nix installed, you can directly run my config from anywhere

You can try running mine with:

```shell
nix run 'github:Ryder-C/nixvim'
```

## Installing into NixOS configuration

This `nixvim` flake will output a derivation that you can easily include
in either `home.packages` for `home-manager`, or
`environment.systemPackages` for `NixOS`. Or whatever happens with darwin?

You can add my `nixvim` configuration as an input to your `NixOS` configuration like:

```nix
{
 inputs = {
    nixvim.url = "github:Ryder-C/nixvim";
 };
}
```

### Direct installation

With the input added you can reference it directly.

```nix
{ inputs, system, ... }:
{
  # NixOS
  environment.systemPackages = [ inputs.nixvim.packages.${pkgs.system}.default ];
  # home-manager
  home.packages = [ inputs.nixvim.packages.${pkgs.system}.default ];
}
```

The binary built by `nixvim` is already named as `nvim` so you can call it just
like you normally would.

### Installing as an overlay

Another method is to overlay your custom build over `neovim` from `nixpkgs`.

This method is less straight-forward but allows you to install `neovim` like
you normally would. With this method you would just install `neovim` in your
configuration (`home.packges = with pkgs; [ neovim ]`), but you replace
`neovim` in `pkgs` with your derivation from `nixvim`.

```nix
{
  pkgs = import inputs.nixpkgs {
    overlays = [
      (final: prev: {
        neovim = inputs.nixvim.packages.${pkgs.system}.default;
      })
    ];
  }
}
```

### Bonus lazy method

You can just straight up alias something like `nix run
'github:Ryder-C/nixvim'` to `nvim`.

### Bonus extend method

If you want to extend this configuration is your own NixOS config, you can do so using `nixvimExtend`. See [here](https://nix-community.github.io/nixvim/modules/standalone.html) for more info.

Example for overwriting the theme

```nix
{
  inputs,
  lib,
  ...
}: let
  nixvim' = inputs.nixvim.packages."x86_64-linux".default;
  nvim = nixvim'.nixvimExtend {
    config.theme = lib.mkForce "jellybeans";
  };
in {
  home.packages = [
    nvim
  ];
}
```

## Credits

- [yavko](https://github.com/yavko) for the logo
- [nixvim](https://github.com/nix-community/nixvim) and all their maintainers/contributors
