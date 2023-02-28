# NPM

安装指定版本的npm

```sh
npm i -g npm@5.5.1
```

### package.json

使用`npm init {--yes}`来创建一个项目，会出现一个`pack.json`

### 安装一个包

`npm i underscore`

发生两件事

- package.json中出现新的依赖目录
- 在我们项目种出现一个`node_module`文件夹，存放我们下载的库
  - lib文件中也包含`package.json`,用来描述库文件

> 在老版本中如果没有`--save` package.json中不会出现对应的依赖

```js
const _ = require("underscore");
// if it is a file use uderscore.js
// if it is a folder use underscore/index.js

// find list
// 1. core module
// 2. file or folder
// 3. node_modules

const result  = _.contains([1,2,3], 2);
console.log(result);
```

### 项目共享

1. 如何同步依赖

只要有package.json就可以同步依赖，里面记录了我们需要的依赖，我们只需要使用`npm i`就可以自动安装需要的依赖

2. git项目管理

在`.gitignore`文件内添加`node_modules/`就可以忽略node_modules文件夹，和之前的方法一起操作可以很方便的让我们共享项目

### Semantic Version

在package.json中我们可以看到dependencies中的版本号如下所示`"^1.13.6"`

三个数据的含义分别如下：主要版本号(Major)，Minor,Patch(补丁版本)

Patch 就是修改了bug

Minor：在不修改原本api的情况下添加了新的特性

Major：修改了项目中代码的依赖

如果项目更新的话，使用以下盖规则(这些符号实在版本号字符串的开头)

- `^`的作用表明使用`1`号Major版本,也就是表明`1.x`

- `~`表示使用major和minor版本，也就是说`1.13.x`版本

- 都没有的话说明使用指定版本

