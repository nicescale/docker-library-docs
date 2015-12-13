# 支持的tag和其 `Dockerfile` 链接地址

-	[`2.7.10`, `2.7`, `2` (*2.7/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/2.7/Dockerfile)
-	[`2.7.10-onbuild`, `2.7-onbuild`, `2-onbuild` (*2.7/onbuild/Dockerfile*)](https://github.com/docker-library/python/blob/7663560df7547e69d13b1b548675502f4e0917d1/2.7/onbuild/Dockerfile)
-	[`2.7.10-slim`, `2.7-slim`, `2-slim` (*2.7/slim/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/2.7/slim/Dockerfile)
-	[`2.7.10-wheezy`, `2.7-wheezy`, `2-wheezy` (*2.7/wheezy/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/2.7/wheezy/Dockerfile)
-	[`3.2.6`, `3.2` (*3.2/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.2/Dockerfile)
-	[`3.2.6-onbuild`, `3.2-onbuild` (*3.2/onbuild/Dockerfile*)](https://github.com/docker-library/python/blob/7663560df7547e69d13b1b548675502f4e0917d1/3.2/onbuild/Dockerfile)
-	[`3.2.6-slim`, `3.2-slim` (*3.2/slim/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.2/slim/Dockerfile)
-	[`3.2.6-wheezy`, `3.2-wheezy` (*3.2/wheezy/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.2/wheezy/Dockerfile)
-	[`3.3.6`, `3.3` (*3.3/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.3/Dockerfile)
-	[`3.3.6-onbuild`, `3.3-onbuild` (*3.3/onbuild/Dockerfile*)](https://github.com/docker-library/python/blob/7663560df7547e69d13b1b548675502f4e0917d1/3.3/onbuild/Dockerfile)
-	[`3.3.6-slim`, `3.3-slim` (*3.3/slim/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.3/slim/Dockerfile)
-	[`3.3.6-wheezy`, `3.3-wheezy` (*3.3/wheezy/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.3/wheezy/Dockerfile)
-	[`3.4.3`, `3.4` (*3.4/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.4/Dockerfile)
-	[`3.4.3-onbuild`, `3.4-onbuild` (*3.4/onbuild/Dockerfile*)](https://github.com/docker-library/python/blob/7663560df7547e69d13b1b548675502f4e0917d1/3.4/onbuild/Dockerfile)
-	[`3.4.3-slim`, `3.4-slim` (*3.4/slim/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.4/slim/Dockerfile)
-	[`3.4.3-wheezy`, `3.4-wheezy` (*3.4/wheezy/Dockerfile*)](https://github.com/docker-library/python/blob/15798abb6cfb145344462a345db4b572227fb859/3.4/wheezy/Dockerfile)
-	[`3.5.0`, `3.5`, `3`, `latest` (*3.5/Dockerfile*)](https://github.com/docker-library/python/blob/e4a0ed26c086a48a75e9ea2b163c8262dcdff2af/3.5/Dockerfile)
-	[`3.5.0-onbuild`, `3.5-onbuild`, `3-onbuild`, `onbuild` (*3.5/onbuild/Dockerfile*)](https://github.com/docker-library/python/blob/0fa3202789648132971160f686f5a37595108f44/3.5/onbuild/Dockerfile)
-	[`3.5.0-slim`, `3.5-slim`, `3-slim`, `slim` (*3.5/slim/Dockerfile*)](https://github.com/docker-library/python/blob/e4a0ed26c086a48a75e9ea2b163c8262dcdff2af/3.5/slim/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/python`)](https://github.com/docker-library/official-images/blob/master/library/python). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# What is Python?

Python 是一种解释型、交互性、面向对象的开源编程语言。它集成了众多的高级功能，比如：模块、异常处理、动态输入、高级的动态数据以及动态类。Python 兼备异常清晰的语法。Python 拥有丰富的系统调用接口和库，还包括各式各样的窗口系统，同时 Python 还可扩展使用 C 语言和 C++ 语言。对于需要编程接口的应用而言，Python 还可以作为一种扩展语言进行使用。最后，Python 还具备强大的移植性：它可以很方便地运行在很多的 Unix 系统、Mac OS、以及 Windows 2000 及后续版本平台上。

> [wikipedia.org/wiki/Python_(programming_language)](https://en.wikipedia.org/wiki/Python_%28programming_language%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/python/logo.png)

# 如何使用此镜像

## 在你的app项目下创建一个`Dockerfile`文件

```dockerfile
FROM python:3-onbuild
CMD [ "python", "./your-daemon-or-script.py" ]
```

如果你需要使用Python 2:

```dockerfile
FROM python:2-onbuild
CMD [ "python", "./your-daemon-or-script.py" ]
```
这些镜像都包含了多个ONBUILD触发器，在大部分情况下这些触发器能满足你的需求。构建部分会COPY一个requirements.txt文件，在 Dockerfile 中RUN pip install，然后将当前目录复制至/usr/src/app

然后，你就可以构建和运行 Docker 镜像了:

```console
$ docker build -t my-python-app .
$ docker run -it --rm --name my-running-app my-python-app
```

## 运行单个Python脚本

对于很多简单的单个文件项目而言，编写一个完整的Dockerfile会带来些许的不方便。在这种情况下，你可以通过直接使用 Docker 镜像来运行 Python 脚本：

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp python:3 python your-daemon-or-script.py
```

如果你需要使用Python 2

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp python:2 python your-daemon-or-script.py
```

# 镜像变种

`python`有许多种, 每个都是为特定的用例设计的

## `python:<version>`

这是最原生的镜像。如果你不确定需要什么类型的 镜像，那么你最有可能需要使用的就是这样的镜像。这类镜像设计的初衷，就是为了既满足直接运行的容器（挂载你的 Python 源代码并直接启动容器来运行你的应用），又满足基础镜像的需求，来构建其他的镜像。这些标签是[`buildpack-deps`](https://registry.hub.docker.com/_/buildpack-deps/) 的衍生和成熟版本。buildpacks－deps是设计用来满足那些在系统中拥有很多镜像的 Docker 用户的。从设计原则出发，它会包含很多冗余的 Debian 软件包。这将减少镜像在再构建时所需安装的软件数量，同时降低镜像在你系统中的存储空间。

## `python:onbuild`

为了更容易的构建衍生镜像这个镜像自动将`requirements.txt`注入到`pip`. 对于大多数的用例, 在你的项目根目录下创建一个带有`FROM python:onbuild`的 `Dockerfile` 文件将足以为你的项目创建一个独立的镜像

当`onbuild`变体对于"getting off the ground running" (在很短的时间内从0到docker化)是真有用的, *当*`ONBUILD`触发的时候 不建议长期在项目里使用这种缺乏控制权的,(请查看[`docker/docker#5714`](https://github.com/docker/docker/issues/5714), [`docker/docker#8240`](https://github.com/docker/docker/issues/8240), [`docker/docker#11917`](https://github.com/docker/docker/issues/11917)).

## `python:slim`

此镜像不包含默认标签的通用包，只包含了运行`python`所需的最少的软件包.除非你只工作在一个环境下, python镜像将被部署并且你有空间限制, 我们强烈建议使用这个库的默认镜像

# 许可证

查看许可证信息[Python 2](https://docs.python.org/2/license.html) and [Python 3](https://docs.python.org/3/license.html).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[`python/` directory](https://github.com/docker-library/docs/tree/master/python) .
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过 [GitHub issue](https://github.com/docker-library/python/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/docker-library/python/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
