# Amazon Web Services Cheat Sheet

> **Applies to:** Current AWS services and AWS CLI version 2
> **Last reviewed:** 2026-07-14

A practical reference for AWS account identity, IAM Identity Center authentication, common services, EC2, S3, IAM, VPC, Lambda, and RDS.

> [!WARNING]
> AWS CLI commands act against the credentials, account, region, and profile currently in use. Verify all four before making a change, especially when profiles have similar names across development and production accounts.

## Account and resource model

| Concept | Purpose |
|---|---|
| Organization | Multi-account governance and consolidated management |
| Account | Primary security, billing, quota, and resource-isolation boundary |
| Region | Geographic deployment area containing multiple Availability Zones |
| Availability Zone | Isolated location within a Region |
| ARN | Amazon Resource Name that uniquely identifies a resource |
| Tag | Key-value metadata used for ownership, cost, policy, and automation |

Use multiple accounts to separate environments and security boundaries rather than placing every workload into one account.

## Common services

| Area | Services |
|---|---|
| Compute | EC2, Lambda, ECS, EKS, Fargate, Elastic Beanstalk |
| Storage | S3, EBS, EFS, FSx, S3 Glacier storage classes |
| Databases | RDS, Aurora, DynamoDB, ElastiCache, Redshift |
| Networking | VPC, Elastic Load Balancing, Route 53, CloudFront, Transit Gateway, Direct Connect, Site-to-Site VPN |
| Security | IAM, IAM Identity Center, KMS, Secrets Manager, GuardDuty, Security Hub, WAF |
| Operations | CloudWatch, CloudTrail, AWS Config, Systems Manager |
| Integration | SQS, SNS, EventBridge, Step Functions, API Gateway |
| Infrastructure as code | CloudFormation, AWS CDK, Service Catalog |

## CLI setup and version

```bash
aws --version
aws configure list
aws configure list-profiles
aws configure get region --profile <profile>
```

Use named profiles instead of repeatedly overwriting the default profile.

## IAM Identity Center authentication

Configure a profile using the interactive wizard:

```bash
aws configure sso
```

Sign in and verify identity:

```bash
aws sso login --profile <profile>
aws sts get-caller-identity --profile <profile>
```

Sign out of cached IAM Identity Center sessions:

```bash
aws sso logout
```

AWS CLI version 2 uses browser-based Proof Key for Code Exchange by default on current releases. Use device authorization only where required:

```bash
aws sso login --profile <profile> --use-device-code
```

Prefer IAM Identity Center, role assumption, workload identity, and other short-lived credentials over long-lived IAM user access keys.

## Verify identity before every change

```bash
aws sts get-caller-identity --profile <profile>
aws configure get region --profile <profile>
```

A useful shell pattern:

```bash
export AWS_PROFILE=<profile>
export AWS_REGION=<region>
aws sts get-caller-identity
```

> [!CAUTION]
> Environment variables override profile-file settings in many AWS SDK and CLI scenarios. Inspect `AWS_PROFILE`, `AWS_REGION`, `AWS_DEFAULT_REGION`, and credential variables when behavior is unexpected.

## Output, queries, and pagination

```bash
aws <service> <operation> help
aws <service> <operation> --output json
aws <service> <operation> --output table
aws <service> <operation> --query '<jmespath-expression>'
aws <service> <operation> --no-cli-pager
```

Example:

```bash
aws ec2 describe-instances \
  --query 'Reservations[].Instances[].{Name:Tags[?Key==`Name`]|[0].Value,ID:InstanceId,State:State.Name,AZ:Placement.AvailabilityZone}' \
  --output table
```

Do not use `--no-paginate` on large inventories unless you understand the resulting API and output volume.

## Amazon S3

List buckets and objects:

```bash
aws s3 ls
aws s3 ls s3://<bucket>/<prefix>/
aws s3api get-bucket-location --bucket <bucket>
aws s3api get-bucket-versioning --bucket <bucket>
aws s3api get-object-lock-configuration --bucket <bucket>
```

Copy and synchronize data:

```bash
aws s3 cp <local-file> s3://<bucket>/<object>
aws s3 cp s3://<bucket>/<object> <local-path>
aws s3 sync <local-directory>/ s3://<bucket>/<prefix>/
aws s3 sync s3://<bucket>/<prefix>/ <local-directory>/
```

Preview a sync that would delete destination objects:

```bash
aws s3 sync <source> <destination> --delete --dryrun
```

> [!DANGER]
> `aws s3 sync --delete` removes objects from the destination that are absent from the source. Always run with `--dryrun`, inspect versioning and retention, and confirm the direction of the sync.

Create a bucket with region-appropriate configuration:

```bash
aws s3api create-bucket \
  --bucket <globally-unique-bucket-name> \
  --region <region> \
  --create-bucket-configuration LocationConstraint=<region>
```

The `us-east-1` API has special create-bucket behavior; verify the current command reference when automating bucket creation across Regions.

## Amazon EC2

Inventory:

```bash
aws ec2 describe-instances
aws ec2 describe-instances --instance-ids <instance-id>
aws ec2 describe-instance-status --include-all-instances
aws ec2 describe-volumes --filters Name=attachment.instance-id,Values=<instance-id>
aws ec2 describe-security-groups --group-ids <security-group-id>
```

Lifecycle operations:

```bash
aws ec2 stop-instances --instance-ids <instance-id>
aws ec2 start-instances --instance-ids <instance-id>
aws ec2 reboot-instances --instance-ids <instance-id>
```

Terminate only after inspecting attached storage and termination protection:

```bash
aws ec2 describe-instance-attribute \
  --instance-id <instance-id> \
  --attribute disableApiTermination

aws ec2 terminate-instances --instance-ids <instance-id>
```

