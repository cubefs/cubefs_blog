# CubeFS helps enable space-based intelligent computing to connect the space computing storage chain

Author: Space-based Distributed Operating System Project Group, Space-Based Center, Zhijiang Laboratory

Space-based intelligent computing project refers to the deployment of intelligent computing facilities in space to build space-based intelligent computing infrastructure.

A space computing satellite constellation was launched on a Long March-2F carrier rocket from the Jiuquan Satellite Launch Center at 12:12 a.m. on May 14, marking the official networking stage of China's first orbit interconnected space computing constellation. The first launch of the "three-body calculation constellation" led by Zhijiang Laboratory and the first launch of the "star calculation" program of Guoxing Aerospace.

![img_11.png](/images/blog/5/img_11.png)

The "Three-body" computing constellation project of Zhijiang Laboratory aims to jointly build a thousand-star scale space-based intelligent computing infrastructure through global partners, and solve the bottleneck problem in the traditional satellite data processing mode, that is, the "celestial sense and earth computing" mode. The data collected by the satellite needs to be transmitted back to the Earth for processing, which is not only limited by bandwidth, but also leads to low efficiency and information loss. The "three-body" computing constellation is committed to the realization of "heavenly sense and heavenly calculation", and realizes the satellite on-orbit data processing function, so as to improve the efficiency of data processing and reduce information loss.

Among them, space-based distributed operating system is one of the key technologies to ensure the realization of "celestial sensing and computing", and it is also a key research sub-topic of the project. It needs to coordinate computing power, storage, load and other resources in a complex system composed of thousands of satellites, realize the scheduling of on-orbit tasks and data forwarding and storage between satellites and ground satellites, and provide cross-satellite data coordination and satellite-ground data synchronization capabilities, so as to ensure the efficient operation of the system.

![img_14.png](/images/blog/5/img_14.png)

## Storage Technology Selection
After preliminary investigation, comparative selection and combined with business scenarios, we choose CubeFS as the storage base of space-based distributed operating system to manage data distributed on many satellites. Mainly based on the following considerations:

1. High availability

Due to the radiation, particle upset and other factors in space, the single node failure probability is high, and high availability is the key to ensure the stable operation of the system. The metadata system of CubeFS is designed using Multi-Raft protocol, which can make full use of on-orbit computing resources to ensure high availability and strong consistency of data, effectively deal with single point of failure, and can recover quickly.

2. Massive small file processing ability

The on-orbit application is mainly oriented to the processing of remote sensing data and outer space signal observation data, and the application will produce a large number of small file read and write operations in the process of processing. CubeFS is optimized for this scenario, which can efficiently process a large number of small files, reduce the delay of storage and retrieval, ensure that data can be quickly stored and accessed, and improve the overall data processing efficiency.

3. Domestic open source

Choosing CubeFS, as the first domestic open source storage product graduated from CNCF, not only meets the strategic needs of national independence and controllability, but also obtains more advantages in technical support and community ecology. The open source model allows project teams to customize and optimize for their needs, while also receiving the latest technical updates and solutions from an active community.

## Overall Structure
In early 24, we started to build CubeFS test environment to simulate the distributed storage scenario of satellite nodes to verify the feasibility of CubeFS as an on-board distributed file system. It fully verified CubeFS from the aspects of file read and write speed, dynamic scaling capacity, system availability under node failure and network failure, and finally confirmed that CubeFS meets the project requirements in terms of functionality and performance.

After that, we completed the architecture design of the distributed file system within and between satellites. Each satellite is regarded as an independent storage cluster, and the distributed characteristics of CubeFS are used to achieve efficient data storage and unified management of each storage unit. The system control layer is designed to be responsible for resource allocation, task scheduling and data collaboration. The inter-satellite architecture is designed based on the reliable network transmission protocol, and the inter-satellite data synchronization is realized through the S3 gateway to ensure the consistency of data in the satellite network. The breakpoint continuation transmission mechanism is designed to deal with the instability of the inter-satellite network.

![img_12.png](/images/blog/5/img_12.png)

