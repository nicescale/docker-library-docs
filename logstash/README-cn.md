# 简介

Logstash 是一个搜集、处理、转发程序日志或事件的工具。搜集由一系列可配置的input插件完成，这些插件可以通过从Socket中读取，跟踪日志文件等方式进行搜集，数据搜集后由若干指定的filter插件进行过滤和处理并标记某种事件。最后事件被提交到一系列output插件，并被转发到外部的处理程序，包括Elasticsearch，本地文件或者其他消息处理程序。

> [wikitech.wikimedia.org/wiki/Logstash](https://wikitech.wikimedia.org/wiki/Logstash)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/logstash/logo.png)

# 使用

## 从命令行指定参数启动

如果想要从命令行上读取配置参数并启动logstash，按照如下方式运行：

```console
$ docker run -it --rm logstash logstash -e 'input { stdin { } } output { stdout { } }'
```

## 从配置文件指定参数启动

如果想要从文件`logstash.conf`中读取配置参数并启动，按照如下方式运行:

```console
$ docker run -it --rm -v "$PWD":/config-dir logstash logstash -f /config-dir/logstash.conf
```

# 授权

查看[授权](https://github.com/elastic/logstash/blob/master/LICENSE) 
