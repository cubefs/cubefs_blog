# Kubernetes CSI history and current situation

## background

Storage systems can be divided into block storage, file storage and object storage. Currently, kubernetes mostly uses CSI to interface with block storage and file storage, while COSI is used to interface with object storage. However, the support for COSI is still relatively early and in alpha. state. Both CSI and COSI are protocol interfaces. These interfaces will be built into kubernetes and evolve with the version. Kubernetes only maintains interface specifications and does not maintain the specific implementation of CSI or COSI of each storage manufacturer. This article mainly explains CSI. If you want to know about COSI, you can refer here: https://container-object-storage-interface.github.io .

The process of connecting kubernetes to storage did not start with CSI, and there were still some detours in the middle. At present, it has gone through three major stages:
- in-tree
- FlexVolume
- CSI

### in-tree

At the beginning of kubernetes, all storage docking codes were mixed with the core code. For example, the initial members of Google Cloud, Amazon Cloud, and Microsoft Cloud all put their own storage code into the kubernetes core code warehouse. This method is called is "in-tree". This approach will bring some inconveniences: users who want the latest storage docking features must use the latest kubernetes version, because these features are bound to the kubernetes version, but at this time the codes for both are placed in In the same warehouse, they cannot be separated.

Students who have conducted k8s operation and maintenance should have a deep understanding. Generally speaking, kubernetes in the production environment will not be upgraded unless there are special circumstances. Many factors are involved, such as cost, stability, etc. Therefore, this method of strong coupling between storage docking and Kubernetes core code is not flexible enough and not user-friendly.

Kubernetes developers want to separate these storage-related codes from the core code. After all, this strongly coupled approach is difficult to operate and maintain. So the second generation storage docking method FlexVolume came out.

### FlexVolume

FlexVolume is a feature introduced from kubernetes v1.2, and v1.8 is officially GA. The FlexVolume protocol is relatively simple and can be implemented in any language. The protocol specification requires that json content that conforms to the specification be returned after executing the relevant interface. Each specific storage plug-in implementation is maintained by the manufacturer, and the code is out-of-tree. Only the protocol interface of FlexVolume is in-tree.

FlexVolume needs to be installed in a directory with a specified name so that kubelet can discover and find the corresponding execution program. Installing FlexVolume requires installing relevant dependencies on the host, so it has requirements for the host environment. K8s developers feel that this method is not very elegant. They do not want FlexVolume to have requirements or impact on the host environment. It is best to run it in a container, so they designed and continued to evolve the next generation storage standard interface: CSI.

### CSI

CSI is an RPC protocol specification launched by kubernetes storage SIG. The protocol is defined by protobuf. As of now (2024.4), the latest protocol version is v1.9.0. The specific content can be viewed in the official website warehouse at: https:// github.com/container-storage-interface/spec/blob/master/spec.md .

CSI was introduced in kubernetes v1.9 and is still evolving, with protocol content constantly being added and improved. CSI can also be implemented in any language as long as RPC communication is possible. From the beginning, CSI was not only designed for kubernetes, but also intended to be applied to all container orchestration systems, such as Swarm/Nomad, etc.

It is worth noting that the CSI protocol is not strictly forward compatible, and there have been some major destructive improvements during its evolution, resulting in some older Kubernetes being unable to use the new CSI driver. This will be explained in the following chapter on the evolution of CSI.

Similar to FlexVolume, each specific CSI storage plug-in implementation is maintained by the manufacturer, and the code is out-of-tree. Only the protocol interface of CSI is in-tree, which evolves with the kubernetes version.

## CSI vs FlexVolume

There should be many companies currently using FlexVolume. FlexVolume still has its place. It does not mean that FlexVolume will be useless after CSI comes out. In some scenarios, the efficiency of FlexVolume implementation will be better than CSI, such as no need for snapshots, clones, statelessness, and data does not need to be persisted.

You can briefly compare CSI and FlexVolume to give your friends a general understanding of these two storage docking methods.

