# 支持的镜像tag 和 `Dockerfile` 链接

-	[`2.0.0-p647`, `2.0.0`, `2.0` (*2.0/Dockerfile*)](https://github.com/docker-library/ruby/blob/74ee8aec9c17ea2134db8a8ef199cf092c829576/2.0/Dockerfile)
-	[`2.0.0-p647-onbuild`, `2.0.0-onbuild`, `2.0-onbuild` (*2.0/onbuild/Dockerfile*)](https://github.com/docker-library/ruby/blob/5d04363db6f7ae316ef7056063f020557db828e1/2.0/onbuild/Dockerfile)
-	[`2.0.0-p647-slim`, `2.0.0-slim`, `2.0-slim` (*2.0/slim/Dockerfile*)](https://github.com/docker-library/ruby/blob/74ee8aec9c17ea2134db8a8ef199cf092c829576/2.0/slim/Dockerfile)
-	[`2.1.7`, `2.1` (*2.1/Dockerfile*)](https://github.com/docker-library/ruby/blob/74ee8aec9c17ea2134db8a8ef199cf092c829576/2.1/Dockerfile)
-	[`2.1.7-onbuild`, `2.1-onbuild` (*2.1/onbuild/Dockerfile*)](https://github.com/docker-library/ruby/blob/5d04363db6f7ae316ef7056063f020557db828e1/2.1/onbuild/Dockerfile)
-	[`2.1.7-slim`, `2.1-slim` (*2.1/slim/Dockerfile*)](https://github.com/docker-library/ruby/blob/74ee8aec9c17ea2134db8a8ef199cf092c829576/2.1/slim/Dockerfile)
-	[`2.2.3`, `2.2`, `2`, `latest` (*2.2/Dockerfile*)](https://github.com/docker-library/ruby/blob/74ee8aec9c17ea2134db8a8ef199cf092c829576/2.2/Dockerfile)
-	[`2.2.3-onbuild`, `2.2-onbuild`, `2-onbuild`, `onbuild` (*2.2/onbuild/Dockerfile*)](https://github.com/docker-library/ruby/blob/5d04363db6f7ae316ef7056063f020557db828e1/2.2/onbuild/Dockerfile)
-	[`2.2.3-slim`, `2.2-slim`, `2-slim`, `slim` (*2.2/slim/Dockerfile*)](https://github.com/docker-library/ruby/blob/74ee8aec9c17ea2134db8a8ef199cf092c829576/2.2/slim/Dockerfile)

# 什么是ruby?

Ruby 是动态,反射,面向对象,通用,开源的编程语言.根据其作者了解,ruby 受到了Perl, Smalltalk, Eiffel, Ada, and Lisp 等语言影响.它支持多种编程模式,保括函数式,面向对象,命令式.它同样有动态类型系统和自动内存管理.

> [wikipedia.org/wiki/Ruby_(programming_language)](https://en.wikipedia.org/wiki/Ruby_%28programming_language%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/ruby/logo.png)

# 怎么使用镜像

## 在你的ruby应用项目下创建`Dockerfile`

```dockerfile
FROM ruby:2.1-onbuild
CMD ["./your-daemon-or-script.rb"]
```

将此文件放入你的ruby项目顶层目录下, 紧邻 `Gemfile`.

这个镜像包括多个 `ONBUILD` 指令, 它们应该能满足你引导大部分应用的需要.构建会执行以下两条指令`COPY . /usr/src/app`, `RUN  bundle install`.

构建运行 Ruby 镜像:

```console
$ docker build -t my-ruby-app .
$ docker run -it --name my-running-script my-ruby-app
```

### 创建一个 `Gemfile.lock`

`onbuild` 标记期待你的应用目录下存在`Gemfile.lock`. `docker run` 会辅助你生成一个.

```console
$ docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app ruby:2.1 bundle install
```

## 运行一个单ruby脚本

对于许多简单的单文件项目,你可能发现写一个完整 `Dockerfile`不太方便.这种情况下,可以使用docker 镜像直接运行ruby脚本:

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp ruby:2.1 ruby your-daemon-or-script.rb
```

# 镜像变体

ruby 镜像有多种版本,每一个都有其特定的使用场景.

## `ruby:<version>`

这是实际上的镜像.如果你不确定你需要什么,可能此版本就是你想要的.它被设计可用于随时抛弃的容器(挂载你的源码,立即启动你的应用),同样可以用在作为基础镜像使用.此标记基于[`buildpack-deps`](https://registry.hub.docker.com/_/buildpack-deps/).`buildpack-deps` 是为那些自己系统上有大量镜像的用户设计的.在设计上,它拥有大量的公共debian包.这会减少那些源自此镜像的镜像需要安装的软件包数目,如此减少了你系统上所有镜像的整体大小.

## `ruby:onbuild`

这个镜像版本使得构建派生镜像更容易.对于大部分使用场景,在你的项目顶层目录以`FROM ruby:onbuild`指令开始编写`Dockerfile`, 为你的项目构建单独的镜像.

## `ruby:slim`

这个镜像版本没有包含默认tag里常见的软件包,仅有运行`ruby`所需要的最小化安装包.除非你的环境只需要部署ruby镜像同时还有磁盘空间的约束,我们高度推荐使用本仓库的默认镜像.
