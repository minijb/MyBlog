# moving

`:h motion.txt`

## Character Navigation

```
h   Left
j   Down
k   Up
l   Right
```

```
noremap <Up> <NOP>
```

## Relative Numbering

`set relativenumber number`

## Count Your Move

```shell
[count] + motion
```

## Word Navigation

You can move to the beginning of the next word (`w`), to the end of the next word (`e`), to the beginning of the previous word (`b`), and to the end of the previous word (`ge`).

```
w     Move forward to the beginning of the next word
W     Move forward to the beginning of the next WORD
e     Move forward one word to the end of the next word
E     Move forward one word to the end of the next WORD
b     Move backward to beginning of the previous word
B     Move backward to beginning of the previous WORD
ge    Move backward to end of the previous word
gE    Move backward to end of the previous WORD
```

>A WORD consists of a sequence of non-blank characters, separated with white space.  An empty line is also considered to be a WORD.
>
>简单来说就是
>
>- word --- 标点/bloc分割
>- WORD ---  只有block分割

## Current Line Navigation

```
0     Go to the first character in the current line
^     Go to the first nonblank char in the current line
g_    Go to the last non-blank char in the current line
$     Go to the last char in the current line
n|    Go the column n in the current line
```

```
f    Search forward for a match in the same line
F    Search backward for a match in the same line
t    Search forward for a match in the same line, stopping before match
T    Search backward for a match in the same line, stopping before match
;    Repeat the last search in the same line using the same direction
,    Repeat the last search in the same line using the opposite direction
```

## Sentence And Paragraph Navigation

```
(    Jump to the previous sentence
)    Jump to the next sentence
{    Jump to the previous paragraph
}    Jump to the next paragraph
```

## Match Navigation

```
%    Navigate to another match, usually works for (), [], {}
```

## Line Number Navigation

```
gg    Go to the first line
G     Go to the last line
nG/ngg    Go to line n
n%    Go to n% in file
```

## Window Navigation

```
H     Go to top of screen
M     Go to medium screen
L     Go to bottom of screen
nH    Go n line from top
nL    Go n line from bottom
```

## Scrolling

```
Ctrl-E    Scroll down a line
Ctrl-D    Scroll down half screen
Ctrl-F    Scroll down whole screen
Ctrl-Y    Scroll up a line
Ctrl-U    Scroll up half screen
Ctrl-B    Scroll up whole screen

You can also scroll relatively to the current line (zoom screen sight):
zt    Bring the current line near the top of your screen
zz    Bring the current line to the middle of your screen
zb    Bring the current line near the bottom of your screen
```

## Search Navigation

```
/    Search forward for a match
?    Search backward for a match
n    Repeat last search in same direction of previous search
N    Repeat last search in opposite direction of previous search
```

**关闭开启高亮**

```shell
set hlsearch
:nohlsearch #:noh

#nnoremap <esc><esc> :noh<return><esc>
```

### 快速查找一个单词

You can quickly search for the text under the cursor with `*` to search forward and `#` to search backward. If your cursor is on the string "one", pressing `*` will be the same as if you had done `/\<one\>`.

Both `\<` and `\>` in `/\<one\>` mean whole word search. It does not match "one" if it is a part of a bigger word. It will match for the word "one" but not "onetwo". If your cursor is over "one" and you want to search forward to match whole or partial words like "one" and "onetwo", you need to use `g*` instead of `*`.

```
*     Search for whole word under cursor forward
#     Search for whole word under cursor backward
g*    Search for word under cursor forward
g#    Search for word under cursor backward
```

## Marking Position

```
ma    Mark position with mark "a"
`a    Jump to line and column "a"
'a    Jump to line "a"
```

x can be any alphabetical letter `a-zA-Z`

其中大小写是有区别的，小写---local mark   大小----global mark。

每一个buffer都有自己的local mark，但是global mark是共享的。

```
''    Jump back to the last line in current buffer before jump
``    Jump back to the last position in current buffer before jump
`[    Jump to beginning of previously changed / yanked text
`]    Jump to the ending of previously changed / yanked text
`<    Jump to the beginning of last visual selection
`>    Jump to the ending of last visual selection
`0    Jump back to the last edited file when exiting vim
```

## Jump

```
'       Go to the marked line
`       Go to the marked position
G       Go to the line
/       Search forward
?       Search backward
n       Repeat the last search, same direction
N       Repeat the last search, opposite direction
%       Find match
(       Go to the last sentence
)       Go to the next sentence
{       Go to the last paragraph
}       Go to the next paragraph
L       Go to the the last line of displayed window
M       Go to the middle line of displayed window
H       Go to the top line of displayed window
[[      Go to the previous section
]]      Go to the next section
:s      Substitute
:tag    Jump to tag definition
```

