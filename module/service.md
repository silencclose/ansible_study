# service模块

## 基本介绍

这个模块可以控制远端主机进行服务管理，包括服务的启动停止重启和其他相关配置

## 格式用法

```shell
 ansible host -m service -a 'name=*** state=***'
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m service -a 'name=nfs state=started'
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "name": "nfs",
    "state": "started",
    "status": {
        "ActiveEnterTimestampMonotonic": "0",
        "ActiveExitTimestampMonotonic": "0",
        "ActiveState": "inactive",
        "After": "rpcbind.socket system.slice systemd-journald.socket rpc-gssd.service network-online.target gssproxy.service nfs-idmapd.service proc-fs-nfsd.mount nfs-mountd.service nfs-config.service local-fs.target rpc-statd.service",
        "AllowIsolate": "no",
        "AmbientCapabilities": "0",
        "AssertResult": "no",
        "AssertTimestampMonotonic": "0",
        "Before": "rpc-statd-notify.service"
```

## 常用命令

| 命令    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| name    | 服务                                                         |
| state   | 用于指定服务的状态，可用值有 started、stopped、restarted、reloaded |
| enabled | 指定是否将服务设置为开机启动项，设置为 yes 表示将对应服务设置为开机启动，设置为 no 表示不会开机启动 |
| args    | 服务启动参数                                                 |
| sleep   | restarted时，在stop and start 之间沉睡时长，单位为秒，不是所有的服务都支持，当服务使用systemd管理时将忽略 |

```shell
[root@localhost ~]# ansible testserver -m service -a 'name=nfs state=restarted sleep=10'
[WARNING]: Ignoring "sleep" as it is not used in "systemd"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "name": "nfs",
    "state": "started",
    "status": {
        "ActiveEnterTimestamp": "Tue 2020-08-11 23:44:41 CST",
        "ActiveEnterTimestampMonotonic": "1124790899474",
        "


```

