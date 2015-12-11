# 什么是 `buildpack-deps`?

`buildpack-deps` 类似 [Heroku 的 stack images](https://github.com/heroku/stack-images/blob/master/bin/cedar.sh). 它包含了各种包管理，比如 Rugy Gems, PyPI 等需要的头文件和开发包。 比如，`buildpack-deps` 可以在任意应用目录下运行 `bundle install` 而无需提前处理构建模板时依赖的 `ssl.sh` 文件。

![logo](https://raw.githubusercontent.com/docker-library/docs/master/buildpack-deps/logo.png)

# 如何使用此镜像

这个镜像是作为其它语言的基础基础镜像而设计的。

## 包含了哪些内容

这个镜像的大部分 tag 使用了 battries-included 的方式实现。这样使得类似 `gem install` / `npm install` / `pip install` 等命令可以成功执行无需安装额外的头文件或者开发包。

有些语言则没有这种需求, 比如 Go 或者 Java, 这种链接外部 C 库的需求则很少见，那么他们的镜像包含的内容就会少一些。

### `curl`

这个 tag 的镜像只包含了 `curl`, `wget`, 和 `ca-certificates`包。这种的镜像适合 Java JRE 的环境：经常下载 JAR 文件，而很少从代码仓库下载文件。

### `scm`

这个 tag 的镜像基于 `curl` 镜像，另外还包含了源码管理的工具。目前，包含了`bzr`，`git`, `hg` 和 `svn`。这个镜像适合像 Java JDK 这样的环境: 既经常需要下载 JAR ，也会有 checkout 代码的需求。
