# ansible-role-cfn-cloudwatch-alarms

## Parameters
| Parameter        | Required           | Default  | Description |
|:--------------------------------:|:-------------:| -----:| -------------------------------------------------------------------------:|
| cloudwatch_stack_name | yes | | CloudWatch alarms stack name |
| cloudwatch_region | yes | | Region into which alarms are deployed |
| cloudwatch_alarms | yes | | A list of cloudwatch alarms |
| cloudwatch_sns_topic_name | no | | Name of SNS topic to create. Across all alarms in stack. Conflicts with <strong>sns_topic_name</strong> in cloudwatch_alarms list item. Don't specify both.  |
| cloudwatch_sns_topic_subscriptions | no | | Optional subscription for the topic |

### Parameters per list item in cloudwatch_alarms
| Parameter        | Required           | Default  | Description |
|:--------------------------------:|:-------------:| -----:| -------------------------------------------------------------------------:|
| name | yes | | Name of the alarm |
| namespace | yes | | Namespace of metric to use with the alarm |
| metric | yes | | Metric to use with the alarm |
| dimensions | yes | | List of dimensions to use with the metric |
| threshold | yes | | Threshold for metric which triggers alarm |
| comparison_operator | yes | | Comparison operator to use against the threshold |
| period_length | yes | | Length of time that constitutes a period. Combined with <strong>eval_periods</strong> to determine how long before triggering alarm |
| statistic | yes | |
| unit | no | Default unit for metric | Unit of threshold |
| sns_topic_name | no | | Name of topic to create for alarm. Conflicts with <strong>cloudwatch_sns_topic_name</strong>, which applies across all alarms |
| sns_topic_subscription | no | | List of subscriptions for SNS topic |

#### Parameters per list item in dimensions
| Parameter        | Required           | Default  | Description |
|:--------------------------------:|:-------------:| -----:| -------------------------------------------------------------------------:|
| name | yes | | Name of dimension to use |
| value | yes | | Value of dimension to use |

#### Parameters per list item in cloudwatch_sns_topic_subscriptions and sns_topic_subscriptions (per alarm)
| Parameter        | Required           | Default  | Description |
|:--------------------------------:|:-------------:| -----:| -------------------------------------------------------------------------:|
| endpoint | yes | | Endpoint of subscription |
| protocol | yes | | Protocol of subscription |

## Example Playbook
```
- hosts: localhost
  vars:
    cloudwatch_stack_name: "cloudwatch-alarms-test"
    cloudwatch_region: "us-west-2"
    cloudwatch_sns_topic_name: "SomeAlarms"
    cloudwatch_sns_topic_subscriptions:
      - endpoint: "ajohnson@mgage.com"
        protocol: "email"
    cloudwatch_alarms:
      - name: "Test"
        namespace: "AWS/ES"
        metric: "WriteLatency"
        eval_periods: 1
        statistic: "Average"
        threshold: 0.0005
        unit: "Seconds"
        period_length: 60
        comparison_operator: "GreaterThanOrEqualToThreshold"
        dimensions:
          - name: "DomainName"
            value: "elastic-elasti-14r8s9colp4zz"
          - name: "ClientId"
            value: "602503371133"

      - name: "Testing"
        namespace: "AWS/ES"
        metric: "ClusterStatus.yellow"
        eval_periods: 1
        statistic: "Average"
        threshold: 0
        period_length: 60
        comparison_operator: "GreaterThanThreshold"
        dimensions:
          - name: "DomainName"
            value: "elastic-elasti-14r8s9colp4zz"
          - name: "ClientId"
            value: "602503371133"

      - name: "Testineuog"
        namespace: "AWS/ES"
        metric: "DiskQueueDepth"
        eval_periods: 1
        statistic: "Average"
        threshold: 0
        unit: "Seconds"
        period_length: 60
        comparison_operator: "GreaterThanThreshold"
        dimensions:
          - name: "DomainName"
            value: "elastic-elasti-14r8s9colp4zz"
          - name: "ClientId"
            value: "602503371133"


  roles:
    - role: ansible-role-cfn-cloudwatch-alarms
```
