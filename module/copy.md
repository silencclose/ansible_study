# copy模块

## 基本介绍

这个模块用于将本地文件复制到远程主机，同时支持给定内容生成文件和修改权限等。 

将远程主机的文件复制到本地，使用 [fetch](./fetch.md) 模块。

如果要在windows上使用，请使用[win_copy](./win_copy.md)模块

## 格式用法

```shell
 ansible host -m copy -a 'src=** dest=** OPTIONS' 
```

例子如下：

```shell
[root@localhost testfile]# ansible testserver -m copy -a 'src=./testfile.txt dest=/home/'
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "fcff57d112692c8f1c3d330714358c59f1c268cc",
    "dest": "/home/testfile.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "e9409172a4036cc688f169c72131e921",
    "mode": "0644",
    "owner": "root",
    "size": 9,
    "src": "/root/.ansible/tmp/ansible-tmp-1594345113.518484-106479-157854089062426/source",
    "state": "file",
    "uid": 0
}
```

## 常用选项

| 命令    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| src     | 复制文件的源文件/目录。                                      |
| dest    | 复制文件的目标文件/目录。如果`src`是一个目录，`dest`也必须是一个目录，如果`dest`是不存在的路径，并且如果dest以`/`结尾或者`src`是目录，则`dest`被创建。如果`src`和`dest`是文件，如果`dest`的父目录不存在，任务将失败。 |
| backup  | yes/no，选择在覆盖之前是否将原文件备份，备份文件包含时间信息，默认为no |
| content | 用于替换`src`，可以直接指定文件的值，目的直接将字符串写入到文件里头，此时dest必须为指定的文件名 |
| group   | 指定文件拷贝到远程主机后的属组，但是远程主机上必须有对应的组，否则会报错 |
| owner   | 指定文件拷贝到远程主机后的属主，但是远程主机上必须有对应的用户，否则会报错 |
| mode    | 指定文件拷贝到远程主机后的文件属性                           |
| force   | 当远程主机的目标路径中已经存在同名文件，并且与ansible主机中的文件内容不同时，是否强制覆盖，可选值有yes和no，默认值为yes，表示覆盖，如果设置为no，则不会执行覆盖拷贝操作，远程主机中的文件保持不变 |

例子如下：

```shell
----将/root/test.sh复制到目标机的/opt/目录下
[root@localhost ~]# ansible testserver -m copy -a "src=/root/test.sh dest=/opt/"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "a26aba13067108907311b2d4bc62a7c36e6b7331",
    "dest": "/opt/test.sh",
    "gid": 0,
    "group": "root",
    "md5sum": "85d8c125dacf9f8b2eb22d9512699e26",
    "mode": "0644",
    "owner": "root",
    "size": 18681,
    "src": "/root/.ansible/tmp/ansible-tmp-1594863326.884352-2311-167755938788866/source",
    "state": "file",
    "uid": 0
}
```

```shell
ansible testserver -m copy -a "content="ket=value" dest=/opt/key.txt"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "43e0c0555b304c1c588954dd7ba523bd7acdc470",
    "dest": "/opt/key.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "3b4192889b732e0ee3f1a60f27355781",
    "mode": "0644",
    "owner": "root",
    "size": 9,
    "src": "/root/.ansible/tmp/ansible-tmp-1594864467.575513-9100-260979737863301/source",
    "state": "file",
    "uid": 0
}

```

