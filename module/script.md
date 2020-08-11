# script模块

## 基本介绍

这个模块是将脚本传输至远程节点然后执行

## 格式用法

```shell
 ansible host -m script -a 'chdir=/opt /opt/***.sh'
```

例子如下：

```shell
[root@localhost ~]#   ansible testserver -m script -a 'chdir=/opt echothing.sh'
10.10.10.1 | CHANGED => {
    "changed": true,
    "rc": 0,
    "stderr": "Shared connection to 10.10.10.1 closed.\r\n",
    "stderr_lines": [
        "Shared connection to 10.10.10.1 closed."
    ],
    "stdout": "this is a shell script\r\n",
    "stdout_lines": [
        "this is a shell script"
    ]
}
```

## 常用命令

| 命令       | 描述                                                     |
| ---------- | -------------------------------------------------------- |
| chdir      | 在执行命令之前，先切换到该目录                           |
| executable | 切换shell来执行命令，需要使用命令的绝对路径              |
| free_form  | 要执行的Linux指令，一般使用Ansible的-a参数代替。         |
| creates    | 一个文件名，当这个文件存在，则不执行脚本，可以用来做判断 |
| removes    | 一个文件名，这个文件不存在，则不执行脚本，可以用来做判断 |

例子如下：

```shell
----将主机上/home/testfile.txt进行二次更改，并插入到上次更改的block下方
[root@localhost ~]#  ansible testserver -m script -a 'removes=/opt/echothing echothing.sh'
10.10.10.1 | SKIPPED
[root@localhost ~]#  ansible testserver -m script -a 'creates=/opt/echothing echothing.sh'
10.10.10.1 | CHANGED => {
    "changed": true,
    "rc": 0,
    "stderr": "Shared connection to 10.10.10.1 closed.\r\n",
    "stderr_lines": [
        "Shared connection to 10.10.10.1 closed."
    ],
    "stdout": "this is a shell script\r\n",
    "stdout_lines": [
        "this is a shell script"
    ]
}


```

