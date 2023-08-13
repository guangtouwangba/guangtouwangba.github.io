---
layout:   page
title: 打造DevOps和Kubernetes友好的终端：必备工具大揭秘！
tag: devops
category: Kubernetes
order: 1
author:      "aqiu"
image: ''
published: true
summary: "如果你在 DevOps 和 Kubernetes 领域工作，你就知道命令行界面（CLI）对于管理任务是多么重要。幸运的是，有一些工具可以使在这些环境中使用终端更加便捷。在这篇文章中，我们将探讨一些顶级工具，它们可以简化你的工作流程，并帮助你自信地在 DevOps 和 Kubernetes 中操作终端。"
categories: ['Devops']
date: 2023-08-06
---

# 打造DevOps和Kubernetes友好的终端：必备工具大揭秘！

如果你在 DevOps 和 Kubernetes 领域工作，你就知道命令行界面（CLI）对于管理任务是多么重要。幸运的是，有一些工具可以使在这些环境中使用终端更加便捷。在这篇文章中，我们将探讨一些顶级工具，它们可以简化你的工作流程，并帮助你自信地在 DevOps 和 Kubernetes 中操作终端。



## Zsh

Zsh（Z Shell）是一款强大而高度可定制的命令行 shell 和终端仿真器，相较于传统的 Bash 等 shell，它提供了增强功能和提高工作效率的改进。以下提供的选项使它成为开发人员和DevOps工程师的热门选择。



### Oh My ZSH

[Oh My Zsh](https://www.zsh.org/) 是一个开源的、由社区驱动的框架，用于管理你的 Zsh 配置。你可以使用以下命令通过 curl 安装它：

```shell
sh -c "$(curl -fsSL <https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh>)"
```



### zsh-syntax-highlighting

[zsh-syntax-highlighting]([zsh-syntx-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)) 是 Zsh shell 的一个插件，它能够在你输入命令及其参数时提供实时的语法高亮。它可以帮助视觉上区分不同类型的命令、选项、路径和变量，从而更容易发现错误并理解终端中命令的结构。



按照[这里](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)的安装指南进行安装。

这是在安装这个工具之前和之后，我的终端的样子：

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061842765.png)

<center>安装前 </center>

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061844945.png)

<center>安装后 </center>

### zsh-autosuggestions

[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) 是 Zsh shell 的一个实用插件，它在你输入时提供智能命令建议。它会分析你的命令历史，并为完成命令提供预测性的建议。

按照[这里](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)的安装指南进行安装。

在安装这个工具之前和之后，我的终端的样子如下：

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061852803.png)

<center>安装前</center>

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061852057.png)

<center>安装后</center>

## Terraform

如果你使用 Terraform 和 Terragrunt 作为基础设施即代码工具，那么你可能会发现以下相关工具在处理 Terraform 和 Terragrunt 时非常有用。

### tfswitch and tgswitch

[tfswitch](https://github.com/warrensbox/terraform-switcher) 和 [tgswitch](https://github.com/warrensbox/tgswitch) 是命令行工具，它们可以简化在不同版本的 Terraform 和 Terragrunt 基础设施即代码工具之间切换。它们允许开发人员和运维人员轻松管理和切换不同项目或环境中的 Terraform 和 Terragrunt 版本。

在 Mac 上，你可以按照以下步骤安装这些工具：

1. 安装 Homebrew（如果你还没有安装）：
   打开终端并运行以下命令：

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. 安装 tfswitch：
   在终端中运行以下命令：

```
brew install warrensbox/tap/tfswitch
```

3. 安装 tgswitch：
   在终端中运行以下命令：

```
brew install warrensbox/tap/tgswitch
```

**注意：在 Mac 上使用 tfswitch 和 tgswitch 需要在 Zsh 中添加以下行到你的 .zshrc 文件中**

### Infracost

[Infracost](https://www.infracost.io/)  是一款强大的工具，它可以帮助你估算和跟踪在类似 Terraform 平台上的基础设施即代码（IaC）的成本。通过分析你的基础设施配置文件，Infracost 提供实时的成本估算，让你可以做出明智的决策，并通过识别潜在的节省成本的机会来优化你的云资源开支。

我对一个项目运行 Infracost 得到了以下结果：

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061902006.png)

### TfSec

TFSec 是一个专门为 Terraform 代码设计的安全扫描工具。它能够帮助识别基础设施即代码中潜在的安全漏洞和最佳实践违规问题，让你能够主动解决安全问题，并确保符合行业标准和组织政策的合规性。

可以使用下面的命令进行安装：

```
brew install tfsec
```

这是我对我的项目运行 TFSec 后得到的结果：

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061904270.png)



## Kubernetes

为了应对 Kubernetes 复杂性，许多额外的工具被创造出来，以帮助 DevOps 团队高效利用它。这些工具旨在简化流程，让 DevOps 专业人员能够无缝地与 Kubernetes 协同工作，优化他们的部署和管理任务。



### Kubernetes aliases

就像使用 Git 命令的别名一样，为 Kubernetes 命令使用别名也是非常有益的。别名可以使得在处理 Kubernetes 命令时更加简便高效，节省时间和精力，让你在与 Kubernetes 集群和资源交互时事半功倍。

```shell
alias k='kubectl'
```

