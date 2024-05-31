# Background

By default, data in CubeFS is kept permanently, and in order to store data cost-effectively throughout its lifecycle, CubeFS supports an AWS S3-compatible object storage lifecycle management strategy. CubeFS currently supports lifecycle expiration deletion policies, which can effectively clean up expired data and release storage resources. This article will guide you to understand how to use CubeFS object storage lifecycle management policies.

# Protocol parsing

A lifecycle configuration is an XML file that consists of a set of rules that predefine the actions you want to perform on an object during its lifetime.

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

Root-level label for life cycle configuration

### Rule

Life Cycle Configuration Rule (Required)

* Life cycle configuration requests require at least one Rule and multiple rules can be configured. Each Rule is an independent life cycle Rule. If multiple rules are configured, the rules take effect simultaneously.
### Expiration

Life Cycle Configuration Rule object expiration time (Required)

* Each Rule can be configured with an object expiration time that is applied to the rule. Two types of expiration time rules can be set: based on the object lifetime or based on the specified date. You must set only one of them.
#### Based on the days（Days）

You can specify how many days after an object is uploaded to execute the life cycle rule. When the specified number of days is reached, the life cycle rule is applied to all qualified objects.

When specifying Days in the life cycle configuration Expiration, note the following:

* Days Indicates the number of days between the creation of an object and the execution of a lifecycle rule, that is, the number of days that an object exists.
* If Days is set, the value must be a positive integer greater than 0.
* Calculate the expiration time of an object: Add the number of days specified in the rule to the creation time of the object, and round the resulting time to 00:00 UTC of the next day.
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
This example shows a life cycle configuration rule that specifies that the expiration time is 3 days since the object was created. For example, if an object is created at 10:30 am UTC on November 1, 2021, this rule will be enforced on that object at 00:00 UTC on November 5, 2021.
#### Based on the specified date（Date）

A lifecycle rule can be executed on a specified date, and when the specified date is reached, the rule is applied to all eligible objects.

When specifying a Date in the Expiration section of the life cycle configuration, note the following:

* If you set Date, the date value must conform to ISO 8601 format and the time must be 00:00 UTC.
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
This example shows a life cycle configuration rule that specifies an expiration time of 00:00 UTC on November 2, 2021. For example, if an object is created at 10:30 am UTC on November 1, 2021, then this rule will be executed on November 2, 2021 at 00:00 UTC.
### Filter

Life cycle Configuration rule Object Name Filtering Criteria (optional)

* For each Rule, you can configure valid filtering conditions to specify that the rule applies to all or some objects in the bucket. Currently, only prefix conditions are supported.
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
This example shows a lifecycle configuration rule that applies to objects whose object names in the bucket are prefixed with 'logs/'. For example, this lifecycle rule applies to the objects' logs/ a.xt 'and' logs/ b.xt ', but not to the object 'example.jpg'.
### ID

Life Cycle Configuration Rule ID (Required)

* Each Rule has its own unique ID. The ids of multiple rules in a lifecycle configuration request cannot be the same. The length of the ids must be smaller than 255 bytes.
### Status

Life Cycle Configuration Rule switch (Required)

* You can set Status for each Rule, which can be Enabled or Disabled. If Rule is set to Disabled, none of the operations defined in the Rule are performed.
# Example

### example 1

In this example, the life cycle rule takes effect for all objects in the bucket. The object expiration time is one day after the object is created.

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
		Region:           aws.String("<region>"),
		Endpoint:         aws.String("<endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<AccessKeyId>", "<SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.PutBucketLifecycleConfigurationInput{
		Bucket: aws.String("<bucket name>"),
		LifecycleConfiguration: &s3.BucketLifecycleConfiguration{
			Rules: []*s3.LifecycleRule{
				{
					ID:     aws.String("<rule id>"),
					Status: aws.String("<Enabled/Disabled>"),
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

### example 2

In this example, the configured lifecycle rule takes effect for objects whose object names in the bucket are prefixed with logs/, and the object expires at 00:00 UTC on November 2, 2021.

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
		Region:           aws.String("<region>"),
		Endpoint:         aws.String("<endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<AccessKeyId>", "<SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.PutBucketLifecycleConfigurationInput{
		Bucket: aws.String("<bucket name>"),
		LifecycleConfiguration: &s3.BucketLifecycleConfiguration{
			Rules: []*s3.LifecycleRule{
				{
					ID:     aws.String("<rule id>"),
					Status: aws.String("<Enabled/Disabled>"),
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

### example 3

In this example, show how to query a configured lifecycle rule.

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
		Region:           aws.String("<region>"),
		Endpoint:         aws.String("<endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<AccessKeyId>", "<SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.GetBucketLifecycleConfigurationInput{
		Bucket: aws.String("<bucket name>"),
	}
	result, err := svc.GetBucketLifecycleConfiguration(input)
	if err != nil {
		fmt.Println("Get BucketLifecycle Failed ", err)
	} else {
		fmt.Println("Get BucketLifecycle Succeed ", result)
	}
}
```

### example 4

In this example, show how to remove an already configured lifecycle rule.

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
		Region:           aws.String("<region>"),
		Endpoint:         aws.String("<endpoint>"),
		S3ForcePathStyle: aws.Bool(true),
		Credentials:      credentials.NewStaticCredentials("<AccessKeyId>", "<SecretKeyId>", ""),
	}
	sess := session.Must(session.NewSession(conf))
	svc := s3.New(sess)
	input := &s3.DeleteBucketLifecycleInput{
		Bucket: aws.String("<bucket name>"),
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

 

