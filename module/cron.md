# cron模块

## 基本介绍

这个模块用于创建定时任务，执行完之后，目标机上可以通过crontab -l 进行查看

## 格式用法

```shell
 ansible host -m cron -a 'name="string"  OPTIONS' 
```

例子如下：

```shell
[root@localhost ~]# ansible testserver -m cron -a "name='echo a string to log' minute=*/1 job='echo `date` > 1111.log'"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "envs": [],
    "jobs": [
        "echo a string to log"
    ]
}
##目标主机上查看
[root@localhost home]# crontab -l
#Ansible: echo a string to log
*/1 * * * * echo Mon Jul 20 09:17:07 CST 2020 > 1111.log
```

## 常用选项

| 命令         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| day          | 设置一月的几日执行( 1-31, *, */2, )，不设置则默认每天        |
| hour         | 设置一日的几时执行( 0-23, *, */2, )，不设置则默认每小时      |
| minute       | 设置一个小时的几分执行( 0-59, *, */2, )，不设置则默认每分钟  |
| month        | 设置一年的几月执行( 1-12, *, /2, )，不设置则默认每月         |
| weekday      | 设置每周的周几执行( 0-6 for Sunday-Saturday,, )，不设置则默认每天 |
| job          | 定时执行的任务内容                                           |
| special_time | 执行任务的特殊时间范围                                       |
| state        | 状态，默认值为present表示添加，absent表示删除                |

例子如下：

```shell
###删除定时任务
[root@localhost ~]# ansible testserver -m cron -a  "name='echo a string to log' minute=*/1 job='echo `date` > 1111.log' state=absent"
10.10.10.1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "envs": [],
    "jobs": []
}
##查看目标主机定时任务已经删除
```
