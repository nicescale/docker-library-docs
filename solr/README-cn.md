# 支持的镜像tag 和 `Dockerfile` 链接

-	[`5.3.1`, `5.3`, `5`, `latest` (*5.3/Dockerfile*)](https://github.com/docker-solr/docker-solr/blob/d722c55dd320a812e278a23c60a5d5616515df0b/5.3/Dockerfile)

# Solr 是什么?

Solr 是高可靠,伸缩,容错,提供分布式索引,复制,负载均衡的查询,自动失败备份和恢复,中心化的配置等.Solr 驱动了众多世界上最大站点的搜索和导航服务.

Learn more on [Apache Solr homepage](http://lucene.apache.org/solr/) and in the [Apache Solr Reference Guide](https://www.apache.org/dyn/closer.cgi/lucene/solr/ref-guide/).

> [wikipedia.org/wiki/Apache_Solr](https://en.wikipedia.org/wiki/Apache_Solr)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/solr/logo.png)

# 怎么是docker 镜像

运行单个Solr 服务:

```console
$ docker run --name my_solr -d -p 8983:8983 -t solr
```

然后打开浏览器访问`http://localhost:8983/`, 进入管理员控制台(调整你的docker主机的主机名).

为了使用Solor, 你需要创建 "core", 即数据索引.比如:

```console
$ docker exec -it --user=solr my_solr bin/solr create_core -c gettingstarted
```

在web 界面上,如果你点击"Core Admin", 应该能看见"gettingstarted"

加载一些样例数据:

```console
$ docker exec -it --user=solr my_solr bin/post -c gettingstarted example/exampledocs/manufacturers.xml
```
在界面上,找到"Core selector"弹出菜单,选择"gettingstarted" ,接着选择"Query"菜单项.如此会使用"*:*"条件,返回所有文档.点击"Execute Query"按钮,你应该能看到一些文档和数据.恭喜!

进一步了解Solr, 查看 [Apache Solr Reference Guide](https://cwiki.apache.org/confluence/display/solr/Apache+Solr+Reference+Guide).

## 分布式 Solr

你可以运行分布式Solr 配置,Solr 节点放在单独的容器里,节点共享单个Zookeeper 服务:

启动zookeeper, 给容器命名,如此,我们可以链接到上面:

```console
$ docker run --name zookeeper -d -p 2181:2181 -p 2888:2888 -p 3888:3888 jplock/zookeeper
```

运行两个solr 容器, 链接到 zookeeper 容器:

```console
$ docker run --name solr1 --link zookeeper:ZK -d -p 8983:8983 \
      solr \
      bash -c '/opt/solr/bin/solr start -f -z $ZK_PORT_2181_TCP_ADDR:$ZK_PORT_2181_TCP_PORT'

$ docker run --name solr2 --link zookeeper:ZK -d -p 8984:8983 \
      solr \
      bash -c '/opt/solr/bin/solr start -f -z $ZK_PORT_2181_TCP_ADDR:$ZK_PORT_2181_TCP_PORT'
```

创建一个集合:

```console
$ docker exec -i -t solr1 /opt/solr/bin/solr create_collection \
        -c collection1 -shards 2 -p 8983
```

访问 `http://localhost:8983/solr/#/~cloud` (根据你的docker主机名,可能需要更改localhost) 可以看到两个分片和Solor节点.


