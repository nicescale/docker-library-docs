# 支持的tag和其 `Dockerfile` 链接地址

-	[`3.5.6`, `3.5`, `3`, `latest` (*Dockerfile*)](https://github.com/docker-library/rabbitmq/blob/60e5665854131f7fcb9ab0285d7d52ac932efb43/Dockerfile)
-	[`3.5.6-management`, `3.5-management`, `3-management`, `management` (*management/Dockerfile*)](https://github.com/docker-library/rabbitmq/blob/60e5665854131f7fcb9ab0285d7d52ac932efb43/management/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/rabbitmq`)](https://github.com/docker-library/official-images/blob/master/library/rabbitmq). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# 什么是 RabbitMQ?

RabbitMQ 是开源的消息队列软件（有时候也被叫做消息中间件），它实现了高级的消息队列协议（AMQR）. RabbitMQ 服务是由 Erlang 编程语言写的,并且它为了集群和故障转移在开放电信平台框架上构建. 客户端的接口几乎兼容所有的主流编程语言.

> [wikipedia.org/wiki/RabbitMQ](https://en.wikipedia.org/wiki/RabbitMQ)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/rabbitmq/logo.png)

# 如何使用此镜像

## 运行这个实例

RabbitMQ的一个重要的事情需要注意,它存储数据是基于所谓的"节点名称", 默认是主机名. 这意味着使用Docker时我们应该需要给每个实例明确指定`-h`/`--hostname`,所以我们不能获取到随机主机名并且持续跟踪我们的数据

```console
$ docker run -d --hostname my-rabbit --name some-rabbit rabbitmq:3
```

如果你给一分钟时间, 然后执行 `docker logs some-rabbit`, 你将看到一块类似的输出:

	=INFO REPORT==== 6-Jul-2015::20:47:02 ===
	node           : rabbit@my-rabbit
	home dir       : /var/lib/rabbitmq
	config file(s) : /etc/rabbitmq/rabbitmq.config
	cookie hash    : UoNOcDhfxW9uoZ92wh6BjA==
	log            : tty
	sasl log       : tty
	database dir   : /var/lib/rabbitmq/mnesia/rabbit@my-rabbit

注意 `database dir` 这里, 尤其是它将我的"主机名"添加到文件存储的末尾. 这个镜像会使所有的`/var/lib/rabbitmq`作为默认.

### Erlang Cookie

查看[RabbitMQ "Clustering Guide"](https://www.rabbitmq.com/clustering.html#erlang-cookie)来获取更多关于cookies的信息,并且知道为什么它们是必要的.

使用`RABBITMQ_ERLANG_COOKIE`设置一致的cookie(特别适合集群但是也可以通过`rabbitmqctl`远程/跨容器管理)

```console
$ docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' rabbitmq:3
```

可以使用这个连接从一个单独的实例去连接:

```console
$ docker run -it --rm --link some-rabbit:my-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl -n rabbit@my-rabbit list_users
Listing users ...
guest   [administrator]
```

另外, 也可以用使用`RABBITMQ_NODENAME`去更简单的重复调用`rabbitmqctl`:

```console
$ docker run -it --rm --link some-rabbit:my-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' -e RABBITMQ_NODENAME=rabbit@my-rabbit rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl list_users
Listing users ...
guest   [administrator]
```

### 管理插件

已经有一些自带[管理插件](https://www.rabbitmq.com/management.html)的镜像。用这些镜像创建的容器实例可以直接使用默认的 15672 端口访问，默认账号密码是guest/guest

```console
$ docker run -d --hostname my-rabbit --name some-rabbit rabbitmq:3-management
```

你可以通过浏览器访问`http://container-ip:15672`,如果你需要访问8080端口的外部主机

```console
$ docker run -d --hostname my-rabbit --name some-rabbit -p 8080:15672 rabbitmq:3-management
```

你可以通过浏览器访问`http://localhost:8080` 或者 `http://host-ip:8080`.

## 设置默认用户名和密码

如果你希望改变`guest` / `guest`的默认用户名和密码,你可以带着`RABBITMQ_DEFAULT_USER`和`RABBITMQ_DEFAULT_PASS`环境变量这样做

```console
$ docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password rabbitmq:3-management
```

你可以通过浏览器访问`http://localhost:8080` 或者 `http://host-ip:8080`,并且使用`user`/`password` 获得管理控制台的访问权限

## 设置默认的vhost

如果你希望改变默认的vhost,你可以带着`RABBITMQ_DEFAULT_VHOST`环境变量这样做

```console
$ docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_DEFAULT_VHOST=my_vhost rabbitmq:3-management
```

## 连接到进程

```console
$ docker run --name some-app --link some-rabbit:rabbit -d application-that-uses-rabbitmq
```

# 许可证

查看此镜像所包含软件的[许可证信息](https://www.rabbitmq.com/mpl.html).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[`rabbitmq/` directory](https://github.com/docker-library/docs/tree/master/rabbitmq) of the [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs).
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过 [GitHub issue](https://github.com/Perl/docker-perl/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/docker-library/rabbitmq/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
