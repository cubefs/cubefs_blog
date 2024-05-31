# 背景

默认情况下，CubeFS中的数据会永久保存，为了使数据在整个生命周期内经济高效地存储，CubeFS支持兼容AWS S3的对象存储生命周期管理策略。目前CubeFS支持生命周期过期删除策略，可以有效清理过期数据，释放存储资源，本文将带领大家了解如何使用CubeFS对象存储生命周期管理策略。

# 协议解析

生命周期配置是 XML 文件，由一组规则组成，这些规则预定义了您希望在对象的生命周期内对对象执行的操作。

```plain
<LifecycleConfiguration>
   <Rule>
      <Expiration>
         <Date>timestamp</Date>
         <Days>integer</Days>
      </Expiration>
      <Filter>
         <Prefix>string</Prefix>
      </Filter>
      <ID>string</ID>
      <Status>string</Status>
   </Rule>
   ...
</LifecycleConfiguration>
```

### LifecycleConfiguration

生命周期配置的根级别标签

### Rule

生命周期配置规则（必填项）

* 生命周期配置请求最少要配置一条Rule，可以配置多条Rule，每条Rule是一个独立的生命周期规则，如果配置多条Rule则同时生效。
### Expiration

生命周期配置规则对象过期时间（必填项）

* 每一条Rule可以配置应用于此规则的对象过期时间，支持两种形式的过期时间规则设置：基于对象的存在期限或基于指定的日期，必须且只能设置其中一种。
#### 基于对象的存在期限（Days）

可以指定对象上传多少天后执行生命周期规则，到达指定的天数时，会向所有符合条件的对象应用此规则。

当在生命周期配置中的Expiration中指定Days时，请注意以下事项：

* Days是自对象创建到执行生命周期规则的间隔天数，即对象存在的天数。
* 如果设置Days，必须设置一个大于0的正整数。
* 计算对象过期时间方式：将规则中指定的天数与对象的创建时间相加，然后将得出的时间舍入至下一日的00:00 UTC。
```plain
<LifecycleConfiguration>
   <Rule>
      <Expiration>
         <Days>3</Days>
      </Expiration>
      ...
   </Rule>
</LifecycleConfiguration>
```
此示例显示了一个生命周期配置规则，规则中指定了过期时间是自对象创建3天。例如，如果一个对象的创建时间是2021年11月1日上午10:30 UTC，那么此对象在2021年11月5日凌晨00:00 UTC会被执行此规则。
#### 基于指定的日期（Date）

可以在指定的日期执行生命周期规则，到达指定的日期时，会向所有符合条件的对象应用此规则。

当在生命周期配置中的Expiration中指定Date时，请注意以下事项：

* 如果设置Date，则此日期值必须符合ISO 8601格式，时间必须为00:00 UTC。
```plain
<LifecycleConfiguration>
   <Rule>
      <Expiration>
         <Date>2021-11-02T00:00:00Z</Date>
      </Expiration>
      ...
   </Rule>
</LifecycleConfiguration>
```
此示例显示了一个生命周期配置规则，规则中指定了过期时间为2021年11月2日凌晨00:00 UTC。例如，如果一个对象的创建时间是2021年11月1日上午10:30 UTC，那么此对象在2021年11月2日凌晨00:00 UTC会被执行此规则。
### Filter

生命周期配置规则对象名过滤条件（选填项）

* 每一条Rule可以配置生效的过滤条件来指定此规则应用于存储桶内的所有对象或一部分对象，目前只支持前缀条件。
```plain
<LifecycleConfiguration>
   <Rule>
      <Filter>
         <Prefix>logs/</Prefix>
      </Filter>
      ...
   </Rule>
</LifecycleConfiguration>
```
此示例显示一个生命周期配置规则，此规则应用于存储桶内对象名前缀为`"logs/"`的对象。例如，此生命周期规则应用于对象`logs/a.txt`和`logs/b.txt`，但不应用于对象`example.jpg`。 
### ID

生命周期配置规则ID（必填项）

* 每一条Rule有自己独特的ID，一次生命周期配置请求中的多条Rule的ID不能重复，ID的长度应小于255个字节。
### Status

生命周期配置规则开关（必填项）

