## View

It is a collection of settings for one window. If you spend a long time working on a window and you want to preserve the maps and folds, you can use a View.

Let's create a file called `foo.txt`:

```
foo1
foo2
foo3
foo4
foo5
foo6
foo7
foo8
foo9
foo10
```

In this file, create three changes:

1. On line 1, create a manual fold `zf4j` (fold the next 4 lines).
2. Change the `number` setting: `setlocal nonumber norelativenumber`. This will remove the number indicators on the left side of the window.
3. Create a local mapping to go down two lines each time you press `j` instead of one: `:nnoremap <buffer> j jj`.

Your file should look like this:

```
+-- 5 lines: foo1 -----
foo6
foo7
foo8
foo9
foo10
```

Run:

```
:set viewoptions?
```

By default it should say (yours may look different depending on your vimrc):

```
viewoptions=folds,cursor,curdir
```

Let's configure `viewoptions`. The three attributes you want to preserve are the folds, the maps, and the local set options. If your setting looks like mine, you already have the `folds` option. You need to tell View to remember the `localoptions`. Run:

```
:set viewoptions+=localoptions
```

To learn what other options are available for `viewoptions`, check out `:h viewoptions`. Now if you run `:set viewoptions?`, you should see:

```
viewoptions=folds,cursor,curdir,localoptions
```

### Saving The View

With the `foo.txt` window properly folded and having `nonumber norelativenumber` options, let's save the View. Run:

```
:mkview
```

Vim creates a View file.

之后开启`:loadview`就可以加载设置

### Multiple Views

Vim lets you save 9 numbered Views (1-9).

Suppose you want to make an additional fold (say you want to fold the last two lines) with `:9,10 fold`. Let's save this as View 1. Run:

```
:mkview 1
```

If you want to make one more fold with `:6,7 fold` and save it as a different View, run:

```
:mkview 2
```

Close the file. When you open `foo.txt` and you want to load View 1, run:

```
:loadview 1
```

To load View 2, run:

```
:loadview 2
```

To load the original View, run:

```
:loadview
```

### Automating View Creation

One of the worst things that can happen is, after spending countless hours organizing a large file with folds, you accidentally close the window and lose all fold information. To prevent this, you might want to automatically create a View each time you close a buffer. Add this in your vimrc:

```
autocmd BufWinLeave *.txt mkview
```

Additionally, it might be nice to load View when you open a buffer:

```
autocmd BufWinEnter *.txt silent loadview
```

Now you don't have to worry about creating and loading View anymore when you are working with `txt` files. Keep in mind that over time, your `~/.vim/view` might start to accumulate View files. It's good to clean it up once every few months.

## Session

a Session saves the information of all windows (including the layout).

```
:mksession
```

在本地创建一个.vim文件

**使用session**

```
:source Session.vim
vim -S Session.vim
```

