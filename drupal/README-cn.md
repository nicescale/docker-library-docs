# 支持的tag标签，以及相关`Dockerfile`链接

-	[`7.41`, `7` (*7/Dockerfile*)](https://github.com/docker-library/drupal/blob/9e1ff6c719c7a6dec88b1089f9128dab428c4eed/7/Dockerfile)
-	[`8.0.0`, `8.0`, `8`, `latest` (*8/Dockerfile*)](https://github.com/docker-library/drupal/blob/898b3178e7d1737347fe0602726b38c4bb300330/8/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/drupal`)](https://github.com/docker-library/official-images/blob/master/library/drupal)。本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

关于镜像每个layer以及上述每个tag的详细信息，请查看位于[the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[the `drupal/tag-details.md` file](https://github.com/docker-library/docs/blob/master/drupal/tag-details.md)。

# Drupal是什么？

Drupal是一个免费、开源的内容管理框架，使用PHP进行编写并基于GNU General Public License协议。它被用于全世界2.1% Web站点的后台框架，包括大量的个人博客以及政府网站，例如WhiteHouse.gov和data.gov.uk。 

> [wikipedia.org/wiki/Drupal](https://en.wikipedia.org/wiki/Drupal)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/drupal/logo.png)

# 如何使用本镜像

创建`drupal`实例的最基本方法是：

```console
$ docker run --name some-drupal -d drupal
```

如果你并不希望使用容器的IP地址进行访问，那么请进行如下映射：

```console
$ docker run --name some-drupal -p 8080:80 -d drupal
```

然后就可以在浏览器中通过`http://localhost:8080`或`http://host-ip:8080`来进行访问了。

本镜像支持多种数据库，在默认配置下SQLite可被用于防止另一个容器对文件进行覆写。有关更多其他数据库的使用须知，请参考接下来的内容。

当第一次访问本镜像提供的web服务器时，你可能需要首先完成一个简短的配置过程，本文档基于默认的配置设定。

## MySQL

```console
$ docker run --name some-drupal --link some-mysql:mysql -d drupal
```

-	Database type: `MySQL, MariaDB, or equivalent`
-	Database name/username/password: `<details for accessing your MySQL instance>` (`MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DATABASE`; 关于环境变量的设置请参考：[`mysql`](https://registry.hub.docker.com/_/mysql/))。
-	ADVANCED OPTIONS; Database host: `mysql` （使用由`--link`选项增加的`/etc/hosts`入口来访问其他容器上的MySQL实例）。

## PostgreSQL

```console
$ docker run --name some-drupal --link some-postgres:postgres -d drupal
```

-	Database type: `PostgreSQL`
-	Database name/username/password: `<details for accessing your PostgreSQL instance>` (`POSTGRES_USER`, `POSTGRES_PASSWORD`;关于环境变量的设置请参考：[`postgres`](https://registry.hub.docker.com/_/postgres/))。
-	ADVANCED OPTIONS; Database host: `postgres`（使用由`--link`选项增加的`/etc/hosts`入口来访问其他容器上的PostgreSQL实例）。

## 使用额外的库和拓展插件

本镜像并没有提供任何其他的拓展库，即使它们被最流行的插件所依赖，这保证了镜像体积的小巧。

如果你需要额外的PHP拓展功能，那么需要创建自己的镜像，并使用`FROM`来将本镜像作为基础镜像。[documentation of the `php` image](https://github.com/docker-library/docs/blob/master/php/README.md#how-to-install-more-php-extensions)描述了如何完成这一过程，你也可以参考如下示例：[`drupal:7` Dockerfile](https://github.com/docker-library/drupal/blob/bee08efba505b740a14d68254d6e51af7ab2f3ea/7/Dockerfile#L6-9)。

下面的Docker Hub让你的镜像永远保持最新的时效性：

-	[Automated Builds](https://docs.docker.com/docker-hub/builds/) 让Docker Hub在你每次push之后自动地进行镜像的构建。
-	[Repository Links](https://docs.docker.com/docker-hub/builds/#repository-links) 则保证了在`drupal`更新后重新构建你的镜像。

# 证书信息

有关本镜像所包含软件的证书信息，可以参见：[license information](https://www.drupal.org/licensing/faq)。

# 支持的Docker版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`drupal/` directory](https://github.com/docker-library/docs/tree/master/drupal)目录中。在提交新的pull request之前，请确保已经熟悉于[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/docker-library/drupal/issues)来提交。

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[GitHub issue](https://github.com/docker-library/drupal/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
