# 支持的tag标签，以及相关`Dockerfile`链接

-	[`1.8.7-python2`, `1.8-python2`, `1-python2`, `python2` (*2.7/Dockerfile*)](https://github.com/docker-library/django/blob/55ab84ddbf16db89defb54925a0ebcab8b8f4209/2.7/Dockerfile)
-	[`python2-onbuild` (*2.7/onbuild/Dockerfile*)](https://github.com/docker-library/django/blob/cecbb2bbbcb69d1b8358398eaf8d9638e3bdd447/2.7/onbuild/Dockerfile)
-	[`1.8.7-python3`, `1.8.7`, `1.8-python3`, `1.8`, `1-python3`, `1`, `python3`, `latest` (*3.4/Dockerfile*)](https://github.com/docker-library/django/blob/55ab84ddbf16db89defb54925a0ebcab8b8f4209/3.4/Dockerfile)
-	[`python3-onbuild`, `onbuild` (*3.4/onbuild/Dockerfile*)](https://github.com/docker-library/django/blob/cecbb2bbbcb69d1b8358398eaf8d9638e3bdd447/3.4/onbuild/Dockerfile)

关于本镜像的更详细信息，请访问：[the relevant manifest file (`library/django`)](https://github.com/docker-library/official-images/blob/master/library/django)。本镜像的更新依赖：[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)。

关于镜像每个layer以及上述每个tag的详细信息，请查看位于[the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的 [the `django/tag-details.md` file](https://github.com/docker-library/docs/blob/master/django/tag-details.md)。

# Django是什么?

Django是一个免费、开源的web应用框架，用Python编写，符合MVC架构。Django的初衷是降低构建数据驱动网站的构建难度及复杂度，提供了更高的组件复用性和插件灵活性。

> [wikipedia.org/wiki/Django_(web_framework)](https://en.wikipedia.org/wiki/Django_%28web_framework%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/django/logo.png)

# 如何使用本镜像

## 在你的Django应用目录下创建一个`Dockerfile`文件：

```dockerfile
FROM django:onbuild
```

将本文件放在项目的根目录下。

这个镜像包含了多个`ONBUILD`触发器，足以满足大多数需求。构建过程将`COPY . /usr/src/app`, `RUN pip install`、`EXPOSE 8000`然后将容器的默认命令设置为`python manage.py runserver`。

然后你就可以启动这个Docker镜像了：

```console
$ docker build -t my-django-app .
$ docker run --name some-django-app -d my-django-app
```

你可以在浏览器中访问`http://container-ip:8000`来进行测试，或者如果你希望使用`http://localhost:8000`来进行访问，那么请使用以下命令：

```console
$ docker run --name some-django-app -p 8000:8000 -d my-django-app
```

## 不使用`Dockerfile`

如果你不希望任何`ONBUILD`触发器提供的便利之处，那么每次只需直接执行`docker run` 就可以了，并不需要向项目中`Dockerfile`文件。

```console
$ docker run --name some-django-app -v "$PWD":/usr/src/app -w /usr/src/app -p 8000:8000 -d django bash -c "pip install -r requirements.txt && python manage.py runserver 0.0.0.0:8000"
```

## 启动一个全新的Django应用

如果你希望从头创建一个Django项目，那么可以尝试以下步骤：

```console
$ docker run -it --rm --user "$(id -u):$(id -g)" -v "$PWD":/usr/src/app -w /usr/src/app django django-admin.py startproject mysite
```

这将在你的当前目录下创建一个名为`mysite` 的子目录。

# 镜像间的区别

`django`镜像包含了针对不同需求的多个版本。

## `django:<version>`

如果你不清楚自己的需求，那么这也许是最好的选项。它既可以作为用完即抛型容器（将你的源代码挂载到容器中并启动应用），也可以作为其他镜像的基础镜像来使用。

## `django:onbuild`

这个镜像让创建衍生镜像变得更加容易，对大多数应用情景来说，只需在项目目录中创建一个`Dockerfile`文件，并在其中加入`FROM django:onbuild`，然后就可以为你的项目创建独立的镜像了。

`onbuild`代表着低成本的容器化过程，所以并不建议应用于需要长期维护的项目。

当你对如何将项目Docker容器化非常熟练时，就可以考虑使用一个非`onbuild`版本的镜像来取代它，在自己的`Dockerfile`中将`ONBUILD`移到文件结尾，并移除`ONBUILD`关键词，这可以让你获得对项目更有效的控制，并且能让其他人更清楚本镜像的所作所为。

# 证书信息

关于本镜像包含软件的证书信息，请查看：[license information](https://github.com/django/django/blob/master/LICENSE)。

# 支持的Docker版本

本镜像官方提供了对于Docker1.9.1的支持。

而对于较老版本（低于1.6）则只能尽量做到对基本功能的支持。

关于如何升级Docker版本，请查看：[the Docker installation documentation](https://docs.docker.com/installation/)。

# 用户反馈

## 文档

本镜像的文档位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下的[`django/` directory](https://github.com/docker-library/docs/tree/master/django)目录中在提交新的pull request之前，请确保已经熟悉于[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)。

## 问题

如果你有关于本镜像的任何问题，请通过[GitHub issue](https://github.com/docker-library/django/issues)来提交。

你也可以从[Freenode](https://freenode.net)的`#docker-library`聊天频道中，获得来自本镜像维护者的直接帮助。

## 提供帮助

我们欢迎任何包括新功能、Bug修复在内的pull request提交，并且会尽快作出回复。

在动手编码之前，我们建议首先在[GitHub issue](https://github.com/docker-library/django/issues)提出你的设想，这让其他开发者有机会为你提供一些指导性的建议，并且对设计提出反馈，同时还可以让你知道是否已经有人在着手开发该功能了。
