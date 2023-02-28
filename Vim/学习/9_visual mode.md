# visual mode

```
v         Character-wise visual mode
V         Line-wise visual mode
Ctrl-V    Block-wise visual mode
```

```
gv    Go to the previous visual mode
```

## Visual Mode Navigation

使用`o`在块中的开始和结尾处切换。

With `o` or `O` in visual mode, the cursor jumps from the beginning to the end of the highlighted block, allowing you to expand the highlight area.

## Visual Mode Grammar

here is no noun in visual mode。动作会直接作用在块中。

比如：r/x是修改或者删除单个字符，在visual mode中就是对每一个选中字符进行修改。

`r=`就是将选中的所有字符变为=

## Visual Mode And Command-line Commands

在visual mode中同样可以使用命令行工具

```
const one = "one";
const two = "two";
const three = "three";
```

选中前两行，然后使用`:s/const/let/g`

```
let one = "one";
let two = "two";
const three = "three";
```

## Adding Text On Multiple Lines

```
const one = "one"
const two = "two"
const three = "three"
```

- Run block-wise visual mode and go down two lines (`Ctrl-V jj`).
- Highlight to the end of the line (`$`).
- Append (`A`) then type ";".
- Exit visual mode (`<Esc>`).

> 两个标志
>
> ```
> `<    Go to the first place of the previous visual mode highlight
> `>    Go to the last place of the previous visual mode highlight
> ```

## Entering Visual Mode From Insert Mode

`Ctrl-O v`

## Select Mode

和visual mode类似，但是不执行命令，仅仅是替换