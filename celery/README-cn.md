# 支持的tag标签，以及相关`Dockerfile`链接

-	[`3.1.19`, `3.1`, `3`, `latest` (*Dockerfile*)](https://github.com/docker-library/celery/blob/e2bc6ae91c1b584fd6c2b65976e0ccb42c17db4e/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/celery`)](https://github.com/docker-library/official-images/blob/master/library/celery)。 本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

关于镜像每个layer以及上述每个tag的详细信息，请查看：[the `celery/tag-details.md` file](https://github.com/docker-library/docs/blob/master/celery/tag-details.md)以及[the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)。

# Celery

Celery是一个基于消息分发机制的开源异步任务队列，主要关注实时操作，并且支持调度功能。

> [wikipedia.org/wiki/Celery_Task_Queue](https://en.wikipedia.org/wiki/Celery_Task_Queue)

# 如何使用本镜像

## 启动一个celery任务(RabbitMQ Broker)

```console
$ docker run --link some-rabbit:rabbit --name some-celery -d celery
```

### 查看集群的状态

```console
$ docker run --link some-rabbit:rabbit --rm celery celery status
```

## 启动一个celery任务(Redis Broker)

```console
$ docker run --link some-redis:redis -e CELERY_BROKER_URL=redis://redis --name some-celery -d celery
```

### 查看集群的状态

```console
$ docker run --link some-redis:redis -e CELERY_BROKER_URL=redis://redis --rm celery celery status
```

# 支持的Docker版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`celery/` directory](https://github.com/docker-library/docs/tree/master/celery)目录中。在提交新的pull request之前，请确保已经熟悉于[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/docker-library/celery/issues)联系我们。

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[GitHub issue](https://github.com/docker-library/celery/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
