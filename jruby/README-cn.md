# 简介

JRuby是由Java实现的基于JVM运行的Ruby解释器

Ruby是一种动态的、面向对象的、通用的开源编程语言。据作者介绍，Ruby受Perl, Smalltalk, Eiffel, Ada, Lisp等编程语言的影响。它支持多种编程范式，包括函数式编程，面向对象编程，指令式编程。同时它也有动态类型和内存管理特性。

JRuby利用JVM的鲁棒性和性能速度为大家提供了Ruby的解释器，使用JRuby你可以使用真正的多线程，增强的垃圾回收，甚至可以导入Java的现有库。

> [wikipedia.org/wiki/Ruby_(programming_language)](https://en.wikipedia.org/wiki/Ruby_%28programming_language%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/jruby/logo.png)

# 使用

## 在你的Ruby项目代码中创建`Dockerfile`

```dockerfile
FROM jruby:1.7-onbuild
CMD ["./your-daemon-or-script.rb"]
```

此镜像包含了多个 `ONBUILD` ，其中有 `COPY . /usr/src/app` 和 `RUN bundle install`

构建镜像并运行:

```console
$ docker build -t my-ruby-app .
$ docker run -it --name my-running-script my-ruby-app
```

### 生成`Gemfile.lock`

此镜像的 `onbuild` 会在当前目录下寻找 `Gemfile.lock` 文件. 也可以使用如下 `docker run` 创建一个`Gemfile.lock`文件

```console
$ docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app jruby:1.7 bundle install --system
```

## 运行单个的Ruby脚本

对许多单个Ruby脚本文件的简单的项目而言，单独再写一个`Dockerfile`文件并不是很方便，这种情况下，你可以直接使用如下`docker run`命令

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp jruby:1.7 jruby your-daemon-or-script.rb
```

# 授权

查看[授权](https://github.com/jruby/jruby/blob/master/COPYING)
