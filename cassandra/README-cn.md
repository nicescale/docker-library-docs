# 什么是 Cassandra?

Apache Cassandra 是一个开源的分布式数据库, 适合处理分布于大量服务器的数据，支持高可用没有单点故障。Cassandra 提供了健壮的多数据中心的集群支持，通过异步的无 master 概念的复制技术实现低延迟的数据操作。

> [wikipedia.org/wiki/Apache_Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/cassandra/logo.png)

# 如何使用此镜像

## 运行 `cassandra` 服务器实例

运行 Cassandra 实例很简单:

```console
$ docker run --name some-cassandra -d cassandra:tag
```

... 其中 `some-cassandra` 是你为这个容器设置的名字，而 `tag` 则是用于指定使用哪一个版本的 Cassandra。

## 将 Cassandra 连接到某个基于 Docker 的应用

Cassadra 镜像 expose 了标准的 Cassadra 端口，(详见 [Cassandra FAQ](https://wiki.apache.org/cassandra/FAQ#ports))，通过Docker的 Link 功能，连接到 Cassandra 的应用可以直接访问这些端口。使用下面这样的命令来把你的应用程序容器连接到 Cassandra 容器:


```console
$ docker run --name some-app --link some-cassandra:cassandra -d app-that-uses-cassandra
```

## 集群设置

使用下文中的环境变量，可以实现两种不同的集群场景：所有的实例运行在同一机器上或者运行在不同的机器上。对于同一个机器，按上面的方式创建实例。创建新的实例时，只需要额外告诉它第一个实例的地址即可。

```console
$ docker run --name some-cassandra2 -d -e CASSANDRA_SEEDS="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' some-cassandra)" cassandra:tag
```

... `some-cassandra` 是最初创建的 Cassandra 容器，这里通过 `docker inspect` 来获取容器的 IP 地址

对于另外的主机(比如，一个云上的两个虚拟机器)，你需要告诉 Cassandra 其它的节点使用哪一个 IP 地址来访问(因为容器的IP地址在其它主机上无法直接连接的)。

```console
$ docker run --name some-cassandra -d -e CASSANDRA_BROADCAST_ADDRESS=10.42.42.42 -p 7000:7000 cassandra:tag
```

然后在第二个主机上创建 Cassandra 容器，使用映射的端口和IP来连接第一个主机

```console
$ docker run --name some-cassandra -d -e CASSANDRA_BROADCAST_ADDRESS=10.43.43.43 -p 7000:7000 -e CASSANDRA_SEEDS=10.42.42.42 cassandra:tag
```

## 通过 `cqlsh` 连接 Cassandra


下面的命令将创建另外一个 Cassandra 容器实例，并运行 `cqlsh` (Cassandra Query Language Shell) 来连接之前创建的 Cassandra 容器，并执行 CQL 查询：

```console
$ docker run -it --link some-cassandra:cassandra --rm cassandra sh -c 'exec cqlsh "$CASSANDRA_PORT_9042_TCP_ADDR"'
```

... 也可以通过 docker link 创建的 `/etc/hosts` 来直接使用主机名称访问:

```console
$ docker run -it --link some-cassandra:cassandra --rm cassandra cqlsh cassandra
```

... `some-cassandra` 是最开始创建的 Cassandra 容器

关于 CQL 的更多信息，请参考 [Cassandra 文档](https://cassandra.apache.org/doc/cql3/CQL.html).

## 容器的命令行访问和日志查看

通过 `docker exec` 可以在容器内部运行命令。下面的命令行可以让你获取 `cassandra` 内部的 bash shell

```console
$ docker exec -it some-cassandra bash
```

Cassandra 服务器的服务器日志可以通过容器的日志进行查看

```console
$ docker logs some-cassandra
```

## 环境变量

当你运行 `cassandra` 镜像时，你可以通过对 `docker run` 命令设置以下的环境变量来修改 Cassandra 实例的配置。

### `CASSANDRA_LISTEN_ADDRESS`

用于控制 Cassandra 从哪个 IP 地址来监听外部连接。默认值是 `auto`，它将会把`cassandra.yaml` 中的 [`listen_address`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__listen_address) 设置成容器的 IP 地址。这个参数大部分情况下不需要修改就可以正常工作。

### `CASSANDRA_BROADCAST_ADDRESS`

用于控制将哪个 IP 地址广播给其它的节点。默认将会使用`CASSANDRA_LISTEN_ADDRESS` 的设置。它将会设置 `cassandra.yaml` 中的[`broadcast_address`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__broadcast_address) 和 [`broadcast_rpc_address`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__broadcast_rpc_address) 这两个配置项。

### `CASSANDRA_RPC_ADDRESS`

用于控制 thrift rpc server 绑定到的 IP 地址。如果没有指定，默认将使用`0.0.0.0` 这个地址。它将会设置 `cassandra.yaml` 中的 [`rpc_address`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__rpc_address) 配置。

### `CASSANDRA_START_RPC`

用于控制是否启动 thrift rpc server. 它将会设置 `cassandra.yaml` 中的 [`start_rpc`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__start_rpc) 配置。

### `CASSANDRA_SEEDS`

用逗号分隔的IP地址列表，用于启动时连接到这些节点的集群。它将会设置
`cassandra.yaml` 中 [`seed_provider`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__seed_provider) 配置的 `seed` 的值。 `CASSANDRA_BROADCAST_ADDRESS` 也会被添加到 `seed` 中。

### `CASSANDRA_CLUSTER_NAME`

设置集群的名字，这个名字需要和集群中其它节点的设置保持一致。对应于
`cassandra.ymal` 中的 [`cluster_name`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__cluster_name) 配置项。

### `CASSANDRA_NUM_TOKENS`

设置这个节点的 token 数量，对应于 `cassandra.yaml' 中的
[`num_tokens`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__num_tokens) 配置项。

### `CASSANDRA_DC`

设置这个节点的数据中心名称，对应于 `cassandra-rackdc.properties` 中的 [`dc`](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archsnitchGossipPF.html) 配置项。

### `CASSANDRA_RACK`

设置这个节点的 rack 名称，对应于 `cassandra-rackdc.properties` 中的 [`rack`](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archsnitchGossipPF.html) 配置项。

### `CASSANDRA_ENDPOINT_SNITCH`

设置节点使用的 snitch 实现，对应于 `cassandra.ymal` 中的 [`endpoint_snitch`](http://docs.datastax.com/en/cassandra/3.0/cassandra/configuration/configCassandra_yaml.html?scroll=configCassandra_yaml__endpoint_snitch) 配置项。

# 常见问题

## 数据存储到何处

注意: Docker 容器支持多种不同的数据存储方式。我们推荐使用 `cassandra` 镜像来了解以下几种数据存储方式：

-	使用 Docker 自己的 [数据卷](https://docs.docker.com/userguide/dockervolumes/#adding-a-data-volume). 这是默认而且对用户来说最简单透明的方式。不好的地方对于直接运行在主机上的应用，不能很轻易地定位和访问数据目录
-	在主机上创建数据目录并将其[映射到容器](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume). 这将使容器直接使用主机上的固定目录来存储数据 ，方便主机上的其它工具或者应用访问到这个目录。不方便的地方是，用户需要自己确认这个目录是否存在，并且有正确的权限设置使得容器中的进程能够访问这个目录下的数据。

通过上述链接到的两个 Docker 文档可以详细了解不同的数据存储方式。这里我们简单地对第二种使用方式进行介绍:

1.	在主机的合适位置创建一个数据目录，比如 `/my/own/datadir`
2.	使用下面的命令创建 `cassandra` 容器:

	```console
	$ docker run --name some-cassandra -v /my/own/datadir:/var/lib/cassandra -d cassandra:tag
	```

这里通过 `-v /my/own/datadir:/var/lib/cassandra` 将主机上的 `/my/own/datadir` 映射到容器的 `/var/lib/cassandra` 目录, 也就是 Cassandra 默认读写数据的目录。

需要注意的是如果主机上启用了 SELinux，直接这样使用可能会遇到一些问题。解决办法是为这个目录增加一个 SELinux policy 设置，使得容器可以有权限访问这个目录:

	chcon -Rt svirt_sandbox_file_t /my/own/datadir


## 在 Cassandra 完成初始化之前无法进行连接

如果容器启动时，没有任何已经初始化过的数据库，则容器将会创建默认的数据库。这也意味着在数据库初始化完成之前，Cassandra 将无法接受外部的连接。这可能会导致一些自动化的工具，比如 `docker-compose` 并行创建多个容器时，产生一些问题。

