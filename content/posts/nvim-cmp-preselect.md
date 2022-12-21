+++
draft = false
date = 2022-02-20T21:36:11-05:00
title = "Nvim Cmp Preselect Woes"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

[nvim-cmp](https://github.com/hrsh7th/nvim-cmp) is a cool modular completion
engine for Neovim. For a long time, it has been annoying me that
certain language servers (I'm looking at you `gopls` and `rust_analyzer`)
would always select the first entry irrespective of the `completeopt` settings
```lua
-- Completion
vim.opt.completeopt = {'menu', 'menuone', 'noinsert', 'noselect'}
```

Run [`:h completeopt`](http://vimdoc.sourceforge.net/htmldoc/options.html#'completeopt')
for more info.

Well I finally figured out the magic encantation to get it working!!!

```lua
local cmp = require'cmp'
cmp.setup{
  ...
  preselect = cmp.PreselectMode.None
}
```

This will disable the preselect logic and we can continue on our merry way.
