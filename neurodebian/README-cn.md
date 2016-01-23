# 支持的tag标签，以及相关`Dockerfile`链接

-	[`precise`, `nd12.04` (*dockerfiles/precise/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/precise/Dockerfile)
-	[`trusty`, `nd14.04` (*dockerfiles/trusty/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/trusty/Dockerfile)
-	[`vivid`, `nd15.04` (*dockerfiles/vivid/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/vivid/Dockerfile)
-	[`wily`, `nd15.10` (*dockerfiles/wily/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/wily/Dockerfile)
-	[`squeeze`, `nd60` (*dockerfiles/squeeze/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/squeeze/Dockerfile)
-	[`wheezy`, `nd70` (*dockerfiles/wheezy/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/wheezy/Dockerfile)
-	[`jessie`, `nd80`, `latest` (*dockerfiles/jessie/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/jessie/Dockerfile)
-	[`stretch`, `nd90` (*dockerfiles/stretch/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/stretch/Dockerfile)
-	[`sid`, `nd` (*dockerfiles/sid/Dockerfile*)](https://github.com/neurodebian/dockerfiles/blob/dc947232d6c7bf67d1dba642fdb1234fbaa95ab7/dockerfiles/sid/Dockerfile)

关于本镜像的更详细信息，请访问：[清单文件(`library/neurodebian`)](https://github.com/docker-library/official-images/blob/master/library/neurodebian). 镜像更新依赖于[`docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)的[`neurodebian/tag-details.md`](https://github.com/docker-library/docs/blob/master/neurodebian/tag-details.md)文件。

# 关于 NeuroDebian

NeuroDebian 提供了大量[Debian](http://www.debian.org)，[Ubuntu](http://www.ubuntu.com) 以及和其他衍生品的受欢迎的软件. 常用包包括*AFNI*, *FSL*, *PyMVPA*, 及其他的。 我们会努力维持高水平的质量, 但我们不能保证每一个给定的包都是预期的,所以使用它们需要你考虑自己的风险。

> [neuro.debian.net](http://neuro.debian.net/)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/neurodebian/logo.png)

# 关于镜像

NeuroDebian 镜像仅仅加入了NeuroDebian 仓库及其 GPG key. 下载了不恰当的索引，所以在执行`apt-get`之前需要执行 `apt-get update` 。

`nd` 标签用于可获得的包的后缀。

`neurodebian:latest` 标签始终是 Neurodebian-enabled 最新版本的 Debian (在撰写本文时，是 `debian:wheezy`).

## sources.list

NeuroDebian APT 文件被安装在 `/etc/apt/sources.list.d/neurodebian.sources.list` ，目前只允许 `main` (DFSG-compliant) 区域:

```console
$ docker run neurodebian:latest cat /etc/apt/sources.list.d/neurodebian.sources.list
deb http://neuro.debian.net/debian wheezy main
deb http://neuro.debian.net/debian data main
#deb-src http://neuro.debian.net/debian-devel wheezy main
```

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon

