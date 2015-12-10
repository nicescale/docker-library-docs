# 支持的镜像tag 和 `Dockerfile` 链接

-	[`0.7.0`, `0.7`, `0`, `latest` (*Dockerfile*)](https://github.com/RocketChat/Docker.Official.Image/blob/69f967fd139561a7eee0af140a8a07c28899731a/Dockerfile)

# Rocket.Chat

Rocket.Chat 是一款使用javascript, Meteor 全栈框架开发的 web 聊天服务器.

无论对于那些想要私自托管自己聊天服务的社区和企业, 还是寻找构建定制自己聊天平台的开发者, Rocket.Chat 都是一个很好的解决方案.

![logo](https://rawgit.com/docker-library/docs/master/rocket.chat/logo.svg)

# 怎么使用镜像

首先,启动一个mongo 实例:

	docker run --name db -d mongo --smallfiles

然后启动 Rocket.Chat 链接到这个 mongo 实例:

	docker run --name rocketchat --link db -d rocket.chat

这个命令会启动Rocket.Chat 实例, 监听在Meteor 默认端口3000 上.

如果你想要在主机上直接通过标准端口访问实例:

	docker run --name rocketchat -p 80:3000 --env ROOT_URL=http://localhost --link db -d rocket.chat

然后, 在浏览器里访问`http://localhost`.如果托管在自己的域名之下,使用自己的域名替换`ROOT_URL` 中的`localhost`.

如果你使用第三方Mongo 驱动, 或者搭配 Kubernetes 使用, 你需要覆盖`MONGO_URL` 环境变量:

	docker run --name rocketchat -p 80:3000 --env ROOT_URL=http://localhost --env MONGO_URL=mongodb://mymongourl/mydb -d rocket.chat

