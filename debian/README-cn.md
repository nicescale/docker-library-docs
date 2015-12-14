# 支持的tag标签，以及相关`Dockerfile`链接

-	[`8.2`, `8`, `jessie`, `latest` (*jessie/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/jessie/Dockerfile)
-	[`jessie-backports` (*jessie/backports/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/jessie/backports/Dockerfile)
-	[`oldstable` (*oldstable/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/oldstable/Dockerfile)
-	[`oldstable-backports` (*oldstable/backports/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/oldstable/backports/Dockerfile)
-	[`sid` (*sid/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/sid/Dockerfile)
-	[`6.0.10`, `6.0`, `6`, `squeeze` (*squeeze/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/squeeze/Dockerfile)
-	[`stable` (*stable/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/stable/Dockerfile)
-	[`stable-backports` (*stable/backports/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/stable/backports/Dockerfile)
-	[`stretch` (*stretch/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/stretch/Dockerfile)
-	[`testing` (*testing/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/testing/Dockerfile)
-	[`unstable` (*unstable/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/unstable/Dockerfile)
-	[`7.9`, `7`, `wheezy` (*wheezy/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/wheezy/Dockerfile)
-	[`wheezy-backports` (*wheezy/backports/Dockerfile*)](https://github.com/tianon/docker-brew-debian/blob/d8d023e100f72982b31a8c932570080c3101b7e7/wheezy/backports/Dockerfile)
-	[`rc-buggy` (*debian/rc-buggy/Dockerfile*)](https://github.com/tianon/dockerfiles/blob/22a998f815d55217afa0075411b810b8889ceac1/debian/rc-buggy/Dockerfile)
-	[`experimental` (*debian/experimental/Dockerfile*)](https://github.com/tianon/dockerfiles/blob/22a998f815d55217afa0075411b810b8889ceac1/debian/experimental/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/debian`)](https://github.com/docker-library/official-images/blob/master/library/debian)。本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

关于镜像每个layer以及上述每个tag的详细信息，请查看位于[the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[the `debian/tag-details.md` file](https://github.com/docker-library/docs/blob/master/debian/tag-details.md)。

# Debian是什么?

Debian是由免费及开源软件组成的操作系统，主要基于GNU General Public License协议，Debian项目是一个独立的、分散的组织，由3000人志愿者组成。Debian是使用最为广泛的Linux发行版之一，被应用于个人计算机和网络服务器，并且被用于其他一些Linux发行版的基础。

> [wikipedia.org/wiki/Debian](https://en.wikipedia.org/wiki/Debian)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/debian/logo.png)

# 关于本镜像

`debian:latest`tag标签永远指向最新的稳定版本（例如在本文档写作时，指向的是`debian:jessie`），其他稳定版本也都被标注了各自的tag标签（例如，`debian:8`等同于`debian:jessie`，`debian:7`等同于`debian:wheezy`等等）。

滚动标签（包括`debian:stable`、`debian:testing`等等）使用定义在`/etc/apt/sources.list`文件中的名称（例如, `deb
http://httpredir.debian.org/debian testing main`）。

## `/etc/apt/sources.list`

有关在不同版本镜像间的选择，可以参考文档：[httpredir.debian.org](http://httpredir.debian.org)。

```console
$ docker run debian:jessie cat /etc/apt/sources.list
deb http://httpredir.debian.org/debian jessie main
deb http://httpredir.debian.org/debian jessie-updates main
deb http://security.debian.org jessie/updates main
```

# 支持的Docker版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`debian/` directory](https://github.com/docker-library/docs/tree/master/debian)目录中。在提交新的pull request之前，请确保已经熟悉于[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/tianon/docker-brew-debian/issues)来提交。

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[GitHub issue](https://github.com/tianon/docker-brew-debian/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
