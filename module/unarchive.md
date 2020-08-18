# unarchive模块

## 基本介绍

解压缩模块

## 格式用法

```shell
 ansible host -m  unarchive -a 'src=** dest=** '
```

例子如下：

```shell
[root@localhost ~]#  ansible testserver -m unarchive -a 'src=apache-ant-1.9.13-bin.tar.gz dest=/opt/'
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "dest": "/opt/",
    "extract_results": {
        "cmd": [
            "/usr/bin/gtar",
            "--extract",
            "-C",
            "/opt/",
            "-z",
            "-f",
            "/root/.ansible/tmp/ansible-tmp-1597133725.8448982-32593-174444606428201/source"
        ],
        "err": "",
        "out": "",
        "rc": 0
    },
    "gid": 0,
    "group": "root",
    "handler": "TgzArchive",
    "mode": "0755",
    "owner": "root",
    "size": 103,
    "src": "/root/.ansible/tmp/ansible-tmp-1597133725.8448982-32593-174444606428201/source",
    "state": "directory",
    "uid": 0
}

```

## 常用命令

| 命令       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| src        | 被解压缩的文件路径                                           |
| dest       | 远端主机解压缩路径                                           |
| remote_src | 设置为yes表示解压缩目标上已存在的存档文件，设置为no表示将文件复制到远端主机再压缩，默认为no |
| creates    | 判断某个文件加是否存在，若存在，则不进行解压缩               |
| mode       | 设置解压缩后的文件权限，类似chmod                            |
| owner      | 设置解压锁后的文件的所有者，类似chown                        |
| keep_newer | 不要替换比归档文件更新的现有文件                             |

```shell

[root@localhost ~]#  ansible testserver -m unarchive -a 'src=apache-ant-1.9.13-bin.tar.gz dest=/opt/ keep_newer=yes'
10.10.10.1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "dest": "/opt/",
    "gid": 0,
    "group": "root",
    "handler": "TgzArchive",
    "mode": "0755",
    "owner": "root",
    "size": 103,
    "src": "/root/.ansible/tmp/ansible-tmp-1597135497.4007685-43101-112313424920681/source",
    "state": "directory",
    "uid": 0
}

```

