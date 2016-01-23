# 支持的tag标签，以及相关`Dockerfile`链接

-	[`0.6.8`, `latest` (*Dockerfile*)](https://github.com/nats-io/nats-docker/blob/09e3ad91cb1716629a24a731fc618028d995c9ea/Dockerfile)

关于本镜像的更详细信息，请访问： [清单文件 (`library/nats`)](https://github.com/docker-library/official-images/blob/master/library/nats). 镜像更新依赖于 [the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs) 的 [ `nats/tag-details.md` ](https://github.com/docker-library/docs/blob/master/nats/tag-details.md) 文件。

# [NATS](https://nats.io): 高性能云计算本地消息系统

![logo](https://raw.githubusercontent.com/docker-library/docs/master/nats/logo.png)

`nats` 是一个 NATS 消息系统的高性能服务器.

# 使用示例

```bash
# Run a NATS server
# Each server exposes multiple ports
# 4222 is for clients.
# 8222 is an HTTP management port for information reporting.
# 6222 is a routing port for clustering.
# use -p or -P as needed.

$ docker run -d --name nats-main nats
[INF] Starting gnatsd version 0.6.6
[INF] Starting http monitor on port 8222
[INF] Listening for route connections on 0.0.0.0:6222
[INF] Listening for client connections on 0.0.0.0:4222
[INF] gnatsd is ready

...

# To run a second server and cluster them together..
$ docker run -d --name=nats-2 --link nats-main nats --routes=nats-route://ruser:T0pS3cr3t@nats-main:6222

# If you want to verify the routes are connected, try
$ docker run -d --name=nats-2 --link nats-main nats --routes=nats-route://ruser:T0pS3cr3t@nats-main:6222 -DV
[INF] Starting gnatsd version 0.6.6
[INF] Starting http monitor on port 8222
[INF] Listening for route connections on :6222
[INF] Listening for client connections on 0.0.0.0:4222
[INF] gnatsd is ready
[DBG] Trying to connect to route on nats-main:6222
[DBG] 172.17.0.52:6222 - rid:1 - Route connection created
[DBG] 172.17.0.52:6222 - rid:1 - Route connect msg sent
[DBG] 172.17.0.52:6222 - rid:1 - Registering remote route "ee35d227433a738c729f9422a59667bb"
[DBG] 172.17.0.52:6222 - rid:1 - Route sent local subscriptions
```

下面的服务器将加载配置文件。所有命令行标志都可以覆盖这些默认值.

## 默认配置文件

```bash
# Client port of 4222 on all interfaces
port: 4222

# HTTP monitoring port
monitor_port: 8222

# This is for clustering multiple servers together.
cluster {

  # Route connections to be received on any interface on port 6222
  port: 6222

  # Routes are protected, so need to use them with --routes flag
  # e.g. --routes=nats-route://ruser:T0pS3cr3t@otherdockerhost:6222
  authorization {
    user: ruser
    password: T0pS3cr3t
    timeout: 0.75
  }

  # Routes are actively solicited and connected to from this server.
  # This Docker image has none by default, but you can pass a
  # flag to the gnatsd docker image to create one to an existing server.
  routes = []
}
```

## 命令行参数

```bash
Server Options:
    -a, --addr HOST                  Bind to HOST address (default: 0.0.0.0)
    -p, --port PORT                  Use PORT for clients (default: 4222)
    -P, --pid FILE                   File to store PID
    -m, --http_port PORT             Use HTTP PORT for monitoring
    -c, --config FILE                Configuration File

Logging Options:
    -l, --log FILE                   File to redirect log output
    -T, --logtime                    Timestamp log entries (default: true)
    -s, --syslog                     Enable syslog as log method.
    -r, --remote_syslog              Syslog server addr (udp://localhost:514).
    -D, --debug                      Enable debugging output
    -V, --trace                      Trace the raw protocol
    -DV                              Debug and Trace

Authorization Options:
        --user user                  User required for connections
        --pass password              Password required for connections

Cluster Options:
        --routes [rurl-1, rurl-2]    Routes to solicit and connect

Common Options:
    -h, --help                       Show this message
    -v, --version                    Show version
```

# 许可证

详见 [许可信息](https://github.com/nats-io/gnatsd/blob/master/LICENSE) 了解更多软件授权.

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon

