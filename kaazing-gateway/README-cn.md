# 简介

Kaazing Gateway 是一个开源的HTML 5 webSocket Server

![logo](https://raw.githubusercontent.com/docker-library/docs/master/kaazing-gateway/logo.png)

# 使用

## 启动和运行

默认情况下，Kaazing Gateway容器运行了一个简单的echo服务。就像[websocket.org](https://www.websocket.org/echo.html)

```console
$ docker run --name some-kaazing-gateway -h somehostname -d -p 8000:8000 kaazing-gateway
```

这样你就可以从[WebSocket echo test](https://www.websocket.org/echo.html)连接 ws://yourhostname:8000 

注意：`yourhostname` 应该是一个可以直接从外部访问你的主机的地址

## 自定义配置

使用指定的配置来启动容器:

```console
$ docker run --name some-kaazing-gateway -v /some/gateway-config.xml:/kaazing-gateway/conf/gateway-config.xml:ro -d kaazing-gateway
```

关于Kaazing Gateway配置文件的语法格式，请参阅[官方文档](http://developer.kaazing.com/documentation/5.0/index.html)

如果你想获取一个默认的配置文件，可以从一个运行的容器中copy:

```console
$ docker cp some-kaazing:/conf/gateway-config-minimal.xml /some/gateway-config.xml
```

使用`Dockerfile`衍生一个新的镜像:

```dockerfile
FROM kaazing-gateway
COPY gateway-config.xml /conf/gateway-config.xml
```

构建镜像`docker build -t some-custom-kaazing-gateway .`并运行:

```console
$ docker run --name some-kaazing-gateway -d some-custom-kaazing-gateway
```

# 授权

查看[授权](https://github.com/kaazing/gateway/blob/master/LICENSE.txt)
