# command模块

## 基本介绍

这个模块可以直接在远程主机上执行命令，并将结果返回本主机。 

命令不会通过shell处理，因此诸如“$HOME”的变量和诸如“<”、“>”、“\”、“；”、“&”等操作将不会被处理，如果需要这些功能，请使用[shell](./shell.md)模块。

如果要在windows上使用，请使用[win_command](./win_command.md)模块

## 格式用法

```shell
 ansible host -m command -a 'OPTIONS and CMD' 
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m command -a "chdir=/root ls"
10.10.50.10 | CHANGED | rc=0 >>
anaconda-ks.cfg
Desktop
Documents
Downloads
initial-setup-ks.cfg
init_v1.sh
iso
Music
ops_scripts
Pictures
Public
Templates
Videos

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
----进入/root用户后，验证iso目录是否存在，若不存在执行ls
[root@localhost ~]# ansible testserver -m command -a "chdir=/root removes=/root/iso cmd='ls /root/iso'"
10.10.50.10 | SUCCESS | rc=0 >>
skipped, since /root/iso exists
```

```shell
----进入/home用户后，验证nginx目录是否存在，若存在执行ls
[root@localhost ~]# ansible testserver -m command -a "chdir=/home removes=nginx ls nginx "
10.10.50.10 | CHANGED | rc=0 >>
nginx
nginx-1.16.1
```

