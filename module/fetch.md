# fetch模块

## 基本介绍

这个模块用于将远程主机中的文件拷贝到本机中。 拉取到本机后，会在目标目录下对应目标主机ip的文件夹，里头的文件路径就是远程机的文件路径

## 格式用法

```shell
 ansible host -m fetch -a 'src=** dest=** OPTIONS' 
```

例子如下：

```shell

[root@localhost ~]# ansible testserver -m fetch -a 'src=/home/testfile.txt dest=/root/'
10.10.10.1 | CHANGED => {
    "changed": true,
    "checksum": "fcff57d112692c8f1c3d330714358c59f1c268cc",
    "dest": "/root/10.10.10.1/home/testfile.txt",
    "md5sum": "e9409172a4036cc688f169c72131e921",
    "remote_checksum": "fcff57d112692c8f1c3d330714358c59f1c268cc",
    "remote_md5sum": null
}
```

## 常用选项

| 命令              | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| src               | 拉取文件的源文件，不能为目录。                               |
| dest              | 用来存放文件的目录。                                         |
| fail_on_missing   | 当源文件不存在的时候，是否标识为失败                         |
| flat              | 允许覆盖默认行为从ip/path到/file的，如果dest以/结尾，它将使用源文件的基础名称 |
| validate_checksum | 指定文件拷贝到远程主机后的属组，但是远程主机上必须有对应的组，否则会报错 |

例子如下：

```shell
----将目标主机上的/home/testfile.txt文件拉取到/root目录下
[root@localhost ~]# ansible testserver -m fetch -a 'src=/home/testfile.txt dest=/root/ flat=yes'
192.168.1.205 | CHANGED => {
    "changed": true,
    "checksum": "fcff57d112692c8f1c3d330714358c59f1c268cc",
    "dest": "/root/testfile.txt",
    "md5sum": "e9409172a4036cc688f169c72131e921",
    "remote_checksum": "fcff57d112692c8f1c3d330714358c59f1c268cc",
    "remote_md5sum": null
}
```

```shell
----将目标主机上的/home/testfile.txt文件拉取到/root目录下,并重命名为test222.txt
[root@localhost ~]# ansible testserver -m fetch -a 'src=/home/testfile.txt dest=/root/test222.txt flat=yes'
192.168.1.205 | CHANGED => {
    "changed": true,
    "checksum": "fcff57d112692c8f1c3d330714358c59f1c268cc",
    "dest": "/root/test222.txt",
    "md5sum": "e9409172a4036cc688f169c72131e921",
    "remote_checksum": "fcff57d112692c8f1c3d330714358c59f1c268cc",
    "remote_md5sum": null
}
```

