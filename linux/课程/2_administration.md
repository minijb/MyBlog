## sed

- replace a string in a file with new string
- find and delete a line
- remove empty lines
- remove the first or n lines in a file
- replace tabs with space
- show defined lines from a file
-  substitute within vi 

## user account management

```shell
#commands
useradd
groupadd
userdel
groupdel
usermod
#files
/etc/passwd
/etc/group
/etc/shadow
```

## switch user

```shell
su -/[username]
visudo
#权限管理
/etc/sudoers
```

## monitor user

```shell
who
last
w
finger
id.
```

## talk to user

```shell
wall
users #查看当前使用的用户
write [user]
```

```shell
which
 - Search the PATH environment variable and display the location of any matching executables:
   which {{executable}}

 - If there are multiple executables which match, display all:
   which -a {{executable}}

```

## processes and jobs

```shell
application = service #programe ppt apache
script #shell script or commands are list of instruction
process #service --> process
daemon #守护进程 --- run until interrupted ---- keep running
thread #process have many threads
job #created by  scheduler = run a service or process at a schedula time
```

常用命令

```shell
systemctl or service #process and programe
ps #查看进程
top #查看进程 --- 相当于任务管理器
kill #kill procress
crontab #schedule the programme
at 
Execute commands once at a later time.Service atd (or atrun) should be running for the actual executions.More information: https://manned.org/at.

 - Execute commands from standard input in 5 minutes (press Ctrl + D when done):
   at now + 5 minutes

 - Execute a command from standard input at 10:00 AM today:
   echo "{{./make_db_backup.sh}}" | at 1000

 - Execute commands from a given file next Tuesday:
   at -f {{path/to/file}} 9:30 PM Tue
```

> systemd 就是通过systemctl 来管理进程的
>
> 现在一般都是是哦应该能systemd。

- crontab 

https://www.runoob.com/linux/linux-comm-crontab.html

- systemd

https://www.liuvv.com/p/c9c96ac3.html

- ps

```shell
-e  #show all running procrsses  
-f # formate 一般直接-ef
ps aus #show all running procrsses in BSD formate
ps -u [username]
```

- top

```shell
-u [username] #只看某个用户的
press c #看绝对路径
press k #杀死某个进程
```

- kill

```shell
-1 #restart
-2 #interrupt from the keyboard
-9 #forcefully kill
-15 #kill a process gracefully
```

> 其他kill
>
> ```
> killall
> pkill
> ```
>

- crontab

```shell
crontab -e #edit
crontab -l #list
crontab -r #remove
crontab #crontab daemon/service
systemctl status crond # manage the crond service
* * * * *
minutue hour day month (day of week) <command>
```

> four types of cronjobs
>
> - hour
> - daily
> - weekly
> - monthly
>
> all the above crons are set up in
>
> - /etc/cron.xxx
>
> the timing for 
>
> - /etc/anacrontab
>
> for hourly
>
> - /etc/cron.d/0hourly

## manage process

```shell
#put a process background 会暂停进程！！！！
Ctrl-z
jobs # 查看停止的进程
bg #让进程在后台运行
#foreground 
fg

#无论前台还是后台，终端退出

#run process even after exit
nohub [process] &
nohub sleep 100 &
#kill process by name 
pkill
#process priority
nice -n [num] [process]
#-20 - 19 the lower more priority
#查看进程
top   ps
```

> nohub
>
> /dev/null 表示空设备文件
> 0 表示stdin标准输入
> 1 表示stdout标准输出
> 2 表示stderr标准错误
> \> file 表示将标准输出输出到file中，也就相当于 1>file
>
> 2> error 表示将错误输出到error文件中
>
> 2>&1 也就表示将错误重定向到标准输出上
>
> 2>&1 >file ：错误输出到终端，标准输出重定向到文件file，等于 > file 2>&1(标准输出重定向到文件，错误重定向到标准输出)。
>
> & 放在命令到结尾，表示后台运行，防止终端一直被某个进程占用，这样终端可以执行别到任务，配合 >file 2>&1可以将log保存到某个文件中，但如果终端关闭，则进程也停止运行。如 command > file.log 2>&1 & 。
>
> nohup放在命令的开头，表示不挂起（no hang up），也即，关闭终端或者退出某个账号，进程也继续保持运行状态，一般配合&符号一起使用。如nohup command &。

## system monitoring

```shell
top
df #查看系统硬盘存储情况 -h常用
dmesg #system related warning
iostat 1
netstat
free
cat /proc/cpuinof
cat /proc/meminfo
```

## log monitoring

in `/var/log`

```shell
boot
chronyd
cron
maillog
secure
messages
httpd
```

## system maintenace command

```shell
shutdown
init 0-6
reboot
halt
```

## change hostname

```
hostnamectl set-hostname [name]
```

## 系统信息

```
uname
arch
```

## ctrl

```shell
ctrl-u #清除本行
ctrl-c
ctrl-z
ctrl-d # exit the terminal
```

## 环境变量

```shell
#查看 environment
env
#view one environment variable
echo $[name]
#set environment variables 临时的
export xxx=xxx
echo $xxx
#set environment variable permanently
vi .bashrc
TEST = xxx
export TEST
#set global environment variable permanently
vi /etc/profile or /etc/bashrc
```

