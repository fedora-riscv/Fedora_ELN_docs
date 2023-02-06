# Fedora ELN project

## 概述

ELN 是 Fedora 的一个新的 buildroot 和 compose 过程，它采用 Fedora Rawhide dist-git 源并模拟红帽企业 Linux 组成。从这一构建、组合和集成测试中得到的反馈将提供给 Fedora 打包者，以便他们可以看到他们的变更对 RHEL 开发的潜在影响。

ELN 允许我们能够探索新的想法，例如在不影响 Fedora 其他部分的情况下提高 CPU 架构的基线。它还可以帮助我们在未来验证一些 RHEL ≤ 8 的规格文件条件。

ELN 源码来自 dist-git 的 Rawhide 分支。没有单独的 ELN 分支。

ELN 由 [ELN 特殊兴趣小组](https://docs.fedoraproject.org/en-US/eln/sig/) 管理。

如果您正试图解决 ELN 构建失败的问题，请[查阅](https://docs.fedoraproject.org/en-US/eln/ftbfs/)。

## 目的

- 创建允许对 Fedora 发行版的构建过程进行实验的基础设施和工具。  
- 使用这一基础设施来用类似于 CentOS 和 RHEL 的方式构建 Fedora 软件包，并为 Fedora 维护者提供反馈。  
- 其他功能（例如 CPU 更新）如果可以并行测试，可以纳入同一个构建过程中。新的功能请求将在 FESCo 和 Fedora Council 的监督下由 ELN SIG 批准。

## 命名

项目被命名为 EL Niño。  

这个名字是由 Stephen Gallagher 提议的。它包含了 "enterprisy "引用，但也强调了一个事实，即它是一个用于实验和开发的进展的永久性工作，而不是一个成品。  

但是这个名字的解释发生了变化，你可以把它看作是 Enterprise Linux Next。

### 关于双关语的解释

- EL 指 RHEL / CentOS 的发行标签："EL "代表 Enterprise Linux。 

- Niño 是西班牙语中的 "男孩子"。意味着该项目应该始终代表 "不成熟 "的版本并将 "成长 "为企业级 Linux。  

- 总的来说，"EL Niño "也是一种天气模式类型的名称，每隔三四年就会出现一次，并经常带来风暴。此处的含义应该是相当明显的！

## 范围

虽然最初这个项目命名为 "Alternative buildroot"，但它的范围包括 Fedora 源代码构建并组成可交付成果的整个过程。包括：

- buildroot 配置、rpm 宏和编译标志。  

- comps 文件和 compose 的内容。  

- compose 本身和构建它的工作流。

## 利益相关

从 ELN 项目中受益的人有：

- Fedora 基础设施
  
  - 因为我们要尝试和测试 compose 的新基础设施。

- Fedora 最小化
  
  - 我们将根据最小化的目标来修改 compose 的内容。

- CentOS Stream, EPEL 和 RHEL
  
  - 因为我们将尝试把 Fedora 软件包构建到类似于 CentOS 和 RHEL 的多仓库结构中。

- Fedora 社区
  
  - 为项目建立的反馈流程将允许下游开发者开放他们的工作并使其更接近 Fedora。
  
  - 为该项目开发的工具将允许在 Fedora 基础架构中进行变更，而不会阻碍常规的 Fedora 打包和发布过程。

## 链接

- 变更的第一个缩小范围的版本：[Changes/Additional buildroot to test x86-64 micro-architecture update - Fedora Project Wiki](https://fedoraproject.org/wiki/Changes/Additional_buildroot_to_test_x86-64_micro-architecture_update)

- 变更讨论：[Fedora 32 Self-Contained Change proposal: Additional buildroot to test x86-64 micro-architecture update](https://lists.fedoraproject.org/archives/list/devel@lists.fedoraproject.org/thread/IFBHS2WKKPKJH6H54OX4DV3U7A4XYOPU/#LUWYM7CBG5LVB2MFPO3R3JQEBFCPYPLK)

- alternative buildroot作为开发工具的观点：[Alternative buildroot as a development tool](https://lists.fedoraproject.org/archives/list/devel@lists.fedoraproject.org/thread/3Z5SMF6CPL3MK2CNYPUW4OZYMA6TZJBN/#MKXPMQOZNLECPPWRM6UBUMR2WHVG5DL6)
