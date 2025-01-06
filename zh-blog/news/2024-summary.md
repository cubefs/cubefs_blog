2024年是CubeFS发展的关键一年。

* CNCF云原生计算基金会正式宣布了CubeFS顺利毕业；
* 社区治理更加规范化；
* 基础夯实，在稳定性、性能、易用性方面大量优化；
* 更好的支持AI业务负载；
值此新年来临之际，我们对过去一年进行总结盘点，并展望未来一年的规划目标。

## 毕业一览

2024年12月24日，CNCF宣布CubeFS成功晋升为毕业（Graduated）项目， CubeFS成为首个由中国人主导研发在CNCF毕业的具备完整存储能力的项目。

![CubeFS graduation details](/images/blog/summary_1.png)

![CNCF graduation list](/images/blog/summary_2.png)

## 版本迭代

在过去一年中，CubeFS积极推进多个项目：首先，为增强系统稳定性，推出了3.3.2版；接着，为提升运维自动化能力，发布了3.4.0版，以解决大容量HDD坏盘等繁重的运维任务。此外，针对当前备受关注的AI场景，推出了3.5.0版，实现数据的自动冷热分层，同时在性能方面进行RDMA和内核文件系统的研发，并为公有云、混合云场景中推进分布式缓存的建设。

### 版本发布

#### 已发版本

* release-3.3.2
    * 在数据迁移方面大量性能优化，实现HDD盘迁移性能提升200%
    * 针对大容量HDD磁盘，优化DataNode启动速度
    * 支持卷禁止写、冻结、延迟删除能力
    * ObjectNode 支持sts and signature auth，post object协议
    * 完善优化SDK，提升存算分离场景访问信息
    * 支持客户端路由元数据压缩，海量客户端场景Master流量降低95%
* release-3.4.0
    * 支持坏盘自动化迁移，迁移任务可管理可查询，大幅提升自动化运维水平
    * 支持异常数据分区自动修复，最大程度保障分布式多节点分区多步操作一致性
    * 支持卷快照实验版本，保障全局一致性快照能力
    * 优化DataNode并行删除能力，降低大量数据删除时负载
* release-3.4.0-beta-rdma
    * 基于RDMA实现零拷贝能力，在数据读和direct写方面表现较好，约有10~40%性能提升，适用于对写入性能要求极高的业务场景
#### 待发版

* release-3.5.0
    * 存储分级： 引入数据冷热分层机制，使得数据可以根据其生命周期进行降冷操作，帮助业务进一步优化成本。
#### 待发布beta版

另外达到BETA标准的版本的有2个

* release-3.5.1
    * 分布式缓存：基于一致性hash，实现分布式缓存加速，支持跨云、多云场景的数据缓存加速，缓存支持多副本、就近访问等特性。
* develop-v3.6.0-kernel-rdma2
 内核版本，着眼于GPU加速等高性能场景。

### 主干迭代

由于3.4.0和3.5.0的开发周期较长，当前版本与主干分叉。我们计划在3.5.1版本中重新回主干。因此，部分功能未在这两个版本中体现，但它们已合入主干，并将持续迭代，预计将在3.5.1版本中集中发布。

#### 纠删码子系统

1. blobnode本地数据巡检优化、修复慢盘导致panic问题

2. scheduler 支持批量删除

3. scheduler 支持并发磁盘下线能力，便于机柜规整

4. 支持卷分配优化，提升分配效率

5. blobnode支持区分读和写的限流

#### 对象子系统

跨区域复制，使用跨区域复制功能可以将跨不同 CubeFS 数据中心的存储桶 Bucket 中的数据自动、异步（近实时）复制到目标存储区域，满足业务的多地域备份或数据的"一地生产、异地分析"需求。

## 社区发展

### CubeFS实用性跻身Batch/AI/ML 计算技术领域TOP4

在2024年11月北美KubeCon大会上发布的CNCF技术景观雷达报告中，在 Batch/AI/ML 计算技术领域，开源分布式存储系统CubeFS 与 Apache Airflow、Kubeflow 等一同处于技术雷达的 “采用（adopt）” 位置，实用性评分达 +40，跻身前四，是Batch/AI/ML相关项目中的最成熟和已经被Adopt的少数项目之一。

