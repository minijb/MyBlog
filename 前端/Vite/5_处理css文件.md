# 处理css

vite天生就支持css文件的直接处理

```js
import { count } from "./counter";
import "./index.css"
import _ from "lodash"

console.log( count );

console.log("import.meta.env", import.meta.env);
```

就直接导入成功了

1. vite识别到应用`index.css`
2. 直接使用fs模块去读取`index.css`
3. 直接创建一个style标签，将文件内容直接copy进去
4. 将style标签插入到head中
5. 将css文件内容直接替换为js脚本(方便热更新和css模块化)

```js
// css文件内容
import { createHotContext as __vite__createHotContext } from "/@vite/client";import.meta.hot = __vite__createHotContext("/index.css");import { updateStyle as __vite__updateStyle, removeStyle as __vite__removeStyle } from "/@vite/client"
const __vite__id = "D:/Project/js/vite/index.css"
const __vite__css = "html , body {\n    width: 100%;\n    height: 100%;\n    background-color: purple;\n}"
__vite__updateStyle(__vite__id, __vite__css)
import.meta.hot.accept()
export default __vite__css
import.meta.hot.prune(() => __vite__removeStyle(__vite__id))
```

可能遇到的问题：

- class重名问题可能覆盖css样式

此时我们可以使用cssnmodule

```js
import componentACsss from xxxx
import componentBCsss from xxxx

div1.className = componentACsss.footer;
div2.className = componentBCsss.footer;
```

此时css中的代码就会自动添加一个字符串来区分

基本的步骤

1. 会css中类名通过规则替换为另一个
2. 同时创建一个映射对象 {footer: "xxxxxx"}
3. 我们导入包之后就可以直接使用这个映射中的类名

less(预处理器)：将less代码编译为css代码，之后vite再将css变为js代码

## vite中的配置(module)

- `localsConvention`: 修改类名的展现形式
  - `camelCase` 会转换出一个驼峰类名
  - `camelCaseOnly`: 只保留驼峰类名的，除去`-`间隔的类名
  - `dashes`: 会转换出一个`-`间隔的类名
  - `dashesOnly`: 同理
- `scopeBehaviour`: `global | local` 配置当前的模块化行为是全局的还是局部的
  - `global`就会关闭模块化
- `generateScopedName`: 生成类名的规则,可以是函数可以是字符串

> `generateScopedName: "[name]_[local]_[hash:5]"`
> 详见： postcss modules

- `hashPrefix`: 哈希是根据类名生成的，我们可以添加一个前缀来添加到原始类名前，以此来增加难度
- `globalModulePaths: [],//不会参加到模块化的css文件路径`

## vite中的配置(preprocessorOptions)

主要是用来配置css预处理的一些全局参数

如果没有配置工具，我们又想缺编译less文件，需要`npm install --save less #less的编译器`我们可以使用`lessc xxx`来编译less文件，此时我们可以添加一些参数。

常见参数

- `math="always"`

sass同理。

```js
preprocessorOptions: { // key + config
      less : { //整个的配置对象都会最终给到less的执行参数(全局参数)中去
        math: "always",
        globalVars: {//全局变量
          mainColor: "red",
        }
      }
    }
```



### 全局参数

less是可以定义变量的，比如`@mainColor: red `

使用可以直接使用`@mainColor`

也可以import , `@import url(xxxx)`，如果我们有一些常用的变量可以设置全局变量，需要在lessc中手动配置！！！

```js
globalVars: {//全局变量
          mainColor: "red",
        }
```



### sourceMap

```js
css: {
    // 配置css行为
    // 配置css模块化的行为
    modules: {
      localsConvention: "dashes",
      scopeBehaviour: "local",
      generateScopedName: "[name]_[local]_[hash:5]",
      // hashPrefix: "xxx",
      globalModulePaths: [],//不会参加到模块化的css文件路径
    },
    preprocessorOptions: { // key + config
      less : { //整个的配置对象都会最终给到less的执行参数(全局参数)中去
        math: "always",
        globalVars: {//全局变量
          mainColor: "red",
        }
      }
    },
    devSourcemap: true,#!!!######################
  },
```

文件之间的索引：

如果我们的代码被编译了，这时候如果程序出错，那么不会产生正确的错误位置信息，如果有了soueceMap，会索引到我们源文件，方便调试

## postcss

保证css的执行起来万无一失

使用css的原生变量

```less
//index.module.less
:root {
    // css原生变量
    --globalColor: lightblue;
}

//index.less
html , body {
    width: 100%;
    height: 100%;
    background-color: purple;
}

.content {
    .main {
        background: var(--globalColor);
        padding: (100px / 2);
        margin: 100px/2;
    }
}
```

隐患：

- 系统兼容性，预处理器并不能解决
  - 对css属性的一些使用降级问题
  - postcss类似于babel，进行降级
- 前缀补全：`--webkit`

我们的代码----》postcss---->将语法进行编译(嵌套语法，函数，变量)(less,sass)  ----》 对css进行降级  ----》前缀补全 ----》 浏览器

babel同理

js代码 ----》 babel ----》 将ts转化为js ---》 语法降级 ----》 去客户端执行

## 使用postcss

**安装**

```sh
npm install --save postcss-cli postcss 
npm install --save postcss-cli postcss -D
```

如果我们直接使用`npx postcss index.css -o result.css `那没有任何效果，我们没有设置任何功能

我们需要编写`postcss.config.js`，并安装插件

postcss-preset-env插件：预设环境

里面有很多预设的插件比如：语法降级，编译插件(post-compiler)

```js
const postcssPresetEnv = require("postcss-preset-env");

module.exports = {
    plugins: [postcssPresetEnv()] //加入中间件
}
```

同理我们可以加入 less语法，sass语法做编译(postcss中的less插件，sass插件已经不再维护)，现在我们先使用编译器编译完，之后再送给postcss进行其他操作，现在我们都说postcss是后处理器

###  配置postcss

```js

import { defineConfig } from "vite";
const postcssPresetEnv  = require("postcss-preset-env");


export default defineConfig({
  css: {
    // 配置css行为
    // 配置css模块化的行为
    modules: {
      localsConvention: "dashes",
      scopeBehaviour: "local",
      generateScopedName: "[name]_[local]_[hash:5]",
      // hashPrefix: "xxx",
      globalModulePaths: [],//不会参加到模块化的css文件路径
    },
    preprocessorOptions: { // key + config
      less : { //整个的配置对象都会最终给到less的执行参数(全局参数)中去
        math: "always",
        globalVars: {//全局变量
          mainColor: "red",
        }
      }
    },
    devSourcemap: true,
//----------------------------------------
    postcss: {
      plugins: [postcssPresetEnv()]
    }
//----------------------------------------
  },
});
```

也可以再postcss.config.js中编写，vite会自动寻找

```css
.content {
    .main {
        background: var(--globalColor);
        padding: (100px / 2);
        margin: 100px/2;
        filter: blur(100px);
        width: clamp(100px, 50% , 200px);
        //clamp 响应布局，最小100px 最大200px 再100-200之间就是页面的30%
        // 浏览器中的  width: max(100px, min(50%, 200px));
        // 这个是所有浏览器支持的方法
        user-select: none;
    }
}
```



