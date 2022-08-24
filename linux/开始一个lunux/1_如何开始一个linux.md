### 安装git

```sh
sudo apt install git
```

简单的配置一下

设置用户名,邮箱

```shell
git config --global user.name xxx
git config --global user.email xxx
```

### 安装clash

>查看ip地址
>
>需要安装ifconfig
>
>```sh
>sudo apt install net-toos
>ifconfig
>```

- github地址https://github.com/Fndroid/clash_for_windows_pkg/releases

<img src="https://s2.loli.net/2022/08/14/KwRvdzAsFJy64qB.png" alt="image.png" style="zoom:50%;" />

- 使用ssh工具将压缩包传输到linux上

> ubuntu开启ssh
>
> 安装openssh-service
>
> ```sh
> sudo apt-get install openssh-server
> #启动ssh
> sudo service ssh start
> #查看是否移动
> sudo ps -e | grep ssh
> ```

- 解压缩包

> https://blog.csdn.net/qq_43605009/article/details/118445287

- 使用

```sh
./cfw
```

### 配置代理

#### 全局代理

在ubuntu的设置->网络->网络代理中设置

#### 终端代理

https://www.zhihu.com/question/472418041

