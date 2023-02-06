# ELN 分支活动

## Fedora 分支

Fedora 分支每6个月产生一次，在 Fedora Rawhide 为下一个版本建立分支的时候。例如。当 Fedora Rawhide 分支为 F35，fedora-release 被设置为f36。

### ELN 工作流触发器

eln 构建触发器需要更新到 fedora-release 中的最新版本。使用我们开始的例子。我们会把它改为 f36。

pull request 示例：<https://github.com/fedora-ci/eln-build-trigger/pull/2>

### ELN fedora 发布版本和仓库

#### 检查 Fedora-release

在 Fedora 分支产生后，fedora-release 已经更新并为新版本构建，检查 fedora-release 的 rpm 规格文件。验证`rhel_dist_version`是否被设置为正确的版本。

如果这是与 RHEL 版本变化相对应的 Fedora 分支，Fedora-release 的检查就显得尤为重要。

#### 首先建立 fedora-release

```shell
koji tag-build eln fedora-release-35-0.1
koji wait-repo eln-build --build fedora-release-35-0.1
# build fedora-release for ELN
# Example: fedpkg build --skip-nvr-check --target eln
koji wait-repo eln-build --build fedora-release-35-0.1.eln110
koji untag-build eln fedora-release-35-0.1
```

#### 在 fedora-release 进入 buildroot 后，建立 fedora-repos

```shell
koji tag-build eln fedora-repos-35-0.2
koji wait-repo eln-build --build fedora-repos-35-0.2
# build fedora-repos for ELN
# Example: fedpkg build --skip-nvr-check --target eln
koji wait-repo eln-build --build fedora-repos-35-0.2.eln110
koji untag-build eln fedora-repos-35-0.2
```

## RHEL 分支

RHEL 分支每3年产生，在 RHEL 为下一个版本建立分支的时候。例如：当 RHEL 分支达到RHEL 10 时。
