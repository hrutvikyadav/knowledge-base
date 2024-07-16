```lua
signs = {
    add = { hl = "DiffAdd", text = "│", numhl = "GitSignsAddNr" },
    change = { hl = "DiffChange", text = "│", numhl = "GitSignsChangeNr" },
    delete = { hl = "DiffDelete", text = "󰍵", numhl = "GitSignsDeleteNr" },
    topdelete = { hl = "DiffDelete", text = "‾", numhl = "GitSignsDeleteNr" },
    changedelete = { hl = "DiffChangeDelete", text = "~", numhl = "GitSignsChangeNr" },
    untracked = { hl = "GitSignsAdd", text = "│", numhl = "GitSignsAddNr", linehl = "GitSignsAddLn" },
  },
  on_attach = function(bufnr)
    utils.load_mappings("gitsigns", { buffer = bufnr })
  end,
```
## History help
![[Pasted image 20230726145955.png]]
![[Pasted image 20230726150008.png]]
## Lines to put in `CT()`
![[Pasted image 20230726150020.png]]
![[Pasted image 20230726153246.png]]

---
## Alternate `lualine` config
![[Pasted image 20230803174328.png]]

---
# Final - almost
```vimscript
hi SignColumn
hi clear SignColumn
hi GitSignsChange
hi GitGutterAdd
hi GitGutterAdd guifg=#ea9a97 guibg=NONE
hi GitGutterChange
hi GitGutterChange guifg=#9ccfd8 guibg=NONE
hi GitGutterDelete
hi GitGutterDelete guifg=#ecebf0 guibg=
hi GitGutterDelete guifg=#ecebf0 guibg=NONE
highlight SignColumn
hi LineNr
hi LineNr ctermfg=11 guifg=#817c9c guibg=NONE
```

![[Pasted image 20230831160257.png]]

```lua
[[
hi clear SignColumn
hi GitGutterAdd guifg=#ea9a97 guibg=NONE    -- red // cyan
hi GitGutterChange guifg=#9ccfd8 guibg=NONE -- cyan // violet
hi GitGutterDelete guifg=#ecebf0 guibg=NONE --white // red
hi LineNr ctermfg=11 guifg=#817c9c guibg=NONE
]]
```
![[Pasted image 20230831161839.png]]
> Some other colors

```
-- optional hi clear NormalNC
hi clear SignColumn
hi LineNr ctermfg=11 guifg=#817c9c guibg=NONE

hi GitGutterAdd guifg=#9ccfd8 guibg=NONE
hi GitGutterChange guifg=#817c9c guibg=NONE
hi GitGutterDelete guifg=#ea9a97 guibg=NONE
```

---
# Showcase
![[Pasted image 20230831172702.png]]

---

# Error signs
![[Pasted image 20230831175211.png]]

---

### Making rosepine default in CT()
theme(base16): nvim-base16 is not loaded yet, you should update your configuration to load it before lualine so that the colors from your colorscheme can be used, fallback to "tomorrow-night" theme.

---
# Debugging
![[Pasted image 20230901125343.png]]

```vimscript
set
```

---
# Worktrees
![[Pasted image 20230901161505.png]]

[reference](https://git-scm.com/docs/git-worktree)

# Mood change
![[Pasted image 20230914115627.png]]

> `#ffffff` is also a good one

