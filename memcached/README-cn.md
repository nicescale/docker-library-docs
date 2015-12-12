# 支持的tag标签，以及相关`Dockerfile`链接

-	[`1.4.25`, `1.4`, `1`, `latest` (*Dockerfile*)](https://github.com/docker-library/memcached/blob/1944b045661a2fabcf2b9677f7edaa07e04f8d8d/Dockerfile)

关于本镜像的更详细信息，请访问：[清单文件 (`library/memcached`)](https://github.com/docker-library/official-images/blob/master/library/memcached). 镜像更新依赖于 [the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)中的[`memcached/tag-details.md`](https://github.com/docker-library/docs/blob/master/memcached/tag-details.md)文件.

# 关于 Memcached

Memcached memcached是一套分布式的高速缓存系统，它常被作为缓存用来加速动态数据库驱动网站，通过加载到内存中的对象来减少外部数据源的读取次数。

memcached的API使用三十二比特的循环冗余校验（CRC-32）计算键值后，将数据分散在不同的机器上。当表格满了以后，接下来新增的数据会以LRU机制替换掉。由于memcached通常只是当作缓存系统使用，所以使用memcached的应用程序在写回较慢的系统时（像是后端的数据库）需要额外的代码更新memcached内的数据。

> [wikipedia.org/wiki/Memcached](https://en.wikipedia.org/wiki/Memcached)

# 镜像的使用

```console
$ docker run --name my-memcache -d memcached
```

上面的命令启动了memcached容器,然后你可以你的应用程序上通过与标准链接连接

```console
$ docker run --link my-memcache:memcache -d my-app-image
```

memcached 服务信息可以通过ENV变量或`/etc/hosts`中的`memcache`获得。

设置 memcached 的内存配额

```console
$ docker run --name my-memcache -d memcached memcached -m 64
```

这将允许 memcache 使用64兆的内存空间。

更多的meecache配置信息请参考 [wiki](https://code.google.com/p/memcached/wiki/NewStart).

# 许可证

查看 [许可信息](https://github.com/memcached/memcached/blob/master/LICENSE)了解更多镜像的授权。

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon


