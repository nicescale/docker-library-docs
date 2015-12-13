# 支持的tag和其 `Dockerfile` 链接地址

-	[`2015.09`, `latest` (*Dockerfile*)](https://github.com/perl6/docker/blob/a04fb8ba173f1079b307a2db4b6de124a5b425ae/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/rakudo-star`)](https://github.com/docker-library/official-images/blob/master/library/rakudo-star) 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# 什么是 Rakudo Star?

Rakudo Star是一个为了早期尝鲜Perl 6语言的分布式设计. 它包含一个虚拟机(JVM或者MoarVM), Rakudo Perl 6编译器和一套用户也许能找到有用功能的模块.这个镜像包含MoarVM后端的编译器.

项目主页: [http://rakudo.org](http://rakudo.org)

GitHub仓库: [https://github.com/rakudo/star](https://github.com/rakudo/star)

Dockerfile文件: [http://github.com/perl6/docker/tree/master/Dockerfile](http://github.com/perl6/docker/tree/master/Dockerfile)

Perl 6 语言规范: [http://design.perl6.org/](http://design.perl6.org/)

Perl 6 语言文档: [http://doc.perl6.org/](http://doc.perl6.org/)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/rakudo-star/logo.png)

# 如何使用此镜像

简单的运行一个有镜像的容器将启动一个Perl 6 REPL:

```console
$ docker run -it rakudo-star
> say 'Hello, Perl!'
Hello, Perl!
```


你也可以提供一个perl6命令行模式切换到 `docker run`:

```console
$ docker run -it rakudo-star -e 'say "Hello!"'
```

# 贡献/获得帮助

许多Perl 6的开发者都在Freenode #perl6

Rakudo的问题追踪在RT上: [https://rt.perl.org/](https://rt.perl.org/)


# 许可证

查看此镜像所包含软件的[许可证信息](http://dev.perl.org/licenses/).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的`rakudo-star/` directory](https://github.com/docker-library/docs/tree/master/rakudo-star).
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过[GitHub issue](https://github.com/docker-library/rakudo-star/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/docker-library/rakudo-star/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
