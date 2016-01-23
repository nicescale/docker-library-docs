# 支持的tag标签，以及相关`Dockerfile`链接

-	[`latest`, `1`, `1.9`, `1.9.7` (*Dockerfile*)](https://github.com/nginxinc/docker-nginx/blob/08eeb0e3f0a5ee40cbc2bc01f0004c2aa5b78c15/Dockerfile)

关于本镜像的更详细信息，请访问：[清单文件 (`library/nginx`)](https://github.com/docker-library/official-images/blob/master/library/nginx). 镜像更新依赖于 [`docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于 [ `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)的 [`nginx/tag-details.md` 文件](https://github.com/docker-library/docs/blob/master/nginx/tag-details.md) .

# 关于 Nginx?

Nginx（发音同engine x）是一款由俄罗斯程序员Igor Sysoev所开发轻量级的网页服务器、反向代理服务器以及电子邮件（IMAP/POP3）代理服务器。起初是供俄国大型的门户网站及搜索引擎Rambler（俄语：Рамблер）使用。此软件BSD-like协议下发行，可以在UNIX、GNU/Linux、BSD、Mac OS X、Solaris，以及Microsoft Windows等操作系统中运行。

> [wikipedia.org/wiki/Nginx](https://en.wikipedia.org/wiki/Nginx)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/nginx/logo.png)

# 镜像的使用

## 代理简单的静态内容

```console
$ docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx
```

或者, 也可以使用一个简单的 `ockerfile`生成一个新的镜像,包括必要的内容(这是一个比上面更简介明了的方式)：

```dockerfile
FROM nginx
COPY static-html-directory /usr/share/nginx/html
```

把静态文件放到同一目录下 ("static-html-directory"), 运行 `docker build -t some-content-nginx .`启动容器：

```console
$ docker run --name some-nginx -d some-content-nginx
```

## 暴露的端口

```console
$ docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

这个时候，你可以在浏览器中访问 `http://localhost:8080` 或者 `http://host-ip:8080`了。

## 更多配置

```console
$ docker run --name some-nginx -v /some/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx
```

需要了解更多nginx的配置信息，请查看 [官方文档](http://nginx.org/en/docs/) (尤其是[入门指南](http://nginx.org/en/docs/beginners_guide.html#conf_structure)).

在你的自定义nginx配置文件中务必包含 `daemon off;`，否则你的容器将在开始后立即停止!

如果你想改变的默认配置,可以使用类似下面的方式:

```console
$ docker cp some-nginx:/etc/nginx/nginx.conf /some/nginx.conf
```

和上文一样，以上也可以使用 `Dockerfile`的方式对其简化:

```dockerfile
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf
```

然后构建 `docker build -t some-custom-nginx .` 并运行:

```console
$ docker run --name some-nginx -d some-custom-nginx
```

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon


