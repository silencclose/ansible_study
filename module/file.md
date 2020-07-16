# file模块

## 基本介绍

这个模块用于进行文件的管理，比如创建文件、创建链接文件、删除文件、修改文件、以及对文件权限属性的维护和管理。 

如果要在windows上使用，请使用[win_command](./win_command.md)模块

## 格式用法

```shell
 ansible host -m file -a 'path="filePath" OPTIONS ' 
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m file -a "path=/opt/test.sh state=touch"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "dest": "/opt/test.sh",
    "gid": 0,
    "group": "root",
    "mode": "0644",
    "owner": "root",
    "size": 0,
    "state": "file",
    "uid": 0
}
```

## 常用命令

| 命令 | 描述 |
| ---- | ---- |
|path|必选项，定义需要操作的目标主机上的文件/目录的路径|
|state|对文件操作的状态描述，包括如下选项：<br/>    directory：如果目录不存在，创建目录<br/>    file：即使文件不存在，也不会被创建<br/>    link：创建软链接<br/>    hard：创建硬链接<br/>    touch：如果文件不存在，则会创建一个新的文件，如果文件或目录已存在，则更新其最后修改时间<br/>    absent：删除目录、文件或者取消链接文件|
|force|需要在两种情况下强制创建软连接，一种是源文件不存在但之后会建立的情况，另一种是目标软连接已存在，需要先取消之前的软连接，然后在创建软连接，两种选项yes/no|
|group|定义文件/目录属性，用于指定被操作文件的属组，属组对应的组必须在远程主机中存在，否则会报错|
|mode|定义文件/目录的权限|
|owner|定义文件/目录的属性，用于指定被操作文件的属主，属主对应的用户必须在远程主机中存在，否则会报错。|
|recurse|递归的设置文件的属性，只对目录有效|
|src|要被软连接的源文件的路径，只适用于state=link的情况|
|dest|被连接到的路径，，只适用于state=link的情况|
例子如下：

```shell
----删除文件/opt/test.sh
[root@localhost ~]# ansible testserver -m file -a "path=/opt/test.sh state=absent force=yes"
10.10.10.1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "path": "/opt/test.sh",
    "state": "absent"
}
```

```shell
----为/otp/test.sh创建软连接/opt/ls-test.sh，若已经存在则取消
[root@localhost ~]# ansible testserver -m file -a "path=/opt/ls-test.sh src=/opt/test.sh force=no state=link"
192.168.1.205 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "dest": "/opt/ls-test.sh",
    "gid": 0,
    "group": "root",
    "mode": "0777",
    "owner": "root",
    "size": 12,
    "src": "/opt/test.sh",
    "state": "link",
    "uid": 0
}
```
