# 支持的tag和其 `Dockerfile` 链接地址

-	[`4.2.5`, `4.2`, `4`, `latest` (*Dockerfile*)](https://github.com/docker-library/rails/blob/1d9e5d95eaff6030dd28be4de0b415cc415e0497/Dockerfile)
-	[`onbuild` (*onbuild/Dockerfile*)](https://github.com/docker-library/rails/blob/9fb5d2b7e0f2e7029855028e07e86ab7ec54abaa/onbuild/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/rails`)](https://github.com/docker-library/official-images/blob/master/library/rails). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# 什么是Ruby on Rails?

Ruby on Rails 或 Rails, 是一个运行在ruby上的开源web应用程序框架. 它是一个全栈框架. 这意味着"out of the box", Rails可以创建网页,从web服务器获取信息的应用程序,查询数据库, 渲染模板. 因此, Rails具有独立于web服务器的路由系统

> [wikipedia.org/wiki/Ruby_on_Rails](https://en.wikipedia.org/wiki/Ruby_on_Rails)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/rails/logo.png)

# 如何使用此镜像

## 在你的Rails项目下创建一个`Dockerfile`文件

```dockerfile
FROM rails:onbuild
```

将文件放在你的应用程序根目录下, 挨着 `Gemfile`.

这个镜像包括多个 `ONBUILD` 触发器 它应该覆盖了许多应用程序. 构建 `COPY . /usr/src/app`, `RUN bundle install`, `EXPOSE 3000`,并且设置默认命令 `rails server`.

然后你可以构建并运行这个docker镜像:

```console
$ docker build -t my-rails-app .
$ docker run --name some-rails-app -d my-rails-app
```

你可以通过浏览器访问`http://container-ip:3000`进行测试, 如果你需要访问外部主机, 8080端口上面的:

```console
$ docker run --name some-rails-app -p 8080:3000 -d my-rails-app
```
然后你可以在浏览器里打开`http://localhost:8080` 或者 `http://host-ip:8080`

### 生成`Gemfile.lock`

`onbuild`标签在你的应用文件夹下会得到一个`Gemfile.lock`文件, `docker run`将帮助你生成一个. 在你的程序目录下运行它, 挨着`Gemfile`

```console
$ docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app ruby:2.1 bundle install
```

## 引导一个新的Rails应用程序

如果你想生成一个新的Rails项目基架, 你可以按照下面做

```console
$ docker run -it --rm --user "$(id -u):$(id -g)" -v "$PWD":/usr/src/app -w /usr/src/app rails rails new --skip-bundle webapp
```
在你的当前文件夹下创建一个子文件夹`webapp`

# 镜像变体

`rails`的镜像有许多种,每个设计都是为了特定的使用案例

## `rails:<version>`

这是一个事实镜像.
This is the defacto image. 如果你不确定你需要什么, 你有可能会用到这个. 它被设计用于可同时扔掉容器(挂载你的源码并启动这个容器来启动你的应用程序),以及基础构建其他的镜像

## `rails:onbuild`

这个镜像更容易构建衍生镜像,对于大多数用例,在你的项目文件夹下创建一个带有`FROM rails:onbuild`的`Dockerfile`基础文件, 它将足够为你的项目创建一个独立的镜像。

当`onbuild`变体对于"getting off the ground running" (在很短的时间内从0到docker化)是真有用的, *当*`ONBUILD`触发的时候 不建议长期在项目里使用这种缺乏控制权的,(请查看 [`docker/docker#5714`](https://github.com/docker/docker/issues/5714), [`docker/docker#8240`](https://github.com/docker/docker/issues/8240), [`docker/docker#11917`](https://github.com/docker/docker/issues/11917)).

一旦你了解过在你的项目里面加入docker, 你可能调整你的`Dockerfile`继承自一个不是`onbuild`变种，并且从`onbuild`变种`Dockerfile`(移动`ONBUILD`这行到末尾并且删除`ONBUILD`)文件复制命令到你自己的文件,这样你自己会有更高的控制权和透明度并且别人看你的`Dockerfile`文件也知道它是做什么的. 随着时间的推移这也使得它更容易添加额外的需求(比如在执行`ONBUILD`之前安装更多的包)

# 许可证

查看此镜像所包含软件的[许可证信息](https://github.com/rails/rails#license).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`rails/` directory](https://github.com/docker-library/docs/tree/master/rails)下面的[`perl/` directory](https://github.com/docker-library/docs/tree/master/perl) .
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过[GitHub issue](https://github.com/docker-library/rails/issues).联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/Perl/docker-perl/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
