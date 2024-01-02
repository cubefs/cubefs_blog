---
blog: true
---

## 前言

这次我们重点介绍一下master模块提供的HTTP服务。因为HTTP服务里面的一些功能需要通过网络链接和meta node/data node通讯，所以我们也附带介绍了通讯模块的实现机制。

## 启动HTTP

### 启动服务

这部分是从Server.Start开始启动的，通过startHTTPService函数挂载HTTP的URL。在startHTTPService中，通过registerAPIRoutes挂载处理函数。然后在serveAPI()函数中，调用server.ListenAndServe()启动HTTP服务。

### 重定向

在startHTTPService函数中，通过registerAPIMiddleware挂载了中间件处理函数。这部分就是通过route.Use(interceptor)挂载一个中间转发的函数。其目的是根据当前节点是否是raft的主节点来响应服务。如果是主节点，或者isFollowerRead=true，就响应HTTP请求，否则就重定向到主节点上面。其代码流程如下：

![图片](/images/blog/3/1.jpg)

## HTTP服务

在master/http_server.go文件里面，registerAPIRoutes注册了该模块提供的对外HTTP服务。

模块master提供的HTTP服务接口非常之多，文章篇幅有限，我们没法一一进行阐述。

下面我们就重点选择其中的几个接口说明这个代码流程。大部分的接口流程都有相似的流程，区别是具体实现的内容功能不一样。

### 创建volume

volume卷就是一组元数据和数据分区的表示集合。

创建卷的URL是/admin/createVol。其处理函数是Server.createVol。

![图片](/images/blog/3/2.jpg)


这个代码流程还是比较简单清晰的。接下来我们看看cluster中创建卷的函数createVol。其代码流程如下：

![图片](/images/blog/3/3.jpg)

简单来说，创建volume的主要内容就是检查卷名，分配ID，然后添加新卷到raft和vols字典里面，然后初始化meta partition，最后和用户账号关联起来。

这部分代码和raft有关联的操作就是在syncPutVolInfo函数里面向raft发送了一条命令。其实质就是往raft日志里面提交数据，最后就是把记录保存到rocksDB里面。

### 创建数据分区

创建一个数据分区的函数在文件master/cluster.go里面的createDataPartition。其代码流程：

1. 选择一个可用的数据节点。
2. 把partition ID设置给该分区。
3. 和该节点通讯，同步创建数据分区。
4. 成功以后，通过raft同步复制数据，并且持久化到RocksDB里面。
![图片](/images/blog/3/4.jpg)

### 下线数据分区

下线数据分区的动作必须由管理员主动发起。

把一个数据分区下线的函数是migrateDataPartition，在文件master/cluster.go里面。

检查该分区是否可以下线，在下面的情况下，下线操作是禁止的：

1. 复制本没有在最新的主机列表中。
2. 已经有一个复制本下线。
3. 剩余的复制本少于阈值。
整个HTTP下线数据分区的流程包含很多操作，我们只分析其中最主要的函数migrateDataPartition。

**其代码的下线的流程**是：

1. 选择一个新的数据节点。
2. 同步下线数据分区。
3. 同步创建新的数据分区。
4. 把旧的数据分区设置为只读。
5. 持久化更新最新主机列表。
![图片](/images/blog/3/5.jpg)

### 添加数据副本

这个的URL是/dataReplica/add，入口函数是Server.addDataReplica，其实现主体则是Cluster.addDataReplica。

代码的流程如下：

![图片](/images/blog/3/6.jpg)

简单来说，在master模块上面添加一个数据分区的副本，主要的操作就是向data node节点发送创建raft成员和数据分区的命令，然后添加日志到raft集群里面，并且更新主控节点记录的信息。

这个逻辑也是很好理解，就是给data node发送创建命令，添加到raft集群里面，然后持久化保存配置信息。

### 删除数据副本

该HTTP服务的URL是/dataReplica/delete，入口函数是deleteDataReplica，主体函数是removeDataReplica。

![图片](/images/blog/3/7.jpg)

### 添加数据节点

添加数据节点的入口函数是addDataNode，它主要实现代码流程如下：

1. 判断node地址是否已经存在字典里面。如果已存在，就返回出错。
2. 分配一个data node ID。
3. 调用syncAddDataNode，同步添加节点。
4. 调用syncUpdateNodeSet，更新集群信息。
5. 调用putDataNode，把节点添加到拓扑记录里面。
6. 调用addNodeSetGrp，添加节点到组里面。
7. 把节点添加到dataNode字典里面，具体操作是c.dataNodes.Store。
![图片](/images/blog/3/8.jpg)

## 通讯模块

和元数据/数据进行通讯的代码在master/admin_task_manager.go里面，AdminTaskManager向metaNode或者dataNode发送管理命令。

这个模块的工作模式分成2种，一种是同步发送命令，一种是异步模式，每隔2秒钟发送一批次。

### 初始化

创建一个通信模块的函数是newAdminTaskManager。主要传入两个参数targetAddr和clusterID。其中targetAddr是包含了端口地址。

### 同步模式

函数是syncSendAdminTask，其主要流程是：

1. 调用sender.getConn()，从连接池中获得一个链接。useConnPool是true，所以是走连接池的代码。
2. 调用packet.WriteToConn(conn)，发送报文。
3. 调用packet.ReadFromConn，读取回应。
4. 调用sender.putConn，释放连接。如果出错，就通过设置forceClose=true，强制释放连接。
### 异步模式

异步模式是初始化时，在后台启动了一个处理进程sender.process()。这个后台进行每隔2秒就调用sender.doDeleteTasks()和sender.doSendTasks()。

异步模式通过AddTask和DelTask向sender.TaskMap字典里面添加任务。然后后台进程定期发送这些任务。

其代码流程如下：

![图片](/images/blog/3/9.jpg)

## 结尾

本次我们重点介绍了HTTP服务的代码流程，顺带介绍了master同metanode，datanode通信的机制。应该来说，这部分的代码就是master的主要部分。

