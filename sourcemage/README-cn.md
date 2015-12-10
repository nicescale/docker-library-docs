# 支持的镜像tag 和 `Dockerfile` 链接

-	[`latest`, `0.61` (*stable/Dockerfile*)](https://github.com/vaygr/docker-sourcemage/blob/2d7efdca5633554194e2b12fae334ac4b6804b25/stable/Dockerfile)

# Source Mage GNU/Linux 镜像

[Source Mage](https://beta.sourcemage.ru/) (缩写 *SMGL* )是一个源码基于的 *GNU/Linux* 发行版.名字来自"casting", "dispelling"程序的魔法隐喻, 包我们称之为 "**spells**", 包管理器称之为"**Sorcery**".我们的包被设计成允许用户以任何他们想要的方式定制(定制CFLAGS,LDFLAGS,`./configure` 选项等),同样给用户前端提供尽可能多的包选项(你不需要提前了解一个包有哪些选项或者可选依赖).源码都是从发布者站点下载,几乎未被修改过.SMGL同样包含了许多像自愈合,子依赖等高级特性.

我们所有脚本都是以[GPL](https://www.gnu.org/licenses/gpl.html)条款发布,我们的包管理器和包是用[bash](https://www.gnu.org/software/bash/)编写,所以很容易学习修改.Sorcery支持定制用户维护的包,它们可以覆盖默认包,而且从来不会受到更新影响.

![logo](https://raw.githubusercontent.com/docker-library/docs/master/sourcemage/logo.png)

# 镜像

这些镜像基于我们的 [chroot images](https://beta.sourcemage.ru/Install/Chroot). 为了使用它们,按如下所做:

```shell
$ docker run -it sourcemage
```

或者

```shell
$ docker run -it sourcemage:0.61
```

---

# 注意

-	为了完全受益于[castfs](https://beta.sourcemage.ru/castfs) 你需要额外的标志 (`--device /dev/fuse --cap-add SYS_ADMIN`)在容器内访问 `/dev/fuse`设备 , 但是你可能因为授予容器这样的能力/特权,带来安全隐患,而受到警告;否则,会使用[installwatch](https://beta.sourcemage.ru/installwatch).
-	`0.61` 表示镜像的使用的grimoire 版本, 否则拉下`latest`镜像

