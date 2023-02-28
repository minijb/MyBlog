# VimGrammar

There is only one grammar rule in Vim language:

```shell
verb + [num]/[in/out] + noun
```

## Nouns (Motions)

```shell
h    Left
j    Down
k    Up
l    Right
w    Move forward to the beginning of the next word
}    Jump to the next paragraph
$    Go to the end of the line
```

## Verbs (Operators)

most use

```shell
y    Yank text (copy)
d    Delete text and save to register
c    Delete text, save to register, and start insert mode
```

## More Nouns (Text Objects)

```shell
i + object    Inner text object
a + object    Outer text object
```

inner --- 范围内部

outer --- 包含范围符号

- To delete the text inside the parentheses without deleting the parentheses: `di(`.
- To delete the parentheses and the text inside: `da(`.

### example

```js
const hello = function() {
  console.log("Hello Vim");
  return true;
}
```

- To delete the entire "Hello Vim": `di(`.
- To delete the content of function (surrounded by `{}`): `di{`.
- To delete the "Hello" string: `diw`.

```html
<div>
  <h1>Header1</h1>
  <p>Paragraph1</p>
  <p>Paragraph2</p>
</div>
```

If your cursor is on "Header1" text:

- To delete "Header1": `dit`.
- To delete `<h1>Header1</h1>`: `dat`.

If your cursor is on "div":

- To delete `h1` and both `p` lines: `dit`.
- To delete everything: `dat`.
- To delete "div": `di<`.

> ```
> w         A word
> p         A paragraph
> s         A sentence
> ( or )    A pair of ( )
> { or }    A pair of { }
> [ or ]    A pair of [ ]
> < or >    A pair of < >
> t         XML tags
> "         A pair of " "
> '         A Pair of ' '
> `         A pair of ` `
> ```
>
> 更多查看 --- `h text-objects`

## Composability And Grammar

我们可以使用终端命令来修改我们的内容

```
Id|Name|Cuteness
01|Puppy|Very
02|Kitten|Ok
03|Bunny|Ok
```

This cannot be easily done with Vim commands, but you can get it done quickly with `column` terminal command (assuming your terminal has `column` command). With your cursor on "Id", run `!}column -t -s "|"`. Voila! Now you have this pretty tabular data with just one quick command.

```
Id  Name    Cuteness
01  Puppy   Very
02  Kitten  Ok
03  Bunny   Ok
```

Let's break down the command. The verb was `!` (filter operator) and the noun was `}` (go to next paragraph). The filter operator `!` accepted another argument, a terminal command, so I gave it `column -t -s "|"`. I won't go through how `column` worked, but in effect, it tabularized the text.

Suppose you want to not only tabularize your text, but to display only the rows with "Ok". You know that `awk` can do the job easily. You can do this instead:

```
!}column -t -s "|" | awk 'NR > 1 && /Ok/ {print $0}'
```

