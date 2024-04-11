# Kubernetes CSI 历史与现状

注意：本文不会讲解 CSI 的技术原理实现，它只是一篇 kubernetes 存储相关的科普文。

## 背景

目前 kubernetes 对接存储大多数都是使用 CSI 的方式，而且官方也是推荐这种方式。但是 kubernetes 对接存储的过程并不是一开始就是采用 CSI，中间还是走了一些弯路的。

kubernetes 对接存储目前总共经历了如下三个大阶段：
- in-tree
- FlexVolume
- CSI

### in-tree

kubernetes 最开始的时候将所有的存储对接代码都与核心代码混在一起，比如谷歌云、亚马逊云、微软云这些初始成员都将对接自家存储的代码放到 kubernetes 核心代码仓库中，这种方式即称为 "in-tree"。这种方式会带来一些不便之处：用户想要最新的存储对接特性则必须使用最新的 kubernetes 版本，因为这些特性是与 kubernetes 版本进行绑定的，但此时这两者的代码都放到了同一个仓库里，无法分离。

进行过 k8s 运维的同学应该深有体会，一般来说生产环境中的 kubernetes 没有特殊情况是不会升级的，其中涉及很多因素在里面，比如成本、稳定性等。所以说这种存储对接与 kubernetes 核心代码强耦合的方式不够灵活，对用户不友好。

kubernetes 的开发者们就想着将这些存储相关的代码与核心代码分离，毕竟这种强耦合的方式实在难以运维。于是第二代存储对接方式 FlexVolume 出来了。

### FlexVolume

FlexVolume 这个功能特性从 kubernetes v1.2 引入，v1.8 版本正式 GA。FlexVolume 的协议比较简单，它可以使用任何语言实现，协议规范是要求执行相关接口后要返回符合规范的 json 内容。

FlexVolume 需要安装到一个指定的名称目录下，这样 kubelet 才能发现并找到对应的执行程序。安装 FlexVolume 需要在宿主机上安装相关的依赖项，所以它是对宿主机环境有要求的。k8s 的开发者们觉得这个方式不太优雅，他们不希望 FlexVolume 对宿主机环境有要求或者影响，最好是运行在容器里面，所以设计并持续演进了下一代存储标准接口：CSI。

### CSI

CSI 是一种 RPC 协议规范，由 kubernetes storage SIG 推出，协议采用 protobuf 定义，最新的协议版本是 v1.9.0，具体内容可以到官网仓库去查看，地址：https://github.com/container-storage-interface/spec/blob/master/spec.md。

CSI 从 kubernetes v1.9 开始引入，到现在还在不断的演进，协议内容也不断的增加和完善。CSI 也可以使用任何语言来实现，只要能进行 RPC 通信即可。CSI 从设计之初就不单单只是为了 kubernetes，它还打算应用在所有容器编排系统上，比如 Swarm/Nomad 等。

值得注意的是，CSI 协议不是严格向前兼容的，它的演进过程中也有一些重大破坏性改进，导致一些稍微老一些的 kubernetes 是没有办法使用新的 CSI 驱动的。这个会在下面的 CSI 的进化历程篇章中进行阐述。

## CSI vs FlexVolume

目前应该还有很多企业在使用 FlexVolume。FlexVolume 还是有它的用武之地的，不是说 CSI 出来了之后 FlexVolume 就没有用了。在一些场景下 FlexVolume 实现的效率会比 CSI 更好，比如不需要快照、克隆，无状态，数据不需要持久化。

可以将 CSI 与 FlexVolume 简单进行对比一下，让小伙伴对这两个存储对接方式有个大概
的认知。

|             | FlexVolume  |       CSI     |
| :---:       |    :----:   |  :---:        |
| 实现难度     |     小       |       较大    |
| 接口数量     |     少       |       多      |
|    挂载     |    有        |       有      |
|    卸载     |    有        |       有      |
|    创卷     |    无        |       有      |
|    删卷     |    无        |       有      |
|    快照     |    无        |       有      |
|    克隆     |    无        |       有      |
|    扩容     |    无        |       有      |

