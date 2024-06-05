# Introduction to CubeFS Quota Feature

Original by Yao Hu, CubeFS, November 10, 2023, 09:27, Guangdong

## 01 Preface

In storage systems, quotas are a technique for managing and controlling the use of storage resources. They can help system administrators effectively manage storage resources, ensuring their rational allocation and utilization.

Let's take a look at how quotas are commonly used in file systems.

### 1.1 XFS
For xfs, quotas can only be set for the entire file system. For example, if the test user wants to set quotas for the xfs file system mount point /mnt/xfs with a capacity soft limit of 40M and a hard limit of 80M, and a file number soft limit of 4 and a hard limit of 8, the command is:

```bash
xfs_quota -x -c 'limit bsoft=40m bhard=80m isoft=4 ihard=8 test' /mnt/xfs
```

### 1.2 CephFS

For CephFS, quotas can be set based on directories, using special extended attribute fields to set them. If you want to set quotas for a directory /mnt/cephfs/dir1 with a file limit of 1000 and a capacity limit of 1000 bytes, the command is:

```bash
setfattr -n ceph.quota.max_bytes -v 1000 /mnt/cephfs/dir1
setfattr -n ceph.quota.max_files -v 1000 /mnt/cephfs/dir1
```
### 1.3 Summary

From the analysis of quotas in commonly used file systems, we can see:

- In terms of limitation, quotas can be divided into hard limits and soft limits.
**Hard limit**: Users are not allowed to exceed this limit under any circumstances.

**Soft limit**: Resource usage can exceed the soft limit within a grace period, but cannot exceed the hard limit.

- In terms of statistical methods, quotas can be based on users and user groups for statistics, based on directories for statistics, or based on a file system in the storage system for statistics.
- In terms of resources being monitored, quotas generally include file numbers and capacity.
- In terms of management methods, management is carried out through proprietary protocols using self-developed tools or system tools.

### 1.4 Requirement Analysis

Based on the previous research summary and combined with the business requirements of CubeFS, the analysis includes:

1. Use soft limits. In a distributed system with multiple clients reading and writing concurrently, to achieve true hard limits, real-time capacity updates are required, which will inevitably affect the performance of normal IO operations.
2. According to the actual business situation, we have both user-based restriction requirements and directory-based restriction requirements. Therefore, both of these statistical methods will be implemented, and users can set them according to the actual needs of their business.
3. Support both capacity statistics and file number statistics. Because both are basic attributes of files, the logic for statistics is consistent, and there is no additional overhead for counting one or two.
4. Reuse the current cubefs's cfs-cli tool and add quota-related management interfaces.
5. According to the actual needs of users, for directory quotas, we achieve multiple directories as a group for unified resource statistics and restriction usage.

## 02 Design Concept and Overall Architecture

The key design of quotas includes two aspects: resource statistics and request restrictions, with the difficulty mainly in resource statistics (capacity and file number).

### 2.1 Resource Statistics

At the same time, support is required for both user-level resource statistics and directory resource statistics, which cannot be accomplished solely through directory recursion. 

Therefore, considering that each metadata shard (mp) maintains a range of inodes and the inode data structure contains user information (uid), we only need to add quotaId information to each inode to achieve classification statistics for mp based on user quota and directory quota, and then send the statistics results of each mp to the master for summary statistics via heartbeats.

### 2.2 Request Limitation

When the master receives the mp statistics information reported by the metanode heartbeat, it calculates the usage capacity of uid or quotaId and returns a message via heartbeat to notify each metanode node if the threshold is reached. Considering that the write process first writes data and then updates metadata, clients also need to be restricted, so clients will also periodically fetch quota-related information from the master.

### 2.3 Overall Architecture

Based on the above ideas, the overall architecture is as follows:
![Overall Architecture](/images/blog/Introduction_to_CubeFS_quota_features_architecture.png)
**Master:**

1. Responsible for maintaining and persisting all quota information. Quota information includes: quotaId (globally unique), creation time, quota path information, usage information, and maximum limit resource information.
   
2. The master also provides API interfaces to the cfs-cli tool for quota management.
   
**Metanode:**

1. Each metaPartition maintains a metaQuotaManager to real-time statistics the capacity and file number of all quotas on this mp.
   
2. Perform capacity re-statistics and correction through periodic snapshots (which will traverse the inode tree in memory).
   
3. For create/delete inode (update file number) and appendExtent/truncate (update size) operations, calculate the change in file number and capacity in real-time and update it to the statistical result.
   

**Client:**
Periodically fetch quota information, and when writing, determine whether the inode corresponding to the current quota exceeds the quota. If it does, the write operation fails.

## 03 Key Design

### 3.1 metaPartition Resource Statistics
![metaPartition Resource Statistics](/images/blog/Introduction_to_CubeFS_quota_features_mp.png)

As shown in the diagram, each metaPartition (mp) maintains an inode tree and an extent tree (used to store POSIX extended attributes). We have added a metaQuotaManager in the memory of mp, which will statistically count the number of files and their sizes based on uid and quotaId dimensions. Since uid itself exists in the inode, while the quotaId information of each inode is stored in the extent tree through extended attributes. Here, the statistics are divided into three scenarios:

1. Metanode startup scenario: When the metanode starts, it loads metadata to build the inode tree and extent tree. During this process, it needs to read all inodes of mp, so we can also construct the current statistics results.
   
2. In the scenario of creating/deleting inodes (updating file numbers) and appendExtent/truncate (updating size), real-time calculation of the change values of file numbers and capacity, updating them to the statistical results.
   
3. Regular snapshots of mp: Will persist the inode tree and extent tree to the local storage. During this process, it will traverse all inodes, which can also be used to periodically correct the statistical results.

### 3.1 Client Resource Statistics
In the diagram:
![Client Resource Statistics](/images/blog/Introduction_to_CubeFS_quota_features_client.png)

The data writing process first writes data to the extent on the datanode, and then updates the extent and size information to the inode on the metanode through the appendExtent interface. If there is a restriction on the metanode and appendExtent returns overQuota, the previously written data will become garbage data on the datanode. Even if a recovery mechanism is added, it will waste the bandwidth resources of the datanode.

To solve this problem, the restriction on resource writing is implemented on the client side. This requires the client to periodically fetch quota information from the master. At the same time, fine-grained differentiation is made between new writes and overwrite operations. For overwrite operations, since the size does not change, no resource restriction judgment is made. However, for new writes, if the quota belonging to the current file has exceeded the quota, an error will be returned directly, and the data will not be written to the datanode anymore.

## 04 Summary

Considering the complexity of the scenario, some restrictions are also imposed on quotas, such as not allowing nested quotas; the root directory of quotas cannot be directly deleted, it must be deleted before the quota can be deleted; cross-quota rename operations, the time lag of capacity statistics will be affected by the actual file number under the rename directory. Welcome everyone to participate in the optimization and evolution of the quota feature.

## Introduction 
Yao Hu, CubeFS Contributor, responsible for the research and development of object storage, currently working at OPPO.