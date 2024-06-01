# cmp-npm

This is an additional source for [nvim-cmp](https://github.com/hrsh7th/nvim-cmp), it allows you to
autocomplete [npm](https://npmjs.com/) packages and its versions.
The source is only active if you're in a `package.json` file.

![demo-cmp-npm](https://user-images.githubusercontent.com/1009936/138598207-4855e5b5-1a88-4b02-b43a-c67143527f82.gif)

## Requirements

It needs the Neovim plugin [nvim-cmp](https://github.com/hrsh7th/nvim-cmp) and the [npm](https://npmjs.com/) command line tool.

## Installation

For [vim-plug](https://github.com/junegunn/vim-plug):
```
Plug 'nvim-lua/plenary.nvim'
Plug 'David-Kunz/cmp-npm'
```
For [packer](https://github.com/wbthomason/packer.nvim):
```
use {
  'David-Kunz/cmp-npm',
  requires = {
    'nvim-lua/plenary.nvim'
  }
}
```

For [lazy.nvim](https://github.com/folke/lazy.nvim):

```
{
  "David-Kunz/cmp-npm",
  dependencies = { 'nvim-lua/plenary.nvim' },
  ft = "json",
  config = function()
    require('cmp-npm').setup({})
  end
}
```

Run the `setup` function and add the source
```lua
require('cmp-npm').setup({})
cmp.setup({
  ...,
  sources = {
    { name = 'npm', keyword_length = 4 },
    ...
  }
})
```
(in Vimscript, make sure to add `lua << EOF` before and `EOF` after the lua code)

The `setup` function accepts an options table which defaults to:

```lua
{
  ignore = {},
  only_semantic_versions = false,
  only_latest_version = false
}
```

- `ignore` (table): Allows you to filter out all versions which match one of its entries,
e.g. `ignore = { 'beta', 'rc' }`.
- `only_semantic_versions` (Boolean): If `true`, will filter out all versions which don't follow 
  the `major.minor.patch` schema.
- `only_latest_version` (Boolean): If `true`, will only show latest release version.

### Highlighting & Icon

Npm's cmp source creates highlight group `CmpItemKindNpm`. To add an icon for lspkind, add the icon to your lspkind symbol map.

#### Option 1

```lua
-- lspkind.lua
local lspkind = require("lspkind")
lspkind.init({
  symbol_map = {
    Npm = " ",
  },
})

vim.api.nvim_set_hl(0, "CmpItemKindNpm", {fg ="#BD93F9"})
```

#### Option 2

```lua
-- cmp.lua
cmp.setup {
  ...
  formatting = {
    format = lspkind.cmp_format({
      mode = "symbol",
      symbol_map = { Npm = " " }
    })
  }
  ...
}
```

## Limitations

The versions are not correctly sorted (depends on `nvim-cmp`'s sorting algorithm).
