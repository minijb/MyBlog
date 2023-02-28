查看当前模块情况

```js
console.log(module);

:!node app.js
Module {
  id: '.',
  path: '/home/zhouhao/home/js/node_t',
  exports: {},
  filename: '/home/zhouhao/home/js/node_t/app.js',
  loaded: false,
  children: [],
  paths: [
    '/home/zhouhao/home/js/node_t/node_modules',
    '/home/zhouhao/home/js/node_modules',
    '/home/zhouhao/home/node_modules',
    '/home/zhouhao/node_modules',
    '/home/node_modules',
    '/node_modules'
  ]
}

```

###  创建一个module

```js
var url = 'http://mylogger.io/log';

function log(message){
	console.log(message);
}
//until now they are private

module.exports.log = log;
```

### 加载一个module

```js
const logger = require('./logger');
logger.log("hello");
```

### module wrapper function

node会在我们代码外面默认的包裹一层函数，它会提供一些参数如

`exports , require, module, __filename ,__dirname`

我们可以使用这些做一些事情，比如导入模块，输出模块等等等

`module.exports`可以简写为`exports.xxxx=xxx`,但是注意我们不能直接对exports赋值！！！！

## 常用module

### Path module

```js
const path = require("path");
const pathObj = path.parse(__filename);//将位置解析为对象
console.log(pathObj);

```

### OS module

```js
const os = require("os");

console.log(os.totalmem());
console.log(os.freemem());

```

### fs module

同步方法

```js
const fs = require("fs");

const files = fs.readdirSync('./');
console.log(files);

```

异步方法

```js
fs.readdir('./',function(err,files){
	if (err) console.log('Error',err);
	else console.log(files);
})
```

### Event module

EventEmitter node核心组件！！！

> EventEmitter is a class!!!

```js
const EventEmitter = require("events");
const emitter = new EventEmitter();

//register a listener

emitter.on('messageLogged', function(arg){
	console.log("Listener called",arg);
});


//raise an event
emitter.emit("messageLogged", {id: 1, url: "http://"});
```

注入如果跨文件的话，emitter文件就不会触发，我们可以自己创建一个继承EventEmitter的类

```js
//logger.js
const EventEmitter = require("events");


class Logger extends EventEmitter {
	log(message) {
		console.log(message);
		this.emit("messageLogged",{id:1 , http:"http://"});
	}
}

module.exports = Logger;



//app.js
const EventEmitter = require("events");

const logger = require('./logger');

const logger = new Logger();
logger.on("messageLogged", (arg)=>{
	console.log('Listener called', arg);
})

logger.log('message');

```

### http module

```js
const http = require("http");

const server = http.createServer( (req , res) => {
	if (req.url === "/"){
		res.write("hello world");
		res.end();
	}
} );
server.on("connection", (socket) => {
	console.log("new connection");
})
server.listen(3000);

console.log('listening on port 3000...');

```





