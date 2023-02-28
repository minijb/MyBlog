# nginx

高性能的http和反向代理web服务器

## 1. 常用命令

```sh
#启动nginx
start nginx
#停止nginx
nginx -s stop
nginx -s quit
#重载nginx
nignx -s reload
#查看进程
ps -ef | grep nginx
```

## 2. 配置文件

文件位置`conf/nginx.conf`

**文件结构**

**1- 全局快**

开头：`events`主要设置一些影响Nginx服务器整体运行的配置命令，作用域是nginx全局。

**2- events块**

主要影响Nginx服务器与用户的网络链接

- `accept_mutex`默认开启，开启后niginx的多个worker将会以串行的方式来梳理，只有一个worker会被唤起，其他worker继续睡眠
- worker_connections 单个进行最大连接数

**3-http块**

处理代理、缓存和日志定义等绝大数任务的配置

- include ：引入其他的配置文件
- default_type ：如果web程序没有设置，nginx也没对应文件的扩展名就使用default_type 的处理方式进行处理
- log_formal：定义日志格式，只能在http模块中进行配置
- sendfile ： 启用sendfile()系统调用来代替read和write调用，减少系统上下文切换从而提高性能，如果是静态服务器的话，能够极大的提升性能
- keepalive_timeout：http有一个keepAlive模式，告诉webserver在处理一个请求后保持TCP链接的打开时间
- gzip：开启gzip压缩功能，可以是网站的css,js,html,xml文件在传输进行要锁，提高访问速度

**4-server块**

每一个http块都可以包含多个server块，每一个server块相当于一台虚拟主机

- `listen`监听端口或者ip

```nginx
listen 127.0.0.1:8000#只监听127.0.0.1的80端口
listen 8000
listen 127.0.0.1
listen *:8000
listen localhost:8000
```

- server_name nginx 允许一个虚拟主机有一个或多个名字，也可以使用通配符"*"来设置虚拟主机的名字 支持 ip 域名 通配符 正则等

```nginx
 server_name  localhost;
```

**5- localtion块**

之后在反向代理中详细表述





