# 支持的tag标签，以及相关`Dockerfile`链接

-	[`latest`, `enterprise`, `4.0.0`, `enterprise-4.0.0` (*enterprise/couchbase-server/4.0.0/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/enterprise/couchbase-server/4.0.0/Dockerfile)
-	[`community`, `community-4.0.0` (*community/couchbase-server/4.0.0/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/community/couchbase-server/4.0.0/Dockerfile)
-	[`3.1.0`, `enterprise-3.1.0` (*enterprise/couchbase-server/3.1.0/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/enterprise/couchbase-server/3.1.0/Dockerfile)
-	[`3.0.3`, `enterprise-3.0.3` (*enterprise/couchbase-server/3.0.3/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/enterprise/couchbase-server/3.0.3/Dockerfile)
-	[`3.0.2`, `enterprise-3.0.2` (*enterprise/couchbase-server/3.0.2/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/enterprise/couchbase-server/3.0.2/Dockerfile)
-	[`community-3.0.1` (*community/couchbase-server/3.0.1/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/community/couchbase-server/3.0.1/Dockerfile)
-	[`2.5.2`, `enterprise-2.5.2` (*enterprise/couchbase-server/2.5.2/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/enterprise/couchbase-server/2.5.2/Dockerfile)
-	[`community-2.2.0` (*community/couchbase-server/2.2.0/Dockerfile*)](https://github.com/couchbase/docker/blob/a997f494aae7d8fa572dbc581ab289e8eaa72279/community/couchbase-server/2.2.0/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/couchbase`)](https://github.com/docker-library/official-images/blob/master/library/couchbase)。本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于[the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs) 下的[the `couchbase/tag-details.md` file](https://github.com/docker-library/docs/blob/master/couchbase/tag-details.md)。

# Couchbase Server是什么？

[Couchbase Server](http://en.wikipedia.org/wiki/Couchbase_Server) 是一个开源的面向文档型分布式NoSQL数据库，特别针对交互式应用进行了相关的优化。

关于软件证书信息请参考本文档的末尾。

+如果你有关于本镜像的任何问题，请通过[Couchbase support forum](https://forums.couchbase.com/) 或irc.freenode.net上的`#couchbase`频道获取帮助。

![logo](https://raw.githubusercontent.com/docker-library/docs/master/couchbase/logo.png)

# 如何使用本镜像：快速上手

```console
$ docker run -d -p 8091:8091 couchbase
```

然后在浏览器中访问：http://localhost:8091来查看管理员管理界面，更多细节和相关截图请参见 **单主机，单容器** 部分。

# 背景信息

## 网络

Couchbase Server 可以同时处理不同的通信请求（参见[Couchbase Server documentation](http://docs.couchbase.com/admin/admin/Install/install-networkPorts.html)）。其原本并不适用应对于位于同一集群下的不同NAT主机，因此Docker的默认网络配置并非部署Couchbase Server的适当选项。

下面会介绍几个本镜像可以轻易实现的应用情景，我们会给出对每种网络配置的相关建议。

## 卷

一个Couchbase Server的Docker容器会将所有数据保存在`/opt/couchbase/var`目录下，而该目录实际会被当作一个Docker挂载卷来处理，因此会独立于文件系统被保存，这会带来性能方面的显著提升。这还可以让你可以向正在运行的Couchbase Server容器导入数据，而不用担心现有数据的丢失：

```console
$ docker stop my-couchbase-container
$ docker run -d --name my-new-couchbase-container --volumes-from my-couchbase-container ....
$ docker rm my-couchbase-container
```

默认情况下，该卷由Docker进程来管理，并不对外公开。在特殊情况下，为了对其进行管理，我们建议将该卷在主机文件系统上映射到特殊的目录下（使 `docker run`命令的 `-v`参数。

下面的示例会默认你已经完成了映射。

*SELinux workaround*

如果你设置了SELinux，那么在容器中挂载卷需要额外的步骤，假设你希望对`~/couchbase`目录进行挂载，你需要运行如下命令：

```console
$ mkdir ~/couchbase && chcon -Rt svirt_sandbox_file_t ~/couchbase
```

## 上限设定

Couchbase默认会采用以下的上限设定:

```console
$ ulimit -n 40960        # nofile: max number of open files
$ ulimit -c unlimited    # core: max core file size
$ ulimit -l unlimited    # memlock: maximum locked-in-memory address space
```

这些限制主要针对于高负载运行，但如果只适用于轻量级的测试和开发，你可以忽视这些限制，一切都会照常运行的。

为了在容器中应用这些限制设定，你需要使用`--ulimit`选项：

```console
$ docker run -d --ulimit nofile=40960:40960 --ulimit core=100000000:100000000 --ulimit memlock=100000000:100000000 couchbase
```

这会为core和memlock文件设置100 GB的大小上限。如果你的系统拥有大于100 GB的内存，那么可以考虑增大这个设定值。

注意: `--ulimit`只工作在Docker 1.6以上版本。

# 常见部署情景

## 单主机，单容器

	┌───────────────────────┐
	│   Host OS (Linux)     │
	│  ┌─────────────────┐  │
	│  │  Container OS   │  │
	│  │    (Ubuntu)     │  │
	│  │  ┌───────────┐  │  │
	│  │  │ Couchbase │  │  │
	│  │  │  Server   │  │  │
	│  │  └───────────┘  │  │
	│  └─────────────────┘  │
	└───────────────────────┘

这可能是在自己主机上尝试Couchbase Server的最简单方法了：只需*下载并运行*就可以了。在本情景下，任何网络模式都可以很好地工作，你所需要的只是将8091端口暴露出来。

**运行容器**

```console
$ docker run -d -v ~/couchbase:/opt/couchbase/var -p 8091:8091 --name my-couchbase-server couchbase
```

我们使用`--name`选项来让日后对该容器的管理变得更容易。

**确认容器运行状态**

使用刚才设置的容器名称（例如上面的`my-couchbase-server`）来查看其日志：

```console
$ docker logs my-couchbase-server
Starting Couchbase Server -- Web UI available at http://<ip>:8091
```

**链接到管理员控制面板**

在主机的浏览器中访问：http://localhost:8091，你将会看到Couchbase Server的欢迎界面：

![Welcome Screen](http://couchbase-mobile.s3.amazonaws.com/github-assets/couchbase_welcome_2.png)

**配置**

-	点击"Setup"按钮

-	对于安装中的每一步操作，你都可以使用默认值并不停"Next"来继续。

安装配置过程结束之后，你将会看到：

![Servers Screen](http://couchbase-mobile.s3.amazonaws.com/github-assets/couchbase_post_welcome.png)

**从SDK连接**

现在你已经可以从任何一个[Couchbase Client SDKs](http://docs.couchbase.com/couchbase-sdk-python-1.2/)客户端来进行连接了。

你需要在主机上运行SDK，并将其指向`http://localhost:8091/pools`

## 单主机，多容器

	┌──────────────────────────────────────────────────────────┐
	│                     Host OS (Linux)                      │
	│                                                          │
	│  ┌───────────────┐ ┌───────────────┐  ┌───────────────┐  │
	│  │ Container OS  │ │ Container OS  │  │ Container OS  │  │
	│  │   (Ubuntu)    │ │   (Ubuntu)    │  │   (Ubuntu)    │  │
	│  │ ┌───────────┐ │ │ ┌───────────┐ │  │ ┌───────────┐ │  │
	│  │ │ Couchbase │ │ │ │ Couchbase │ │  │ │ Couchbase │ │  │
	│  │ │  Server   │ │ │ │  Server   │ │  │ │  Server   │ │  │
	│  │ └───────────┘ │ │ └───────────┘ │  │ └───────────┘ │  │
	│  └───────────────┘ └───────────────┘  └───────────────┘  │
	└──────────────────────────────────────────────────────────┘

-	非常适用于测试多主机节点集群上的情况。
-	不建议在生产环境下使用。
-	让你可以进行集群负载均衡方面的实验。
-	每个容器都会被Docker分配各自的IP地址，这些IP地址对于同一主机上的所有容器都是可见的。
-	你可以在管理员面板中使用内网IP向集群内添加新的主机。

```console
$ docker run -d -v ~/couchbase/node1:/opt/couchbase/var couchbase
$ docker run -d -v ~/couchbase/node2:/opt/couchbase/var couchbase
$ docker run -d -v ~/couchbase/node3:/opt/couchbase/var -p 8091:8091 couchbase
```

**设置你的Couchbase集群**

1.	当运行完上面的最后一个`docker run`命令后，我们得到了其容器ID，例如`<node_3_container_id>`

2.	我们可以通过命令`docker inspect --format '{{ .NetworkSettings.IPAddress }}' <node_3_container_id>`来得到其IP地址信息，让我们称其为 `<node_3_ip_addr>`。

3.	在主机的浏览器中通过http://localhost:8091来访问管理员面板，并点击"Setup"按钮。

4.	在hostname项里，填入`<node_3_ip_addr>`

5.	其他均采用默认设置，并为其设置密码。

6.	点击Server Nodes菜单。

7.	点击Add Servers按钮。

8.	对于其他两个容器

	1.	通过命令`docker inspect --format '{{ .NetworkSettings.IPAddress }}' <node_x_container_id>`来得到其IP地址，让我们称其为 `<node_x_ip_addr>`。

	2.	在Server IP Address项里，填入`<node_x_ip_addr>`。

	3.	在密码项里，填写我们刚才创建的密码。

## 多主机，单容器（对每一台主机而言）

	┌───────────────────────┐  ┌───────────────────────┐  ┌───────────────────────┐
	│   Host OS (Linux)     │  │   Host OS (Linux)     │  │   Host OS (Linux)     │
	│  ┌─────────────────┐  │  │  ┌─────────────────┐  │  │  ┌─────────────────┐  │
	│  │  Container OS   │  │  │  │  Container OS   │  │  │  │  Container OS   │  │
	│  │    (Ubuntu)     │  │  │  │    (Ubuntu)     │  │  │  │    (Ubuntu)     │  │
	│  │  ┌───────────┐  │  │  │  │  ┌───────────┐  │  │  │  │  ┌───────────┐  │  │
	│  │  │ Couchbase │  │  │  │  │  │ Couchbase │  │  │  │  │  │ Couchbase │  │  │
	│  │  │  Server   │  │  │  │  │  │  Server   │  │  │  │  │  │  Server   │  │  │
	│  │  └───────────┘  │  │  │  │  └───────────┘  │  │  │  │  └───────────┘  │  │
	│  └─────────────────┘  │  │  └─────────────────┘  │  │  └─────────────────┘  │
	└───────────────────────┘  └───────────────────────┘  └───────────────────────┘

这是一个典型的Couchbase Server集群，每个节点都运行在各自独立的主机上，各自之间通过高速的网络连接进行通信。

当前为了使用这一Couchbase Server架构的唯一方法是使用`--net=host`选项。

使用`--net=host` 选项会带来如下效果:

-	容器会使用主机自己的网络栈。
-	Removes networking complications with Couchbase Server being behind a NAT.
-	From a networking perspective, it is effectively the same as running Couchbase Server directly on the host.
-	There is no need to use `-p` to "expose" any ports. Each container will use the IP address(es) of its host.
-	Increased efficiency, as there will be no Docker-imposed networking overhead.

通过命令来在*每台主机*启动各自的容器:

```console
$ docker run -d -v ~/couchbase:/opt/couchbase/var --net=host couchbase
```

配置Couchbase Server:

-	登录Couchbase Server管理员面板（通过8091端口）。
-	按照*多容器，单主机*的步骤，不过直接使用主机的IP地址而不是通过`docker inspect`。

## 多主机，多容器（对每一台主机而言）

	┌─────────────────────────────────────────┐  ┌─────────────────────────────────────────┐
	│            Host OS (Linux)              │  │            Host OS (Linux)              │
	│ ┌─────────────────┐ ┌─────────────────┐ │  │ ┌─────────────────┐ ┌─────────────────┐ │
	│ │  Container OS   │ │  Container OS   │ │  │ │  Container OS   │ │  Container OS   │ │
	│ │    (Ubuntu)     │ │    (Ubuntu)     │ │  │ │    (Ubuntu)     │ │    (Ubuntu)     │ │
	│ │  ┌───────────┐  │ │  ┌───────────┐  │ │  │ │  ┌───────────┐  │ │  ┌───────────┐  │ │
	│ │  │ Couchbase │  │ │  │ Couchbase │  │ │  │ │  │ Couchbase │  │ │  │ Couchbase │  │ │
	│ │  │  Server   │  │ │  │  Server   │  │ │  │ │  │  Server   │  │ │  │  Server   │  │ │
	│ │  └───────────┘  │ │  └───────────┘  │ │  │ │  └───────────┘  │ │  └───────────┘  │ │
	│ └─────────────────┘ └─────────────────┘ │  │ └─────────────────┘ └─────────────────┘ │
	└─────────────────────────────────────────┘  └─────────────────────────────────────────┘

-	对位于不同主机上容器间相互的IP访问，Docker并没有提供原生的支持。
-	尽管存在一些软件层网络解决方案例如[Flannel](https://github.com/coreos/flannel)和[Weave](https://github.com/weaveworks/weave)，但这超出了本文档所涵盖的范围。
-	这并不是一个常见的测试或发布情景，所以也许你应该考虑更适当的解决方案（[cloud hosting scenarios](https://github.com/couchbase/docker/wiki#container-specific-cloud-hosting-platforms)）。

## 云环境

尽管超出了本文档的涵盖范围，但你可以从[github wiki](https://github.com/couchbase/docker/wiki#container-specific-cloud-hosting-platforms) 获得如何在云环境上运行Couchbase Server容器的相关指导。

# 证书信息

Couchbase Server包含两个版本：

-	[社区版（Community Edition）](http://www.couchbase.com/community) -- 可以免费获取。

-	[企业版（Enterprise Edition）](http://www.couchbase.com/agreement/subscription) -- 对于商业产品发布需要额外的花费。

默认情况下`latest`tag标签指向最新的企业版版本，或者你也可以直接指明使用`enterprise` tag。如果你希望使用社区版，那么请指明`community` tag，这将指向最新的社区版版本：

	Docker run couchbase:community

# 支持的Docker版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`couchbase/` directory](https://github.com/docker-library/docs/tree/master/couchbase)目录中。在提交新的pull request之前，请确保已经熟悉于 [repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/docker-library/couchbase/issues).

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[GitHub issue](https://github.com/docker-library/couchbase/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
