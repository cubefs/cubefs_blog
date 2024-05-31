---
blog: true
---

## Background

The Object Subsystem of CubeFS provides an access protocol compatible with standard S3 semantics, and storage resources can be accessed through tools such as Amazon S3 SDK or s3cmd. This article mainly introduces how to configure the object gateway and access storage resources through the s3cmd tool.

## Object gateway config

For the distributed deployment cluster steps of CubeFS and the startup commands of each subsystem, please refer to the distributed deployment cluster chapter in the official documentation [1]. Only the configuration file example of the object gateway is explained here:

``` yaml
{
     "role": "objectnode", 
     "listen": "17410",
     "domains": [
         "object.cfs.local"
     ],
     "logDir": "/cfs/Logs/objectnode",
     "logLevel": "info",
     "masterAddr": [
         "192.168.0.1:17010",
         "192.168.0.2:17010",
         "192.168.0.3:17010"
     ],
     "exporterPort": 9503,
     "prof": "7013"
}
```

## The mean of each param is as follows:

| Param   | Type         | Description                                     | Required |
|---------|--------------|-------------------------------------------------|----------|
| role    | string       | Process role                                    | Yes      |
| listen  | string       | Port number for the object storage subsystem     | Yes      |
| domains | []string     | Configure domain names for S3 compatible API     | No       |
| logDir  | string       | Log directory                                   | Yes      |
| logLevel | string       | Log level                                       | No       |
| masterAddr | []string     | Addresses of the resource management subsystem  | Yes      |
| exporterPort | string     | Port for Prometheus to access monitoring data    | No       |
| prof    | string       | Debug and admin API interface                    | Yes      |

## S3cmd access storage resources

###  S3cmd access storage resources

There are many open source Amazon S3 tools on the market, such as s3cmd, S3Browser, etc. This article mainly introduces how to use s3cmd to connect to the object gateway of CubeFS.
s3cmd is a free and open source Amazon S3 command line tool that supports operations such as creating buckets and uploading and downloading objects. It can be installed in the centos system through the following command:

```bash
yum install s3cmd
```
### S3cmd tool config

Before configuring the s3cmd tool, you first need to create a user for the cluster. To execute the command, refer to the user management chapter [2] in the official documentation. After the user is created, you can use its Access Key and Secret Key to configure the s3cmd tool:

```bash
# s3cmd --configure
Access Key: user AK
Secret Key: user SK
Default Region [US]: the clusterName of the cluster master configuration file is the cluster name.
S3 Endpoint [s3.amazonaws.com]: object gateway IP address + listening port, such as 192.168.0.5:17410
DNS-style bucket+hostname:port template for accessing a bucket [%(bucket)s.s3.amazonaws.com]: object gateway IP address/%(bucket), such as 192.168.0.5/%(bucket)
Encryption password: Press Enter to use the default value
Path to GPG program [/usr/bin/gpg]: Press Enter to use the default value
Use HTTPS protocol [Yes]: no
HTTP Proxy server name: Press Enter to use the default value
```

At this point, the s3cmd tool is configured.

## Create Bucket

Execute the following command to create a bucket.

```bash
# s3cmd mb s3://my-bucket-test  
Bucket 's3://my-bucket-test/' created
```

After successful creation, you can use the s3cmd enumeration command to confirm whether the creation is successful.

```bash
# s3cmd ls |grep my-bucket-test 
2023-05-04 06:40  s3://my-bucket-test
```

## Upload object

Execute the following command to upload objects to the created bucket.

```bash
# s3cmd put test s3://my-bucket-test 
upload: 'test' -> 's3://my-bucket-test/test'  [1 of 1] 
 4 of 4   100% in    0s     5.43 B/s  done 
```

After the upload is successful, you can use the s3cmd enumeration command to confirm whether the creation is successful.

```bash
# s3cmd ls s3://my-bucket-test 
2023-05-04 06:41         4   s3://my-bucket-test/test
```

### Download object

Execute the following command to download the bucket object.

```bash
# s3cmd get s3://my-bucket-test/test  
download: 's3://my-bucket-test/test' -> './test'  [1 of 1] 
 4 of 4   100% in    0s    45.62 B/s  done 
```

### Delete object

Execute the following command to delete the bucket object.

```bash
# s3cmd del s3://my-bucket-test/test     
delete: 's3://my-bucket-test/test' 
```

After successful execution, confirm whether the deletion is successful by using the s3cmd enumeration command.

```bash
# s3cmd ls s3://my-bucket-test
```

##  S3SDK access storage resources

In addition to using the third-party Amazon S3 tool to connect to the CubeFS object gateway, you can also access storage resources through the S3SDK. For specific code examples, please refer to the official website's Object Storage Usage Guide [3], which will not be described in this article.

## Summary

Through this article, users are familiar with the configuration of CubeFS object gateway and how to access object resources through the open source Amazon S3 command line tool s3cmd.

## References

1. [Distributed deployment cluster](https://cubefs.io/zh/docs/master/deploy/manual-deploy.html)

2. [User management](https://cubefs.io/zh/docs/master/maintenance/admin-api/master/user.html)

3. [Object Storage Usage Guide](https://cubefs.io/docs/master/user-guide/objectnode.html)
