# 支持的tag标签，以及相关`Dockerfile`链接

-   [`3.10.0`, `3.10` (*3.10.0/Dockerfile*)](https://github.com/mono/docker/blob/adc7a3ec47f7d590f75a4dec0203a2103daf8db0/3.10.0/Dockerfile)
-   [`3.10.0-onbuild`, `3.10-onbuild` (*3.10.0/onbuild/Dockerfile*)](https://github.com/mono/docker/blob/66226b17125b72685c2022e4fecaee2716b0fb3a/3.10.0/onbuild/Dockerfile)
-   [`3.12.1`, `3.12.0`, `3.12`, `3` (*3.12.1/Dockerfile*)](https://github.com/mono/docker/blob/39c80bc024a4797c119c895fda70024fbc14d5b9/3.12.1/Dockerfile)
-   [`3.12.1-onbuild`, `3.12.0-onbuild`, `3.12-onbuild`, `3-onbuild` (*3.12.1/onbuild/Dockerfile*)](https://github.com/mono/docker/blob/39c80bc024a4797c119c895fda70024fbc14d5b9/3.12.1/onbuild/Dockerfile)
-   [`3.8.0`, `3.8` (*3.8.0/Dockerfile*)](https://github.com/mono/docker/blob/adc7a3ec47f7d590f75a4dec0203a2103daf8db0/3.8.0/Dockerfile)
-   [`3.8.0-onbuild`, `3.8-onbuild` (*3.8.0/onbuild/Dockerfile*)](https://github.com/mono/docker/blob/66226b17125b72685c2022e4fecaee2716b0fb3a/3.8.0/onbuild/Dockerfile)
-   [`4.0.5.1`, `4.0.5`, `4.0` (*4.0.5.1/Dockerfile*)](https://github.com/mono/docker/blob/39c80bc024a4797c119c895fda70024fbc14d5b9/4.0.5.1/Dockerfile)
-   [`4.0.5.1-onbuild`, `4.0.5-onbuild`, `4.0-onbuild` (*4.0.5.1/onbuild/Dockerfile*)](https://github.com/mono/docker/blob/39c80bc024a4797c119c895fda70024fbc14d5b9/4.0.5.1/onbuild/Dockerfile)
-   [`4.2.1.102`, `4.2.1`, `4.2`, `4`, `latest` (*4.2.1.102/Dockerfile*)](https://github.com/mono/docker/blob/39c80bc024a4797c119c895fda70024fbc14d5b9/4.2.1.102/Dockerfile)
-   [`4.2.1.102-onbuild`, `4.2.1-onbuild`, `4.2-onbuild`, `4-onbuild`, `onbuild` (*4.2.1.102/onbuild/Dockerfile*)](https://github.com/mono/docker/blob/39c80bc024a4797c119c895fda70024fbc14d5b9/4.2.1.102/onbuild/Dockerfile)

关于本镜像的更详细信息，请访问[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)下的[相关的清单文件 (`library/mono`)](https://github.com/docker-library/official-images/blob/master/library/mono). 

# 关于Mono

由 Xamarin 提供, Mono 项目创建了一系列符合ECMA标准的.NET工具，为C#程序提供微软.Net框架的运行环境。完整的解决方案体系和有活力的开源社区使得Mono成为了跨平台应用开发的首选。

-   [Mono项目主页](http://www.mono-project.com/)
-   [http://en.wikipedia.org/wiki/Mono_(software)](http://en.wikipedia.org/wiki/Mono_%28software%29)

%%LOGO%%

# 镜像使用

该镜像可以独立运行控制台应用程序。

## 在你的项目中创建一个 `Dockerfile`文件

该示例将运行`TestingConsoleApp.exe`程序。

```dockerfile
FROM mono:3.10-onbuild
CMD [ "mono",  "./TestingConsoleApp.exe" ]
```

将此文件放置在应用程序的根目录下，旁边的.sln是解决方案文件。修改exectuable名称，以匹配要运行的程序。

该镜像有 `ONBUILD` 触发器，会添加你的程序代码到 `/usr/src/app/source`, 修复NuGet包 并编译程序, 输出到 `/usr/src/app/build`目录下.

在放置Dockerfile文件的目录下，你可以创建容器运行你的应用程序了。

```console
$ docker build -t my-app .
$ docker run my-app
```

现在你可以看到程序的输出结果了。

# Credits

镜像由Mono项目的用户Xamarin提供。

感谢 [Michael Friis](http://friism.com/) 的前期工作

# 镜像的变种

 `mono` 镜像存在多个变种，用于面向特定的不同任务。

## `mono:<version>`

这是默认的镜像，如果并不确定自己需要的是哪个版本，那么你需要的很可能就是它。它被设计为使用后可随意抛弃的容器（将源码挂载进容器里，并启动你的应用）。

## `mono:onbuild`

这个版本用于构建衍生镜像，在你的项目中添加一个`Dockerfile`，并使用`FROM mono:onbuild`作为基础镜像，这样可以很方便地为你的项目创建一个独立的镜像。

尽管`onbuild`非常适用于将项目快速Docker容器化，但对于长期维护的项目来说并不是非常建议使用它. (参考 [`docker/docker#5714`](https://github.com/docker/docker/issues/5714), [`docker/docker#8240`](https://github.com/docker/docker/issues/8240), [`docker/docker#11917`](https://github.com/docker/docker/issues/11917)).

当你对如何将项目Docker容器化感到很熟悉之后，那么就可以考虑转而使用一个基于非`onbuild`镜像的`Dockerfile`，并且可以复用之前的大多数配置代码，这可以让你获得对项目更高的可控性，并且也更加利于日后对项目的升级改造。

# 许可证

这个镜像使用MIT协议. 可以从[Mono 项目证书 FAQ](http://www.mono-project.com/docs/faq/licensing/) 了解更多关于如何授权Mono和相关库的信息。

# 支持版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

提交问题到[ GitHub ](https://github.com/mono/docker).
