再node中我们是可以直接使用相对路径进行读取的

但是node端再读取文件或者操作文件的时候，如果使用相对路径，则会使用`pross.cwd()`进行拼接

> process.cwd:获得当前的执行目录

因此如果我们跨文件进行使用node的时候，如下情况

```
-
|-main
|-text
	|-tt.js
	|-variable.css
```

- 当前我们所处为main文件夹，有一个text同级目录
- text中有两个文件，tt.js,variable.css

```js
//tt.js
 
const fs = require("fs");
const result = fs.readFileSync("./varibale.css");
```

- 此时我们再main文件夹下使用`node ../text/tt/js `

会报错！！！找不到varibale.css

**原因：**node进行拼接的是node运行目录和相对路径也就是读取`main/variable.css`

commonjs规范会注入几个变量如:`__dirname`:当前目录，我们可以直接将相对路径和当前目录进行拼接，用绝对路径代替相对路径就可以解决这个问题，因此我们可以是使用`path.resolve()`方法进行路径的拼接



