# 支持的tag标签，以及相关`Dockerfile`链接

-	[`latest`, `lein-2.5.3` (*Dockerfile*)](https://github.com/Quantisan/docker-clojure/blob/a22cdfcdf10fd5c99fa4bb993c71847ab00ee2a9/Dockerfile)
-	[`onbuild`, `lein-2.5.3-onbuild` (*onbuild/Dockerfile*)](https://github.com/Quantisan/docker-clojure/blob/a22cdfcdf10fd5c99fa4bb993c71847ab00ee2a9/onbuild/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/clojure`)](https://github.com/docker-library/official-images/blob/master/library/clojure)。本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

关于镜像每个layer以及上述每个tag的详细信息，请查看位于 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[the `clojure/tag-details.md` file](https://github.com/docker-library/docs/blob/master/clojure/tag-details.md)。

# Clojure是什么?

Clojure是Lisp编程语言的一种方言，具有很强的函数式编程特征么，它运行于Java虚拟机、通用语言运行库（Common Language Runtime）以及JavaScript引擎中。像其他Lisp方言一样，Clojure将代码当作数据进行处理，并提供了一套宏系统。

> [wikipedia.org/wiki/Clojure](http://en.wikipedia.org/wiki/Clojure)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/clojure/logo.png)

# 如何使用本镜像

## 运行一个Lein/Clojure实例：

本镜像默认你会以最传统的方式来使用Clojure：[Leiningen (`lein`)](http://leiningen.org/)，你只需将以下`Dockerfile`任何已存在的 Leiningen/Clojure项目目录中：

```dockerfile
FROM clojure
COPY . /usr/src/app
WORKDIR /usr/src/app
CMD ["lein", "run"]
```

然后使用以下命令来运行镜像：

```console
$ docker build -t my-clojure-app .
$ docker run -it --rm --name my-running-app my-clojure-app
```

尽管这是最直接的方式，但这个`Dockerfile`却存在一些不足，`lein run`命令会下载所有的依赖，并对项目进行编译，然后才会运行它。这会花费比较长的时间，而你可能并不希望每次都重复这一过程。所以为了节省这些时间，你可以使用下面的`Dockerfile`：

```dockerfile
FROM clojure
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
COPY project.clj /usr/src/app/
RUN lein deps
COPY . /usr/src/app
RUN mv "$(lein uberjar | sed -n 's/^Created \(.*standalone\.jar\)/\1/p')" app-standalone.jar
CMD ["java", "-jar", "app-standalone.jar"]
```

这个`Dockerfile`会下载依赖（并对它们进行缓存，然后只会在其发生改变时才再次进行下载），并将其编译为一个独立的jar包，从而大大节省了运行的时间。

现在你就可以使用刚才的镜像了。

## 将你的Lein/Clojure项目打包进容器里

如果你有一个已经存在的Lein/Clojure项目，那么你可以很方便地在容器里将其打成jar包:

```console
$ docker run -it --rm -v "$PWD":/usr/src/app -w /usr/src/app clojure lein uberjar
```

这会将你的项目构建成一个jar文件，就位于项目的`target/uberjar`目录中。

# 镜像的变种

`clojure`镜像存在多个变种，用于面向特定的不同任务。

## `clojure:<version>`

这是默认的镜像，如果并不确定自己需要的是哪个版本，那么你需要的很可能就是它。它被设计为使用后可随意抛弃的容器（将源码挂载进容器里，并启动你的应用）。

## `clojure:onbuild`

这个版本用于构建衍生镜像，在你的项目中添加一个`Dockerfile`，并使用`FROM clojure:onbuild`作为基础镜像，这样可以很方便地为你的项目创建一个独立的镜像。

尽管`onbuild`非常适用于将项目快速Docker容器化，但对于长期维护的项目来说并不是非常建议使用它： [`docker/docker#5714`](https://github.com/docker/docker/issues/5714), [`docker/docker#8240`](https://github.com/docker/docker/issues/8240), [`docker/docker#11917`](https://github.com/docker/docker/issues/11917))。

当你对如何将项目Docker容器化感到很熟悉之后，那么就可以考虑转而使用一个基于非`onbuild`镜像的`Dockerfile`，并且可以复用之前的大多数配置代码，这可以让你获得对项目更高的可控性，并且也更加利于日后对项目的升级改造。

# 证书

你可以从[license information](http://clojure.org/license)查看本镜像所使用的软件相关信息。

# 支持的Docker版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`clojure/` directory](https://github.com/docker-library/docs/tree/master/clojure)目录中。在提交新的pull request之前，请确保已经熟悉于[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/Quantisan/docker-clojure/issues)来提交。

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[GitHub issue](https://github.com/Quantisan/docker-clojure/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
