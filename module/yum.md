# yum模块

## 基本介绍

 yum模块，通过yum源管理软件包。该模块是通过python2进行管理，如果是python3，请使用[dnf模块](./dnf.md)

## 格式用法

```shell
 ansible host -m yum -a 'OPTIONS and CMD' 
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m yum -a "name=gcc-c++"
10.10.10.1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "msg": "",
    "rc": 0,
    "results": [
        "gcc-c++-4.8.5-28.el7.x86_64 providing gcc-c++ is already installed"
    ]
}
```

## 常用命令

| 命令          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| name          | 需要安装的包名字                                             |
| state         | 用于指定软件包的状态 ，默认值为。present，表示确保软件包已经安装，除了。present，其他可用值有 installed、latest、absent、removed，其中 installed 与present 等效，latest 表示安装 yum 中最新的版本，absent 和 removed 等效，表示删除对应的软件包。 |
| download_only | 只下载包，不进行安装                                         |
| download_dir  | 当`download_only=yes`时，指定下载目录                        |


例子如下：

```shell
[root@localhost ~]# ansible testserver -m yum -a "name=telnet  download_only=yes download_dir=/root/"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "changes": {
        "installed": [
            "telnet"
        ]
    },
    "msg": "",
    "rc": 0,
    "results": [
        "Loaded plugins: fastestmirror, langpacks\nLoading mirror speeds from cached hostfile\nResolving Dependencies\n--> Running transaction check\n---> Package telnet.x86_64 1:0.17-64.el7 will be installed\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package       Arch          Version                 Repository            Size\n================================================================================\nInstalling:\n telnet        x86_64        1:0.17-64.el7           centos-source         64 k\n\nTransaction Summary\n================================================================================\nInstall  1 Package\n\nTotal download size: 64 k\nInstalled size: 113 k\nBackground downloading packages, then exiting:\nexiting because \"Download Only\" specified\n"
    ]
}

```