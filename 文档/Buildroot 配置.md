# Buildroot 配置

## 版本管理

在 ELN 项目中，我们致力于 buildroot 配置，而不是 Fedora 软件包的内容。由于 ELN 的buildroot 配置可能会频繁变更，并且独立于 Fedora 版本，因此它有自己的版本。  

这种方法使我们能够在 buildroot 变化时重建 RPM 包，而不需要改变 dist-git 中包的规格文件。  

ELN Buildroot 版本写成`XY`。  

`X`版本指的是 ELN buildroot 的主要变更（通常是那些需要大规模重建的事件）。`Y`版本指的是 buildroot 配置中的变更，这些变更是兼容的，将适用于 ELN buildroot 中未来的构建。  

我们把这个数字从`100`开始，其中`1`是X版本，`00`是Y版本。

## 分布式相关的宏定义

在 ELN buildroot 中，我们以类似 EL 的设置来构建 Fedora Rawhide 软件包。因此，以下宏值是默认设置的：

| Macro       | Value        | Description                                 |
| ----------- | ------------ | ------------------------------------------- |
| `%{fedora}` | `%{nil}`     | 未设置                                         |
| `%{rhel}`   | `9`          | 比最近发布的RHEL主要版本高一的整数。                        |
| `%{eln}`    | `XY`         | ELN buildroot 的版本。不与任何 RHEL 或 Fedora 版本相联系。 |
| `%{dist}`   | `.eln%{eln}` | eln-disttag，例如`.eln101`                     |

我们在 Koji 中为 eln-build 标签设置这些值，如下：

```shell
koji edit-tag \
        -x rpm.macro.fedora=%{nil} \
    -x rpm.macro.rhel=9 \
    -x rpm.macro.eln=101 \
    -x 'rpm.macro.dist=%{!?distprefix0:%{?distprefix}}%{expand:%{lua:for i=0,9999 do print("%{?distprefix" .. i .."}") end}}.eln%{eln}%{?with_bootstrap:~bootstrap}' \
    eln-build
```

## 编译器标志和其他调整

| Arch    | Rawhide                   | ELN                     |
| ------- | ------------------------- | ----------------------- |
| aarch64 | -                         | -                       |
| i686    | -                         | -                       |
| ppc64le | -                         | -                       |
| s390x   | `-march=zEC12 -mtune=z13` | `-march=z14 -mtune=z15` |
| x86_64  | -                         | `-march=x86-64-v2`      |

编译器标志的调整可以在 /usr/lib/rpm/redhat/macros 中找到，上述选项来自 redhat-rpm-config-200-1

## 用 ELN buildroot 进行构建

在使用`fedpkg`启动 koji 构建时，只需指定`eln`为发布版本即可：

```shell
fedpkg --release eln build [--scratch] [--srpm [SRPM]] [...]
```

`mock`也可以用来在本地进行构建。  

支持的`mock`配置在 33 版或更高版本的`mock-core-configs`包中可用。这使得 ELN 环境可以通过运行以下命令（对于arch x86_64）在本地使用：

```shell
mock -r fedora-eln-x86_64 ...
```

> 注意：如果官方的`mock`配置还不能用，可以在[这里](https://docs.fedoraproject.org/en-US/eln/_attachments/fedora-eln-x86_64.cfg)下载一个不支持的配置文件。参见[mock#649](https://github.com/rpm-software-management/mock/pull/649)
