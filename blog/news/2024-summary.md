2024 is a key year for the development of CubeFS.

* CNCF officially announced the graduation of CubeFS;
* Community governance has become more standardized;
* Foundation solidified with significant optimizations in stability, performance, and usability;
* Better support for AI workloads.

As the new year approaches, we summarize the past year and look forward to the goals for the coming year.

## Graduation Overview

On December 24, 2024, CNCF announced that CubeFS successfully graduated, becoming the first fully capable storage project developed by Chinese developers to graduate from CNCF.

![CubeFS graduation details](/images/blog/summary_1.png)

![CNCF graduation list](/images/blog/summary_2.png)

## Version Iteration

In the past year, CubeFS has actively promoted several projects: First, to enhance system stability, version 3.3.2 was released; next, to improve operational automation capabilities, version 3.4.0 was released to address heavy operational tasks such as the migration of large-capacity HDDs. Additionally, for the current focus on AI scenarios, version 3.5.0 was released, achieving automatic hot and cold data tiering while developing RDMA and kernel file systems for performance, and promoting distributed caching in public cloud and hybrid cloud scenarios.

### Version Releases

#### Released Versions

* **release-3.3.2**
    * Significant performance optimizations in data migration, achieving a 200% performance improvement in HDD migration.
    * Optimized DataNode startup speed for large-capacity HDDs.
    * Supported volume write prohibition, freezing, and delayed deletion capabilities.
    * ObjectNode supports STS and signature authentication, post object protocol.
    * Enhanced SDK to improve access in separation of storage and computing scenarios.
    * Supported client routing metadata compression, reducing master traffic by 95% in massive client scenarios.

* **release-3.4.0**
    * Supported automated migration of bad disks, with manageable and queryable migration tasks, greatly enhancing automation in operations.
    * Supported automatic repair of anomalous data partitions, maximizing the consistency of multi-step operations across distributed nodes.
    * Supported experimental volume snapshot version, ensuring global consistency of snapshots.
    * Optimized DataNode parallel deletion capability to reduce load during mass data deletions.

* **release-3.4.0-beta-rdma**
    * Implemented zero-copy capability based on RDMA, showing good performance in data reading and direct writing, with about a 10-40% performance improvement, suitable for high-write performance business scenarios.

#### Upcoming Versions

* **release-3.5.0**
    * Storage Tiering: Introduced a hot and cold data tiering mechanism, allowing data to undergo cooling operations based on its lifecycle, helping businesses further optimize costs.

#### Upcoming Beta Versions

Additionally, there are two versions that have reached BETA standards:

* **release-3.5.1**
    * Distributed Caching: Based on consistent hashing, achieving distributed caching acceleration, supporting cross-cloud and multi-cloud scenarios with caching acceleration, with features such as multiple replicas and proximity access.

* **develop-v3.6.0-kernel-rdma2**
    * Kernel version, focusing on high-performance scenarios such as GPU acceleration.

### Mainline Iteration

Due to the extended development cycles of 3.4.0 and 3.5.0, the current version has diverged from the mainline. We plan to return to the mainline in version 3.5.1. Therefore, some features are not reflected in these two versions but have been merged into the mainline and will continue to iterate, expected to be concentrated in version 3.5.1.

#### Erasure Coding Subsystem

1. Optimized local data inspection for blobnode, fixing panic issues caused by slow disks.
2. Scheduler supports batch deletion.
3. Scheduler supports concurrent disk offline capabilities for cabinet organization.
4. Supports volume allocation optimization, improving allocation efficiency.
5. Blobnode supports distinguishing between read and write throttling.

#### Object Subsystem

Cross-region replication: The cross-region replication feature allows data to be automatically and asynchronously (near real-time) replicated from buckets across different CubeFS data centers to target storage areas, meeting the business needs for multi-region backups or "local production, remote analysis" data requirements.

## Community Development

