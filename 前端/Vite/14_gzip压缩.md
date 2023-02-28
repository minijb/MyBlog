# gzip压缩

将所有的静态文件进行压缩，达到减少体积的目的

服务点 --》压缩文件  --》 客户端  --》 解压缩

> 有的时候vite build的时候会有提示chunk太大：chunk会映射为js文件，但是chunk不是js文件。
>
> 解决方法：
>
> - 使用分包策略
> - 动态导入
> - 配置gzip压缩
> - 忽略warning

需要插件`vite-plugin-compression`

`yarn add vite-plugin-compression -D`

```js
import { defineConfig } from "vite";
import checker from "vite-plugin-checker";
import path from "path";
//***********************************************
import viteCompression from "vite-plugin-compression";
//***********************************************
export default defineConfig({
    "build": {
        "minify":false,
        "rollupOptions" : {
            "input":{
                main: path.resolve(__dirname , "./index.html"),
                product: path.resolve(__dirname , "./product.html")
            },
            "output" :{
                "manualChunks": (id) => {
                    console.log(id);
                    if( id.includes("node_modules")){
                        // return "vendor";
                    }
                }
            }
        }
    },
    plugins: [
        checker({typescript:true}),
        //***********************************************
        viteCompression()
        //***********************************************
    ]
})


/*
✨ [vite-plugin-compression]:algorithm=gzip - compressed file successfully:    
dist/C:/temp/Project/vue-pro/vite-ts/assets/lodash-7f3da59a.js.gz   208.34kb / gzip: 40.33kb
*/
```

需要和后端人员配合，读取gzip文件后设置一个响应头`content-encoding --> gzip`浏览器收到响应结果，知道了gzip文件就会解压，浏览器会有一定的解压时间因此如果体积不是很大就别gzip压缩。