|                             | FlexVolume     |       CSI       |
| :---:                       |    :----:      |  :---:          |
| Difficulty of implementation|     Small      |       Larger    |
| Number of interfaces        |     few        |       many      |
| mount                       |    have        |       have      |
| umount                      |    have        |       have      |
| create volume               |    none        |       have      |
| delete volume               |    none        |       have      |
| snapshot                    |    none        |       have      |
| clone                       |    none        |       have      |
| expand volume               |    none        |       have      |

It can be found that FlexVolume supports fewer functions. This is because FlexVolume stopped evolving early and some advanced functions have not evolved. The advanced functions of CSI were not available at the beginning, but were slowly iterated through small versions.


## The evolution of CSI


Every feature in kubernetes goes through three stages: alpha->beta->GA, and the same goes for every feature in CSI. The following is the evolution process of CSI-related features in kubernetes. This process includes the v1.9 version where it was born to the v1.30 version that is ready to be released in the middle of this month.



### kubernetes v1.9

CSI version v0.1.0 was born, which can support basic functions such as creation/deletion, mount/unmount and so on.

### kubernetes v1.10

CSI evolved to v0.2.0, fixed some bugs, increased the security of existing interfaces, and launched CSI's E2E testing tool.

### kubernetes v1.11

CSI evolved to v0.3.0 and fixed some bugs.

[alpha] Added support for Block Volume feature. This feature allows CSI to directly provide raw block devices to pods, which is more useful for some special pod usage scenarios. For example, Oracle needs to directly use raw block devices, then this feature can meet this need.

### kubernetes v1.12

Fixed some bugs.

[alpha] Add snapshot feature to support creation and deletion of snapshots (volumesnapshot)

[alpha] Support Topology

[alpha] supports skipping the Attach stage, which needs to be configured in storageclass

[alpha] supports adding pod information when mounting, which needs to be configured in storageclass

### kubernetes v1.13 

The evolution of the CSI protocol to v1.0.0 is a major destructive evolution. The v1.0.0 protocol does not support forward compatibility, but it promises that versions after v1.0.0 will be compatible. The CSI implemented based on the previous v0.3.0 protocol needs to be converted to v1.0.0 before kubernetes v1.17, because the CSI interface implemented based on v0.3.0 is not supported after kubernetes v1.17, which means that it cannot operate normally. The reason why v1.0.0 does not support forward compatibility is that the fields in the RPC interface have changed.

No new features are introduced.

### kubernetes v1.14 

The CSI protocol has evolved to v1.1.0 and some bugs have been fixed.

[alpha] Added volume expansion feature

[beta] Added snapshot feature to support creating and deleting snapshots (volumesnapshot)

[beta] Support Topology

[beta] supports skipping the Attach stage, which needs to be configured in storageclass

[beta] Supports adding pod information when mounting, which needs to be configured in storageclass


### kubernetes v1.15

Fix some bugs.

[alpha] Add volume cloning feature. This feature can create clone volumes directly from the source PVC without using volumesnapshot to create clones.

[alpha] Add the Ephemeral local volumes feature


### kubernetes v1.16

Fix some bugs.

[beta] Volume cloning feature

[beta] Volume expansion feature

[beta] Ephemeral local volumes feature

### kubernetes v1.17

The CSI protocol has evolved to v1.2.0 and some bugs have been fixed. This version no longer supports v0.3.0 of CSI.

[beta] Volume snapshot feature

[beta] CSI migration for AWS EBS and GCE PD drivers

[GA] Topology

[GA] Volume mount restrictions


### kubernetes v1.18

Fix some bugs. In this version, kubelet no longer creates mount points when mounting volumes to pods, and CSI is responsible for this.

[alpha] Add CSI support for windows system

[beta] CSI migration for Openstack cinder driver

[GA] Volume clone feature

[GA] Topology

[GA] Skip the Attach stage

[GA] Add pod information when mounting


### kubernetes v1.19

Fix some bugs.

[alpha] Add volume health monitoring feature

[alpha] Add fsGroup feature

[alpha] Added storage capacity tracking feature

