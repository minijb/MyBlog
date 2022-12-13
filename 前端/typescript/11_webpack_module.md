现在的一些问题：因为前端也是一个服务器，因此也在做http请求，但是因为我们每一个文件的大小都很小，需要多次请求，这就比较花费时间，我们可以进行webpack进行打包

### 1. install webpack

```sh
npm install --save-dev webpack webpack-cli webpack-dev-server ts-loader typescript
```

tsconfig

```json
{
  "compilerOptions": {
    "target": "ES6",                                 
    "module": "ES6",                                
    "rootDir": "./src",                                  
    "outDir": "./dist",                                   
    "esModuleInterop": true,                             
    "forceConsistentCasingInFileNames": true,            
    "strict": true,                                     
    "experimentalDecorators": true,
    "sourceMap": true
  }
}
```

webpack.config.js

基础配置

```js
const path = require('path');

module.exports = {
    entry: './src/app.ts', //root entry file 类似于c++中的main文件
    output: {
        filename: 'bundle.[contenthash].js', //我们最终输出的js文件,可以使用[]来告诉webpack我们需要为每一个js文件创造一个cash我呢见
        path: path.resolve(__dirname, 'dist') //webpack需要绝对路径
    }
};
```

在webpack中导入typescript

```js
const path = require('path');

module.exports = {
    entry: './src/app.ts', //root entry file 类似于c++中的main文件
    output: {
        filename: 'bundle.[contenthash].js', //我们最终输出的js文件,可以使用[]来告诉webpack我们需要为每一个js文件创造一个cash我呢见
        path: path.resolve(__dirname, 'dist') //webpack需要绝对路径
    },
    devtool: 'inline-source-map',//告诉webpack，map文件直接生成到outfile中
    module: {
        rules: [
            {
                test: /\.ts$/, //check the file end with .ts
                use: 'ts-loader',   //tell webpack any typescirpt file should be handle by ts-loader
                exclude: /node_modules/
            }
        ] 
    },
    resolve: {
        extensions: ['.ts', '.js'], //import 只会默认加载js文件，我们需要它默认加载ts和js文件
    }
};
```

如果使用webpack----说实话需要系统的学习学习

