# 简介

[Apache Maven](http://maven.apache.org) 是一个基于项目对象模型(POM)来管理项目的构建，报告和文档的软件项目管理工具。

![logo](https://raw.githubusercontent.com/docker-library/docs/master/maven/logo.png)

# 使用

## 为Maven项目编写Dockerfile

```dockerfile
FROM maven:3.2-jdk-7-onbuild
CMD ["do-something-with-built-packages"]
```

将`Dockerfile`文件放在项目的根目录中，和`pom.xml`文件一起。

此镜像包含了若干`ONBUILD`指令，包括：`COPY . /usr/src/app`和`RUN mvn install`。

构建镜像并运行：

```console
$ docker build -t my-maven .
$ docker run -it --name my-maven-script my-maven
```

## 单独运行Maven命令

对许多简单的项目而言，单独再写一个`Dockerfile`文件并不是很方便，这种情况下，你可以直接使用如下`docker run`命令运行你的Maven项目：

```console
$ docker run -it --rm --name my-maven-project -v "$PWD":/usr/src/mymaven -w /usr/src/mymaven maven:3.2-jdk-7 mvn clean install
```

# 衍生镜像

有多种的`maven`镜像，以适应不同的使用环境:

## `maven:<version>`

如果不能确认需要哪种maven镜像，你应该选择此类镜像，可以直接用此类镜像挂载你的项目代码并在容器中运行你的应用，还可以基于此类镜像构建你的其他衍生的镜像。

## `maven:onbuild`

此类镜像更适合于构建你的衍生镜像。大多数情况，你只需要在你的项目顶层代码目录中创建一个`Dockerfile`，并加入指令`FROM maven:onbuild`来构建为你的项目运行的独立镜像。

当你更加熟悉自己的项目在Docker中是如何运行的，你应该尝试调整`Dockerfile`，从一个非`nobuild`的镜像来构建你自己的项目镜像，也就是不再使用此类镜像`ONBUILD`指令中预设的指令，而是完全自己控制构建的过程。这样当你日后想要增加更多额外的变动时，你能够更加轻松的去完成。

# License

查看[授权](https://www.apache.org/licenses/)
