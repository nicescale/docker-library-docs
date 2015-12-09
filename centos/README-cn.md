# 支持的tag标签，以及相关`Dockerfile`链接

-	[`latest`, `centos7`, `7` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/281559d6864e84fe365ef4007d4db27c197b50fb/docker/Dockerfile)
-	[`centos6`, `6` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/20732d1bb34a9aba45c1c6f41576ed6bf3d8c619/docker/Dockerfile)
-	[`centos5`, `5` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/c8d1a81b0516bca0f20434be8d0fac4f7d58a04a/docker/Dockerfile)
-	[`centos7.1.1503`, `7.1.1503` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/bc561dfdd671d612dbb9f92e7e17dd8009befc44/docker/Dockerfile)
-	[`centos7.0.1406`, `7.0.1406` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/f1d1e0bd83baef08e257da50e6fb446e4dd1b90c/docker/Dockerfile)
-	[`centos6.7`, `6.7` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/d0b72df83f49da844f88aabebe3826372f675370/docker/Dockerfile)
-	[`centos6.6`, `6.6` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/8911843d9a6cc71aadd81e491f94618aded94f30/docker/Dockerfile)
-	[`centos5.11`, `5.11` (*docker/Dockerfile*)](https://github.com/CentOS/sig-cloud-instance-images/blob/2d0554464ae19f4fd70d1b540c8968dbe872797b/docker/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/centos`)](https://github.com/docker-library/official-images/blob/master/library/centos)。 本镜像的更新依赖于：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

关于镜像每个layer以及上述每个tag的详细信息，请查看：[the `centos/tag-details.md` file](https://github.com/docker-library/docs/blob/master/centos/tag-details.md) in [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)。

# CentOS

CentOS是一个由社区提供支持的Linux发行版，它是自[Red Hat](ftp://ftp.redhat.com/pub/redhat/linux/enterprise/)企业版（RHEL）依照开放源代码规定释出的源代码所编译而成，因此其目标是提供和RHEL一致的功能。CentOS项目的不同在于并不包含封闭源代码软件，任何人都可以开放、免费地使用它，任何一个CentOS版本都会提供10年的维护支持。CentOS通常会以两年为周期来进行新版本的发布，并且每个CentOS版本都会获得周期性的更新（约6个月左右）以提供对新硬件的支持，因此它提供了一个安全、稳定且值得信赖的Linux使用环境。

> [wiki.centos.org](https://wiki.centos.org/FrontPage)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/centos/logo.png)

# CentOS镜像的文档

`centos:latest`tag标签总是指向当前可用的最新版本。

## 滚动升级

CentOS项目会周期性地对之前的版本提供更新，以保证这些镜像的稳定性及时效性，这类滚动升级的tag标签只会包含主版本号信息，例如: `docker pull centos:6`或`docker pull centos:7`。

## 小版本tag标签

镜像还提供了指向特定小版本的tag标签，如果你希望使用这些版本，我们强烈建议在你的Dockerfile中加上`RUN yum -y update && yum clean all`，并且对安全性保持更高的关注度。为了使用这些镜像，请注明具体的版本信息，例如:

`docker pull centos:5.11` or `docker pull centos:6.6`

# 软件包文档

CentOS容器在构建时，默认会使用yum的`nodocs`选项，这有助于减小镜像的体积。如果你发现某些软件包的安装出现了文件丢失的情况，请将位于`/etc/yum.conf`中的`tsflags=nodocs`注释掉，并重新尝试安装该软件包。

# Systemd

目前在CentOS7中，systemd已经被`fakesystemd`软件包所取代，从而获得读取主机cgroups信息的能力，如果你依然希望使用systemd 那么请遵照下面的操作步骤。

## Dockerfile for systemd base image

```dockerfile
FROM centos:7
MAINTAINER "you" <your@email.here>
ENV container docker
RUN yum -y swap -- remove fakesystemd -- install systemd systemd-libs
RUN yum -y update; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i ==
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
```

然后，你需要构建自己的基础镜像：

```console
$ docker build --rm -t local/c7-systemd .
```

## systemd版容器的使用示例

为了使用我们之前创建的systemd可用版基础镜像，你需要创建自己的对应`Dockerfile`，类似如下所示：

```dockerfile
FROM local/c7-systemd
RUN yum -y install httpd; yum clean all; systemctl enable httpd.service
EXPOSE 80
CMD ["/usr/sbin/init"]
```

构建该镜像：

```console
$ docker build --rm -t local/c7-systemd-httpd
```

## 启动一个systemd可用版容器

为了运行一个使用systemd的容器，你需要使用之前提到过的`--privileged` 选项，从而挂载主机的cgroups卷，可以参考下面的示例：

```console
$ docker run --privileged -ti -v /sys/fs/cgroup:/sys/fs/cgroup:ro -p 80:80 local/c7-systemd-httpd
```

# 支持的Docker版本

本镜像官方提供了对于Docker 1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/) 。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`centos/` directory](https://github.com/docker-library/docs/tree/master/centos)目录中。在提交新的pull request之前，请确保已经熟悉于 [repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[https://bugs.centos.org](https://bugs.centos.org)或[GitHub issue](https://github.com/CentOS/sig-cloud-instance-images/issues)来联系我们。

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[https://bugs.centos.org](https://bugs.centos.org)或[GitHub issue](https://github.com/CentOS/sig-cloud-instance-images/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
