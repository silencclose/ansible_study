# 通过ansible安装hugegraph过程

## 主机信息

```shell
vim /etc/ansible/hosts 
    [hugegraph]
    10.10.10.1
    10.10.10.2
    10.10.10.3
    10.10.10.4
```
## 安装过程

- 配置ssh信任关系

  ```shell
  sshd-copy-id -i ~/.ssh/id_rsa.pub hugegraph@10.10.10.1
  sshd-copy-id -i ~/.ssh/id_rsa.pub hugegraph@10.10.10.2
  sshd-copy-id -i ~/.ssh/id_rsa.pub hugegraph@10.10.10.3
  sshd-copy-id -i ~/.ssh/id_rsa.pub hugegraph@10.10.10.4
  ```

- 将文件传输至对应目录下

  ```shell
  ansible hugegraph -m unarchive -a "src=/root/hugegraph-0.10.4.tar.gz dest=/home/hugegraph"  -u hugegraph
  ```

- 更改gremlin-server.yml文件

  ```shell
  ansible hugegraph -m blockinfile -a "path=/home/hugegraph/hugegraph-0.10.4/conf/gremlin-server.yaml insertbefore=BOF block='host: 0.0.0.0\nport: 8183\nscriptEvaluationTimeout: 3000000' force=yes"  -u hugegraph
  ansible hugegraph -m lineinfile -a "path=/home/hugegraph/hugegraph-0.10.4/conf/gremlin-server.yaml line='scriptEvaluationTimeout: 30000' state=absent force=yes"  -u hugegraph 
  ```

- 更改rest-server.properties文件

  ```shell
  ansible hugegraph -m lineinfile -a "path=/home/hugegraph/hugegraph-0.10.4/conf/rest-server.properties line='batch.max_vertices_per_batch=1000' state=present force=yes"  -u hugegraph
  ansible 10.10.10.1 -m blockinfile -a "path=/home/hugegraph/hugegraph-0.10.4/conf/rest-server.properties insertbefore='# gremlin server url, need to be consistent with host and port in gremlin-server.yaml' block='gremlinserver.url=http://10.10.10.1:8183' force=yes"  -u hugegraph
  ansible 10.10.10.2 -m blockinfile -a "path=/home/hugegraph/hugegraph-0.10.4/conf/rest-server.properties insertbefore='# gremlin server url, need to be consistent with host and port in gremlin-server.yaml' block='gremlinserver.url=http://10.10.10.2:8183' force=yes"  -u hugegraph
  ansible 10.10.10.3 -m blockinfile -a "path=/home/hugegraph/hugegraph-0.10.4/conf/rest-server.properties insertbefore='# gremlin server url, need to be consistent with host and port in gremlin-server.yaml' block='gremlinserver.url=http://10.10.10.3:8183' force=yes"  -u hugegraph
  ansible 10.10.10.4 -m blockinfile -a "path=/home/hugegraph/hugegraph-0.10.4/conf/rest-server.properties insertbefore='# gremlin server url, need to be consistent with host and port in gremlin-server.yaml' block='gremlinserver.url=http://10.10.10.4:8183' force=yes"  -u hugegraph
  
  ```
  
  