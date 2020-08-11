# group模块

## 基本介绍

这个模块进行组的管理

## 格式用法

```shell
 ansible host -m group -a 'name=string   OPTIONS' 
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m group -a 'name=silence gid=3333'
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "gid": 3333,
    "name": "silence",
    "state": "present",
    "system": false
}
```

## 常用选项

| 命令   | 描述                                    |
| ------ | --------------------------------------- |
| name   | 设置的组名                              |
| gid    | 设置组id                                |
| state  | 默认值为present表示添加，absent表示删除 |
| system | 若为yes表示该组为系统组                 |

例子如下：

```shell
###删除silence组
[root@localhost ~]#  ansible testserver -m group -a 'name=silence gid=3333 state=absent'
10.10.10.1| CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "name": "silence",
    "state": "absent"
}
```