[alpha] Add Generic ephemeral volumes feature



### kubernetes v1.20

Fix some bugs.

[alpha] Add CSIServiceAccountToken feature, which can be used to enhance security

[beta] fsGroup features

[GA] Volume Snapshot Features


### kubernetes v1.21 

Fix some bugs.

[beta] CSIServiceAccountToken features

[beta] Storage capacity tracking feature

[beta] Generic ephemeral volumes features


### kubernetes v1.22


Fix some bugs.

[alpha] Support ReadWriteOncePod access mode

[GA] Windows support

[GA] CSIServiceAccountToken characteristics



### kubernetes v1.23


Fix some bugs. Another thing worth noting is that this version officially deprecates FlexVolume and recommends CSI for all.

[alpha] RBD CSI Migration

[alpha] Portworx CSI Migration

[beta] Azure Disk CSI Migration

[beta] AWS EBS CSI Migration

[beta] GCE PD CSI Migration

[GA] fsGroup characteristics

[GA] Generic ephemeral volumes features


### kubernetes v1.24 

Fix some bugs.

[GA] Volume expansion feature

[GA] Storage capacity tracking feature

[GA] Azure Disk CSI Migration

[GA] OpenStack Cinder CSI Migration

### kubernetes v1.25

Fix some bugs.

[beta] vSphere CSI Migration

[beta] Portworx CSI Migration

[GA] CSI ephemeral inline volumes

[GA] AWS EBS CSI Migration

[GA] GCE PD CSI Migration

### kubernetes v1.26 

Fix some bugs.

[alpha] Add cross-namespace snapshot cloning feature. Note: This feature will no longer be promoted in the future. You can check KEP-3294 and issue-3294 for details. It is a pity that such a good feature will not continue to be promoted.

[GA] Azure File CSI Migration

[GA] vSphere CSI Migration

### kubernetes v1.27

Fix some bugs.

[alpha] Add snapshot group characteristics. This feature can be used to create a set of snapshots for a specific set of PVCs.

[beta] ReadWriteOncePod access mode

### kubernetes v1.28

Fix some bugs. Removed Ceph RBD in-tree plugin and Ceph FS in-tree plugin code.

### kubernetes v1.29

Fix some bugs.

### kubernetes v1.30 

Fix some bugs.

The CSI details corresponding to each of the above versions can be found in the official github warehouse of kubernetes. Interested friends can read more: https://github.com/kubernetes/kubernetes/tree/master/CHANGELOG 。



## Current status of CSI


From the evolution of CSI, we can find that the features of CSI are relatively complete, but we also find that these new features of CSI are released following the kubernetes version. The newer the kubernetes version, the more features it supports. So we are back to the original problem that troubled users: if you want the latest and most complete features, you need to upgrade kubernetes. But this is different from the original scenario in that users can develop various features under the current protocol version framework. This gives storage docking developers a lot of room for imagination and play, rather than like Nothing could be done as before.

It can also be seen from the above evolution process that the in-tree storage code in the kubernetes core code is basically gradually removed, and all are converted to CSI implementation. Major public cloud and storage vendors are actively embracing CSI, which is also a good thing for the core code of kubernetes. Many previous in-tree plug-in codes have been removed, making the core code cleaner and easier to maintain.

Let me also make an advertisement here: The CubeFS community has recently established a CSI SIG, and all friends are welcome to come and work together. For details on how to participate, please see the official warehouse cubefs-community documentation:

https://github.com/cubefs/cubefs-community/blob/master/sig-csi/README.md



## reference



[1] https://kubernetes-csi.github.io/docs/

[2] https://github.com/kubernetes/kubernetes/tree/master/CHANGELOG

[3] https://kubernetes.feisky.xyz/extension/volume/flex-volume

[4] https://github.com/kubernetes/enhancements/tree/master/keps/sig-storage

[5]https://ibm.github.io/kubernetes-storage/flexvolume/



## about the author

Mingwei Gong is currently responsible for the interface between K8S and core storage in the CubeFS community.