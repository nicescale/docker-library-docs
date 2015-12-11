# 什么是 ArangoDB?

ArangoDB 是一个多数据模型，开源的数据库，支持如 documents, graphts, key-values 多种灵活的数据模型。支持使用类似 SQL 的查询或者 Javascript 扩展来构建高性能的应用。同时可以支持 ACID 事务处理。仅需几个鼠标点击就可以进行水平或者垂直扩展。

ArangoDB 支持的多种数据模型可以混合使用，方便你将多种不同的数据进行聚合。

> [arangodb.com](https://arangodb.com)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/arangodb/logo.png)

## ArangoDB 的核心功能

**多Model** Documents, graphs and key-value — 你可以使用最适合你的应用使用的数据模型。

**Join 查询** 支持灵活的 join 查询，可以大大地减少数据冗余。

**事务** 简化应用程序业务逻辑同时保持数据可靠性。

Join 查询和事务是 RDBMS 数据库实现灵活，安全的数据设计的核心功能，也是NoSQL 产品中一般不具有的功能。使用 ArangoDB 你可以拥有 NoSQL 的高性能和强大扩展能力，同时可以在关键的地方，使用一般 RDBMS 才具有的 Join 查询和事务功能。

同时，ArangoDB 还提供了一个微服务框架：[Foxx](https://www.arangodb.com/foxx) 让你可以通过很少的代码就能构建 Restful API。

ArangoDB 文档

-	[ArangoDB 文档](https://www.arangodb.com/documentation)
-	[ArangoDB 教程](https://www.arangodb.com/tutorials)

## 如何使用此镜像

### 启动一个 `ArangoDB` 实例


运行下面的命令来启动一个 ArangoDB 实例

```console
$ docker run -d --name arangodb-instance arangodb
```

上述命令将会创建并运行 arangodb 的 docker 实例，实例的名称为 *arangodb-instance*。ArangoDB 默认监听端口是 8529，并且这个镜像包含了 `EXPOSE 8529` 指令。你可以将应用容器和这个容器通过 Docker 的 `link` 连接，具体请参看后续示例。

获取 arango 的监听地址:

```console
$ docker inspect --format '{{ .NetworkSettings.IPAddress }}' arangodb-instance
```

### 使用 ArangoDB 容器

将你的应用和 ArangoDB 容器连接

```console
$ docker run --name my-arangodb-app --link arangodb-instance:db-link arangodb
```

这将会把这个应用实例和名为 `arangodb-instance` 的实例连接。应用实例将会获得以下的环境变量

	DB_LINK_PORT_8529_TCP=tcp://172.17.0.17:8529
	DB_LINK_PORT_8529_TCP_ADDR=172.17.0.17
	DB_LINK_PORT_8529_TCP_PORT=8529
	DB_LINK_PORT_8529_TCP_PROTO=tcp
	DB_LINK_NAME=/naughty_ardinghelli/db-link

通过这些环境变量就可以对 ArangoDB 进行访问和操作。

### 将 ArangoDB 的端口对外映射

如果你想将 ArangoDB 的端口映射出现，请运行

```console
$ docker run -p 8529:8529 -d arangodb
```

ArangoDB 在 8529 端口进行监控，并且镜像中包含了 `EXPOSE 8529`. 通过 `-p 8529:8529` 将会把这个端口映射到主机上。

## 数据持久化

ArangoDB 使用卷 `/var/lib/arangodb` 作为数据目录来存储数据，而使用卷 `/var/lib/arangodb-apps` 作为应用目录来存储应用扩展。这两个目录都被标记成 Docker 的 Volume.

可以查看这两个文档了解详细的 Docker 数据持久化的说明: [Docker In-depth: Volumes](http://container42.com/2014/11/03/docker-indepth-volumes/), [Why Docker Data Containers are Good](https://medium.com/@ramangupta/why-docker-data-containers-are-good-589b3c6c749e)

### 使用主机的数据目录

你可以将容器的卷映射到主机的目录上。下面例子中的 `/tmp/arangodb` 目录，通过不会是实例使用中的合理目录位置，请在实例使用中根据情况修改。


```console
$ mkdir /tmp/arangodb
$ docker run -p 8529:8529 -d \
          -v /tmp/arangodb:/var/lib/arangodb \
          arangodb
```

这将会将主机上的 `/tmp/arangodb` 作为 ArangoDB 容器的数据目录

## 使用自定义的 ArangoDB 配置文件

ArangoDB 默认将会使用 `/etc/arangodb/arangodb.conf` 这个配置文件。如果你需要使用自己的配置文件，你可以将配置放到主机的特定目录，同时把它映射到容器的 `/etc/arangodb` 目录。

例如你的配置文件是`/my/custom/arangod.conf`，那么你可以通过下面的命令启动你的 `arangodb` 容器


```console
$ docker run --name some-arangodb -v /my/custom:/etc/arangodb -d arangodb:tag
```

这将会创建一个名为 `some-arangodb` 的容器，并从你指定的位置读取 ArangoDB 配置，而不是使用默认的配置。

需要注意的是，如果用户的主机上启用了 SELinux，上述的命令可能会有问题。一个解决办法是为配置文件目录指定一个 SELinux 策略类型，使得容器可以访问到这个目录：


```console
$ chcon -Rt svirt_sandbox_file_t /my/custom
```

### 使用数据容器

另外一个办法是创建数据容器来持久化数据

```console
$ docker run -d --name arangodb-persist -v /var/lib/arangodb debian:8.0 true
```

在 ArangoDB 容器上使用数据容器

```console
$ docker run --volumes-from arangodb-persist -p 8529:8529 arangodb
```

如果你希望数据容器更小一些，那么可以使用诸如: [hello-world](https://registry.hub.docker.com/_/hello-world/), [busybox](https://registry.hub.docker.com/_/busybox/) or [alpine](https://registry.hub.docker.com/_/alpine/) 等基础镜像来创建数据容器。

```console
$ docker run -d --name arangodb-persist -v /var/lib/arangodb alpine alpine
```