### CubeFS Ranked in the Top 4 of Batch/AI/ML Computing Technologies

In the CNCF technology landscape radar report released at the North America KubeCon conference in November 2024, the open-source distributed storage system CubeFS, along with Apache Airflow and Kubeflow, was positioned in the "Adopt" category in the Batch/AI/ML computing technology field, with a usability score of +40, ranking among the top four, making it one of the most mature and adopted projects in the Batch/AI/ML domain.

![CubeFS Top 4](/images/blog/summary_3.png)

### Activity Status

* Commits: 5165 -> 7883 (Growth %52)
* Code Committers: 76 (11 companies) -> 142 (22 companies) (Growth 86%)
* Pull Requests: 2149 -> 2620 (Growth 21%)
* Contributors: 229 (23 companies) -> 379 (42 companies) (Growth 65%)
* Contributions: 12922 -> 16845 (Growth 30%)

### Community Governance

* Established a complete community governance system and formed a Technical Steering Committee (TSC).
* Provided decision-making and oversight related to its charter; defined the project's values and structure; responsible for the project's high-level design, roadmap formulation, and community management. Established and maintained overall technical governance guidelines for the project.
* Improved the promotion channels for Maintainers and Committers, clarifying selection qualifications.
* Clarified the steps and rules for roadmap formulation.
* Established SIG groups, clarifying the positioning of SIG organizations, member management rules, and lifecycle.

### New Users (Partial)

| **Company**    | **Application Scenario** | **Usage Method** | **Data Storage Format** |
|:---------------|:------------------------|:-----------------|:-----------------------|
| Shopee         | AI/Big Data            | POSIX            | Multiple Replicas       |
| DingQiao Tech  | Video Files             | POSIX/S3         | Multiple Replicas       |
| Digital Guangdong | -                    | S3               | Multiple Replicas       |
| KuaWa Robot    | Big Data                | S3               | Multiple Replicas       |
| MetaApp        | Big Data                | S3               | Erasure Code           |
| NetEase Mail   | Backup Data             | POSIX/S3         | Multiple Replicas       |
| Greaterheat    | Blockchain              | POSIX            | Erasure Code           |
| Leap Motor     | AI/Big Data            | S3               | Multiple Replicas       |
| Hexun          | K8s                     | POSIX            | -                       |

## Future Plans

### System Dimension

1. Metadata system RocksDB persistence; there is a strong demand for metadata persistence in the community, and collaboration among community teams in this area will be enhanced.
2. In hybrid cloud management, we will better support public and hybrid clouds, enhancing CubeFS's cloud capabilities. For example, by integrating with the S3 interface, we will achieve hybrid cloud functionality, utilizing the low cost and unlimited elastic resources of public clouds for seamless management of CubeFS native storage and public cloud storage, shielding the differences of underlying storage systems and resources.
3. In distributed caching, we will continue to iterate to meet the increasingly broad acceleration needs in the cloud.
4. Achieve multi-version capability for objects, supporting multiple versions of objects in buckets and allowing retrieval, deletion, or restoration of specified versions. This will help recover data lost due to user deletion or application failure.

### Community Governance

1. Strengthen the implementation of community self-governance rules, promoting standardized practices within the community.
2. Increase the project's visibility abroad, attracting more international users.

## Acknowledgements

Thanks to Kevin Wangzefeng, a member of the CNCF TOC, who also served as the CubeFS graduation sponsor, for providing timely guidance and assistance during the graduation process, helping CubeFS successfully graduate. On behalf of the CubeFS community, we express our gratitude for Kevin's hard work!

Thanks to the CNCF Storage-tag, and all members of the CNCF TOC, for providing significant guidance and support after joining CNCF, bringing broader attention and support to CubeFS.

Thanks to the large user groups from NetEase, BIGO, Beike, JD.com, OPPO, and others for their support and trust.

Thanks to the contributors in the community (in alphabetical order) for their tireless efforts in improving the project!

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