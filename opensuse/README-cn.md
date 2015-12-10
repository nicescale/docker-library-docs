# 支持的镜像tag 和 `Dockerfile` 链接

-	[`13.2`, `harlequin` (*docker/Dockerfile*)](https://github.com/openSUSE/docker-containers-build/blob/d9e75cd826dffd35984b8c7c9322494810b4e29c/docker/Dockerfile)
-	[`42.1`, `leap`, `latest` (*docker/Dockerfile*)](https://github.com/openSUSE/docker-containers-build/blob/da3cf11b9e96b61ffbe92de846e77075bc11e326/docker/Dockerfile)
-	[`tumbleweed` (*docker/Dockerfile*)](https://github.com/openSUSE/docker-containers-build/blob/be79ebd1a6640e08fae0e79c1511546da2b58bcd/docker/Dockerfile)

# openSUSE

此项目包含opensuse稳定版本分发.

# 命名惯例

每一个镜像都同时使用了发布版本数字和代号(比如 *"Bottle"*)做为标记.最新的稳定发布版本总是使用"*latest*"标记.

# 构建

这些镜像使用[KIWI](https://github.com/openSUSE/kiwi)生成.它们的源码文件可以在[this repository](https://github.com/openSUSE/docker-containers)找到.

# 仓库和包

包的选择保持最小化,来精简镜像大小.

以下仓库已经是此镜像的一部分:

-	OSS
-	OSS Updates
-	Non-OSS
-	Non-OSS Updates

