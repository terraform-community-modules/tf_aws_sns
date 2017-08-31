sns terraform module
===========

A terraform module to provide Simple Notification Service (SNS) in AWS.

[![CircleCI](https://circleci.com/gh/tuxpower/tf_aws_sns.svg?style=svg)](https://circleci.com/gh/tuxpower/tf_aws_sns)

Module Input Variables
----------------------

- `name` - friendly name for the SNS topic
- `display_name` - display name for the SNS topic
- `policy` - fully-formed AWS policy as JSON
- `delivery_policy` - SNS delivery policy

Usage
-----

```hcl
module "sns" {
  source = "github.com/terraform-community-modules/tf_aws_sns"

  name         = "my-sns"

  display_name = "my-topic"
  policy       = <<POLICY
{
  "Version": "2012-10-17",
  "Id": "snspolicy",
  "Statement": [
    {
      "Sid": "First",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "sns:Publish",
      "Resource": "${aws_sns_topic.my-sns.arn}",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "${aws_sns_topic.my-sns.arn}"
        }
      }
    }
  ]
}
POLICY
  delivery_policy = "{\"http\":{\"defaultHealthyRetryPolicy\":{\"numRetries\":5}}}"
}
```

Outputs
=======

 - `id` - ARN of the SNS topic
 - `arn` - ARN of the SNS topic

Authors
=======

Created and maintained by [Jose Gaspar](https://github.com/tuxpower)

License
=======

Apache 2 Licensed. See LICENSE for full details.
