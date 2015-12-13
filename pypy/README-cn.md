# 支持的tag和其 `Dockerfile` 链接地址

-	[`2-4.0.1`, `2-4.0`, `2-4`, `2` (*2/Dockerfile*)](https://github.com/docker-library/pypy/blob/3826e2d8095e0dda578aca3fa7fb72c3e7aaa47e/2/Dockerfile)
-	[`2-4.0.1-onbuild`, `2-4.0-onbuild`, `2-4-onbuild`, `2-onbuild` (*2/onbuild/Dockerfile*)](https://github.com/docker-library/pypy/blob/b48e8489ab794a2bacfd396c2f8e1a5b06d6ae48/2/onbuild/Dockerfile)
-	[`2-4.0.1-slim`, `2-4.0-slim`, `2-4-slim`, `2-slim` (*2/slim/Dockerfile*)](https://github.com/docker-library/pypy/blob/3826e2d8095e0dda578aca3fa7fb72c3e7aaa47e/2/slim/Dockerfile)
-	[`3-2.4.0`, `3-2.4`, `3-2`, `3`, `latest` (*3/Dockerfile*)](https://github.com/docker-library/pypy/blob/528795c882a9cb55e713586aeb3eb71707438c69/3/Dockerfile)
-	[`3-2.4.0-onbuild`, `3-2.4-onbuild`, `3-2-onbuild`, `3-onbuild`, `onbuild` (*3/onbuild/Dockerfile*)](https://github.com/docker-library/pypy/blob/b48e8489ab794a2bacfd396c2f8e1a5b06d6ae48/3/onbuild/Dockerfile)
-	[`3-2.4.0-slim`, `3-2.4-slim`, `3-2-slim`, `3-slim`, `slim` (*3/slim/Dockerfile*)](https://github.com/docker-library/pypy/blob/528795c882a9cb55e713586aeb3eb71707438c69/3/slim/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/pypy`)](https://github.com/docker-library/official-images/blob/master/library/pypy). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# What is PyPy?

PyPy是Python解释器和编译器. PyPy注重速度、效率与原来的CPython解释器兼容.

PyPy作为一个Python语言编写的Python解释器. 目前PyPy版本是从RPython到C的编译. PyPy JIT ("Just In Time"的简称)编译器能在运行时将Python代码转换为机器码.

> [wikipedia.org/wiki/PyPy](https://en.wikipedia.org/wiki/PyPy)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/pypy/logo.png)

# 如何使用此镜像

## 在你的Python项目下创建一个 `Dockerfile`文件

```dockerfile
FROM pypy:3-onbuild
CMD [ "pypy3", "./your-daemon-or-script.py" ]
```

如果你需要使用PyPy 2:

```dockerfile
FROM pypy:2-onbuild
CMD [ "pypy", "./your-daemon-or-script.py" ]
```

这些镜像都包含了多个ONBUILD触发器，在大部分情况下这些触发器能满足你的需求。构建部分会COPY一个requirements.txt文件，在 Dockerfile 中RUN pip install，然后将当前目录复制至/usr/src/app.

然后，你就可以构建和运行 Docker 镜像了:

```console
$ docker build -t my-python-app .
$ docker run -it --rm --name my-running-app my-python-app
```

## 运行单个Python脚本

对于很多简单的单个文件项目而言，编写一个完整的Dockerfile会带来些许的不方便。在这种情况下，你可以通过直接使用 Docker 镜像来运行 Python 脚本：

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp pypy:3 pypy3 your-daemon-or-script.py
```

如果你需要使用Python 2:

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp pypy:2 pypy your-daemon-or-script.py
```

# 镜像变种

`pypy` 有许多种, 每个都是为特定的用例设计的

## `pypy:<version>`

这是最原生的镜像。如果你不确定需要什么类型的 镜像，那么你最有可能需要使用的就是这样的镜像。这类镜像设计的初衷，就是为了既满足直接运行的容器（挂载你的 Python 源代码并直接启动容器来运行你的应用），又满足基础镜像的需求，来构建其他的镜像。这些标签是[`buildpack-deps`](https://registry.hub.docker.com/_/buildpack-deps/) 的衍生和成熟版本。buildpacks－deps是设计用来满足那些在系统中拥有很多镜像的 Docker 用户的。从设计原则出发，它会包含很多冗余的 Debian 软件包。这将减少镜像在再构建时所需安装的软件数量，同时降低镜像在你系统中的存储空间。

## `pypy:onbuild`

这个镜像将使构建衍生镜像更容易. 对于大多数的用例, 在你的项目根目录下创建一个带有`FROM python:onbuild`的 `Dockerfile` 文件将足以为你的项目创建一个独立的镜像.

当`onbuild`变体对于"getting off the ground running" (在很短的时间内从0到docker化)是真有用的, *当*`ONBUILD`触发的时候 不建议长期在项目里使用这种缺乏控制权的,(请查看[`docker/docker#5714`](https://github.com/docker/docker/issues/5714), [`docker/docker#8240`](https://github.com/docker/docker/issues/8240), [`docker/docker#11917`](https://github.com/docker/docker/issues/11917))..

## `pypy:slim`

此镜像不包含默认标签的通用包，只包含了运行`pypy`所需的最少的软件包.除非你只工作在一个环境下, python镜像将被部署并且你有空间限制, 我们强烈建议使用这个库的默认镜像


# 许可证

查看此镜像所包含软件的[许可证信息](https://bitbucket.org/pypy/pypy/src/c3ff0dd6252b6ba0d230f3624dbb4aab8973a1d0/LICENSE?at=default).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[`pypy/` directory](https://github.com/docker-library/docs/tree/master/pypy) of the [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs) .
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过[GitHub issue](https://github.com/docker-library/pypy/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/docker-library/pypy/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
