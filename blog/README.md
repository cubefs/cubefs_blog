---
pages: [
  {
    text: 'All',
    type: 'all',
  },
  {
    text: 'News',
    children: [
      {
        title: 'Please check! Here is an invitation to become a CubeFS Contributor',
        filePath: 'news/activities01.md',
        date: '2023-04-27',
        author: 'CubeFS',
        coverUrl: '/images/cover/activities0100.png',
        desc: 'In July 2022, the Cloud Native Computing Foundation (CNCF) officially announced that the distributed storage system CubeFS was promoted from a Sandbox project to an Incubating project.As the first domestic open source project to enter the CNCF incubation stage, CubeFS has the characteristics of being secure, reliable, high-performance, highly scalable, and easy to operate, truly filling the gap in the open source cloud native storage field.',
      },
      {
        title: 'CubeFS v3.2.1 Official Release',
        filePath: 'news/v321.md',
        date: '2023-03-16',
        author: 'CubeFS',
        coverUrl: '/images/cover/v32100.png',
        desc: 'CubeFS Release 3.2.1 was released on March 16, 2023, with many updates including new features, stability optimizations, and bugfixes.',
      },
      {
        title: 'CubeFS v3.2.0 Official Release',
        filePath: 'news/v320.md',
        date: '2022-12-06',
        author: 'CubeFS',
        coverUrl: '/images/cover/v32000.png',
        desc: 'CubeFS Release 3.2.0 was released on October 14, 2022. The main focus of this version was the optimization of the BlobStore subsystem for the data layer erasure coding.',
      },
      {
        title: 'CubeFS v3.1.0 Official Release',
        filePath: 'news/v310.md',
        date: '2022-08-23',
        author: 'CubeFS',
        coverUrl: '/images/cover/v31000.png',
        desc: 'CubeFS v3.1.0 introduces the following key features to support more business scenarios:
              Enhanced multi-level read cache for hybrid cloud, improving AI hybrid cloud training acceleration
              Support for volume-level QoS flow control, solving resource allocation issues in multi-tenant scenarios
              Support for dual-replica, meeting the cost reduction demand for businesses with low data durability requirements',
      },
      {
        title: 'CubeFS accepted as a CNCF Incubating project',
        filePath: 'news/incubating.md',
        date: '2022-07-03',
        author: 'CubeFS',
        coverUrl: '/images/cover/incubating00.png',
        desc: 'The Cloud Native Computing Foundation (CNCF) officially announced that the distributed storage system CubeFS has been promoted from a Sandbox project to an Incubating project.CubeFS is the first cloud-native storage product developed by Chinese people that has entered CNCF incubation and has complete object and file storage capabilities, truly filling the gap in open-source distributed storage.',
      },
      {
        title: 'CubeFS v3.0.0 Official Release',
        filePath: 'news/v300',
        date: '2022-04-22',
        author: 'CubeFS',
        coverUrl: '/images/cover/v30000.png',
        desc: 'Version 3.0.0 introduces the following key features to adapt to more business scenarios:
              Hybrid cloud acceleration based on multi-level caching, solving the problem of data latency between cloud and on-premises
              Low-cost online erasure coding storage engine
              Support for single-replica storage, adapting to business needs for low data durability
              Client-side hot updates without service interruption during upgrades
              Performance optimizations, including metadata node migration, large directory page retrieval, data node memory pool usage, and metric grading
              Go mod used for project management',
      },
      {
        title: 'CubeFS completes third-party security audit',
        filePath: 'news/security-audit',
        date: '2024-01-09',
        author: 'CubeFS',
        coverUrl: '/images/cover/security_audit_cover.jpg',
        desc: 'CubeFS is happy to announce the completion of its third-party security audit. The audit was conducted by Ada Logics in collaboration with the CubeFS maintainers, OSTIF and the CNCF.',
      },
      {
        title: 'CubeFS in NetEase game business practice',
        filePath: 'news/cubefs_in_netease_game_business_practice',
        date: '2024-05-24',
        author: 'CubeFS',
        coverUrl: '/images/cover/cubefs_in_netease_game_business_practice.png',
        desc: 'Elasticsearch hosting platform is the elasticsearch saas platform of NetEase Games internal benchmark against external cloud providers. It can quickly create, easily manage, expand and shrink ES clusters, and simplify complex operation and maintenance operations.',
      },
      {
        title: 'Play with object storage',
        filePath: 'news/play_with_object_storage',
        date: '2024-05-24',
        author: 'CubeFS',
        coverUrl: '/images/cover/play_with_object_storage.png',
        desc: 'Through this article, users are familiar with the configuration of CubeFS object gateway and how to access object resources through the open source Amazon S3 command line tool s3cmd.',
      },
      {
        title: 'CubeFS Autofs practice',
        filePath: 'news/cubefs_autofs_practice',
        date: '2024-05-24',
        author: 'CubeFS',
        coverUrl: '/images/cover/cubefs_autofs_practice.png',
        desc: 'This article focuses on CubeFS support for mount, Autofs features, and their application in conjunction with surrounding ecosystems such as SSSD [3], and LDAP [4].',
      },
      {
        title: 'CNCF Report: CubeFS Ranks Among the Top 4 in the Field of Batch/AI/ML Computing Technologies',
        pdfUrl: 'https://cubefs-rs.heytapdownload.com/website-file/CNCF-Tech-Radar-Custom-Survey-Research-Insights.pdf',
        type: 'pdf',
        date: '2024-11-14',
        author: 'CNCF',
        coverUrl: '/images/cover/images/cover/tech-landscape-radar.jpg',
        desc: "In the CNCF Technology Landscape Radar report released at the North America KubeCon conference in November 2024, CubeFS is positioned in the 'Adopt' category on the technology radar, with a usability score of +40, ranking among the top four.",
      },

    ]
  },
  {
    text: 'Use Cases',
    children: [
      {
        title: 'How CubeFS Accelerates AI training 3x in OPPO Hybrid Cloud platform',
        filePath: 'useCases/oppo-ai',
        date: '2023-03-30',
        author: 'Tianjiong Zhang',
        coverUrl: '/images/cover/oppo-ai00.png',
        desc: 'In 2021, Oppo built a one-stop service for a large-scale machine learning platform based on the distributed file system CubeFS. In 2022, to optimize costs, they began to build a hybrid GPU cloud on machine learning platforms. While the majority of the GPUs computing power remained in Oppo’s private cloud, when its resources were insufficient tasks would be scheduled to the public GPU cloud for training. This hybrid GPU-cloud architecture presented several challenges in CubeFS storage, including storage performance, lowered elasticity and capacity, security issues, and high costs.',
      },
    ]
  },
  {
    text: 'Technical Insights',
    children: [
      {
        title: 'CubeFS Erasure Code Subsystem Introduction',
        pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/CubeFs纠删码子系统介绍.pdf',
        type: 'pdf',
        date: '2021-12-09',
        author: 'Zhixiang Tang',
        coverUrl: '/images/cover/20230427100114704.png',
        desc: 'Generally speaking, a storage system ensures reliability by storing a certain amount of redundant data. When the system fails, the redundant data can continue to provide users with various services and repair some data lost due to the failure. The current mainstream fault-tolerant mechanisms are roughly divided into two types: multi-copy mechanism and erasure code mechanism. Since redundant data will increase storage costs, and the redundant storage overhead of multiple copies is too large compared to erasure codes, erasure codes have become the mainstream data redundancy method for current cloud storage systems, and continue to be a popular topic in the academic world. and industry research hotspots in the field of storage.',
      },
      {
        title: 'Secret of CubeFS Technology ｜ Implementation of Api Atomicty ',
        filePath: 'technicalInsights/Secret_of_CubeFS_Technology_Implementation_of_Api_Atomicity',
        date: '2023-08-24',
        author: 'Victor Zeng',
	      coverUrl: '/images/cover/v32102.png',
        desc: 'This article introduces the implementation of API atomicity in the CubefS distributed file system. By explaining the definition and implementation of API atomicity, the author illustrates how file operations in the CubefS system ensure atomicity, thereby improving the reliability and stability of the system.'
      },
      {
        title: 'Secret of CubeFS Technology | Metadata Subsystem Design',
        filePath: 'technicalInsights/Secret_of_CubeFS_Technology_Metadata_Subsystem_Design',
        date: '2023-08-02',
        author: 'Zhixiang Tang',
        coverUrl: '/images/cover/Group802.png',
        desc: 'The previous article CubeFS Storage Technology Revealed - Metadata Management [1] introduced the design concept of the CubeFS metadata system. The article mentioned that the traditional file system has a single-point bottleneck problem of metadata nodes, and the scalable metadata of CubeFS The design effectively solves the single-point bottleneck problem. This article will introduce some implementations and considerations of metadata systems in terms of reliability, scalability, and access performance from a more detailed perspective. Before reading this article, you need to understand the system architecture of CubeFS, as well as the responsibilities of each key component and the noun concept of the system.',
      },
      {
        title: 'Understanding cubefs source code | The RAFT of master',
        filePath: 'technicalInsights/Understanding_cubefs_source_code_The_RAFT_of_master',
        date: '2023-08-29',
        author: 'Huocheng Wu',
        coverUrl: '/images/cover/CubFS_raft_01.png',
        desc: 'This article explains the implementation details of the RAFT protocol in the Master node of the CubefS distributed file system. By analyzing the state machine of the Master node and the interaction process of the RAFT protocol, readers can better understand how the CubefS system works.'
      },
      {
        title: 'Understanding cubefs source code | The HTTP server of master',
        filePath: 'technicalInsights/Understanding_cubefs_source_code_The_HTTP_server_in_master',
        date: '2023-08-29',
        author: 'Huocheng Wu',
        coverUrl: '/images/cover/CubFS_master_http_01.png',
        desc: 'This article introduces the implementation of the HTTP server in the Master node of the CubefS distributed file system. By analyzing the architecture and code implementation of the HTTP server, readers can better understand the working principle and file management function of the Master node in the CubefS system.'
      },
      {
        title: 'Secret of CubeFS｜ Multi-AZ Erasure Coding Disaster Tolerance',
        filePath: 'technicalInsights/Secret_of_CubeFS_Multi_AZ_Erasure_Coding_Disaster_Tolerance',
        date: '2023-11-30',
        author: 'Jie Shen',
        coverUrl: '/images/cover/CubeFS_Multi_AZ_Erasure_Coding_Disaster_Tolerance.png',
        desc: 'This article introduces the AZ-level disaster tolerance scheme of the Blobstore erasure coding subsystem in CubeFS. Due to the disadvantages of high storage cost and low efficiency in the multi-replica storage mode, high network bandwidth consumption in the multi-AZ case, and the need to ensure data consistency in a timely manner, we will only discuss the erasure coding (EC) storage mode. Blobstore uses erasure coding to encode and calculate user data and persistently store it in multiple AZs, achieving high availability and disaster tolerance. The main content of the article includes the EC calculation principle in multiple AZs, as well as how to maintain high availability for writing, efficient reading, and reducing cross-AZ data recovery. The reliability for EC data is not elaborated here, and can be found in our related articles.'
      },
      {
        title: 'Application practice of Raft in CubeFS erasure code system',
        filePath: 'technicalInsights/CubeFS_EC_Engine_Raft_Application_Practice',
        date: '2023-12-22',
        author: 'Zongchao Hu',
        coverUrl: '/images/cover/CubeFS_EC_Engine_Raft_Application_Practice.png',
        desc: 'The given text describes the CubeFS erasure coding service’s metadata management module (Blobstore/ClusterMgr), Raft algorithm practice, and daily operation recommendations.'
      },
      {
        title: 'Kubernetes CSI history and current situation',
        filePath: 'technicalInsights/The_history_of_k8s_csi',
        date: '2024-04-18',
        author: 'Mingwei Gong',
        coverUrl: '/images/cover/CubeFS_k8s_csi_history_banner.png',
        desc: 'A popular science article about kubernetes storage, mainly introducing the evolution of kubernetes storage plugin protocol.'
      },
       {
        title: 'Introduction to CubeFS Quota Feature',
        filePath: 'technicalInsights/Introduction_to_CubeFS_quota_features',
        date: '2023-11-10',
        author: 'Yao Hu',
        coverUrl: '/images/cover/Introduction_to_CubeFS_quota_features.png',
        desc: 'In storage systems, quotas are a technique for managing and controlling the use of storage resources. They can help system administrators effectively manage storage resources, ensuring their rational allocation and utilization.'
      },
      {
        title: 'The Secrets of Kernel File System Technology',
        filePath: 'technicalInsights/The_Secrets_of_Kernel_File_System_Technology',
        date: '2024-12-4',
        author: 'Wu Huocheng',
        coverUrl: '/images/cover/kernel-client.png',
        desc: 'An article introducing a kernel-mode client, briefly describing part of the software architecture and the usage method of the client.'
      },
    ]
  },
  {
    text: 'Best Practices',
    children: [
    {
      title: "Co-creating the Future with CubeFS: BIGO's Practice and Reflection",
      filePath: 'bestPractices/bigo',
      date: '2023-05-05',
      author: 'Bingxing Liu',
      coverUrl: '/images/cover/bigo00.png',
      desc: 'platform grows, more requirements have been proposed for the underlying file system, such as multi-tenancy, high concurrency, multi-datacenter high availability, high performance, cloud-native, and other features',
    },
    {
      title: 'Data Lake Unified Storage Technology Practice',
      filePath: 'bestPractices/datalake_introduction',
      date: '2022-06-23',
      author: 'Xiaochun He',
      coverUrl: '/images/cover/datalake.png',
      desc: 'Oppo is a smart terminal manufacturing company with hundreds of millions of end users. The data generated by mobile phones and IoT devices is endless. The intelligent services of the devices need us to do deeper mining of this data. How to store mass data at low cost and use it efficiently is a problem that big data department must solve. The popular solution in the industry is the data lake, this teacher Xiaochun He introduced the OPPO self-developed data lake storage system CBFS to a large extent can solve the current pain point. ',
    },
    {
      title: 'OPPO Uses CubeFS to Empower Machine Learning',
      pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/ChubaoFs_KubeCon_20211209.pdf',
      type: 'pdf',
      date: '2021-12-09',
      author: 'Liang Chang',
      coverUrl: '/images/cover/20230427093917137.png',
      desc: 'In 2021, Oppo built a one-stop service for a large-scale machine learning platform based on the distributed file system CubeFS. In 2022, to optimize costs, they began to build a hybrid GPU cloud on machine learning platforms. While the majority of the GPUs computing power remained in Oppo’s private cloud, when its resources were insufficient tasks would be scheduled to the public GPU cloud for training. This hybrid GPU-cloud architecture presented several challenges in CubeFS storage, including storage performance, lowered elasticity and capacity, security issues, and high costs.',
    },
    {
      title: 'Exploration and Practice of Cloud-Native Distributed Storage CubeFS in Data Lake Storage',
      pdfUrl: 'https://ocs-cn-north1.heytapcs.com/cubefs/website-file/云原生分布式存储cubefs在数据湖存储的探索与实践-天炯.pdf',
      type: 'pdf',
      date: '2022-06-23',
      author: 'Tianjiong Zhang',
      coverUrl: '/images/cover/20230427094708986.png',
      desc: 'Currently CBFS-2. Version X is open source, and version 3.0, which supports key features such as online EC, Lake Acceleration, and multi-protocol access, is expected to be open source in October of the 2021; Future CBFS will increase the storage HDFS cluster directly mount (no data migration) , hot and cold data intelligent hierarchical features, in order to support big data and AI under the original structure of storage data smooth into the lake.',
    },
    {
      title: 'JD builds a private mirror center through Harbor, reducing maintenance costs by 60%',
      filePath: 'bestPractices/jd',
      date: '2020-08-13',
      author: 'jingdong',
      coverUrl: '/images/cover/jd_cover_en.png',
      desc: 'JD.com’s Infrastructure Department is responsible for developing the hyperscale containerized and Kubernetes platform that powers all facets of JD’s business, including the website, which serves over 380 million active customers. In 2016, the team was in need of a cloud native registry to provide maintenance for its private image central repository.',
    },
    {
      title: 'JD Elasticsearch uses ChubaoFS to realize the separation of computing and storage',
      filePath: 'bestPractices/elasticsearch',
      date: '2020-02-08',
      author: 'jingdong',
      coverUrl: '/images/cover/es_cubefs_cover_zh.png',
      desc: 'Elasticsearch is an open source distributed RESTful search engine. As a distributed, scalable, real-time search and data analysis engine, it can quickly store, search and analyze large amounts of data. At the same time, Elasticsearch also supports the base engine with applications responsible for search functions and requirements, so it can be used in many different scenarios.',
    },
    {
      title: 'Application and Practice of Erasure Code Technology in CubeFS',
      filePath: 'bestPractices/erasure_code_technology_practice',
      date: '2023-07-31',
      author: 'Zeng Yuan',
      coverUrl: '/images/cover/erasure_code_technology_practice.png',
      desc: 'Generally speaking, a storage system ensures reliability by storing a certain amount of redundant data. When the system fails, the redundant data can continue to provide users with various services and repair some data lost due to the failure. The current mainstream fault-tolerant mechanisms are roughly divided into two types: multi-copy mechanism and erasure code mechanism. Since redundant data will increase storage costs, and the redundant storage overhead of multiple copies is too large compared to erasure codes, erasure codes have become the mainstream data redundancy method for current cloud storage systems, and continue to be a popular topic in the academic world. and industry research hotspots in the field of storage.',
    },
    ]
  },
  {
    text: 'Getting Start Series',
    children: [
      {
        filePath: 'startSeries/cubeFS_object_storage_lifecycle_management_strategies',
        date: '2024-04-22',
        author: 'CubeFS',
        title: 'CubeFS 10 Minutes Start - Get to grips with CubeFS object storage lifecycle management strategies',
        coverUrl: '/images/cover/cubeFS_object_storage_lifecycle_management_strategies.jpg',
        desc: 'By default, data in CubeFS is kept permanently, and in order to store data cost-effectively throughout its lifecycle, CubeFS supports an AWS S3-compatible object storage lifecycle management strategy. CubeFS currently supports lifecycle expiration deletion policies, which can effectively clean up expired data and release storage resources. This article will guide you to understand how to use CubeFS object storage lifecycle management policies.',
      },
       {
        filePath: 'startSeries/single-node_Deployment_of_CubeFS_Cluster',
        date: '2023-03-07',
        author: 'Xuewei Zeng',
        title: 'CubeFS 10 Minutes Start - Single-node Deployment of CubeFS Cluster',
        coverUrl: '/images/cover/single-node_Deployment_of_CubeFS_Cluster.png',
        desc: 'Introduces how to quickly set up a single-node CubeFS cluster without using Docker.',
      },
    ]
  },
  {
    text: 'Community Meetings',
    children: [
      {
        filePath: 'communityMeetings/meetings',
        date: '2024-12-05',
        author: 'CubeFS',
        title: 'Community Meeting Minutes',
        coverUrl: '/images/cover/meeting.png',
        desc: 'Community Meeting Minutes',
      },
    ]
  }
]
---
