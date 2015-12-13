# 支持的tag和其 `Dockerfile` 链接地址

-	[`latest`, `5`, `5.22`, `5.22.0` (*5.022.000-64bit/Dockerfile*)](https://github.com/perl/docker-perl/blob/r20150917.0/5.022.000-64bit/Dockerfile)
-	[`5.20`, `5.20.3` (*5.020.003-64bit/Dockerfile*)](https://github.com/perl/docker-perl/blob/r20150917.0/5.020.003-64bit/Dockerfile)
-	[`threaded`, `5-threaded`, `5.22-threaded`, `5.22.0-threaded` (*5.022.000-64bit,threaded/Dockerfile*)](https://github.com/perl/docker-perl/blob/r20150917.0/5.022.000-64bit,threaded/Dockerfile)
-	[`5.20-threaded`, `5.20.3-threaded` (*5.020.003-64bit,threaded/Dockerfile*)](https://github.com/perl/docker-perl/blob/r20150917.0/5.020.003-64bit,threaded/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/perl`)](https://github.com/docker-library/official-images/blob/master/library/perl). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# 什么是 Perl?

Perl 是一个高级的, 通用的, 直译式, 动态的程序语言. Perl语言借鉴了其他程序语言的特性, 包括C, shell scripting (sh), AWK, 和 sed.

> [wikipedia.org/wiki/Perl](https://en.wikipedia.org/wiki/Perl)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/perl/logo.png)

# 如何使用此镜像

## 在你的Perl项目里创建一个 `Dockerfile` 文件

```dockerfile
FROM perl:5.20
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
CMD [ "perl", "./your-daemon-or-script.pl" ]
```

然后, 构建并且运行这个Docker镜像:

```console
$ docker build -t my-perl-app .
$ docker run -it --rm --name my-running-app my-perl-app
```

## 运行一个单一的Perl脚本

  对于许多简单、单一文件的项目, 你可能发现写一个完整的dockerfile并不方便. 在这种情况下, 你可以直接用Perl Docker镜像去运行一个Perl脚本:

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp perl:5.20 perl your-daemon-or-script.pl
```

# 许可证

查看此镜像所包含软件的[许可证信息](http://dev.perl.org/licenses/).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[`perl/` directory](https://github.com/docker-library/docs/tree/master/perl) .
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过 [GitHub issue](https://github.com/Perl/docker-perl/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/Perl/docker-perl/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
