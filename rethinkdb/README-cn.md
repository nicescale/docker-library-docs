# 支持的镜像tag 和 `Dockerfile` 链接

-	[`1.15.1` (*jessie/1.15.1/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/1.15.1/Dockerfile)
-	[`1.15.2` (*jessie/1.15.2/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/1.15.2/Dockerfile)
-	[`1.15.3`, `1.15` (*jessie/1.15.3/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/1.15.3/Dockerfile)
-	[`1.16.0` (*jessie/1.16.0/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/1.16.0/Dockerfile)
-	[`1.16.1` (*jessie/1.16.1/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/1.16.1/Dockerfile)
-	[`1.16.2` (*jessie/1.16.2/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/1.16.2/Dockerfile)
-	[`1.16.3`, `1.16`, `1` (*jessie/1.16.3/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/1.16.3/Dockerfile)
-	[`2.0.0` (*jessie/2.0.0/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.0.0/Dockerfile)
-	[`2.0.1` (*jessie/2.0.1/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.0.1/Dockerfile)
-	[`2.0.2` (*jessie/2.0.2/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.0.2/Dockerfile)
-	[`2.0.3` (*jessie/2.0.3/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.0.3/Dockerfile)
-	[`2.0.4`, `2.0` (*jessie/2.0.4/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.0.4/Dockerfile)
-	[`2.1.0` (*jessie/2.1.0/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.1.0/Dockerfile)
-	[`2.1.1` (*jessie/2.1.1/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.1.1/Dockerfile)
-	[`2.1.2` (*jessie/2.1.2/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.1.2/Dockerfile)
-	[`2.1.3` (*jessie/2.1.3/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.1.3/Dockerfile)
-	[`2.1.4` (*jessie/2.1.4/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.1.4/Dockerfile)
-	[`2.1.5`, `2.1` (*jessie/2.1.5/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.1.5/Dockerfile)
-	[`2.2.0` (*jessie/2.2.0/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.2.0/Dockerfile)
-	[`2.2.1`, `2.2`, `2`, `latest` (*jessie/2.2.1/Dockerfile*)](https://github.com/rethinkdb/rethinkdb-dockerfiles/blob/7388cf6dd7db74566e28f8153ba992ccaef95963/jessie/2.2.1/Dockerfile)


# RethinkDB是什么?

RethinkDB 是一个开源分布式数据库,用来存储json 文档, 轻松伸缩到多台机器. 易于安装使用, 内置简单强大的查询语言,支持表合并, 分组, 聚集, 函数等.

![logo](https://raw.githubusercontent.com/docker-library/docs/master/rethinkdb/logo.png)

# 镜像使用

## 启动一个数据挂载到工作目录的实例

镜像的默认 CMD 指令是 `rethinkdb --bind all`, 如此 RethinkDB 守护进程会绑定容器可用的所有网络接口(默认,它只接受来自 `localhost` 的连接).

```bash
docker run --name some-rethink -v "$PWD:/data" -d rethinkdb
```

## 使用应用容器链接rethinkdb 实例

```bash
docker run --name some-app --link some-rethink:rdb -d application-that-uses-rdb
```

## 访问rethinkdb 的web 管理控制台

```bash
$BROWSER "http://$(docker inspect --format \
  '{{ .NetworkSettings.IPAddress }}' some-rethink):8080"
```

# 使用ssh 连接远程主机上的web 管理控制台

Where `remote` is an alias for the remote user@hostname:

```bash
# 启动端口转发
ssh -fNTL localhost:8080:$(ssh remote "docker inspect --format \
  '{{ .NetworkSettings.IPAddress }}' some-rethink"):8080 remote

# 使用本地默认浏览器打开本地 8080端口
xdg-open http://localhost:8080

# 停止端口转发
kill $(lsof -t -i @localhost:8080 -sTCP:listen)
```

## 配置

查看 [官方文档](http://www.rethinkdb.com/docs/) 信息, 了解使用配置rethinkdb 集群.
