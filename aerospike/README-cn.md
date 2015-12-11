# Aerospike

Aerospike 是一个开源的分布式数据库。基于 "shared nothing" 的架构设计使得 Aerospike 可以支持 TB 级别的可靠存储，并支持自动容错，复制和跨数据中心同步。

![logo](https://raw.githubusercontent.com/docker-library/docs/master/aerospike/logo.png)

Aerospike 的文档: [http://aerospike.com/docs](https://www.aerospike.com/docs).

# 如何使用此镜像

下面的命令将运行 Aerospike 的 `asd` 程序，并将它的监听端口映射到主机上。

```console
$ docker run -d --name aerospike -p 3000:3000 -p 3001:3001 -p 3002:3002 -p 3003:3003 aerospike/aerospike-server
```

**注意** 虽然通过这个最简单的命令就能将 Aerospike 运行起来，但是我们并不推荐这么使用。更合理地运行 Aerospike 容器，请设置**自定义参数**，并设置 **access-address** 参数的值。

# 高级设置

## 自定义参数

`asd` 程序默认会使用从 Dockerfile 添加至 `/etc/aerospike/aerospike.conf` 的配置文件。如果需要使用别的配置文件，将通过 `-v` 参数将该配置文件映射到容器：

	-v <DIRECTORY>:/opt/aerospike/etc

`<DIRECTORY>` 目录对应的是你的 Aerospike 配置文件所在目录。同时，还需要通过参数 `--config-file` 让 `asd` 程序从 `/opt/aerospike/etc` 目录加载配置文件:


	--config-file /opt/aerospike/etc/aerospike.conf

通过这两个配置，`asd` 将会使用从由 `<DIRECTORY>/aerospike.conf` 映射到容器的配置文件 `/opt/aerospike/etc/aerospile.conf` 读取配置

完整例子:

```console
$ docker run -d -v <DIRECTORY>:/opt/aerospike/etc --name aerospike -p 3000:3000 -p 3001:3001 -p 3002:3002 -p 3003:3003 aerospike/aerospike-server asd --foreground --config-file /opt/aerospike/etc/aerospike.conf
```

## access-address 配置

为了让 Aerospike 可以将自已的访问地址广播给集群或者应用，需要设置好 **access-address** 参数。没有设置这个参数的情况下，Aerospike 将会直接使用容器的IP地址，导致无法从其它节点访问。


在 aerospike.conf 中设置 **access-address** 参数:

	network {
	    service {
	        address any                  # 监听的IP地址
	        port 3000                    # 监听端口
	        access-address 192.168.1.100 # 可以由其它的应用和节点访问的IP地址
	    }
	    ...

## 数据持久化

在 Docker 环境下，容器中的数据文件不是持久化的。为了持久化数据，需要把数据目录从主机映射到容器的 `opt/aerospike/data` 目录下:

	-v <DIRECTORY>:/opt/aerospike/data

其中， `<DIRECTORY>` 是主机上用于存放持久数据的目录

完整示例:

```console
$ docker run -d -v <DIRECTORY>:/opt/aerospike/data --name aerospike -p 3000:3000 -p 3001:3001 -p 3002:3002 -p 3003:3003 aerospike/aerospike-server
```

## 集群设置

Aerospike 推荐使用基于 Mesh 的集群配置。Messh 使用 TCP 点对点的心跳连接。集群中的每一个节点都会维持一个到其它节点的心跳连接。详细文档: [http://www.aerospike.com/docs/operations/configure/network/heartbeat/#mesh-unicast-heartbeat](http://www.aerospike.com/docs/operations/configure/network/heartbeat/#mesh-unicast-heartbeat)

### Mesh 集群

Mesh 网络需要设置群集中每个节点互相之间的连接，可以通过以下两个方式实现:

1.	为集群中的每一个节点定义一个配置，详见: [Network Heartbeat Configuration](http://www.aerospike.com/docs/operations/configure/network/heartbeat/#mesh-unicast-heartbeat).
2.	使用 `asinfo` 程序来发送 `tip` 命令，来实现节点之间互相可见, 详见: [tip command in asinfo](http://www.aerospike.com/docs/tools/asinfo/#tip).
