# ping模块

主要用于主机联通性测试，这个模块在联通成功后返回“pong”。它在playbooks中没有意义，主要用来验证ssh配置以及是否配置了可用的Python。

这不是icmp ping，这只是一个简单的测试模块

需要在远程节点上使用Python。

对于Windows目标，请改用[win_ping]模块。对于网络目标，请使用[net_ping]模块。

## 用法

```shell
ansible  host  -m ping
```

如果能够联通，则显示效果如下

```shell
[root@localhost ~]# ansible testserver -m ping
10.10.10.1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

如果不能够联通，则显示效果如下

```shell
[root@localhost ~]# ansible testserver -m ping
10.10.10.1 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: root@10.10.10.1: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).",
    "unreachable": true
}
```

