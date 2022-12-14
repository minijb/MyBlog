### 安装

https://github.com/neovim/neovim/wiki/Installing-Neovim

### 使用plug

https://github.com/junegunn/vim-plug

### 安装最新版的node

更新软件源

```sh
sudo apt update
```

安装 npm

```sh
sudo apt install npm
```

安装 n 模块
注：n 模块是用来安装各种版本的 node 的一个工具。参数 -g 表示全局安装

```sh
sudo npm install n -g
```

安装最新长期支持版 node

```sh
sudo n lts
```

检验

```sh
node -v
```

各种版本简介
n 模块可以安装各种版本的 node，非常方便。

latest – 最新版，但不一定稳定
stable – 最新的稳定版，比 latest 老一点，优点是稳定，但不是长期支持版（api 可能会变动）
lts – 最新的长期支持版，这是最推荐使用的版本。虽然比 stable 老，但是是长期支持的版本（当然也是稳定版本）。是 long term support 的缩写。
除此之外，可以直接通过版本号，安装制定版本的 node

### 安装字体

https://github.com/ryanoasis/nerd-fonts

https://github.com/ryanoasis/vim-devicons

nerd-fonts网页版，可以预览字体https://www.nerdfonts.com/

### 设置剪切板

```sh
#安装xclip
sudo apt install xclip
set clipboard=unnamedplus
```

### 我的vimrc

```
"常用配置
"设置行号
set number
set encoding=UTF-8
syntax on
colorscheme hybrid
set background=dark
let mapleader=','
set tabstop=4
set shiftwidth=4
"切换粘贴模式
set pastetoggle=<F2>
"主要用于which-key的时间设置
set timeoutlen=500
"use crtl+h/j/k/l switch window
"noremap <C-h> <C-w>h
"noremap <C-j> <C-w>j
"noremap <C-k> <C-w>k
"noremap <C-l> <C-w>l
"
set hidden
set updatetime=100
set shortmess+=c

"plug插件
" Specify a directory for plugins
" - For Neovim: stdpath('data') . '/plugged'
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.config/nvim/plugged')
	"initialize plugin system
	Plug 'mhinz/vim-startify'
	"Tree of document
	Plug 'preservim/nerdtree'
	Plug 'ryanoasis/vim-devicons'
	"theme
	Plug 'w0ng/vim-hybrid'
	"缩进划线
	Plug 'vim-airline/vim-airline'
	Plug 'vim-airline/vim-airline-themes'
	Plug 'yggdroot/indentline'
	"模糊查询
	Plug 'kien/ctrlp.vim'
	"页面内快速跳转
	Plug 'easymotion/vim-easymotion'
	Plug 'haya14busa/incsearch-easymotion.vim'
	Plug 'haya14busa/incsearch.vim'
	"四周替换
	Plug 'tpope/vim-surround'
	"模糊搜索插件
	Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
	Plug 'junegunn/fzf.vim'
	Plug 'python-mode/python-mode', { 'for': 'python', 'branch': 'develop' }
	Plug 'preservim/tagbar'
	"代码补全
	Plug 'neoclide/coc.nvim', {'branch': 'release'}
	"代码个格式化
	Plug 'sbdchd/neoformat'
	Plug 'jiangmiao/auto-pairs'
	Plug 'tpope/vim-commentary'
	Plug 'liuchengxu/vim-which-key', { 'on': ['WhichKey', 'WhichKey!'] }
call plug#end()

"airline
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'
let g:airline#extensions#tabline#formatter = 'unique_tail'
"nerdtree
nnoremap <leader>g :NERDTreeToggle<CR>
nnoremap <leader>v :NERDTreeFind<CR>"跳转到目录树
let NERDTreeShowHidden=1
"ctrlp 模糊查询
let g:ctrlp_map='<c-p>'

"vim-easymotion
nmap ss <Plug>(easymotion-s2)
" Move to line
map <Leader>L <Plug>(easymotion-bd-jk)
nmap <Leader>L <Plug>(easymotion-overwin-line)
map z/ <Plug>(incsearch-easymotion-/)
map z? <Plug>(incsearch-easymotion-?)




"fzf setting
let g:ackprg = 'ag --nogroup --nocolor --column'
"python-mode settring
let g:pymode_python='python3'
let g:pymode_trim_whitespaces = 1
let g:pymode_doc=1
let g:pymode_doc_bind='K'
let g:pymode_rope_geto_definition_bind = "<C-]>"
let g:pymode_lint=1
let g:pymode_lint_checker = ['pyflaskes', 'pep8', 'mccabe', 'pylint']
let g:pymode_options_max_line_length = 120
"Coc setting

" Use <c-space> to trigger completion.
inoremap <silent><expr> <c-space> coc#refresh()
" Use `[g` and `]g` to navigate diagnostics
" Use `:CocDiagnostics` to get all diagnostics of current buffer in location list.
nmap <silent> [g <Plug>(coc-diagnostic-prev)
nmap <silent> ]g <Plug>(coc-diagnostic-next)
" GoTo code navigation.
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)
" Use K to show documentation in preview window.
nnoremap <silent> K :call <SID>show_documentation()<CR>
" Highlight the symbol and its references when holding the cursor.
"autocmd CursorHold * silent call CocActionAsync('highlight')
" Symbol renaming.
nmap <leader>rn <Plug>(coc-rename)
" Use tab for trigger completion with characters ahead and navigate.
" NOTE: Use command ':verbose imap <tab>' to make sure tab is not mapped by
" other plugin before putting this into your config.
" inoremap <silent><expr> <TAB>
"       \ pumvisible() ? "\<C-n>" :
"       \ <SID>check_back_space() ? "\<TAB>" :
"       \ coc#refresh()
" inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"


"which-key setting
let g:mapleader = ','
nnoremap <silent> <leader>      :<c-u>WhichKey ','<CR>


"tagbar settings
nmap <F8> :TagbarToggle<CR>
```



