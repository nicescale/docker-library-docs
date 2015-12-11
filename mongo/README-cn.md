# 支持的镜像tag 和 `Dockerfile` 链接

-	[`2.2.7`, `2.2` (*2.2/Dockerfile*)](https://github.com/docker-library/mongo/blob/982328582c74dd2f0a9c8c77b84006f291f974c3/2.2/Dockerfile)
-	[`2.4.14`, `2.4` (*2.4/Dockerfile*)](https://github.com/docker-library/mongo/blob/982328582c74dd2f0a9c8c77b84006f291f974c3/2.4/Dockerfile)
-	[`2.6.11`, `2.6`, `2` (*2.6/Dockerfile*)](https://github.com/docker-library/mongo/blob/982328582c74dd2f0a9c8c77b84006f291f974c3/2.6/Dockerfile)
-	[`3.0.7`, `3.0`, `3`, `latest` (*3.0/Dockerfile*)](https://github.com/docker-library/mongo/blob/35d3a11dd6cee3675fb149593e7a20e42c76fa86/3.0/Dockerfile)
-	[`3.1.9`, `3.1` (*3.1/Dockerfile*)](https://github.com/docker-library/mongo/blob/5216cf8aedcf7634172e607b0c9718cc332e0d71/3.1/Dockerfile)
-	[`3.2.0-rc4`, `3.2.0`, `3.2`, `3.2-rc` (*3.2-rc/Dockerfile*)](https://github.com/docker-library/mongo/blob/8bad644787fba5b30d91f6d86931821deaa69989/3.2-rc/Dockerfile)

关于该镜像的更多信息及其历史, 请参阅 [相关清单文件 (`library/mongo`)](https://github.com/docker-library/official-images/blob/master/library/mongo). 该镜像通过向 [the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)提交```pull requests```来更新.

For detailed information about the virtual/transfer sizes and individual layers of each of the above supported tags, please see [the `mongo/tag-details.md` file](https://github.com/docker-library/docs/blob/master/mongo/tag-details.md) in [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs).

# 关于MongoDB

文档的数据库，归类为NoSQL数据库，MongoDB避开了传统的基于表格的关系型数据库结构，代之以具有动态结构的类JSON文档格式（MongoDB称之为BSON），从而使一些特定类型应用的数据整合更容易、更快。在GNU Affero和Apach许可下发布的MongoDB是一个免费的开源软件。

最开始由软件公司10gen(现在的MongoDB Inc.) 在2007年10月 作为一个服务产品计划平台中的组件开发, 该公司在2009年时转为了开源的模式, 为 10gen 公司提供商业支持和其他服务. 到目前为止, MongoDB已被一些大型的网站和服务作为应用，其中包括 Craigslist, eBay, Foursquare, SourceForge, Viacom, 和纽约时报等。 MongoDB 已成为最流行的 NoSQL 数据库系统。

> [wikipedia.org/wiki/MongoDB](https://en.wikipedia.org/wiki/MongoDB)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/mongo/logo.png)

# 镜像的使用

## 第一个 mongo 实例

```console
$ docker run --name some-mongo -d mongo
```

该镜像包含 `EXPOSE 27017` ( mongo 的服务端口), 所以一个标准的容器连接可以使其自动连接到容器 (如下所示).

## connect to it from an application

```console
$ docker run --name some-app --link some-mongo:mongo -d application-that-uses-mongo
```

## ... or via `mongo`

```console
$ docker run -it --link some-mongo:mongo --rm mongo sh -c 'exec mongo "$MONGO_PORT_27017_TCP_ADDR:$MONGO_PORT_27017_TCP_PORT/test"'
```

## 配置

关于MongoDB更多的使用和配置信息详见 [官方文档](http://docs.mongodb.org/manual/)， 如副本集或分片。

如果你希望在没有配置文件的 MongoDB 3.0 上使用WiredTiger引擎， 只需添加 `--storageEngine` 参数。 一定要注意[文档](http://docs.mongodb.org/manual/release-notes/3.0-upgrade/#change-storage-engine-to-wiredtiger) 如何从旧版本升级。

```console
$ docker run --name some-mongo -d mongo --storageEngine wiredTiger
```

## Where to Store Data

重要提示:使用的Docker容器中运行的应用程序有几种方法来存储数据。 我们鼓励mongo镜像的用户采用的有：:

-   让Docker管理你的数据库文件， [通过Docker的内部卷管理写入数据文件到主机磁盘](https://docs.docker.com/userguide/dockervolumes/#adding-a-data-volume). 这是默认的，而且简单对用户透明。 不好的地方在于，这些文件很难在主机系统上找到和使用。
-   在主机系统上创建一个数据目录并 [挂载该目录到容器内目录](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume). 这样会将数据库文件存储主机系统上的已知的位置, 使得直接在主机系统上操作数据文件变得容易。 这样做的不利在于，用户需要保证主机上的目录已存在, 并且意味着. 该目录的权限和主机系统上的其他安全机制是正确设置的。

**警告**: 因为MongoDB使用了内存映射文件，而这在vboxfs上是不被允许的。 ([vbox bug](https://www.virtualbox.org/ticket/819)).

Docker文档可以很好的帮助你理解不同的存储选项及其差异, 而且有很多博客，帖子和讨论，还提出了一些建议. 这里我们只会显示上面后一项的基本过程:

1.  在主机系统合适的卷上创建一个数据目录，如 `/my/own/datadir`.
2.  启动 `%%REPO%%` 容器，如下：
	```console
	$ docker run --name some-mongo -v /my/own/datadir:/data/db -d mongo:tag
	```

命令行中`-v /my/own/datadir:/data/db` 项挂载了 `/my/own/datadir` 目录挂载到容器内的`/data/db`目录 , 也就是MongoDB默认的数据文件存储位置.

注意,主机系统启用了SELinux的用户可能会遇到这个问题. 目前的解决方案是将有关SELinux策略类型分配给新的数据目录,容器将被允许访问:

```console
$ chcon -Rt svirt_sandbox_file_t /my/own/datadir
```

# 许可证

使用本镜像包含的软件的许可, 请查看[许可信息](https://github.com/mongodb/mongo/blob/7c3cfac300cfcca4f73f1c3b18457f0f8fae3f69/README#L71) 

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).
