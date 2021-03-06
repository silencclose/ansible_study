# user模块

## 基本介绍

用户管理模块，具体使用方法见[运维自动化神器ansible之user模块](https://www.cnblogs.com/miaocunf/p/11157365.html)

## 格式用法

```shell
 ansible host -m user -a 'OPTIONS and CMD' 
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m user -a "name=nginx append=yes"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "append": true,
    "changed": true,
    "comment": "",
    "group": 1001,
    "home": "/home/nginx",
    "move_home": false,
    "name": "nginx",
    "shell": "/sbin/nologin",
    "state": "present",
    "uid": 1001
}
```

## 常用命令

| 命令        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| name        | 需要创建的用户名                                             |
| uid         | 指定用户的UID                                                |
| create_home | 是否创建用户的家目录，默认是yes                              |
| group       | 指定用户所在的组                                             |
| groups      | 指定用户属组，可以创建时候指定，也可以用来进行管理用户更改已经存在的用户所属组 |
| append      | groups参数一起使用管理用户属组，默认为false，如果 `append='yes'`，则从groups参数中增加用户的属组；如果 `append='no'` ，则用户属组只设置为groups中的组，移除其他所有属组 |
| state       | 判断用户是否存在，两个选项absent和present，默认为present     |
| remove      | 在 `state=absent` 时使用，等价于 `userdel --remove` 布尔类型，默认值为 false。 |
| force       | 在 `state=absent` 时使用，等价于 `userdel --force`，布尔类型，默认值为 false。 |
| home        | 用于指定用户home目录，值为路径                               |
| passwd      | 用于指定用户密码，但是这个密码不能是明文密码，而是一个对明文密码加密后的字符串，默认为空 |

例子如下：

```shell
[root@localhost ~]#  ansible testserver -m user  -a "name=testuser uid=2000 "
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "comment": "",
    "create_home": true,
    "group": 2000,
    "home": "/home/testuser",
    "name": "testuser",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 2000
}

```