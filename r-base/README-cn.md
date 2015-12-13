# 支持的tag和其 `Dockerfile` 链接地址

-	[`3.2.2`, `latest` (*r-base/Dockerfile*)](https://github.com/rocker-org/rocker/blob/ee6e1da2e7020978a6aaf3916f8aba9edd16e72d/r-base/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/r-base`)](https://github.com/docker-library/official-images/blob/master/library/r-base). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# 什么是 R?

R 是一种统计计算和图形的系统. 它包括一个语言加上一个图形运行环境, 调试器, 访问以确定的系统函数, 和在脚本文件中运行程序存储的能力

R语言被广泛使用在为了统计软件和数据分析的统计学家和数据挖掘开发者.民意调查和数据挖掘的调查都显示的R的受欢迎程度在最近几年大幅增加

R是一个S编程语言的实现,R的语法是来自Scheme,S是John Chambers在Bell实验室创建的.R是Ross Ihaka和Robert Gentleman在新西兰的Auckland大学创建的并且目前由R核心团队开发, Chambers是其中一员. R的命名来自于头两位作者的姓氏

R是一个GNU项目. 用于R的软件环境的源代码是用主要写在C, Fortran和R. R是GNU许可证下是免费用的, 并且给各种操作系统提供预编译的二进制版本.R使用命令行接口;不过有几个图形界面工用户使用.

> [R FAQ](http://cran.r-project.org/doc/FAQ/R-FAQ.html#What-is-R_003f), [wikipedia.org/wiki/R_(programming_language)](http://en.wikipedia.org/wiki/R_%28programming_language%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/r-base/logo.png)

# 如何使用此镜像

## 交互式的R

直接启动R给交互工作模式

```console
$ docker run -ti --rm r-base
```

## 批量模式

链接工作目录去运行R批量命令.我们建议当链接到容器时用非root用户去必变权限改变,如下所示

```console
$ docker run -ti --rm -v "$PWD":/home/docker -w /home/docker -u docker r-base R CMD check .
```

另外, 只需要先在容器上启动一个bash会话.这允许用户运行批处理命令,并编辑和运行脚本

```console
$ docker run -ti --rm r-base /usr/bin/bash
$ vim.tiny myscript.R
```

在容器里面写脚本,退出`vim`并且运行`Rscript`

```console
$ Rscript myscript.R
```

## Dockerfiles

你自己的Dockerfiles文件使用`r-base`作为基础.例如, 沿着下面这行来编译和运行你的项目

```dockerfile
FROM r-base:latest
COPY . /usr/local/src/myscripts
WORKDIR /usr/local/src/myscripts
CMD ["Rscript", "myscript.R"]
```

使用命令构建你的镜像

```console
$ docker build -t myscript /path/to/Dockerfile
```

没有用命令运行这个容器将执行这个脚本.另外,用户可以像上面说的一样交互模式或者批量模式运行容器,而不是连接volumes

进一步的文档和示例用例可以在[rocker-org](https://github.com/rocker-org/rocker/wiki)项目wiki找到.

# 许可证

查看此镜像所包含软件的[R-project license information](http://www.r-project.org/Licenses/)

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[`r-base/` directory](https://github.com/docker-library/docs/tree/master/r-base).
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过 [GitHub issue](https://github.com/Perl/docker-perl/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/rocker-org/rocker/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