```shell
# For switching context between different clusters
alias kswitch-maryam='kubectl config use-context kind'
alias kswitch-mary='kubectl config use-context kind'
alias kpod='kubectl get pods -A'
alias knode='kubectl get nodes'
alias kdesp='kubectl describe pod'
alias kdp='kubectl delete pod'
alias kgd='kubectl get deployments'
```

这些只是一些例子，你可以根据自己经常使用的 Kubernetes 命令来自定义别名。通过将这些别名添加到你的 shell 配置文件（例如 .bashrc 或 .zshrc），你可以使用这些快捷方式快速轻松地执行 Kubernetes 命令。

### kube-ps1

[kube-ps1](https://github.com/jonmosco/kube-ps1) 可以增强你的命令提示符，显示关于当前 Kubernetes 上下文的相关信息。在我处理多个 Kubernetes 集群并管理不同的集群上下文时，它非常有帮助。通过视觉上突出显示活动集群上下文的细节，它帮助我避免潜在的错误，并在导航和与 Kubernetes 环境交互时提供了清晰的指引。

你可以使用以下命令安装：

```shell
brew install kube-ps1
```

如果你在使用 Zsh，确保将以下内容添加到你的 .zshrc 文件中：

```bash
plugins=(
  kube-ps1
)
```

### kubecolor

[kubecolor](https://github.com/hidetatz/kubecolor) 是一个便利的工具，它通过添加颜色和格式改进了 Kubernetes 命令的输出，使其更易于阅读和理解。kubecolor 提升了可见性，帮助我们在处理 Kubernetes 时快速识别重要信息。（在日常处理 Kubernetes 时，它再次拯救了生命的工具！）

在 Mac 上按照以下步骤安装 kubecolor，并确保将第二行添加到你的 .zshrc 文件中，以便与 kubectl 的自动补全功能配合使用：

1. 安装 kubecolor： 在终端中运行以下命令：

```shell
brew install hidetatz/tap/kubecolor
```

2. 添加配置到 .zshrc 文件： 打开你的 .zshrc 文件，并在其中添加以下内容：

```shell
source <(kubectl completion zsh)
alias kubectl=kubecolor
# make completion work with kubecolor
compdef kubecolor=kubectl
```

完成上述步骤后，你就可以在 Mac 上安装 kubecolor，并确保它与 kubectl 的自动补全功能正常运行了。

这是一个演示该工具如何给 Kubernetes 命令的输出添加颜色的例子：

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061912897.png)

### kubectx + kubens

[kubectx](https://github.com/ahmetb/kubectx) 和 kubens 是管理 Kubernetes 上下文和命名空间的有用工具。kubectx 允许用户在不同的 Kubernetes 上下文之间切换，而 kubens 简化了在特定上下文中切换命名空间，使得在处理多个集群和高效组织资源时更加便捷。

### k9s

[K9s](https://k9scli.io/) 是一个用户友好的命令行工具，为管理 Kubernetes 集群提供了可视化仪表板。它提供了简单直观的界面，让你可以轻松查看和与资源、Pod、日志和事件进行交互，帮助 DevOps 专业人员更方便地监控和排查 Kubernetes 部署。

### k8s Lens

K8s Lens 是一个用户友好的桌面应用程序，提供了图形界面来管理和监控 Kubernetes 集群。它以可视化方式展示资源、Pod 和节点，让用户可以轻松地导航和与 Kubernetes 环境进行交互，为开发人员和管理员在处理 Kubernetes 时带来了方便。

### Popeye

[Popeye](https://github.com/derailed/popeye) 是一个有用的命令行工具，它分析 Kubernetes 集群并提供有关潜在问题或配置错误的宝贵见解。它扫描集群配置、命名空间、部署和 Pod，以识别最佳实践违规、资源效率低下和安全问题，帮助用户确保他们的 Kubernetes 部署经过优化并保持良好维护。

这是对集群进行扫描后，Popeye 将为你提供的示例信息：

![](https://golearning.oss-cn-shanghai.aliyuncs.com/202308061916987.png)

可以使用以下命令来安装命令：

```shell
brew install derailed/popeye/popeye
```



### Kube-shell

[Kube-shell](https://github.com/cloudnativelabs/kube-shell) 是 Kubernetes CLI 的集成式 shell。它提供了用户友好的界面和集群资源的可视化展示，让用户可以轻松地导航、监控和管理他们的 Kubernetes 部署，无需依赖命令行界面。



### Kube-Capacity

[Kube-Capacity](https://github.com/robscott/kube-capacity) 是一个实用的工具，可以提供有关你的 Kubernetes 集群资源使用情况和容量的见解。它帮助你了解集群资源的分配和利用情况，让你可以优化资源分配、规划扩展，并确保在 Kubernetes 环境中进行高效的资源管理。



## 写在最后

命令行工具是 DevOps 和 Kubernetes 专家的绝佳伴侣，特别是对于那些希望优化工作流程和提高工作效率的人。无论你是开发人员、系统管理员还是其他IT专家，上述提到的工具都可以为你提供卓越的助益，帮助你更轻松、更自信地完成任务。

为了使你的命令行体验更为完善，我强烈建议你尝试上述工具，并根据自己的需求进行调整。每个工具都有其特点，但结合使用时会更为强大。当然，还有其他许多工具值得探索，但上述列表为你提供了一个很好的起点，帮助你开始你的命令行增强之旅。