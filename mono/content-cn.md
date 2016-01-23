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

