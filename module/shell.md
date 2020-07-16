# shell模块

## 基本介绍

这个模块可以直接在远程主机上执行shell命令， 支持shell的各种功能，例如管道等。

它几乎完全类似于[command](./command.md)模块，但通过远程节点上的shell（`/bin/sh'）运行命令。 

如果要在windows上使用，请使用[win_shell](./win_shell.md)模块

## 格式用法

```shell
 ansible host -m shell -a 'OPTIONS and CMD' 
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m shell -a "ps -ef|grep java"
10.10.10.1 | CHANGED | rc=0 >>
root      9381  9376  0 17:03 pts/0    00:00:00 /bin/sh -c ps -ef|grep java
root      9383  9381  0 17:03 pts/0    00:00:00 grep java

```

## 常用命令

| 命令       | 描述                                                       |
| ---------- | ---------------------------------------------------------- |
| chdir      | 在执行命令之前，先切换到该目录                             |
| executable | 切换shell来执行命令，需要使用命令的绝对路径                |
| free_form  | 要执行的Linux指令，一般使用Ansible的-a参数代替。           |
| creates    | 一个文件名，当这个文件存在，则该命令不执行，可以用来做判断 |
| removes    | 一个文件名，这个文件不存在，则该命令不执行，可以用来做判断 |

例子如下：

```shell
----进入/root目录后，验证iso目录是否存在，若不存在执行“ps -ef|grep java”
[root@localhost ~]# ansible testserver -m shell -a "chdir=/root creates=/root/iso ps -ef|grep java"
10.10.10.1 | SUCCESS | rc=0 >>
skipped, since /root/iso exists
```

```shell
----进入/root后，验证/root/iso目录是否存在，若存在执行“ps -ef|grep java”
[root@localhost ~]# 
[root@localhost ~]# ansible testserver -m shell -a "chdir=/root removes=/root/iso ps -ef|grep java"
10.10.10.1 | CHANGED | rc=0 >>
root      9643  9638  0 17:05 pts/0    00:00:00 /bin/sh -c ps -ef|grep java
root      9645  9643  0 17:05 pts/0    00:00:00 grep java
```

