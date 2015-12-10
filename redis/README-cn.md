## redis 2.6.17, 2.8.23, 3.0.5  Dockerfile

### 启动redis 实例

```console
$ docker run --name some-redis -d redis
```

### 以持久存储方式启动redis 实例

```console
$ docker run --name some-redis -d redis redis-server --appendonly yes
```

如果以持久方式启动redis, 数据放在 `VOLUME /data`, 可以使用数据卷容器方式 `--volumes-from some-volume-container` 或挂载主机目录映射方式 `-v /docker/host/dir:/data` ([docs.docker volumes](http://docs.docker.com/userguide/dockervolumes/)).


### 从其他应用访问redis 实例

```console
$ docker run --name some-app --link some-redis:redis -d application-that-uses-redis
```

### 或者使用镜像内置的redis 命令行工具 `redis-cli`

```console
$ docker run -it --link some-redis:redis --rm redis sh -c 'exec redis-cli -h "$REDIS_PORT_6379_TCP_ADDR" -p "$REDIS_PORT_6379_TCP_PORT"'
```

### 定义自己的redis配置文件

以此Dockerfile 为基础镜像, 添加自己的redis.conf , 构建新Dockerfile

```dockerfile
FROM redis
COPY redis.conf /usr/local/etc/redis/redis.conf
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]
```

通过把主机目录所在的redis.conf 映射到容器内部某个路径

```console
$ docker run -v /myredis/conf/redis.conf:/usr/local/etc/redis/redis.conf --name myredis redis redis-server /usr/local/etc/redis/redis.conf
```

### `32bit` 版本

32bit 版本是使用make 32bit 编译出来的redis, 优势是基础数据类型暂用内存更少,缺点是存在4G 地址空间限制.["Using 32 bit instances"](http://redis.io/topics/memory-optimization#using-32-bit-instances) 