> [!DANGER]
> Instance termination can delete EBS volumes whose `DeleteOnTermination` flag is enabled. Confirm snapshots, data ownership, Auto Scaling behavior, and replacement dependencies first.

## Systems Manager Session Manager

List managed instances and start a session:

```bash
aws ssm describe-instance-information
aws ssm start-session --target <instance-id>
```

Session Manager can reduce the need for inbound SSH exposure when the instance, IAM role, agent, and network path are correctly configured.

## IAM

Read-only inspection:

```bash
aws iam list-users
aws iam list-roles
aws iam get-role --role-name <role-name>
aws iam list-attached-role-policies --role-name <role-name>
aws iam list-role-policies --role-name <role-name>
aws iam get-account-authorization-details
```

Simulate a principal policy where permissions allow it:

```bash
aws iam simulate-principal-policy \
  --policy-source-arn <principal-arn> \
  --action-names <service:Action>
```

Avoid creating IAM users for applications. Use roles and temporary credentials. For human access, prefer IAM Identity Center.

## Role assumption

```bash
aws sts assume-role \
  --role-arn <role-arn> \
  --role-session-name <session-name>
```

The command returns temporary credentials. Prefer profile-based role configuration, credential processes, or workload federation over manually exporting tokens.

## VPC networking

Inventory:

```bash
aws ec2 describe-vpcs
aws ec2 describe-subnets
aws ec2 describe-route-tables
aws ec2 describe-internet-gateways
aws ec2 describe-nat-gateways
aws ec2 describe-network-acls
aws ec2 describe-security-groups
aws ec2 describe-vpc-endpoints
```

Create a VPC and subnet:

```bash
aws ec2 create-vpc --cidr-block <cidr>
aws ec2 create-subnet \
  --vpc-id <vpc-id> \
  --cidr-block <cidr> \
  --availability-zone <availability-zone>
```

Create and attach an internet gateway:

```bash
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway \
  --internet-gateway-id <internet-gateway-id> \
  --vpc-id <vpc-id>
```

Security-group inspection:

```bash
aws ec2 describe-security-group-rules \
  --filters Name=group-id,Values=<security-group-id>
```

Avoid broad inbound access from `0.0.0.0/0` or `::/0` unless public exposure is explicitly required and protected by additional controls.

## Elastic Load Balancing

```bash
aws elbv2 describe-load-balancers
aws elbv2 describe-listeners --load-balancer-arn <load-balancer-arn>
aws elbv2 describe-target-groups
aws elbv2 describe-target-health --target-group-arn <target-group-arn>
```

Target health often provides the fastest explanation for an apparently healthy load balancer that is not serving traffic.

## AWS Lambda

```bash
aws lambda list-functions
aws lambda get-function --function-name <function-name>
aws lambda get-function-configuration --function-name <function-name>
aws lambda list-versions-by-function --function-name <function-name>
aws lambda list-event-source-mappings --function-name <function-name>
```

Invoke a function:

```bash
aws lambda invoke \
  --function-name <function-name> \
  --cli-binary-format raw-in-base64-out \
  --payload '<json-payload>' \
  <output-file>
```

Update code only after confirming the deployment package, architecture, runtime, alias, and rollback version:

```bash
aws lambda update-function-code \
  --function-name <function-name> \
  --zip-file fileb://<archive.zip> \
  --publish
```

## Amazon RDS

```bash
aws rds describe-db-instances
aws rds describe-db-clusters
aws rds describe-db-snapshots
aws rds describe-events --source-type db-instance --duration 1440
```

Create a manual snapshot:

```bash
aws rds create-db-snapshot \
  --db-instance-identifier <db-instance-id> \
  --db-snapshot-identifier <snapshot-id>
```

Delete an instance with a final snapshot:

```bash
aws rds delete-db-instance \
  --db-instance-identifier <db-instance-id> \
  --final-db-snapshot-identifier <final-snapshot-id>
```

> [!DANGER]
> Avoid `--skip-final-snapshot` unless data destruction is explicitly approved and recoverability is not required. Check deletion protection, replicas, cluster membership, and application dependencies.

## CloudWatch and CloudTrail

```bash
aws cloudwatch list-metrics --namespace <namespace>
aws cloudwatch describe-alarms
aws logs describe-log-groups
aws logs tail <log-group-name> --since 1h --follow
aws cloudtrail lookup-events --max-results 50
```

CloudTrail lookup is useful for recent management events but does not replace a properly configured organization trail or event data store.

## Tagging and inventory

```bash
aws resourcegroupstaggingapi get-resources
aws resourcegroupstaggingapi get-resources \
  --tag-filters Key=<key>,Values=<value>
```

Tagging should support ownership, environment, application, cost allocation, data classification, and lifecycle automation.

## Troubleshooting checklist

1. Run `aws sts get-caller-identity`.
2. Confirm profile and Region.
3. Check whether environment variables override profile settings.
4. Verify the exact ARN and resource Region.
5. Check IAM policies, permission boundaries, service control policies, and resource policies.
6. Confirm quotas and service availability.
7. Inspect CloudTrail and service events.
8. Retry with `--debug` only long enough to collect evidence.

```bash
aws sts get-caller-identity
aws configure list
aws configure list-profiles
env | grep '^AWS_'
```

Debug logs can contain request metadata, account identifiers, endpoints, and signed-request details. Sanitize them before sharing.

## References

- [AWS CLI version 2 user guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)
- [IAM Identity Center authentication with AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)
- [AWS CLI command reference](https://docs.aws.amazon.com/cli/latest/reference/)
- [STS get-caller-identity](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html)
- [AWS services by category](https://aws.amazon.com/products/)
