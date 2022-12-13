# Vite是如何让浏览器识别vue文件的

npm create 实际上就是安装`create-vite`脚手架，使用脚手架的指令来构建项目

vite会将.vue文件内部转化为js格式的代码，*那浏览器是如何读取.vue文件的呢*

<br/>

我门可以实现一个简单的vite开发服务器

1. 解决刚刚的问题
2. 对开发服务器有一个简单的认识(会有node相关知识)

> koa：node端的一个框架

一个基础的node代码，可以打印request和response

```js
const Koa = require("koa");
const fs = require("fs");
const path = require("path");
const app = new Koa();

// 当请求来的时候，会进入到use注册的回调函数中
app.use(async (ctx) => {
  console.log("ctx", ctx.request, ctx.response);
  if (ctx.url === "/") {
    // 获得文件内容
    const indexContent = await fs.promises.readFile(
      path.resolve(__dirname, "./index.html")
    );
    // console.log("indexContent--------\n", indexContent.toString());
    // 我们返回文件，并以html文件的形式来传给浏览器
    ctx.response.set("Content-Type", "text/html");
    ctx.response.body = indexContent;
    // 此时我们已经可以正常显示页面了
  }
  if (ctx.url === "/main.js"){
    const indexContent = await fs.promises.readFile(
      path.resolve(__dirname, "./main.js")
    );
    ctx.response.set("Content-Type", "text/javascript");
    ctx.response.body = indexContent;
  }
  if (ctx.url === "/App.vue"){
    //我们需要转译vue文件为js给是，并传给浏览器，并使用js格式来进行解析
  }
});

//监听端口
app.listen(5173, () => {
  console.log("vite dev server");
});

```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>hello world</h1>
    <script type="module" src="./main.js"></script>
</body>
</html>
```

```js
import "./App.vue";
console.log("main.js");
```

启动导入操作可以通过中间件完成，同时我们会将vue文件进行转译，并通过set方法强制让浏览器识别为js文件