可以发现 FlexVolume 支持的功能较少，这是因为 FlexVolume 早早的停止了演进，有些高级功能并没有演进出来。CSI 那些高级功能也不是一开始就有的，也是通过一个个小版本慢慢迭代出来的。

## CSI 的进化历程

kubernetes 中每个特性都是经历 alpha->beta->GA 三个阶段，CSI 中每个特性也是一样。下面是 kubernetes 中 CSI 相关特性的演进过程，这个过程包括了从它出生的 v1.9 版本到本月中旬准备发布的 v1.30 版本。

### kubernetes v1.9

CSI 版本 v0.1.0 诞生，可以支持创建/删除、挂载/卸载等基本功能。

### kubernetes v1.10 

CSI 演进到 v0.2.0，修复一些 bug，增加已有接口安全性，推出 CSI 的 E2E 测试工具。

### kubernetes v1.11

CSI 演进到 v0.3.0，修复一些 bug。

【alpha】增加支持 Block Volume 特性。这个特性使得 CSI 可以直接将裸的块设备提供给 pod，对一些特殊的 pod 使用场景比较有用，比如 oracle 需要直接使用裸的块设备，那这个特性就可以满足这种需求。

### kubernetes v1.12

修复了一些 bug。

【alpha】增加快照特性，支持创建和删除快照（volumesnapshot）

【alpha】支持 Topology

【alpha】支持跳过 Attach 阶段，需在 storageclass 配置

【alpha】支持挂载时增加 pod 信息，需在 storageclass 配置

### kubernetes v1.13 

CSI 协议演进到 v1.0.0，这是一个重大的破坏性演进，v1.0.0 协议不支持向前兼容，但承诺从 v1.0.0 之后的版本都是兼容的。而基于之前 v0.3.0 协议实现的 CSI 需要在 kubernetes v1.17 之前转到 v1.0.0，因为 kubernetes v1.17 之后就不支持基于 v0.3.0 实现的 CSI 接口，也就意味着无法正常运行。v1.0.0 不支持向前兼容的原因是 RPC 接口里面的字段发生了变化。

没有引入新特性。

### kubernetes v1.14 

CSI 协议演进到 v1.1.0，修复一些 bug。

【alpha】增加卷扩容特性

【beta】增加快照特性，支持创建和删除快照（volumesnapshot）

【beta】支持 Topology

【beta】支持跳过 Attach 阶段，需在 storageclass 配置

【beta】支持挂载时增加 pod 信息，需在 storageclass 配置

### kubernetes v1.15

修复一些 bug。

【alpha】增加卷克隆 volume cloning 特性，这个特性可从源 PVC 直接创建克隆卷，不需要通过 volumesnapshot 快照来创建克隆

【alpha】增加临时本地卷 Ephemeral local volumes 特性

### kubernetes v1.16 

修复一些 bug。

【beta】卷克隆 volume cloning 特性

【beta】卷扩容特性

【beta】临时本地卷 Ephemeral local volumes 特性

### kubernetes v1.17

CSI 协议演进到 v1.2.0，修复一些 bug。此版本不再支持 v0.3.0 的 CSI。

【beta】卷快照特性

【beta】CSI migration for AWS EBS and GCE PD drivers

【GA】Topology

【GA】卷挂载限制

### kubernetes v1.18

修复一些 bug。此版本 kubelet 在给 pod 挂载卷时不再创建挂载点，由 CSI 负责。

【alpha】增加 windows 系统的 CSI 支持

【beta】CSI migration for Openstack cinder driver

【GA】卷克隆特性

【GA】Topology

【GA】跳过 Attach 阶段

【GA】挂载时增加 pod 信息

### kubernetes v1.19

修复一些 bug。

【alpha】增加卷健康监测 Volume health monitoring 特性

【alpha】增加 fsGroup 特性

【alpha】增加存储空间跟踪 Storage capacity tracking 特性

【alpha】增加通用临时卷 Generic ephemeral volumes 特性

### kubernetes v1.20

修复一些 bug。

