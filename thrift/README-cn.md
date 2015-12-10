# 支持的镜像tag 和 `Dockerfile` 链接

-	[`0.9`, `0.9.2`, `latest` (*0.9/Dockerfile*)](https://github.com/ahawkins/docker-thrift/blob/61c3478ab828d3e610f192b442ac2a7221749c47/0.9/Dockerfile)

# 什么是Thrift

> Apache Thrift软件框架,针对伸缩性地跨语言服务部署,组合软件堆栈和代码生成引擎,能在C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, OCaml, Delphi及其他语言之间高效无缝构建服务.

Read more about [Thrift](https://thrift.apache.org).

# 怎么使用镜像

这个镜像是作为可执行程序来运行.通过挂载目录提供文件.这里是一个在当前目录把`service.thrift`编译成 ruby 的例子.

```console
$ docker run -v "$PWD:/data" thrift thrift -o /data --gen rb /data/service.thrift
```

注意, 你可能想引入 `-u $(id -u)`,在生成文件上设置UID.thrift 进程默认以root用户运行,会生成root拥有的文件.

