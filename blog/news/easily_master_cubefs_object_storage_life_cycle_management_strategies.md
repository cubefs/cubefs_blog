---
blog: true
---

## Background

By default, data in CubeFS is saved permanently. In order to store data cost-effectively throughout its life cycle, CubeFS supports an object storage life cycle management strategy that is compatible with AWS S3. Currently, CubeFS supports the life cycle expiration deletion policy, which can effectively clean up expired data and release storage resources. This article will lead you to understand how to use the CubeFS object storage life cycle management policy.

## Protocol analysis

A lifecycle configuration is an XML file that consists of a set of rules that predefine the actions you want to perform on an object during its lifetime.

```yaml
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

Root level label for lifecycle configuration

###  Rule

Lifecycle configuration rules (required)

* A life cycle configuration request must configure at least one Rule. Multiple Rules can be configured. Each Rule is an independent life cycle rule. If multiple Rules are configured, they will take effect at the same time.

### Expiration

Life cycle configuration rule object expiration time (required)

* Each Rule can configure the object expiration time applied to this rule. It supports two forms of expiration time rule settings: based on the existence period of the object or based on a specified date. One of them must and can only be set.

### Object-based lifetime (Days)

You can specify how many days after the object is uploaded to execute the life cycle rule. When the specified number of days is reached, this rule will be applied to all objects that meet the conditions.

When specifying Days in Expiration in the lifecycle configuration, please note the following:

* Days is the number of days between object creation and execution of life cycle rules, that is, the number of days the object exists.
* If Days is set, a positive integer greater than 0 must be set.
* Calculate the object expiration time by adding the number of days specified in the rule to the creation time of the object, and then rounding the resulting time to 00:00 UTC on the next day.

```yaml
<LifecycleConfiguration>
   <Rule>
      <Expiration>
         <Days>3</Days>
      </Expiration>
      ...
   </Rule>
</LifecycleConfiguration>
```

This example shows a lifecycle configuration rule that specifies an expiration time of 3 days from object creation. For example, if the creation time of an object is 10:30 am UTC on November 1, 2021, then this object will be executed at 00:00 am UTC on November 5, 2021.
### Based on the specified date (Date)

Lifecycle rules can be executed on a specified date, and when the specified date is reached, the rule is applied to all eligible objects.

When specifying a Date in Expiration in the lifecycle configuration, please note the following:

* If Date is set, this date value must conform to ISO 8601 format and the time must be 00:00 UTC.

```yaml
<LifecycleConfiguration>
   <Rule>
      <Expiration>
         <Date>2021-11-02T00:00:00Z</Date>
      </Expiration>
      ...
   </Rule>
</LifecycleConfiguration>
```

This example shows a lifecycle configuration rule that specifies an expiration time of November 2, 2021 at 00:00 UTC. For example, if the creation time of an object is 10:30 AM UTC on November 1, 2021, then this rule will be executed on this object at 00:00 AM UTC on November 2, 2021.

### Filter

Life cycle configuration rule object name filter condition (optional)

* Each Rule can be configured with effective filter conditions to specify that this rule applies to all objects or a subset of objects in the bucket. Currently, only prefix conditions are supported.

```yaml
<LifecycleConfiguration>
   <Rule>
      <Filter>
         <Prefix>logs/</Prefix>
      </Filter>
      ...
   </Rule>
</LifecycleConfiguration>
```

This example shows a lifecycle configuration rule that applies to objects in a bucket whose object names are prefixed with "logs/". For example, this lifecycle rule applies to objects logs/a.txt and logs/b.txt, but not to object example.jpg.

### ID

Lifecycle configuration rule ID (required)

* Each Rule has its own unique ID. The IDs of multiple Rules in a life cycle configuration request cannot be repeated, and the length of the ID should be less than 255 bytes.


### Status

Lifecycle configuration rule switch (required)

* Status can be set for each Rule, and the value can be Enabled or Disabled. If the Rule is set to Disabled, no operations defined in the Rule will be performed.

## Usage example

### Example 1

In this example, the configuration life cycle rules take effect on all objects in the barrel, and the target expires time is 1 day after the creation.  

```go

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
    Region:           aws.String("<Fill in region here>"),
    Endpoint:         aws.String("Fill in endpoint here>"),
    S3ForcePathStyle: aws.Bool(true),
    Credentials:      credentials.NewStaticCredentials("<Fill in AccessKeyId here>", "<Fill in SecretKeyId here>", ""),
  }
  sess := session.Must(session.NewSession(conf))
  svc := s3.New(sess)
  input := &s3.PutBucketLifecycleConfigurationInput{
    Bucket: aws.String("<Fill in bucket name here>"),
    LifecycleConfiguration: &s3.BucketLifecycleConfiguration{
      Rules: []*s3.LifecycleRule{
        {
          ID:     aws.String("<Fill in rule id here>"),
          Status: aws.String("<Fill in Enabled or Disabled here>"),
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

### Example 2

In this example, the configured lifecycle rules take effect on objects whose object names are prefixed by logs/ in the bucket, and the object expiration time is 00:00 UTC on November 2, 2021.

```go

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
		Region:           aws.String("<Fill in region here>"),
		Endpoint:          aws.String("Fill in endpoint here>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<Fill in AccessKeyId here>", "<Fill in SecretKeyId here>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.PutBucketLifecycleConfigurationInput{
		Bucket: aws.String("<Fill in bucket name here>"),
		LifecycleConfiguration: &s3.BucketLifecycleConfiguration{
			Rules: []*s3.LifecycleRule{
				{
					ID:     aws.String("<Fill in rule id here>"),
					Status: aws.String("<Fill in Enabled or Disabled here>"),
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

### Example 3

This example shows how to query configured lifecycle rules.

```go
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
		Region:           aws.String("<Fill in region here>"),
		Endpoint:         aws.String("Fill in endpoint here>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<Fill in AccessKeyId here>", "<Fill in SecretKeyId here>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.GetBucketLifecycleConfigurationInput{
		Bucket: aws.String("<Fill in bucket name here>"),
	}
	result, err := svc.GetBucketLifecycleConfiguration(input)
	if err != nil {
		fmt.Println("Get BucketLifecycle Failed ", err)
	} else {
		fmt.Println("Get BucketLifecycle Succeed ", result)
	}
}
```

### Example 4

In this example, it is shown how to delete an already configured lifecycle rule.

```go
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
		Region:           aws.String("<Fill in region here>"),
		Endpoint:         aws.String("Fill in endpoint here>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<Fill in AccessKeyId here>", "<Fill in SecretKeyId here>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.DeleteBucketLifecycleInput{
		Bucket: aws.String("<Fill in bucket name here>"),
	}
	result, err := svc.DeleteBucketLifecycle(input)
	if err != nil {
		fmt.Println("Delete BucketLifecycle Failed ", err)
	} else {
		fmt.Println("Delete BucketLifecycle Succeed ", result)
	}
}
```


## about the author

Chenyang Zhao, CubeFS Contributor, is responsible for the research and development of object storage and currently works for OPPO.
