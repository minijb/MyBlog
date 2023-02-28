```vim
local api = vim.api
local g = vim.g
local opt = vim.opt

g.shiftwidth = 4
g.tabstop = 4
g.softtabstop = 4
g.textwidth =120
g.encoding = "UTF-8"
-- Remap leader and local leader to <Space>
api.nvim_set_keymap("", "<Space>", "<Nop>", { noremap = true, silent = true })

opt.termguicolors = true -- Enable colors in terminal
opt.hlsearch = true --Set highlight on search
opt.number = true --Make line numbers default
opt.relativenumber = true --Make relative number default
opt.mouse = "a" --Enable mouse mode
opt.breakindent = true --Enable break indent
opt.undofile = true --Save undo history
opt.ignorecase = true --Case insensitive searching unless /C or capital in search
opt.smartcase = true -- Smart case
opt.updatetime = 250 --Decrease update time
opt.signcolumn = "yes" -- Always show sign column
opt.clipboard = "unnamedplus" -- Access system clipboard

g.mapleader = "\\"
g.maplocalleader = "\\"

-- Highlight on yank
vim.cmd [[
  augroup YankHighlight
    autocmd!
    autocmd TextYankPost * silent! lua vim.highlight.on_yank()
  augroup end
]]

-- wsl clipboard
if vim.fn.has('wsl') then
	vim.cmd [[
		augroup Yank
		autocmd!
		autocmd TextYankPost * :call system('/mnt/c/windows/system32/clip.exe',@")
		augroup END
		]]
	end

```

