### AWS CLI Cheat Sheet

#### Introduction to AWS CLI
The AWS Command Line Interface (CLI) is a powerful tool to interact with AWS services, enabling scripting and automation for efficient cloud management.

- **Purpose**: Manage AWS services, automate tasks, and script operations.

#### Configuration and Setup
**Configure AWS CLI**
- `aws configure`
- Sets up AWS credentials (Access Key, Secret Key) and default region.

**View Configuration**
- `aws configure list`
- Lists the current AWS CLI configuration settings.

**Using Profiles**
- `aws configure --profile [profile_name]`
- Configure multiple profiles for different AWS accounts or roles.

#### Amazon S3 (Simple Storage Service)
**List S3 Buckets**
- `aws s3 ls`
- Lists all S3 buckets in the account.

**Create S3 Bucket**
- `aws s3 mb s3://[bucket-name]`
- Creates a new S3 bucket.

**Copy Files to/from S3**
- `aws s3 cp [local_path] s3://[bucket-name]/[object]`
- `aws s3 cp s3://[bucket-name]/[object] [local_path]`
- Copies files to or from S3.

**Sync Local Directory with S3**
- `aws s3 sync [local_path] s3://[bucket-name]`
- Synchronizes contents of a local directory with an S3 bucket.

#### EC2 (Elastic Compute Cloud)
**List EC2 Instances**
- `aws ec2 describe-instances`
- Displays information about EC2 instances.

**Start/Stop EC2 Instances**
- `aws ec2 start-instances --instance-ids [instance_id]`
- `aws ec2 stop-instances --instance-ids [instance_id]`
- Starts or stops specified EC2 instances.

**Create EC2 Key Pair**
- `aws ec2 create-key-pair --key-name [key_name]`
- Creates a new key pair for EC2 instances.

**Launch EC2 Instance**
- `aws ec2 run-instances --image-id [ami_id] --count [number] --instance-type [type] --key-name [key_name]`
- Launches a specified number of EC2 instances.

**Terminate EC2 Instance**
- `aws ec2 terminate-instances --instance-ids [instance_id]`
- Terminates specified EC2 instances.

#### IAM (Identity and Access Management)
**List IAM Users**
- `aws iam list-users`
- Lists all IAM users in the AWS account.

**Create IAM User**
- `aws iam create-user --user-name [username]`
- Creates a new IAM user.

**Attach Policy to User**
- `aws iam attach-user-policy --policy-arn [policy_arn] --user-name [username]`
- Attaches a managed policy to an IAM user.

**Create IAM Role**
- `aws iam create-role --role-name [role_name] --assume-role-policy-document [policy_json]`
- Creates a new IAM role with specified trust relationships.

#### Networking with VPC
**Describe VPCs**
- `aws ec2 describe-vpcs`
- Lists all VPCs in the account.

**Create VPC**
- `aws ec2 create-vpc --cidr-block [cidr_block]`
- Creates a new VPC with the specified CIDR block.

**Create Subnet**
- `aws ec2 create-subnet --vpc-id [vpc_id] --cidr-block [cidr_block]`
- Creates a subnet in a specified VPC.

**Create Internet Gateway**
- `aws ec2 create-internet-gateway`
- Creates an internet gateway for VPC.

**Attach Internet Gateway to VPC**
- `aws ec2 attach-internet-gateway --internet-gateway-id [igw_id] --vpc-id [vpc_id]`
- Attaches an internet gateway to a VPC.

#### AWS Lambda
**List Lambda Functions**
- `aws lambda list-functions`
- Lists AWS Lambda functions in the account.

**Create Lambda Function**
- `aws lambda create-function --function-name [name] --runtime [runtime] --role [role_arn] --handler [handler] --zip-file fileb://[file_path]`
- Creates a new Lambda function.

**Invoke Lambda Function**
- `aws lambda invoke --function-name [name] --payload [payload] [output_file]`
- Invokes a Lambda function with specified payload.

**Update Lambda Function Code**
- `aws lambda update-function-code --function-name [name] --zip-file fileb://[file_path]`
- Updates the code of an existing Lambda function.

#### AWS RDS (Relational Database Service)
**List RDS Instances**
- `aws rds describe-db-instances`
- Lists all RDS instances in the account.

**Create RDS Instance**
- `aws rds create-db-instance --db-instance-identifier [id] --allocated-storage [size] --db-instance-class [class] --engine [engine] --master-username [username] --master-user-password [password]`
- Creates a new RDS database instance.

**Delete RDS Instance**
- `aws rds delete-db-instance --db-instance-identifier [id] --skip-final-snapshot`
- Deletes an RDS database instance.

#### Tips for Using AWS CLI
- **Scripting and Automation**: Integrate AWS CLI commands into scripts for automation of AWS services.
- **Output Formatting**: Use the `--output` option to format the CLI output as json, text, or table.
- **Version Update**: Regularly update AWS CLI to the latest version for new features and improvements.
- **Help and Documentation**: Use `aws [service] [command] help` for detailed information and examples.
