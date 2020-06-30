# 安装方式

具体可参照官网，本人使用用pip install ansible安装

下面放入几个整理不错的博客

 [自动化运维工具——ansible详解（一）](https://www.cnblogs.com/keerya/p/7987886.html) 

 [自动化运维工具——ansible详解（二）](https://www.cnblogs.com/keerya/p/8004566.html) 

[ Ansible--Ansible之Playbook](https://www.cnblogs.com/yanjieli/p/10969299.html)

# 命令工具介绍

- ​    **ansible**：Ansibe AD-Hoc 临时命令执行工具，常用于临时命令的执行
- ​    **ansible-doc**：Ansible 模块功能查看工具
- ​    **ansible-galaxy**：载/上传优秀代码或Roles模块 的官网平台，基于网络的
- ​    **ansible-playbook**：Ansible 定制自动化的任务集编排工具
- ​    **ansible-pull**：Ansible远程执行命令的工具，拉取配置而非推送配置（使用较少，海量机器时使用，对运维的架构能力要求较高）
- ​    **ansible-vault**：Ansible 文件加密工具
- ​    **ansible-console**：Ansible基于Linux Consoble界面可与用户交互的命令执行工具

## ansible命令

    ansible <host-pattern> [-f forks] [-m module_name] [-a args]
   usage: ansible [-h] [--version] [-v] [-b] [--become-method BECOME_METHOD]
               [--become-user BECOME_USER] [-K] [-i INVENTORY] [--list-hosts]
               [-l SUBSET] [-P POLL_INTERVAL] [-B SECONDS] [-o] [-t TREE] [-k]
               [--private-key PRIVATE_KEY_FILE] [-u REMOTE_USER]
               [-c CONNECTION] [-T TIMEOUT]
               [--ssh-common-args SSH_COMMON_ARGS]
               [--sftp-extra-args SFTP_EXTRA_ARGS]
               [--scp-extra-args SCP_EXTRA_ARGS]
               [--ssh-extra-args SSH_EXTRA_ARGS] [-C] [--syntax-check] [-D]
               [-e EXTRA_VARS] [--vault-id VAULT_IDS]
               [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
               [-f FORKS] [-M MODULE_PATH] [--playbook-dir BASEDIR]
               [-a MODULE_ARGS] [-m MODULE_NAME]
               pattern

| 参数                                                         | 描述                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| -f FORKS, --forks FORKS                                      | 并行任务数，默认为5                                          |
| -m MODULE_NAME, --module-name MODULE_NAME                    | 执行模块的名称                                               |
| -a MODULE_ARGS, --args MODULE_ARGS                           | 模块的参数，如果执行默认COMMAND的模块，即是命令参数，如： “date”，“pwd”等等 |
| -u REMOTE_USER, --user REMOTE_USER                           | 执行任务的客户机用户                                         |
| --ask-vault-pass                                             | 假设我们设定了加密的密码，则用该选项进行访问                 |
| --list-hosts                                                 | 输出匹配主机列表； 不执行其他任何操作                        |
| --playbook-dir BASEDIR                                       | 可以将其用作 替代剧本目录。 许多功能的路径，包括角色/ group_vars /等等 |
| --syntax-check                                               | 在剧本上执行语法检查，但不执行                               |
| -B SECONDS, --background SECONDS                             | 后台运行超时时间                                             |
| -C, --check                                                  | 模拟运行环境并进行预运行，可以进行查错测试                   |
| -D, --diff                                                   | 更改（小的）文件和模板时，显示这些文件中的差异； 与--check一起使用效果很好 |
| -M MODULE_PATH, --module-path MODULE_PATH                    | 使用自定义模块路径（默认值~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules） |
| -e EXTRA_VARS, --extra-vars EXTRA_VARS                       | 设置额外的变量                                               |
| -i INVENTORY, --inventory INVENTORY, --inventory-file INVENTORY | 指定主机清单的路径，默认为/etc/ansible/host                  |

## ansible常见模块

| 模块名                         | 模块作用                                                     |
| ------------------------------ | ------------------------------------------------------------ |
| [ping](./module/ping.md)       | 主机连通性测试，如：`ansible all -m command -a "" -u root`   |
| [command](./module/command.md) | 默认模块,这个模块可以直接在远程主机上执行命令，并将结果返回管理主机。不支持管道 |
| shell                          | 可以在远程主机上调用shell解释器运行命令，支持shell的各种功能，例如管道等 |
| copy                           | 用于将文件复制到远程主机，同时支持给定内容生成文件和修改权限等 |
| file                           | 主要用于设置文件的属性，比如创建文件、创建链接文件、删除文件等 |
| fetch                          | 用于从远程某主机获取（复制）文件到本地                       |
| cron                           | 适用于管理`cron`计划任务的                                   |
| yum                            | 主要用于软件的安装                                           |
| service                        | 服务管理                                                     |
| user                           | 用户管理                                                     |
| group                          | 组管理                                                       |
| script                         | 用于执行客户端脚本                                           |
| setup                          | 搜集信息，用来做监控分析                                     |