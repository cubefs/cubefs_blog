---
blog: true
---

# CubeFS完成第三方安全审核

CubeFS 宣布完成第三方安全审核， 审核由[Ada Logics](https://adalogics.com/) 与 CubeFS 维护者、OSTIF 和 CNCF 合作完成。

Ada Logics 是一家成熟的安全审计公司，在云原生生态系统中的项目审计方面拥有丰富的经验，过去审计过其他几个 CNCF 项目，例如 Containerd、Helm、 Kubernetes、Vitess、Notary v2 等。 CubeFS 很高兴与 Ada Logics 合作，展示对 CubeFS 消费者安全的坚定承诺。

Ada Logics 对 CubeFS 进行了全面的安全审计，重点关注威胁建模、手动代码审查和供应链安全。审计发现了 12 个安全问题，严重程度从低到中，其中 5 个被分配了 CVE。

|  | Name | CVE | Severity |
|------|-----|------|------|
| 1 |  Authenticated users can crash the CubeFS servers with maliciously crafted requests | CVE-2023-46738 | Moderate |
| 2 |  Timing attack can leak user passwords | CVE-2023-46739 | Moderate |
| 3 |  Insecure random string generator used for sensitive data | CVE-2023-46740 | Moderate |
| 4 |  CubeFS leaks magic secret key when starting Blobstore access service | CVE-2023-46741 | Moderate |
| 5 |  CubeFS leaks users key in logs | CVE-2023-46742 | Moderate |

除了这五个 CVE 之外，其他发现的问题还包括潜在的死锁、零取消引用恐慌、slowloris 问题、发布版本缺少签名、CubeFS 安全策略问题以及不安全的加密原语。

CubeFS 团队已修复审核结束时发现的所有问题。所有问题均已在 CubeFS v3.3.1 中得到解决。
此外，Ada Logics 建议 CubeFS 在其 SAST 套件中添加额外的 SAST 工具，该套件也已添加。在审计过程中，审计人员使用 SAST 工具来发现真正的积极安全问题，这些 SAST 工具现在在 CubeFS 的 CI 中运行，以便测试未来的拉取请求是否存在潜在的有害安全更改。

在软件供应链安全方面，Ada Logics 发现 CubeFS 在某些领域很好地维护了其软件开发生命周期，但缺少减轻众所周知的供应链风险的实质性要素。最突出的是，CubeFS 的版本缺乏来源。来源是 SLSA 的关键要素，SLSA 是减轻供应链风险的最先进的框架。 在版本中添加可验证的来源可以让消费者减轻 CubeFS 软件开发生命周期中的多种攻击媒介。 CubeFS 已创建 [a public issue](https://github.com/cubefs/cubefs/issues/2883) 跟踪正在进行的工作以支持可验证的出处。 

CubeFS 维护开放的安全披露政策。如果您有兴趣为 CubeFS 的安全做出贡献，欢迎您通过以下方式提交您的发现 [the security guidelines](https://github.com/cubefs/cubefs/security). 此外，CubeFS还有 [an open meeting schedule](https://github.com/cubefs/cubefs-community-meetings/wiki/Meeting-Schedule) 欢迎社区成员加入。 

我们要感谢 Ada Logics 团队、Adam 和 David Korczynski 及其团队的辛勤工作和贡献，使这次审计得以成功。我们要感谢 CubeFS 的团队，特别是 Leon Chang、Lei Zhang、Xiaochun He 和 Baijiaruo，感谢他们在整个参与过程中的清晰、专业和友善。他们是一支反应极其灵敏的团队，积极参与审计，不仅有利于参与，而且有利于项目用户和贡献者。最后，如果没有 CNCF 的资助和支持，这是不可能实现的。他们的贡献和基础使开源安全工作成为可能

Links:
*	[Full audit report](https://github.com/cubefs/cubefs/blob/master/security/CubeFS-security-audit-2023-report.pdf)
*	[CNCF blog post](https://www.cncf.io/blog/2024/01/08/cubefs-completes-security-audit/)
*	[Ada Logics blog post]()
*	[OSTIF blog post](https://ostif.org/cubefs-audit-complete/)
