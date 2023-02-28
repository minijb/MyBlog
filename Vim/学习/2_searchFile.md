# searchFile

**open and edit a file**

```shell
:edit {file}
```

if exist, open buffer.

accepts wildcards arguments. `*` matches any file in the current 

```
:edit *.yml<Tab>
```

You can use `**` to search recursively. If you want to look for all `*.md` files in your project, but you are not sure in which directories, you can do this:

```shell
:edit **/*.md<Tab>
```

`:edit` can be used to run `netrw`, Vim's built-in file explorer. To do that, give `:edit` a directory argument instead of file:

> `netrw`vim自带的文件文件管理器

## find

```
:find {file}
```

## find and path

find --- find in path 

path is `:set path?`

```
path=.,/usr/include,,
```

- `.` means to search relative to the directory of the current file.
- `,` means to search in the current directory.
- `/usr/include` is the directory for C compilers header files.

我们可以通过添加path来扩展所有路径

```
:set path+=app/controllers/
```

此时通过tab不全就可以找到子目录下的文件

我们可以通过

```shell
:set path+=$PWD/**
```

将当前文件夹下所有的文件添加到路径上（当我们嵌套文件不多的时候），最好还是指定自己需要的文件

You can add the `set path+={your-path-here}` in your vimrc. Updating `path` takes only a few seconds and doing this will save you a lot of time.

## using grep

If you need to find in files (find phrases in files), you can use grep. Vim has two ways of doing that:

- Internal grep (`:vim`. Yes, it is spelled `:vim`. It is short for `:vimgrep`).
- External grep (`:grep`).

```shell
:vim /pattern/ file
```

- `/pattern/` is a regex pattern of your search term.
- `file` is the file arguments. You can pass multiple arguments. Vim will search for the pattern inside the file arguments. Similar to `:find`, you can pass it `*` and `**` wildcards.

```
:vim /breakfast/ app/controllers/**/*.rb
```

After running that, you will be redirected to the first result. Vim's `vim` search command uses `quickfix` operation. To see all search results, run `:copen`. This opens a `quickfix` window. Here are some useful quickfix commands to get you productive immediately:

```
:copen        Open the quickfix window
:cclose       Close the quickfix window
:cnext        Go to the next error
:cprevious    Go to the previous error
:colder       Go to the older error list
:cnewer       Go to the newer error list
```

## Netrw

常用命令

```shell
%    Create a new file
d    Create a new directory
R    Rename a file or directory
D    Delete a file or directory
```

## Fzf Syntax

To use fzf efficiently, you should learn some basic fzf syntax. Fortunately, the list is short:

- `^` is a prefix exact match. To search for a phrase starting with "welcome": `^welcome`.
- `$` is a suffix exact match. To search for a phrase ending with "my friends": `friends$`.
- `'` is an exact match. To search for the phrase "welcome my friends": `'welcome my friends`.
- `|` is an "or" match. To search for either "friends" or "foes": `friends | foes`.
- `!` is an inverse match. To search for phrase containing "welcome" and not "friends": `welcome !friends`

You can mix and match these options. For example, `^hello | ^welcome friends$` will search for the phrase starting with either "welcome" or "hello" and ending with "friends".

>## Finding Files
>
>To search for files inside Vim using fzf.vim plugin, you can use the `:Files` method. Run `:Files` from Vim and you will be prompted with fzf search prompt.
>
>Since you will be using this command frequently, it is good to have this mapped. I map mine to `Ctrl-f`. In my vimrc, I have this:
>
>```
>Copy
>nnoremap <silent> <C-f> :Files<CR>
>```
>
>## Finding In Files
>
>To search inside files, you can use the `:Rg` command.
>
>Again, since you will probably use this frequently, let's map it. I map mine to `<Leader>f`.
>
>```
>Copy
>nnoremap <silent> <Leader>f :Rg<CR>
>```
>
>## Other Searches
>
>Fzf.vim provides many other search commands. I won't go through each one of them here, but you can check them out [here](https://github.com/junegunn/fzf.vim#commands).
>
>Here's what my fzf maps look like:
>
>```
>Copy
>nnoremap <silent> <Leader>b :Buffers<CR>
>nnoremap <silent> <C-f> :Files<CR>
>nnoremap <silent> <Leader>f :Rg<CR>
>nnoremap <silent> <Leader>/ :BLines<CR>
>nnoremap <silent> <Leader>' :Marks<CR>
>nnoremap <silent> <Leader>g :Commits<CR>
>nnoremap <silent> <Leader>H :Helptags<CR>
>nnoremap <silent> <Leader>hh :History<CR>
>nnoremap <silent> <Leader>h: :History:<CR>
>nnoremap <silent> <Leader>h/ :History/<CR>
>```