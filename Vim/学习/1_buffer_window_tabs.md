# buffer windows and tabs

基础设置

```vim
set hidden
set number
```

`set hidden` --- 在切换buffer的时候  不强制要求保存

## buffer

buffer --- 内存空间，每打开一个文件就创建一个buffer

**查看buffer**

```shell
:buffers
:ls
```

**遍历buffer**

```shell
:bnext -- :bprevious #上一个或下一个buffer
#简写
:[N]bn --- :[N]bp / :[N]bN #N为数字

#buffer --- 简写b
:buffer + [filename]
:buffer + [n]
```

**删除buffer**

```shell
:bdelete [n]/[filname]
#简写:bd
```

**退出buffer**

```
:qall
:wqall
```

> buffer 的三种状态
>
> - active --- a
> - hidden --- h
> - inactive --- ' '

## windows

A window is a viewport on a buffer.A window is what you are seeing a buffer through.

```shell
:split [file]
:vsplit [file]
```

You can have multiple windows displaying the same buffer。

此时修改一个另一个也会跟着修改。

**close windows**

```shell
:quit
ctrl-w c
```

**常见命令**

```shell
Ctrl-W V    Opens a new vertical split
Ctrl-W S    Opens a new horizontal split
Ctrl-W C    Closes a window
Ctrl-W O    Makes the current window the only one on screen and closes other
```

## Tabs

A tab is a collection of windows. it is only a layout

创建新的tab

```shell
:[count]tabe[dit] {file}
:[count]tabnew {file}
:tab {cmd}

Examples:
:tab split      " opens current buffer in new tab page
:tab help gt    " opens tab page with help for "gt"
:.tab help gt   " as above
:+tab help      " opens tab page with help after the next tab page
:-tab help      " opens tab page with help before the current one
:0tab help      " opens tab page with help before the first one
:$tab help      " opens tab page with help after the last one
```

useful tab navigations

```shell
:tabclose           Close the current tab
#:tabc
:tabnext            Go to next tab
#:tabn
:tabprevious        Go to previous tab
#:tabp / :tabN
:tablast            Go to last tab
#:tabl
:tabfirst           Go to first tab
#:tabfir
{count}gt  #go to tab page {count}
```

## working flow

- First I use buffers to store all the required files for the current task. Vim can handle many open buffers before it starts slowing down. Plus having many buffers opened won't crowd my screen. I am only seeing one buffer (assuming I have only one window) at any time, allowing me to focus on one screen. When I need to go somewhere, I can quickly fly to any open buffer anytime.
- I use multiple windows to view multiple buffers at once, usually when diffing files, reading docs, or following a code flow. I try to keep the number of windows opened to no more than three because my screen will get crowded (I use a small laptop). When I am done, I close any extra windows. Fewer windows means less distractions.
- Instead of tabs, I use [tmux](https://github.com/tmux/tmux/wiki) windows. I usually use multiple tmux windows at once. For example, one tmux window for client-side codes and another for backend codes.
