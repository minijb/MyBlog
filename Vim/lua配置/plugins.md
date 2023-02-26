### nvim-tree/nvim-tree.lua

**常用command**

**|**:NvimTreeToggle**|**

**|**:NvimTreeCollapse**|** --- 折叠所有文件夹

**|**:NvimTreeCollapseKeepBuffers**|** --- 除了当前文件夹全部折叠

**mappings**

`<C-]>`           cd                  cd in the directory under the cursor

`<Tab>`           preview             open the file as a preview (keeps the cursor in the tree)

`C`               toggle_git_clean  

`H`               toggle_dotfiles  

`a`               create 

`d`               remove  

`r`               rename   

`x`               cut  

`c`               copy 

`p`               paste   

### nvim-treesitter/nvim-treesitter

### echasnovski/mini.nvim

**surround**

https://github.com/echasnovski/mini.nvim/blob/main/readmes/mini-surround.md

- Add surrounding with `sa` (in visual mode or on motion).
- Delete surrounding with `sd`.
- Replace surrounding with `sr`.
- Find surrounding with `sf` or `sF` (move cursor right or left).
- Highlight surrounding with `sh`.
- Change number of neighbor lines with `sn` (see |MiniSurround-algorithm|).

### linty-org/key-menu.nvim

```
vim.keymap.set( -- define a new mapping
  'n',          -- in Normal mode
  'g',          -- which is bound to the 'g' key
  function()    -- executing this function
    require 'key-menu'.open_window('g')
  end)
```

```c++
-- This Vim setting controls the delay before the popup appears. The way it
-- works is, if you have mappings for, say, the key "a" and the key sequence
-- "ab", if you type "a", then Vim waits timeoutlen, and if you don't press
-- another key before that amount of time, then the "a" mapping is executed, but
-- if you press "b" before timeoutlen, then the "ab" mapping is executed.
vim.o.timeoutlen = 300

-- If you use <Space> as your mapping prefix, then this will make the key-menu
-- popup appear in Normal mode, after you press <Space>, after timeoutlen.
require 'key-menu'.set('n', '<Space>')

-- Use the desc option to Vim's built-in vim.keymap.set to describe mappings.
vim.keymap.set('n', '<Space>w', '<Cmd>w<CR>', {desc='Save'})
vim.keymap.set('n', '<Space>q', '<Cmd>q<CR>', {desc='Quit'})

-- You can also pass Lua functions to vim.keymap.set.
local erase_all_lines = function()
  vim.api.nvim_buf_set_lines(0, 0, -1, false, {})
end
vim.keymap.set('n', '<Space>k', erase_all_lines, {desc='Erase all'})

-- You can put mappings several keys deep, in a mapping group. For some kinds of
-- mappings, even if you don't include a description, key-menu will try to
-- render them nicely, for example by not showing the <Cmd> and <CR>.
vim.keymap.set('n', '<Space>gs', '<Cmd>Git status<CR>')
vim.keymap.set('n', '<Space>gc', '<Cmd>Git commit<CR>')

-- To describe the group of mappings under <Space>g, use key-menu.set.
require 'key-menu'.set('n', '<Space>g', {desc='Git'})

-- The function key-menu.set is just a thin wrapper around vim.keymap.set, and
-- is provided for convenience so that you don't have to type the key sequence
-- twice. The above call to key-menu.set is equivalent to this:
vim.keymap.set('n', '<Space>g',
  function() require 'key-menu'.open_window('<Space>g') end,
  {desc='Git'})

-- The arguments to key-menu.set are the same as those for vim.keymap.set,
-- except that the RHS/callback argument is omitted.

-- Create a buffer-local mapping group to echo some example text.
require 'key-menu'.set('n', '<Space>s',
  {desc = 'Say something', buffer = true})
-- Create buffer-local mappings to say hello and goodbye.
vim.keymap.set('n', '<Space>sh',
  function() print('Hello, world') end,
  {desc = '...hello!', buffer = true})
vim.keymap.set('n', '<Space>sg',
  function() print('Goodbye, world!') end,
  {desc = '...goodbye!', buffer = true})

-- Create a mapping that does not appear in the pop up menu.
vim.keymap.set('n', '<leader>1', '<Cmd>BufferLineGoToBuffer 1<CR>', {desc='HIDDEN'})
```

### which-key

```lua
  local whichkey = require "which-key"

  local conf = {
    window = {
      border = "single", -- none, single, double, shadow
      position = "bottom", -- bottom, top
    },
  }

  local opts = {
    mode = "n", -- Normal mode
    prefix = "<leader>",
    buffer = nil, -- Global mappings. Specify a buffer number for buffer local mappings
    silent = true, -- use `silent` when creating keymaps
    noremap = true, -- use `noremap` when creating keymaps
    nowait = false, -- use `nowait` when creating keymaps
  }

  local mappings = {
    ["w"] = { "<cmd>update!<CR>", "Save" },
    ["q"] = { "<cmd>q!<CR>", "Quit" },

    b = {
      name = "Buffer",
      c = { "<Cmd>bd!<Cr>", "Close current buffer" },
      D = { "<Cmd>%bd|e#|bd#<Cr>", "Delete all buffers" },
    },

    ["r"] = { "gg:%s/\\r//g<enter>", "rm \\r"},
    
    t = {
    	name = "tree",
	t = {":NvimTreeToggle<enter>", "open the tree"},
	f = {
	    name = "fold",
	    f = {":NvimTreeCollapse<enter>", "fold all"},
	    o = {":NvimTreeCollapseKeepBuffers<enter>","fold only"}
	}
    },

    f = {
	name = "find",
	f = {"<cmd>FzfLua files<cr>", "find files"},
	b = {"<cmd>FzfLua buffers<cr>", "find buffers"},
	l = {"<cmd>FzfLua blines<cr>", "find lines"},
    },

    g = {
	name = "grep",
	p = {"<cmd>FzfLua live_grep<cr>", "grep project"},
	b = {"<cmd>FzfLua grep_curbuf<cr>", "grep current buffer"},
    }




  }

  whichkey.setup(conf)
  whichkey.register(mappings, opts)

```

### COQ

if install deps failed

```sh
sudo apt install python3-venv
```