* 可以为每一条Rule设置Status，值可以是Enabled或Disabled。如果将Rule设置为Disabled，不会执行Rule中定义的任何操作。  
# 使用示例

### 示例1

在此示例中，配置的生命周期规则对存储桶中的所有对象生效，对象过期时间为创建后1天。

```plain
package main

import (
	"fmt"

	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/credentials"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

func PutBucketLifecycle() {
	conf := &aws.Config{
		Region:           aws.String("<此处填写region>"),
		Endpoint:         aws.String("<此处填写endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<此处填写AccessKeyId>", "<此处填写SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.PutBucketLifecycleConfigurationInput{
		Bucket: aws.String("<此处填写bucket名字>"),
		LifecycleConfiguration: &s3.BucketLifecycleConfiguration{
			Rules: []*s3.LifecycleRule{
				{
					ID:     aws.String("<此处填写rule id>"),
					Status: aws.String("<此处填写Enabled或Disabled>"),
					Expiration: &s3.LifecycleExpiration{
						Days: aws.Int64(1),
					},
				},
			},
		},
	}
	result, err := svc.PutBucketLifecycleConfiguration(input)
	if err != nil {
		fmt.Println("Put BucketLifecycle Failed ", err)
	} else {
		fmt.Println("Put BucketLifecycle Succeed ", result)
	}
}
```

### 示例2

在此示例中，配置的生命周期规则对存储桶中对象名前缀为logs/的对象生效，对象过期时间为2021年11月2日凌晨00:00 UTC。

```plain
package main

import (
	"fmt"

	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/credentials"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

func PutBucketLifecycle() {
	conf := &aws.Config{
		Region:           aws.String("<此处填写region>"),
		Endpoint:         aws.String("<此处填写endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<此处填写AccessKeyId>", "<此处填写SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.PutBucketLifecycleConfigurationInput{
		Bucket: aws.String("<此处填写bucket名字>"),
		LifecycleConfiguration: &s3.BucketLifecycleConfiguration{
			Rules: []*s3.LifecycleRule{
				{
					ID:     aws.String("<此处填写rule id>"),
					Status: aws.String("<此处填写Enabled或Disabled>"),
					Expiration: &s3.LifecycleExpiration{
						Date: aws.Time(time.Date(2021, 11, 2, 0, 0, 0, 0, time.UTC)),
					},
					Filter: &s3.LifecycleRuleFilter{
						Prefix: aws.String("logs/"),
					},
				},
			},
		},
	}
	result, err := svc.PutBucketLifecycleConfiguration(input)
	if err != nil {
		fmt.Println("Put BucketLifecycle Failed ", err)
	} else {
		fmt.Println("Put BucketLifecycle Succeed ", result)
	}
}
```

### 示例3

在此示例中，展示了如何查询已经配置的生命周期规则。

```plain
package main

import (
	"fmt"

	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/credentials"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

func GetBucketLifecycle() {
	conf := &aws.Config{
		Region:           aws.String("<此处填写region>"),
		Endpoint:         aws.String("<此处填写endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<此处填写AccessKeyId>", "<此处填写SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.GetBucketLifecycleConfigurationInput{
		Bucket: aws.String("<此处填写bucket名字>"),
	}
	result, err := svc.GetBucketLifecycleConfiguration(input)
	if err != nil {
		fmt.Println("Get BucketLifecycle Failed ", err)
	} else {
		fmt.Println("Get BucketLifecycle Succeed ", result)
	}
}
```

### 示例4

在此示例中，展示了如何删除已经配置的生命周期规则。

```plain
package main

import (
	"fmt"

	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/credentials"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/s3"
)

func DeleteBucketLifecycle() {
	conf := &aws.Config{
		Region:           aws.String("<此处填写region>"),
		Endpoint:         aws.String("<此处填写endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<此处填写AccessKeyId>", "<此处填写SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.DeleteBucketLifecycleInput{
		Bucket: aws.String("<此处填写bucket名字>"),
	}
	result, err := svc.DeleteBucketLifecycle(input)
	if err != nil {
		fmt.Println("Delete BucketLifecycle Failed ", err)
	} else {
		fmt.Println("Delete BucketLifecycle Succeed ", result)
	}
}
```
#   

#  

 

