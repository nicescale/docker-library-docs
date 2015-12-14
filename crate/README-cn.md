# 支持的tag标签，以及相关`Dockerfile`链接

-	[`latest`, `0.52`, `0.52.2` (*Dockerfile*)](https://github.com/crate/docker-crate/blob/d8ef1163d5937083a6cb8831732822d0f5d3cb84/Dockerfile)
-	[`0.51`, `0.51.7` (*Dockerfile*)](https://github.com/crate/docker-crate/blob/5d66f67b05395e9b7b4f55c2b3d682d43c7f59d9/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/crate`)](https://github.com/docker-library/official-images/blob/master/library/crate)。本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

F关于镜像每个layer以及上述每个tag的详细信息，请查看位于[the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的 [the `crate/tag-details.md` file](https://github.com/docker-library/docs/blob/master/crate/tag-details.md)。

# Crate是什么？

Crate提供了一个分布式聚合引擎，让你可以通过SQL来完成数据的查询与计算。它提供了高速的多索引查询、分布式的聚合与排序、高速的全文检索、非常简洁的集群管理功能。

[Crate](https://crate.io/)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/crate/logo.png)

## 如何使用本镜像

```console
$ docker run -d -p 4200:4200 -p 4300:4300 crate:latest
```

### 对接已经存在的文件目录

```console
$ docker run -d -p 4200:4200 -p 4300:4300 -v <data-dir>:/data crate
```

### 自定义Crate配置

```console
$ docker run -d -p 4200:4200 -p 4300:4300 crate -Des.config=/path/to/crate.yml
```

任何选项都具有`-D`前缀，例如设置集群名称：

```console
$ docker run -d -p 4200:4200 -p 4300:4300 crate crate -Des.cluster.name=cluster
```

更多配置相关信息请参考[Configuration](https://crate.io/docs/stable/configuration.html)。

### 环境

如果希望为Crate设置环境变量，你需要使用在运行容器时使用`--env`选项。

例如:

```console
$ docker run -d -p 4200:4200 -p 4300:4300 --env CRATE_HEAP_SIZE=32g crate
```

## 多播

Crate默认使用多播机制来进行节点发现，不过Docker只支持在同一台主机上的多播机制。这意味着同一台主机上的节点可以自动地相互发现、识别，而对不同主机上的节点则需要使用单播。

你可以在自己的`crate.yml`允许使用单播，更多信息可以参考：[Crate Multi Node Setup](https://crate.io/docs/en/latest/best_practice/multi_node_setup.html)。

Crate会在其主机所在的集群内进行节点发现，然而容器内的主机地址和实际的主机地址可能并不相同，你可以向Crate指明所需的主机地址信息：

```console
$ docker run -d -p 4200:4200 -p 4300:4300 crate crate -Des.network.publish_host=host1.example.com:
```

如果你希望使用自定义端口来取代默认的`4300`，那么需要告知Crate该端口信息：

```console
$ docker run -d -p 4200:4200 -p 4321:4300 crate crate -Des.transport.publish_port=4321
```

### 多节点安装的一个示例：

```console
$ HOSTS='crate1.example.com:4300,crate2.example.com:4300,crate3.example.com:4300'
$ HOST=crate1.example.com
$ docker run -d \
	-p 4200:4200 \
	-p 4300:4300 \
	--name node1 \
	--volume /mnt/data:/data \
	--env CRATE_HEAP_SIZE=8g \
	crate:latest \
	crate -Des.cluster.name=cratecluster \
		  -Des.node.name=crate1 \
		  -Des.transport.publish_port=4300 \
		  -Des.network.publish_host=$HOST \
		  -Des.multicast.enabled=false \
		  -Des.discovery.zen.ping.unicast.hosts=$HOSTS \
		  -Des.discovery.zen.minimum_master_nodes=2
```

# 证书信息

关于本镜像包含的软件证书信息，可以查看：[license information](https://github.com/crate/crate/blob/master/LICENSE.txt)。

# 支持的Docker版本

支持的Docker版本

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`crate/` directory](https://github.com/docker-library/docs/tree/master/crate)目录中。在提交新的pull request之前，请确保已经熟悉于[repository's `REAMDE.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/crate/docker-crate/issues)来提交。

如果你有任何问题或建议，也欢迎直接加入我们的讨论频道：[Gitter Channel](https://gitter.im/crate/crate)。

关于更多信息或官方联系方式请参考：[https://crate.io](https://crate.io)。

## 提供帮助

我们欢迎任何形式的帮助和建议，在提交具体pull request之前，我们希望你已经阅读过[CLA](https://crate.io/community/contribute/)，更多信息请参考 [CONTRIBUTING.rst](https://github.com/crate/crate/blob/master/CONTRIBUTING.rst)。
