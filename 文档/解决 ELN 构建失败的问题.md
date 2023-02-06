# 解决 ELN 构建失败的问题

本页提供信息以帮助软件包维护者补救 ELN 构建失败。

## 背景

关于 ELN 的一般信息，请访问概述。  

注意 ELN 的 buildroot 有什么不同。  

目前需要为 ELN 构建的软件包集见<https://tiny.distro.builders/view--view-eln.html>。包括构建依赖的扩展软件包集见<https://tiny.distro.builders/view--view-eln-and-buildroot.html>。  

还有一个[ELN 构建状态页面](https://docs.fedoraproject.org/en-US/eln/status/#_eln_builds)，显示哪些软件包目前无法为 ELN 构建，以及那些在 Fedora Rawhide 和 ELN 构建之间有差异的软件包。

## 调试 ELN 构建失败

查看关于`koji`和`mock`如何使用 ELN buildroot 构建的细节：[Buildroot configuration :: Fedora Docs](https://docs.fedoraproject.org/en-US/eln/buildroot/#building)。

## 修复 ELN 构建失败的问题

### 如果该软件包没有为Rawhide构建，请先解决这个问题！

> 重要：如果该软件包没有为Rawhide构建，请先解决这个问题！

如果该软件包没有为Rawhide构建，请先解决这个问题！

这条信息足够响亮和清晰吗？为 Rawhide 修复软件包的构建，很可能就是为 ELN 修复它所需要的一切。

### 对 Fedora 和 RHEL 之间的差异进行核算

大多数构建失败可以通过修改 SPEC 文件来解决。然而，这种修改应该保持在最低限度。

如果你的软件包需要在 Fedora 和 RHEL 之间建立不同的代码路径或软件包依赖关系，重要的是只在需要的版本中考虑到差异。

一般来说，这意味着在 Fedora 的旧版本中限制差异，而让 RHEL 的下一个版本采用 Fedora 的代码路径。

作为需要差异的一种常见情况，RHEL 最终的依赖关系往往比 Fedora 小。定义`%{fedora}`和`%{rhel}`宏时包含每个发行版的主要发布版本。

考虑以下 SPEC 文件片段。

```
%if 0%{?fedora} >= 29 || 0%{?rhel} >= 9
BuildRequires: SomePackage
%endif
```

当`%{rhel}`被定义为9或更大时，这个条件将被满足。它也可以在 Fedora 和 RHEL 的早期和后期版本之间共享。通常情况下，使用`<`将行为限制在RHEL 8和更早的版本会更好：

```
%if 0%{?fedora} < 32 && 0%{?rhel} < 9
  %global rhel8orearlier 1
%else
  %global rhel9orlater 1
%endif
```

最好的规范文件只对 Fedora 或 RHEL 的发布版本改变默认行为，使默认策略成为适用于 Fedora 或 RHEL 当前版本的策略。

作为最后的手段，有一个`%{eln}`宏可用。如果可能的话，应该尽量避免使用它。然而，如果你发现你确实需要使用它，你应该只检查它是否被设置为非零值。比如说：

```
%if 0%{?eln}
  # something specific to ELN only
  ...
%endif

%if 0%{?fedora} || 0%{?eln}
  # something specific to Fedora or ELN
  ...
%endif

%if 0%{?rhel} && ! 0%{?eln}
  # something specific to RHEL only, not ELN.
  ...
%endif
```

### 弃用 Python 2

就像最近的 Fedora 版本已经废弃了 Python 2 一样，RHEL 9 和 ELN 也是如此。ELN 的构建失败有一个反复出现的情况，可以通过调整 SPEC 条件禁用 Python 2 来解决。例如，如果你有这样的东西：

```
%if 0%{?fedora} >= 30
%global python2_enabled 0
%else
%global python2_enabled 1
%endif
```

将条件改为：

```
%if 0%{?fedora} >= 30 || 0%{?rhel} >= 9
```

### 架构依赖

ELN 是为与 Rawhide 相同的一组架构而构建的。然而，ELN 中的内核包不再为任何32位架构产生一个完整的内核。值得注意的是，没有针对`armv7hl`架构的`kernel`包，只有`kernel-headers`包。为了帮助解决这个问题，有一个新的`%{kernel_arches}`宏，可以在 SPEC 文件中使用。比如说：

```

```
