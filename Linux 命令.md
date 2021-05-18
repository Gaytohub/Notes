Linux 命令：

```shell
// 查看当前文件夹下 24 小时内修改过的 .md 文件
find . -name '*.md' -mtime 0 

// 查看当前文件夹下 最近 24 - 48 小时修改过的常规文件
find . -type f -mtime 1

// 查看当前文件夹下 一天前修改过的常规文件
find . -type f -mtime +1

// 查看当前文件夹下 最近 30 分钟修改过的常规文件
find . -type f -mmin -30

atime ： 访问时间 
ctime ： 变更时间
mtime ： 修改时间
```



```shell
// 查找某进程 PID
ps -ef | grep 进程名

// 以上操作能获取某进程名的 PID，以下操作可以根据 pid 关闭进程
kill -9 pid
```



```shell
// grep 是强大的文本搜索工具，使用正则表达式搜素文本，并把匹配的行打印出来。

// 查询且忽略大小写
grep -i
```



Linux 管道符 '|'

Linux 提供的管道符将两个命令隔开，管道符左边命令的输出就会作为管道符右边命令的输入。

**管道命令只处理前一个命令正确输出，不处理错误输出。 管道命令右边命令，必须能够接收标准输入流命令才行。**

```shell
// 使用多个管道的例子
cat /etc/password | grep /bin/bash | wc -l
```

第一个管道将 cat 命令（显示password文件内容）的输出送给 grep 命令， grep 命令找出含有 ‘bin/bash’ 的所有行。第二个管道将 grep 的输出送给 wc 命令，wc 命令统计输出的行数。 **这个命令的功能在于找出系统中有多少个用户使用 bash **



