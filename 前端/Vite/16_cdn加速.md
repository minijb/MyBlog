# cdn 加速

cdn : content delivery network

我们所有的依赖(vue , lodash.....)以及文件在我们进行打包以后会放到服务器上

如果服务端和客户端的距离较远，那么就会卡顿。

我们可以将我们所有的以来写成cdn形式，然后保证我们的代码小体积(那么就比较快)

cdn --》内容分发  （类似dns）

> jsdeliver

cdn:就是一个站址，就会赵一个最近的服务器来加载这个js代码，我们自身的体积就小了

如果我们引入lodash

```
#yarn build
yarn run v1.22.19
$ vite build
vite v4.0.4 building for production...
✓ 6 modules transformed.
dist/index.html                 0.35 kB
dist/assets/index-ddf026df.js  72.66 kB │ gzip: 26.86 kB
Done in 0.89s.
```

需要的包`vite-plugin-cdn-import`,进行cdn加速

```js
import { defineConfig} from "vite";
import viteCDNPlugin from "vite-plugin-cdn-import";

export default defineConfig({
    plugins:[
        viteCDNPlugin({
            modules:[{
                name:"lodash",
                var:"_",//全局需要使用的函数
                path:"https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"

            }]
        })
    ]
})

/*
yarn build
yarn run v1.22.19
$ vite build
vite v4.0.4 building for production...
✓ 3 modules transformed.
dist/index.html                0.42 kB
dist/assets/index-bfdf1920.js  0.76 kB │ gzip: 0.43 kB
Done in 0.48s.
*/
```