![CubeFS Top 4](/images/blog/summary_3.png)


### 活跃度情况

* Commits: 5165 -> 7883 (增长 %52)
* Code committers: 76 (11家公司) -> 142  (22家公司) (增长 86%)
* Pull requests: 2149-> 2620 (增长 21%)
* Contributors: 229 (23家公司) -> 379 (42家公司) (增长  65%)
* Contributions: 12922-> 16845(增长 30%)
### 社区治理

* 构建了完整的社区治理体系，成立技术指导委员会（TSC）
* 提供与其章程相关的决策和监督；定义项目的价值观和结构；负责项目的顶层设计、路线图的制定和社区管理。建立和维护项目的整体技术治理指南
* 完善Maintainer、Committer晋升通道，明确选拔资格
* 明确路线图的制定步骤和规则
* 成立SIG小组，明确SIG组织的定位，成员的管理规则以及生命周期
### 用户新增（部分）

|**公司**|**应用场景**|**使用方式**|**数据存储形式**|
|:----|:----|:----|:----|
|shopee|AI/大数据|posix|多副本|
|鼎桥科技|视频文件|posix/s3|多副本|
|数字广东|-|S3|多副本|
|酷哇机器人|大数据|s3|多幅本|
|MetaApp|大数据|S3|纠删码|
|网易邮箱|备份数据|posix/s3|多副本|
|greaterheat|区块链|posix|纠删码|
|零跑|AI/大数据|S3|多副本|
|和讯|k8s|posix|-|

## 后续规划

### 系统维度

1. 元数据系统 RocksDB持久化， 在社区中对元数据持久性有强烈需求，将在这一领域增强社区团队之间的合作。
2. 在混合云管理方面，我们将更好地支持公共云和混合云，提高CubeFS的云能力。例如，通过与S3接口集成，实现混合云功能，利用公有云的低成本和无限弹性资源，进行CubeFS原生存储与公有云存储的无缝管理，屏蔽底层存储系统和资源的差异。
3. 在分布式缓存方面，持续迭代，满足云上更为广泛的加速方面的需求。
4. 实现对象的多版本能力，支持存储桶中对象的多个版本，并允许对指定版本进行检索、删除或还原。这将有助于恢复因用户误删或应用程序故障而丢失的数据。
### 社区治理方面

5. 加强社区自治规则的实施，推动社区内的标准化的落地。
6. 提高项目在国外的知名度，吸引更多国际用户。
## 致谢

感谢kevin-wangzefeng，kevin是CNCF TOC成员，同时担任CubeFS毕业Sponsor，在毕业流程推进中给予大量及时的指导和帮助，帮助CubeFS得以顺利毕业，在此代表CubeFS社区感谢kevin的辛苦付出！

感谢CNCF Storage-tag、CNCF TOC全体成员，在加入CNCF后给与了很多指导和帮助，为CubeFS带来了更为广泛的关注和支持。

感谢网易、BIGO、贝壳、京东、OPPO等广大的用户群体的支持和信任。

感谢社区的贡献者们（按字母顺序）为项目的完善所做的孜孜不倦的努力！

aaronwu2010

baihailong

baijiaruo

BillXiang

bjguoweilong

chihe

Cloudstriff

cuishuang

dailiwang

Haifeng Liu

Huweicai

huyao2

JasonHu520

Kursat Aktas

leonrayang

lily-lee

linfangrong

liyubo4

lizhenzhen36

M1eyu2018

mawei029

mingwei

morphes1995

nasuiyile

NaturalSelect

Reliey

setcy

shuqiang-zheng

slasher

tangdeyi

true1064

Victor1319

wangxiaodong1

wuwenjia1

xiaojunxiang

xiaopeng42

xiejian

xuxihao1

yhjiango

Youngjun

Yu-Ang Cao

zhangchuanqing1

zhangjianwei

zhangmofei3

zhangwenxin1

zhaochenyang

Zhenzhen-Li-0618

zhuhongyin

zhumingze



