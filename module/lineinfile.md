# lineinfile模块

## 基本介绍

这个模块用对目标主机上的文件进行编辑，编辑前会先进行检查，若存在

## 格式用法

```shell
 ansible host -m lineinfile  -a 'path=filePath block='string'  OPTIONS '
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m blockinfile -a "path=/home/testfile.txt block='host: 0.0.0.0\nport: 8080\n' force=yes"
 ansible testserver -m lineinfile -a "path=/home/testfile.txt line='data: /home/data' force=yes"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "msg": "Block inserted"
}
---------------
###目标主机上/home/testfile.txt文件为
[root@localhost home]# cat testfile.txt
testfile
# BEGIN ANSIBLE MANAGED BLOCK
host: 0.0.0.0
port: 8080
# END ANSIBLE MANAGED BLOCK
```

## 常用命令

| 命令         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| path         | 在目标主机上被操作的具体文件路径                             |
| backup       | 更改前是否创建备份文件，使用时间戳                           |
| block        | 配置文件内容之间的分隔符                                     |
| create       | 设置为yes时，若被更改的文件未存在则会自动创建                |
| group        | 定义文件的组的属性，类似chown                                |
| insertafter  | 在上一个已经进行blockinfile操作的文件中，将新的内容插入到某个block到后面 |
| insertbefore | 在上一个已经进行blockinfile操作的文件中，将新的内容插入到某个block到前 |
| marker_begin | 在block前面插入##开始标记信息                                |
| marker_end   | 在block结尾插入##结束标记信息                                |

例子如下：

```shell
----将主机上/home/testfile.txt进行二次更改，并插入到上次更改的block下方
[root@localhost ~]#  ansible testserver -m blockinfile -a "path=/home/testfile.txt block='logs:/home/testfile.log\npid:/home/testpid' insertafter='host: 0.0.0.0\nport: 8080\n' marker_begin='this is log begin' marker_end='this is pid end'"
192.168.1.205 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "msg": "Block inserted"
}
---------------
###目标主机上/home/testfile.txt文件为
[root@localhost home]# cat testfile.txt
testfile
# BEGIN ANSIBLE MANAGED BLOCK
host: 0.0.0.0
port: 8080
# END ANSIBLE MANAGED BLOCK
# this is log begin ANSIBLE MANAGED BLOCK
logs:/home/testfile.log
pid:/home/testpid
# this is pid end ANSIBLE MANAGED BLOCK

```