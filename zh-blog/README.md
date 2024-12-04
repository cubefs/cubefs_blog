---
pages: [
  {
    text: '全部',
    type: 'all',
  },
  {
    text: '官方动态',
    children: [
      {
        title: '请查收！这有一份成为CubeFS Contributor的邀请函',
        filePath: 'news/activities01',
        date: '2023-04-27',
        author: 'CubeFS',
        coverUrl: '/images/cover/activities0100.png',
        desc: '2022年7月云原生计算基金会 (CNCF) 正式宣布，分布式存储系统 CubeFS 从沙箱项目(Sandbox) 晋级为孵化(Incubating)项目。
作为首个国产并进入 CNCF 孵化阶段的开源项目，CubeFS 具备安全可靠、高性能、强扩展性、易于运维等特点，真正意义上填补了开源云原生存储领域的空白。',
      },
      {
        title: 'v3.2.1 版本正式发布',
        filePath: 'news/v321',
        date: '2023-03-16',
        author: 'CubeFS',
        coverUrl: '/images/cover/v32100.png',
        desc: 'CubeFS Release 3.2.1 版本发布于2023年3月16日，本版更新内容较多，涉及几个大 feature 的新增，部分稳定性优化以及 bugfix。',
      },
      {
        title: 'v3.2.0 版本正式发布',
        filePath: 'news/v320',
        date: '2022-12-06',
        author: 'CubeFS',
        coverUrl: '/images/cover/v32000.png',
        desc: 'CubeFS Release 3.2.0 版本发布于2022年10月14日，本版重点工作是数据层纠删码子系统BlobStore的优化',
      },
      {
        title: 'v3.1.0 版本正式发布',
        filePath: 'news/v310',
        date: '2022-08-23',
        author: 'CubeFS',
        coverUrl: '/images/cover/v31000.png',
        desc: '3.1.0 版本为支撑更多的业务场景，推出以下关键特性：
              增强的混合云多级读缓存功能，提高AI混合云训练加速效果
              支持卷级别QoS流控，解决多租户场景资源分配问题
              支持双副本，满足对数据耐久性要求低的业务的降本诉求'
      },
      {
        title: 'CubeFS从CNCF沙箱升级为孵化项目',
        filePath: 'news/incubating',
        date: '2022-07-03',
        author: 'CubeFS',
        coverUrl: '/images/cover/incubating00.png',
        desc: '云原生计算基金会(CNCF)正式宣布，分布式存储系统CubeFS从沙箱项目(Sandbox) 晋级为孵化(Incubating)项目。
CubeFS是首个由国人研发并进入CNCF孵化，具备完整的对象及文件存储能力的云原生存储产品，真正意义上填补了开源分布式存储的空白。',
      },
      {
        title: 'v3.0.0 版本正式发布',
        filePath: 'news/v300',
        date: '2022-04-22',
        author: 'CubeFS',
        coverUrl: '/images/cover/v30000.png',
        desc: 'v3.0.0版本为适配更多的业务场景，推出以下关键特性：
              基于多级缓存的混合云加速，解决云上云下数据延迟问题
              提供低成本的在线纠删码存储引擎
              支持单副本存储，适配数据低耐久性需求的业务
              客户端支持热更新，升级时服务不中断
              性能优化，包括元数据节点迁移、大目录分页拉取、数据节点的内存池使用和Metric分级等方向
              工程使用go mod管理'
      },
      {
        title: 'CubeFS完成第三方安全审核',
        filePath: 'news/security-audit',
        date: '2024-01-09',
        author: 'CubeFS',
        coverUrl: '/images/cover/security_audit_cover.jpg',
        desc: 'CubeFS 宣布完成第三方安全审核， 审核由 Ada Logics 与 CubeFS 维护者、OSTIF 和 CNCF 合作完成。'
      },
    ]
  },
  {
    text: '用户案例',
    children: [
      {
        title: 'CubeFS 如何在 OPPO 混合云平台中加速 AI 训练3倍',
        filePath: 'useCases/oppo-ai',
        date: '2023-03-30',
        author: '张天炯',
        coverUrl: '/images/cover/oppo-ai00.png',
        desc: '2021年，Oppo基于分布式文件系统CubeFS构建了一个大规模机器学习平台的一站式服务。2022年，为了优化成本，他们开始在机器学习平台上构建了一个混合GPU云。尽管大多数GPU计算能力仍留在Oppo的私有云中，但当其资源不足时，任务会被调度到公共GPU云进行训练。这种混合GPU云架构在CubeFS存储方面提出了几个挑战，包括存储性能、弹性和容量降低、安全问题和高成本。',
      },
    ]
  },
  {
    text: '技术揭密',
    children: [
      {
        title: '纠删码子系统介绍',
        pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/CubeFs纠删码子系统介绍.pdf',
        type: 'pdf',
        date: '2021-12-09',
        author: '唐之享',
        coverUrl: '/images/cover/20230427100114704.png',
        desc: '一般而言，存储系统会通过存储一定量的冗余数据来保证可靠性。当系统出现故障时，冗余数据可以继续为用户提供各种服务，并且修复因故障而丢失的部分数据。目前主流的容错机制大致分为多副本机制和纠删码机制两类。由于冗余数据会增大存储成本，而多副本相较于纠删码的冗余存储开销过于巨大，目前纠删码已成为了当前云存储系统的主流数据冗余方式，并且持续成为学术界和工业界在存储领域的研究热点。',
      },
      {
        title: 'CubeFS技术揭秘 ｜ 接口原子性技术实现 ',
        filePath: 'technicalInsights/Secret_of_CubeFS_Technology_Implementation_of_Api_Atomicity',
        date: '2023-08-24',
        author: 'Victor Zeng',
	      coverUrl: '/images/cover/v32102.png',
        desc: '这篇文章介绍了CubefS分布式文件系统中API原子性的实现方式。作者通过对API原子性的定义和实现进行讲解，阐述了CubefS系统中的文件操作保证了原子性，从而提高了系统的可靠性和稳定性。',
      },
      {
        title: 'CubeFS技术揭秘 ｜ 元数据子系统设计 ',
        filePath: 'technicalInsights/Secret_of_CubeFS_Technology_Metadata_Subsystem_Design',
        date: '2023-08-02',
        author: 'Zhixiang Tang',
	      coverUrl: '/images/cover/Group802.png',
        desc: '前面CubeFS存储技术揭秘-元数据管理【1】的文章中介绍了CubeFS元数据系统的设计理念，文章中有提到传统的文件系统存在元数据节点单点的瓶颈问题，CubeFS可扩展的元数据设计有效的解决单点瓶颈问题。本文将从更详细的角度介绍元数据系统在可靠性、可扩展性以及访问性能的一些实现和思考。阅读本篇文章之前，你需要了解CubeFS的系统架构，以及各个关键组件的职责和系统的名词概念。',
      },
      {
        title: 'cubefs“源”理解读_ Master源码解读之raft',
        filePath: 'technicalInsights/Understanding_cubefs_source_code_The_RAFT_of_master',
        date: '2023-08-29',
        author: 'Huocheng Wu',
	      coverUrl: '/images/cover/CubFS_raft_01.png',
        desc: '这篇文章讲解了CubefS分布式文件系统中的Master节点的RAFT协议实现细节。通过对Master节点的状态机和RAFT协议的交互过程进行分析，读者可以更好地理解CubefS系统的工作原理。',
      },
      {
        title: 'cubefs“源”理解读_ Master源码解读之HTTP',
        filePath: 'technicalInsights/Understanding_cubefs_source_code_The_HTTP_server_in_master',
        date: '2023-08-29',
        author: 'Huocheng Wu',
	      coverUrl: '/images/cover/CubFS_master_http_01.png',
        desc: '这篇文章介绍了CubefS分布式文件系统中Master节点的HTTP服务器实现。通过分析HTTP服务器的架构和代码实现，读者可以更好地理解CubefS系统中Master节点的工作原理和文件管理功能。',
      },
      {
        title: 'CubeFS技术揭秘 ｜ 多AZ纠删码容灾技术',
        filePath: 'technicalInsights/Secret_of_CubeFS_Multi_AZ_Erasure_Coding_Disaster_Tolerance',
        date: '2023-11-30',
        author: 'Jie Shen',
        coverUrl: '/images/cover/CubeFS_Multi_AZ_Erasure_Coding_Disaster_Tolerance.png',
        desc: '本文主要介绍CubeFS纠删码子系统Blobstore的AZ级容灾方案。由于多副本模式存储成本高效率低、多AZ情况下网络带宽消耗很大、且需要及时保证数据一致性等劣势，我们接下来只讨论纠删码（Erasure Coding）存储模式。Blobstore使用纠删码技术将用户数据编码计算并持久化存储于多个AZ中，以实现高可用性和容灾能力。主要包括多AZ的EC计算原理，及如何保持高可用地写入、高效读取、减少跨AZ的数据恢复。关于EC数据的可靠性公式，在此就不赘述，可以参看我们相关的文章。'
      },
      {
        title: 'CubeFS技术揭秘 ｜ CubeFS助力大模型训练加速',
        filePath: 'technicalInsights/cubeFS_helps_accelerate_large_model_training',
        date: '2023-12-11',
        author: 'Hongyan Wang',
        coverUrl: '/images/cover/cubeFS_helps_accelerate_large_model_training.png',
        desc: '当OpenAI于2022年发布ChatGPT，人工智能进入了全新时代，大模型技术的出现引发了各行业的颠覆性创新，带来了应用体验的显著提升和新的商业化机会，同时也对云基础设施提出了新的挑战；存储作为云基础设施，本质是数据服务，是大模型技术栈的重要一环；大型模型时代，寻找适合的云存储解决方案显得尤为重要。本文将结合CubeFS及OPPO自研AndesGPT，探讨大型模型时代的数据存储解决之道，助力企业决胜大模型时代。'
      },
      {
        title: 'Raft在CubeFS 纠删码系统中的应用实践',
        filePath: 'technicalInsights/CubeFS_EC_Engine_Raft_Application_Practice',
        date: '2023-12-22',
        author: 'Zongchao Hu',
        coverUrl: '/images/cover/CubeFS_EC_Engine_Raft_Application_Practice.png',
        desc: '本文主要介绍CubeFS纠删码(Erasure Coding)服务的元数据管理模块(Blobstore/ClusterMgr）、Raft算法实践以及日常运维建议。'
      },
      {
        title: 'Kubernetes CSI 历史与现状',
        filePath: 'technicalInsights/The_history_of_k8s_csi',
        date: '2024-04-18',
        author: 'Mingwei Gong',
        coverUrl: '/images/cover/CubeFS_k8s_csi_history_banner.png',
        desc: '一篇 kubernetes 存储相关的科普文，主要介绍 kubernetes 存储插件协议的演进过程。'
      },
      {
        title: '内核文件系统技术揭秘',
        filePath: 'technicalInsights/The_Secrets_of_Kernel_File_System_Technology',
        date: '2024-12-04',
        author: 'Wu Huocheng',
        coverUrl: '/images/cover/kernel-client.png',
        desc: '一篇介绍内核态客户端的文章，简要地介绍部分软件架构和客户端的使用方法。'
      },
    ]
  },
  {
    text: '最佳实践',
    children: [
      {
        title: '与CubeFS共创未来：BIGO的实践与思考',
        filePath: 'bestPractices/bigo',
        date: '2023-05-05',
        author: '刘冰星',
        coverUrl: '/images/cover/bigo00.png',
        desc: '随着BIGO机器学习平台模型和数据量的增长，对底层依赖的文件系统提出了更多的需求，如多租户、高并发、多机房高可用、高性能、云原生等特性',
      },
      {
        title: 'OPPO数据湖统一存储技术实践',
        filePath: 'bestPractices/datalake_introduction',
        date: '2022-06-23',
        author: '何小春',
        coverUrl: '/images/cover/datalake.png',
        desc: 'OPPO是一家智能终端制造公司，有着数亿的终端用户，手机 、IoT设备产生的数据源源不断，设备的智能化服务需要我们对这些数据做更深层次的挖掘。海量的数据如何低成本存储、高效利用是大数据部门必须要解决的问题。目前业界流行的解决方案是数据湖，本次Xiaochun He老师介绍的OPPO自研数据湖存储系统CBFS在很大程度上可解决目前的痛点。',
      },
      {
        title: '云原生分布式存储CubeFS在数据湖存储的探索与实践',
        pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/云原生分布式存储cubefs在数据湖存储的探索与实践-天炯.pdf',
        type: 'pdf',
        date: '2022-06-23',
        author: '张天炯',
        coverUrl: '/images/cover/20230427094708986.png',
        desc: '目前CBFS-2.x版本已开源，支持在线EC、湖加速、多协议接入等关键特性的3.0版本预计将于2021年10月开源；后续CBFS会增加存量HDFS集群直接挂载（免数据搬迁），冷热数据智能分层等特性，以便支持大数据及AI原架构下存量数据平滑入湖。',
      },
      {
        title: 'OPPO使用CubeFS助力机器学习',
        pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/ChubaoFs_KubeCon_20211209.pdf',
        type: 'pdf',
        date: '2021-12-09',
        author: '常亮',
        coverUrl: '/images/cover/20230427093917137.png',
        desc: '2021年，Oppo基于分布式文件系统CubeFS构建了一个大规模机器学习平台的一站式服务。2022年，为了优化成本，他们开始在机器学习平台上构建了一个混合GPU云。尽管大多数GPU计算能力仍留在Oppo的私有云中，但当其资源不足时，任务会被调度到公共GPU云进行训练。这种混合GPU云架构在CubeFS存储方面提出了几个挑战，包括存储性能、弹性和容量降低、安全问题和高成本。',
      },
      {
        title: '京东如何使用Harbor搭建私有镜像中心，维护时间减少60%',
        filePath: 'bestPractices/jd',
        date: '2020-08-13',
        author: '京东',
        coverUrl: '/images/cover/jd_cover_zh.png',
        desc: 'JD.com的基础设施部门负责开发超大规模容器化和Kubernetes平台，为京东的所有业务提供支持，包括为超过3.8亿活跃用户提供服务的网站。在2016年，该团队需要一个云原生的注册表，为其私有镜像中央仓库提供维护服务。',
      },
      {
        title: '京东Elasticsearch使用ChubaoFS实现计算存储分离',
        filePath: 'bestPractices/elasticsearch',
        date: '2020-02-08',
        author: '京东',
	      coverUrl: '/images/cover/es_cubefs_cover_zh.png',
        desc: 'Elasticsearch 是一个开源的分布式 RElasticsearchTful 搜索引擎，作为一个分布式、可扩展、实时的搜索与数据分析引擎，它可以快速存储、搜索和分析大量数据。同时，Elasticsearch 也支持具有负责搜索功能和要求的应用程序的基础引擎， 因此可以应用在很多不同的场景中。',
      },
      {
        title: '纠删码技术在CubeFS的应用和实践 ',
        filePath: 'bestPractices/erasure_code_technology_practice',
        date: '2023-07-31',
        author: '曾源',
	      coverUrl: '/images/cover/erasure_code_technology_practice.png',
        desc: '一般而言，存储系统会通过存储一定量的冗余数据来保证可靠性。当系统出现故障时，冗余数据可以继续为用户提供各种服务，并且修复因故障而丢失的部分数据。目前主流的容错机制大致分为多副本机制和纠删码机制两类。由于冗余数据会增大存储成本，而多副本相较于纠删码的冗余存储开销过于巨大，目前纠删码已成为了当前云存储系统的主流数据冗余方式，并且持续成为学术界和工业界在存储领域的研究热点。',
      },
    ]
  },
  {
    text: '入门系列',
    children: [
      {
        filePath: 'startSeries/cubeFS_object_storage_lifecycle_management_strategies',
        date: '2024-04-22',
        author: 'CubeFS',
        title: 'CubeFS十分钟入门 - 轻松掌握CubeFS对象存储生命周期管理策略',
        coverUrl: '/images/cover/cubeFS_object_storage_lifecycle_management_strategies.jpg',
        desc: '默认情况下，CubeFS中的数据会永久保存，为了使数据在整个生命周期内经济高效地存储，CubeFS支持兼容AWS S3的对象存储生命周期管理策略。目前CubeFS支持生命周期过期删除策略，可以有效清理过期数据，释放存储资源，本文将带领大家了解如何使用CubeFS对象存储生命周期管理策略。',
      },
    ]
  },
  {
    text: '社区例会',
    children: [
      {
        filePath: 'communityMeetings/meetings',
        date: '2023-04-13',
        author: 'CubeFS',
        title: '往期会议纪要',
        coverUrl: '/images/cover/meeting.png',
        desc: 'CubeFS社区会议纪要',
      },
    ]  
  }
]
---
