# assemble模块

## 基本介绍

这个模块用于将多份配置文件组装为一份配置文件。当某个特定的程序只能采用单个配置文件，并且不支持`conf.d`风格的结构，这就需要将多个配置文件组装成一个配置文件。

该模块不怎么常用，仅作参考。

## 格式用法

```shell
 ansible host -m assemble -a 'src=configdir  dest=configfie.conf OPTIONS '
```

例子如下：

```shell
[root@localhost mysql_conf]# ansible testserver -m assemble -a "src=/root/mysql_conf dest=/etc/my_multi.cnf remote_src=false "
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "4183845ea9681da0328a776c957ea1ada6e6190c",
    "dest": "/etc/my_multi.cnf",
    "gid": 0,
    "group": "root",
    "md5sum": "c5f42eddf777ef2d2b0f7263c094c20f",
    "mode": "0644",
    "owner": "root",
    "size": 8,
    "src": "/root/.ansible/tmp/ansible-tmp-1594948718.466416-115511-165487978346995/src",
    "state": "file",
    "uid": 0
}
```

## 常用命令

| 命令       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| src        | 用来组装的源文件/目录                                        |
| dest       | 必选项，定义需要操作的目标主机上的文件/目录的路径            |
| backup     | 是否创建备份文件，使用时间戳                                 |
| delimiter  | 配置文件内容之间的分隔符                                     |
| attributes | 生成的文件或目录应该具有的属性                               |
| group      | 定义文件/目录的组的属性                                      |
| mode       | 定义文件/目录的权限                                          |
| owner      | 定义文件/目录的属性，用于指定被操作文件的属主，属主对应的用户必须在远程主机中存在，否则会报错 |
| regexp     | 仅当满足正则表达式的文件才被进行组装，若未设置，则表示所有文件 |
| remote_src | 当yes的时候，表示使用目标主机上的src文件<br/>当no的时候，表示使用本地主机（管理主机）上的src文件 |
| dest       | 被连接到的路径，，只适用于state=link的情况                   |

例子如下：

```shell
----将本地/root/mysql_conf目录下的文件组装到目标主机的/etc/my_multi.cnf文件中，并对原来文件进行备份

[root@localhost mysql_conf]# ansible testserver -m assemble -a "src=/root/mysql_conf dest=/etc/my_multi.cnf remote_src=false delimiter='####this is a  mysql conf#### ' backup=yes "
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "backup_file": "/etc/my_multi.cnf.19649.2020-07-17@17:21:12~",
    "changed": true,
    "checksum": "cc47fea4835d9ac945df6ae8b224771ea58f2788",
    "dest": "/etc/my_multi.cnf",
    "gid": 0,
    "group": "root",
    "md5sum": "be7753d1f9f9028d7a3e541ded7a4923",
    "mode": "0644",
    "owner": "root",
    "size": 58,
    "src": "/root/.ansible/tmp/ansible-tmp-1594948930.7069428-116768-129707311055502/src",
    "state": "file",
    "uid": 0
}
```