【alpha】增加 CSIServiceAccountToken 特性，可用来增强安全性

【beta】fsGroup 特性

【GA】卷快照特性

### kubernetes v1.21 

修复一些 bug。

【beta】CSIServiceAccountToken 特性

【beta】存储空间跟踪 Storage capacity tracking 特性

【beta】通用临时卷 Generic ephemeral volumes 特性

### kubernetes v1.22

修复一些 bug。

【alpha】支持 ReadWriteOncePod 访问模式

【GA】Windows 支持

【GA】CSIServiceAccountToken 特性

### kubernetes v1.23

修复一些 bug。还有一个值得注意的是，这个版本正式弃用 FlexVolume，全部推荐上 CSI。

【alpha】RBD CSI Migration

【alpha】Portworx CSI Migration

【beta】Azure Disk CSI Migration

【beta】AWS EBS CSI Migration

【beta】GCE PD CSI Migration

【GA】fsGroup 特性

【GA】通用临时卷 Generic ephemeral volumes 特性

### kubernetes v1.24 

修复一些 bug。

【GA】卷扩容特性

【GA】存储空间跟踪 Storage capacity tracking 特性

【GA】Azure Disk CSI Migration

【GA】OpenStack Cinder CSI Migration

### kubernetes v1.25

修复一些 bug。

【beta】vSphere CSI Migration

【beta】Portworx CSI Migration

【GA】CSI ephemeral inline volumes

【GA】AWS EBS CSI Migration

【GA】GCE PD CSI Migration

### kubernetes v1.26 

修复一些 bug。

【alpha】增加跨命名空间快照克隆特性。注意：此特性后面不再推进，可以查看 KEP-3294 和 issue-3294 了解详情，这么好的特性不继续推进真是可惜了。

【GA】Azure File CSI Migration

【GA】vSphere CSI Migration

### kubernetes v1.27

修复一些 bug。

【alpha】增加快照组特性。可以利用此特性给一组特定的 PVC 创建一组快照。

【beta】ReadWriteOncePod 访问模式

### kubernetes v1.28

修复一些 bug。删除了 Ceph RBD in-tree plugin 和 Ceph FS in-tree plugin 代码。

### kubernetes v1.29 

修复一些 bug。

### kubernetes v1.30 

修复一些 bug。

以上每个版本对应的 CSI 详情都可以在 kubernetes 的官方 github 仓库找到相关记录，有兴趣的小伙伴可以多翻翻：https://github.com/kubernetes/kubernetes/tree/master/CHANGELOG。

## CSI 的现状

从 CSI 的进化历程中可以发现 CSI 的特性是比较完善的，但是也会发现 CSI 的这些新特性是跟随 kubernetes 版本发布的，kubernetes 版本越新，则支持的特性越多。所以又回到了最初那个困扰用户的问题：想要最新最全的特性是需要升级 kubernetes 的。但是这又与最初的场景有不一样的地方就是用户可以在当前的协议版本框架下进行各种特性开发，这就给了存储对接的开发者们很大的想象空间和发挥空间，而不是像之前一样什么都做不了。

从上面的进化历程也能看得到，kubernetes 核心代码中 in-tree 的存储代码基本都慢慢移除，全部转为 CSI 实现。各大公有云和存储厂商都积极拥抱 CSI，这对 kubernetes 的核心代码也是好事，很多之前的 in-tree 插件代码都被移除，核心代码更加干净好维护。

在这里也打个广告：CubeFS 社区最近成立了 CSI SIG，欢迎各位小伙伴来一起搞事。具体如何参与请看官方仓库 cubefs-community 文档：https://github.com/cubefs/cubefs-community/blob/master/sig-csi/README_CN.md。

作者：mingwei gong

## 参考

https://kubernetes-csi.github.io/docs/

https://github.com/kubernetes/kubernetes/tree/master/CHANGELOG

https://kubernetes.feisky.xyz/extension/volume/flex-volume

https://github.com/kubernetes/enhancements/tree/master/keps/sig-storage

https://ibm.github.io/kubernetes-storage/flexvolume/