# 支持的tag和其 `Dockerfile` 链接地址

-	[`7.0.11-apache`, `7.0.11`, `7.0-apache`, `7.0`, `7-apache`, `7` (*7.0/apache/Dockerfile*)](https://github.com/docker-library/owncloud/blob/4e2e35d39456479193a9aa5984be69bd90a3b5aa/7.0/apache/Dockerfile)
-	[`7.0.11-fpm`, `7.0-fpm`, `7-fpm` (*7.0/fpm/Dockerfile*)](https://github.com/docker-library/owncloud/blob/4e2e35d39456479193a9aa5984be69bd90a3b5aa/7.0/fpm/Dockerfile)
-	[`8.0.9-apache`, `8.0.9`, `8.0-apache`, `8.0` (*8.0/apache/Dockerfile*)](https://github.com/docker-library/owncloud/blob/1729d80aabfd0d9a7d345991dbf80cc4b8a00c7f/8.0/apache/Dockerfile)
-	[`8.0.9-fpm`, `8.0-fpm` (*8.0/fpm/Dockerfile*)](https://github.com/docker-library/owncloud/blob/1729d80aabfd0d9a7d345991dbf80cc4b8a00c7f/8.0/fpm/Dockerfile)
-	[`8.1.4-apache`, `8.1.4`, `8.1-apache`, `8.1` (*8.1/apache/Dockerfile*)](https://github.com/docker-library/owncloud/blob/1729d80aabfd0d9a7d345991dbf80cc4b8a00c7f/8.1/apache/Dockerfile)
-	[`8.1.4-fpm`, `8.1-fpm` (*8.1/fpm/Dockerfile*)](https://github.com/docker-library/owncloud/blob/1729d80aabfd0d9a7d345991dbf80cc4b8a00c7f/8.1/fpm/Dockerfile)
-	[`8.2.1-apache`, `8.2.1`, `8.2-apache`, `8.2`, `8-apache`, `8`, `apache`, `latest` (*8.2/apache/Dockerfile*)](https://github.com/docker-library/owncloud/blob/1729d80aabfd0d9a7d345991dbf80cc4b8a00c7f/8.2/apache/Dockerfile)
-	[`8.2.1-fpm`, `8.2-fpm`, `8-fpm`, `fpm` (*8.2/fpm/Dockerfile*)](https://github.com/docker-library/owncloud/blob/1729d80aabfd0d9a7d345991dbf80cc4b8a00c7f/8.2/fpm/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/owncloud`)](https://github.com/docker-library/official-images/blob/master/library/owncloud). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# 什么是ownCloud?

ownCloud 是一个自托管的文件同步和共享的服务. 它提供一个web界面去访问你的数据, 同时提供一个平台去查看同步客户端或WebDAV, 在你的控制下可以很容易的进行跨平台的同步和共享. ownCloud是开放架构,通过简单强大的API扩展应用程序和插件,并且它适用于任何存储.

> [owncloud.org](https://owncloud.org/)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/owncloud/logo.png)

# 如何使用此镜像

## 启动ownCloud

启动ownCloud8.1实例并监听80端口就如下面一样简单:

```console
$ docker run -d -p 80:80 owncloud:8.1
```

然后访问 http://localhost/ 去向导页面. 容器默认使用SQLite数据存储, 但向导应该允许连接到存在的数据库. 另外, tag6.0, 7.0, or 8.0是可用的.

### 注意事项

当使用6.0镜像, 当运行安装向导时你需要将主机端口映射到apache监听的容器端口. 默认是80端口.

# 许可证

查看此镜像所包含软件的[许可证信息](https://owncloud.org/contribute/agreement/).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[`owncloud/` directory](https://github.com/docker-library/docs/tree/master/owncloud). 提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过 [GitHub issue](https://github.com/docker-library/owncloud/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/docker-library/owncloud/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
