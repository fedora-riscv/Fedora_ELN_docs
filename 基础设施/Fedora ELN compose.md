# Fedora ELN compose

## 概述

Fedora ELN compose 是使用 ODCS (On Demand Compose Service) 建立的。有两个 ODCS 实例(production 和 staging)，因此有两个地方有最新的 ELN composes。

| Fedora ELN compose        | 环境         | 链接                                                                    |
| ------------------------- | ---------- | --------------------------------------------------------------------- |
| Latest periodic           | production | https://odcs.fedoraproject.org/composes/production/latest-Fedora-ELN/ |
| Latest manually generated | production | https://odcs.fedoraproject.org/composes/test/latest-Fedora-ELN/       |
| Latest manually generated | staging    | https://odcs.stg.fedoraproject.org/composes/test/latest-Fedora-ELN/   |

每小时都会产生周期性的 compose。最新的 production compose 也可以在以下镜像找到：[Facebook Mirror](http://mirror.facebook.net/fedora-eln/production/latest-Fedora-ELN/)

## Compose 配置文件

主要的 Fedora ELN compose 配置文件存储在官方 <https://pagure.io/pungi-fedora/> 仓库的`eln`分支中。

Fedora ELN compose 使用存储在<https://pagure.io/fedora-comps> 仓库中的 Rawhide comps 文件，以及存储在<https://pagure.io/releng/fedora-module-defaults/> 仓库中的 Rawhide module-defaults。

## 生成 Fedora ELN compose

eln-sig FAS 组(和 eln-sig FAS staging 组)的成员可以使用 `odcs` 命令行客户端生成 Fedora ELN compose。

ODCS 命令客户端可以在 `odcs-client` 包中找到：

```shell
$ sudo dnf install odcs-client
```

然后，Fedora ELN compose 可以通过以下命令在 ODCS staging 上生成：

```shell
$ odcs --staging create-raw-config eln eln
```

或者在 production ODCS 上：

```shell
$ odcs create-raw-config eln eln
```

一旦生成 compose，就会打印出 JSON 响应，最重要的属性是`toplevel_url`，它指向生成 compose 的目录。

> 注意：当使用 ODCS 版本 ≤ 0.2.39 时，需要使用较早的语法来生成Fedora ELN compose：
> 
> ```shell
> odcs --staging create raw_config eln#eln
> ```

> 注意：当使用 ODCS 版本 ≥ 0.2.46 时，可以观看实时 compose 日志：
> 
> ```shell
> $ odcs --watch create-raw-config eln eln
> ```
