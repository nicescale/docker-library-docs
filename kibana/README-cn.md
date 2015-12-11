# 简介

Kibana是一个为ElasticSearch提供日志数据分析可视化的开源的插件。它为在Elasticsearch集群中索引的文档提供可视化分析，用户可根据分析需求在Kibana上创建各种图表甚至生成地图。

> [wikipedia.org/wiki/Kibana](https://en.wikipedia.org/wiki/Kibana)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/kibana/logo.png)

# 使用

直接启动一个默认的`kibana`实例:

```console
$ docker run --link some-elasticsearch:elasticsearch -d kibana
```

传递启动参数:

```console
$ docker run --link some-elasticsearch:elasticsearch -d kibana --plugins /somewhere/else
```

此镜像包含指令`EXPOSE 5601`([默认`port`](https://www.elastic.co/guide/en/kibana/current/_setting_kibana_server_properties.html)),如果你想从主机IP访问kibina实例，请使用docker的端口映射:

```console
$ docker run --name some-kibana --link some-elasticsearch:elasticsearch -p 5601:5601 -d kibana
```

还可以设置环境变量`ELASTICSEARCH_URL`来指定elasticsearch主机的地址

```console
$ docker run --name some-kibana -e ELASTICSEARCH_URL=http://some-elasticsearch:9200 -p 5601:5601 -d kibana
```

现在，你可以使用浏览器直接访问 `http://localhost:5601` 或 `http://host-ip:5601`

# 授权

查看[授权](https://github.com/elastic/kibana/blob/4557a6fc0ba08c5e7ac813a180179e5e2631c90a/LICENSE.md)
