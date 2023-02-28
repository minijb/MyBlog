# dotCommand

dot command repeats the last change. 类似于宏

最常用的就是查找并修改

> ; 重复 f，t，F，T四个指示

 replace all "let" with "const"

```
let one = "1";
let two = "2";
let three = "3";
```

- Search with `/let` to go to the match.
- Change with `cwconst<Esc>` to replace "let" with "const".
- Navigate with `n` to find the next match using the previous search.
- Repeat what you just did with the dot command (`.`).
- Continue pressing `n . n .` until you replace every word.

```
pancake, potatoes, fruit-juice,
```

After you run `f,` then `x`, go to the next comma with `;` to repeat the latest `f`. Finally, use `.` to delete the character under the cursor. Repeat `; . ; .` until everything is deleted. The full command is `f,x;.;.`.

## Multi-line Repeat

使用visually select，执行动作然后使用dot command

## Including A Motion In A Change

之前说过最常用的是搜索并重复执行dot command

但在在修改搜索的单词的时候我们可以使用`gn`来快速选中所有的单词，并进入visual模式，一次来简化操作

After you searched `/let`, run `cgnconst<Esc>` then `. .`.