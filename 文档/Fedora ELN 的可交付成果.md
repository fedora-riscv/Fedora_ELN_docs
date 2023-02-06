# Fedora ELN 的可交付成果

> 警告：Fedora ELN 是一个开发环境。它并不打算用于生产。

## 构成

Fedora ELN 软件包最新的 compose 可以从以下网址获取：[Index of /composes/production/latest-Fedora-ELN](https://odcs.fedoraproject.org/composes/production/latest-Fedora-ELN/)

镜像网址中也可以获取[Facebook Mirror](http://mirror.facebook.net/fedora-eln/production/latest-Fedora-ELN/)

另请参见[Fedora ELN compose :: Fedora Docs](https://docs.fedoraproject.org/en-US/eln/compose/)

## 容器镜像

作为 compose 的一部分，我们还建立了容器镜像，发布在 Quay.io image registry，名为`quay.io/fedoraci/fedora:eln-x86_64`

要运行 Fedora ELN 容器，请运行：

```shell
$ podman run -it quay.io/fedoraci/fedora:eln-x86_64
```

## Mock 环境

要为 ELN 构建自定义软件包，你可以按照 [Buildroot](https://docs.fedoraproject.org/en-US/eln/buildroot/) 的描述配置 Mock 环境。
