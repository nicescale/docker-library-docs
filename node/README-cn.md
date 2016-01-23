# 支持的tag标签，以及相关`Dockerfile`链接

-	[`0.10.40`, `0.10` (*0.10/Dockerfile*)](https://github.com/nodejs/docker-node/blob/d798690bdae91174715ac083e31198674f044b68/0.10/Dockerfile)
-	[`0.10.40-onbuild`, `0.10-onbuild` (*0.10/onbuild/Dockerfile*)](https://github.com/nodejs/docker-node/blob/9c93908dfcdc140c14aa78cbb4830850bcf99012/0.10/onbuild/Dockerfile)
-	[`0.10.40-slim`, `0.10-slim` (*0.10/slim/Dockerfile*)](https://github.com/nodejs/docker-node/blob/d798690bdae91174715ac083e31198674f044b68/0.10/slim/Dockerfile)
-	[`0.10.40-wheezy`, `0.10-wheezy` (*0.10/wheezy/Dockerfile*)](https://github.com/nodejs/docker-node/blob/d798690bdae91174715ac083e31198674f044b68/0.10/wheezy/Dockerfile)
-	[`0.12.8`, `0.12`, `0` (*0.12/Dockerfile*)](https://github.com/nodejs/docker-node/blob/e3ebe31f6cd0d61d2756c08e232d3583e2543079/0.12/Dockerfile)
-	[`0.12.8-onbuild`, `0.12-onbuild`, `0-onbuild` (*0.12/onbuild/Dockerfile*)](https://github.com/nodejs/docker-node/blob/79a29e6a4cb63f6e11824e21123e280a7345990f/0.12/onbuild/Dockerfile)
-	[`0.12.8-slim`, `0.12-slim`, `0-slim` (*0.12/slim/Dockerfile*)](https://github.com/nodejs/docker-node/blob/e3ebe31f6cd0d61d2756c08e232d3583e2543079/0.12/slim/Dockerfile)
-	[`0.12.8-wheezy`, `0.12-wheezy`, `0-wheezy` (*0.12/wheezy/Dockerfile*)](https://github.com/nodejs/docker-node/blob/e3ebe31f6cd0d61d2756c08e232d3583e2543079/0.12/wheezy/Dockerfile)
-	[`4.2.2`, `4.2`, `4`, `argon` (*4.2/Dockerfile*)](https://github.com/nodejs/docker-node/blob/9992908b275546d9dc1b6063a4d0b7bc500e8b3b/4.2/Dockerfile)
-	[`4.2.2-onbuild`, `4.2-onbuild`, `4-onbuild`, `argon-onbuild` (*4.2/onbuild/Dockerfile*)](https://github.com/nodejs/docker-node/blob/9992908b275546d9dc1b6063a4d0b7bc500e8b3b/4.2/onbuild/Dockerfile)
-	[`4.2.2-slim`, `4.2-slim`, `4-slim`, `argon-slim` (*4.2/slim/Dockerfile*)](https://github.com/nodejs/docker-node/blob/9992908b275546d9dc1b6063a4d0b7bc500e8b3b/4.2/slim/Dockerfile)
-	[`4.2.2-wheezy`, `4.2-wheezy`, `4-wheezy`, `argon-wheezy` (*4.2/wheezy/Dockerfile*)](https://github.com/nodejs/docker-node/blob/9992908b275546d9dc1b6063a4d0b7bc500e8b3b/4.2/wheezy/Dockerfile)
-	[`5.1.0`, `5.1`, `5`, `latest` (*5.1/Dockerfile*)](https://github.com/nodejs/docker-node/blob/3622304ad09a84826521e81cb7ddf253c020d89e/5.1/Dockerfile)
-	[`5.1.0-onbuild`, `5.1-onbuild`, `5-onbuild`, `onbuild` (*5.1/onbuild/Dockerfile*)](https://github.com/nodejs/docker-node/blob/3622304ad09a84826521e81cb7ddf253c020d89e/5.1/onbuild/Dockerfile)
-	[`5.1.0-slim`, `5.1-slim`, `5-slim`, `slim` (*5.1/slim/Dockerfile*)](https://github.com/nodejs/docker-node/blob/3622304ad09a84826521e81cb7ddf253c020d89e/5.1/slim/Dockerfile)
-	[`5.1.0-wheezy`, `5.1-wheezy`, `5-wheezy`, `wheezy` (*5.1/wheezy/Dockerfile*)](https://github.com/nodejs/docker-node/blob/3622304ad09a84826521e81cb7ddf253c020d89e/5.1/wheezy/Dockerfile)

关于本镜像的更详细信息，请访问：[清单文件 (`library/node`)](https://github.com/docker-library/official-images/blob/master/library/node). 镜像更新依赖于[`docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)的[`node/tag-details.md` ](https://github.com/docker-library/docs/blob/master/node/tag-details.md)文件.

# 关于 Node.js?

Node.js是一个开放源代码、跨平台的、可用于服务器端和网络应用的运行环境。Node.js应用JavaScript语言写成，在Node.js运行时运行。它支持OS X、Microsoft Windows、Linux、FreeBSD、NonStop、IBM AIX、IBM System z和IBM i。Node.js由Node.js基金会拥有和维护[2]，该基金会与Linux基金会有合作关系。

Node.js提供事件驱动和非阻塞I/O API，可优化应用程序的吞吐量和规模。这些技术通常被用于实时应用程序。

Node.js采用Google的V8引擎来执行代码。Node.js的大部分基本模块都是用JavaScript写成的。Node.js含有一系列内置模块，使得程序可以作为独立服务器运行，从而脱离Apache HTTP Server或IIS运行。

> [wikipedia.org/wiki/Node.js](https://en.wikipedia.org/wiki/Node.js)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/node/logo.png)

# 镜像的使用

## 为你的app创建一个 `Dockerfile`

```dockerfile
FROM node:4-onbuild
# replace this with your application's default port
EXPOSE 8888
```

你可以这样构建和运行你的镜像:

```console
$ docker build -t my-nodejs-app .
$ docker run -it --rm --name my-running-app my-nodejs-app
```

### 注意

The image assumes that your application has a file named [`package.json`](https://docs.npmjs.com/files/package.json) listing its dependencies and defining its [start script](https://docs.npmjs.com/misc/scripts#default-values).

## 运行简单的 Node.js 脚本

对于许多简单的、单文件的项目,您可能会发现它不方便写一个完整的Dockerfile `Dockerfile`. 对于这种情况，你可以直接使用Docker镜像运行Node.js 脚本:

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/app -w /usr/src/app node:4 node your-daemon-or-script.js
```

# 镜像变种

`node` 镜像有很多种，适用于不同场景。

## `node:<version>`

这是基本的镜像。如果你不确定你应该使用那种，可以使用这种。 它设计成适用于一次性的构建容器(挂载源码，并启动容器运行你的程序), 同样适于构建其他的基础镜像. 该标签基于 [`buildpack-deps`](https://registry.hub.docker.com/_/buildpack-deps/). `buildpack-deps` 旨在包含多数用户系统上的镜像的包。它包含了大量的常用Debian包. 这样减少了需要安装包的数量, 从而减少了主机上镜像总的大小。

## `node:onbuild`

该镜像使得构建更加容易. 通常, 在你的项目目录下创建一个 `Dockerfile`文件，使用 `FROM node:onbuild`就可以为你的项目创建一个独立的镜像了。

`onbuild` 版本是十分有用的起步应用(零基础上手), 不建议在项目中长期使用，因为`ONBUILD`引发fire (参考 [`docker/docker#5714`](https://github.com/docker/docker/issues/5714), [`docker/docker#8240`](https://github.com/docker/docker/issues/8240), [`docker/docker#11917`](https://github.com/docker/docker/issues/11917))时缺少控制。

当你了解了Docker中运行你的程序, 你可能希望使用一个不待`onbuild` 的 `Dockerfile`，从`onbuild` 版本中复制出命令到新的 `Dockerfile` (移除 `ONBUILD` 行中的 `ONBUILD` 关键字) ，如此可以更严格的控制和查看 `Dockerfile`做了什么。 随着时间的推移，这也使得它更容易添加额外的需求 (例如在`ONBUILD` 之前安装更多包).

## `node:slim`

该镜像默认不包含常用的包，只包含运行`node`所需要的最少的包，除非你仅仅需要node的运行环境或有空间限制，否则我们推荐使用默认的镜像。

# 许可证

查看 [许可信息](https://github.com/joyent/node/blob/master/LICENSE) 了解更多软件镜像授权。

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon

