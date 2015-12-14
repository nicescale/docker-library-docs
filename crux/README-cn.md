# 支持的tag标签，以及相关`Dockerfile`链接

-	[`latest`, `3.1` (*Dockerfile*)](https://github.com/prologic/docker-crux/blob/c614d61c53c05c02a43a34187fa1370db2c61524/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/crux`)](https://github.com/docker-library/official-images/blob/master/library/crux)。本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

关于镜像每个layer以及上述每个tag的详细信息，请查看位于[the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[the `crux/tag-details.md` file](https://github.com/docker-library/docs/blob/master/crux/tag-details.md)。

# CRUX是什么？

CRUX是一个针对 i686 优化过的轻量级 Linux 发行版，它的目标受众是有经验的 Linux 用户。CRUX 通过 BSD 风格的脚本发行，基于 tar.gz 包系统，它也有一个 ports 系统来安装和升级程序、软件，其目标还包括提供最新的Linux特性及工具。CRUX具有一套端口系统使得应用的安装升级变得十分简单。

# 为什么使用CRUX？

当前已经有数量繁多的Linux发行版可供选择，那么是什么使CRUX脱颖而出呢？尽管发行版的选择往往包含很多个人倾向，这里我们还是要简单介绍一下CRUX的特点和优势，CRUX从诞生起其创造者就把保持简洁作为首要目标。

首先就是简化新软件包的创建和旧软件包的升级，在CRUX中升级软件包通常只需要输入一条`pkgmk -d -u`命令就可泄了。使用端口选项可以帮助你保持软件包永远与最新版本保持同步，并非最新的不稳定开发版，而是最新的稳定版。CRUX的其他特征还包括对软件包创建的优化，例如使用`-march=x86-64`选项来针对特定的处理器架构，或者你还可以将不重要的文件目录排除在外（例如`/usr/doc/*`，通常使用Google就可以很方便地获得文档相关信息）。

最后CRUX还会尽量提供最新的Linux特性和工具，简单来说，如果符合一下条件那么CRUX很可能就适合于你：

-	有经验的Linux用户，希望使用一个更干净的轻量级发行版。
-	相对于GUI，更希望通过编辑配置文件来定义软件行为的用户。
-	并不希望每次都下载源码并自行编译来完成软件安装的用户。

# 支持的Docker版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`crux/` directory](https://github.com/docker-library/docs/tree/master/crux) 中。在提交新的pull request之前，请确保已经熟悉于[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/therealprologic/docker-crux/issues)来提交。

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[GitHub issue](https://github.com/therealprologic/docker-crux/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
