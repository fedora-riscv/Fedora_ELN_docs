# ELN 扩展

ELN-Extras 本质上是 Fedora ELN 的扩展，主要区别在于 ELN-Extras 的内容将由 Fedora/EPEL 社区定义，而 ELN 的内容则主要由 Red Hat 管理层决定。这将为用户提供机会，确保他们的应用程序能够在即将发布的 RHEL 上运行，并为 EPEL 提供一个引导机制。如果最初的软件包已经为 ELN-Extras 构建，那么将更容易和更快地将 EPEL N+1 编译出来。

> 注意：当你的软件包在 ELN Extras 上构建失败时，你要承担责任。记得在 ELN 状态页上检查你的软件包。ELN Extras 软件包的重建要比 EPEL 中的频繁得多。

## 如何向 ELN Extras 中添加一个包

添加软件包到 ELN Extras，和添加它们到 ELN 是一样的。唯一的区别是，任何 Fedora 打包者都可以把包添加到 ELN Extras，而只有少数人可以把包添加到 ELN。  

您可以通过为[Content Resolver](https://github.com/minimization/content-resolver#readme)创建和提交workload来添加二进制包。你在[Content Resolver input](https://github.com/minimization/content-resolver-input)处以 pull requests 的方式提交它们。workload被放在配置目录中。  

一旦workload被合并，Content Resolver会处理工作负载的所有依赖关系，减去那些已经在 ELN 中的包，并在[ELN Extras视图](https://tiny.distro.builders/view--view-eln-extras.html)中显示。  

在Content Resolver完成工作后，ELN 的构建过程会采用该列表并将其添加到 ELN Extras 构建列表中。

### Workload

关于 workload yaml 文件中的内容的完整文档可以在[workload yaml文件的例子](https://github.com/minimization/content-resolver/blob/master/config_specs/workload.yaml)中找到注释。  

要创建你的初始 workload yaml 文件，请将该示例 yaml 文件重命名、编辑，然后将其作为 pull request 提交。  

示例 workload yaml 对所有内容都有一个例子。但对于 ELN Extras 来说，我们并不需要所有内容。

#### Workload 必要的部分

- document: feedback-pipeline-workload
  
  - 这是确定的行，不要改变它  

- version: 1
  
  - 这是确定的行，不要改变它 

- name: \<workload name\>
  
  - 保持简洁，允许有空格  

- description: \<workload description\>
  
  - 较长的描述，但必须在一行之内  

- maintainer: \<FAS and/or Github ID\>
  
  - 谁将是这个 workload 中 ELN 和 EPEL 的软件包的维护者
  
  - 未经允许，不要把别人的名字作为维护者

- labels: eln-extras  
  
  - 尽管可以列出多个标签，但对于 eln-extras，只需列出一个  

- packages:
  
  - 工作组中的二进制包列表  
  
  - 不要在包列表中列出源包名称。  
  
  - 列出所有你需要的二进制包。  
  
  - 如果你在可选的特定 Arch 包列表中列出一个软件包，就不要在这里列出它

#### Workload 可选的部分

- arch_packages:
  
  - 如果你有一个不是建立在所有 arch 上的包，你需要在它建立的所有 arch 上列出它。
  
  - 所有无关 arch 的软件包都放在常规的软件包部分。

- options:
  
  - 如果你需要你可以使用这些 options。

#### Workload 未使用的部分

- modules_disable:
  
  - ELN Extras 中没有 modules

- groups:
  
  - ELN Extras 中没有 groups

- package_placeholders:
  
  - 我们不在 ELN Extras 中处理 package_placeholders

## ELN Extras 的常见问题

*我可以把任何或所有的 Fedora 软件包添加到 ELN-Extras 吗？*

> 您正在附加您的名字作为这些软件包的支持者。您将以 pull requests 的方式提交这些 workload 的软件包。ELN SIG 会审查这些拉取请求，并在事情看起来很奇怪或不正确时提出问题。所以，如果你愿意支持一个软件包，它可以是 ELN 中没有的任何 Fedora 软件包。虽然我们没有对你可以添加到 ELN Extras 的软件包的数量做出明确的限制，但 SIG 正在审查它们，并且不会合并那些对请求者来说似乎过多的请求。

*如果有人已经在他们的工作量中加入了一个包，我可以在我的工作量中加入同样的包吗？*

> 可以。Content Resolver 的设置是为了处理不止一个 workload 中的软件包，或者不止一个 workload 中的依赖关系。它将显示哪些 workload 列出或需要哪些软件包。它甚至会猜测谁应该是软件包的所有者。