The on-board distributed file system controls and manages the distributed file system through the management and control layer, so as to realize the core functions of the on-board distributed file system. The overall architecture diagram is shown in the following figure:

![img_13.png](/images/blog/5/img_13.png)

Provides business capabilities through external API interfaces, including the following capabilities:

· Resource monitoring: It provides data monitoring in various dimensions of cluster, node and process granularity

· Authentication: verify the validity of the request

· Volume management: Volume granularity management file system, associated user permissions each file system isolation

· Mount management: Provide node and container level mount and unmount capabilities

· User management: Provide user creation, deletion and query capabilities

· Data synchronization: It provides satellite-ground data synchronization capability and supports breakpoint continuation transmission

· Data migration: Support data migration between nodes

· Data cleaning: regularly clean data to ensure that data available capacity is below the threshold

## Practice
During the implementation of the project, in addition to the above business function development, we also invested a lot of effort to deeply analyze the source code of CubeFS to better understand its internal mechanism and provide solutions to the problems encountered in practical use. Here are just two recent examples of the problems we encountered and the work we've done.

1. Storage space management optimization

During the testing phase of large-scale algorithm application, we found some challenges related to storage space management. Specifically, in the scenario that a large number of files are added and deleted repeatedly, although the files in the mount directory have been logically deleted, the physical space in the storage disk has not been released in time. In extreme cases, this situation will make the file volume into a non-writable state, which directly affects the operation of the algorithm and the result output.

An in-depth analysis of the CubeFS source code revealed that the root cause of the problem is inconsistent metadata and block management: after a file is deleted, the metadata layer is updated immediately to mark the file as deleted, but there is a delay in inode release operations. To this end, we develop the CubeFS consistency check tool: the tool periodically scans the metadata and data blocks of the file system to detect and clean up invalid files. At the same time, by comparing the status of metadata and data blocks, the logical deleted but not physically freed files are identified, and the space release operation is triggered actively.

2. Enhanced metadata reliability

In the stage of system fault tolerance test, we test the scenario of abnormal power failure of hardware in space, and construct the experimental environment of repeated power down and power up. In several tests, we found an occasional problem: some metadata blocks would show the error Meta partition lack replicas. The specific performance is as follows:

If a single node's metadata is lost, the block data is still in the writable state and can usually be recovered automatically. However, if the metadata of two or more nodes is lost, the block data becomes unavailable and cannot be automatically recovered, making the file volume unavailable.

After digging into the stored metadata, we discovered that one file, called.sign, was missing. After further investigation of the code, it is found that the root of the problem is that the data of the file is written to the cache first, but the disk operation is not carried out in time, resulting in the loss of the cache data when the abnormal power failure occurs, which leads to the loss of the metadata block.

It's worth noting that we talked to the CubeFS team and learned that the community has already fixed these issues in subsequent versions, as well as providing a way to migrate metadata to a new node to fix them. With the above bug fix, the problem of metadata loss caused by abnormal power failure is greatly alleviated, but because the current disk selection has no flush ability during abnormal power failure, extreme cases will still occur. For this we took a temporary measure: a uniform snapshot of the metadata of each node at a specific time node. When a failure occurs, metadata can be quickly recovered through snapshots, minimizing the impact of data loss

## Future Plan
Our next development plan is mainly about how to ensure that CubeFS can still operate stably under the limited physical resources of the space environment and abnormal scenarios. In order to further improve the observability and troubleshooting ability of the system, we will develop a complete set of file system monitoring system to monitor the running status of the file system in real time. And the key status data is transmitted back to the ground control center, which is convenient for the ground team to carry out remote diagnosis and fault handling.

## Reference Material
1. Twelve stars! Space calculating satellite constellation successful launch (https://mp.weixin.qq.com/s/knL2KOR_RTg72LJZb7AEdw)

2. Zhijiang laboratory focus on intelligent computing (https://kjt.zj.gov.cn/art/2025/2/18/art_1228971345_59012623.html)

3. CubeFS open source project (https://github.com/cubefs/cubefs)

4. CubeFS official website (https://cubefs.io/)

 