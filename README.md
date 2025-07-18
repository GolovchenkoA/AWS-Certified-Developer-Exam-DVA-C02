# AWS-Certified-Developer-Exam-DVA-C02
[AWS Certified Developer Exam with this AWS Developer Udemy Course [DVA-C02]](https://www.udemy.com/course/aws-certified-developer-associate-exam-training/) 

[DVA-CO2 Exam link](https://aws.amazon.com/certification/certified-developer-associate/)

[DigitalCloud AWS Hands-On Challenge Labs](https://digitalcloud.training/aws-hands-on-challenge-labs) - Optional. Additional labs using an AWS Sandbox.

[Course Downloads GitHub](https://github.com/nealdct/aws-dva-code)

[Course Downloads](https://digitalcloud.training/aws-certified-developer-course-downloads/)


👉 All additional files (including slides) are stored on Google Disc in the folder `Certification Courses\Kubernetes\AWS\DVA-CO2 AWS Certified Developer Exam\` and there is an email from ...@digitalcloud.training


# Section 2 AWS Accounts and IAM

## 8. AWS Account and Budget
- Account. Add alias
- enable MFA
- Account. Activate AM user and role access to Billing information
- Billing preferences. Alert preferences. Recieve AWS Free Tier Alerts
-  Budgets. create a monthly budget
- Cost Explorer

## Section 3. Command Line Interfica (CLI)

## 18. Configure credentials for the AWS CLI
[Configuration and credential file settings in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

User -> Permissions -> add permissions
User -> Security Credentials -> Generate Access Key
CLI  -> use the generated access key id and secret access key
```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

check `aws/.config` and `aws/.credentials`  files.

### 19. Overview of using the AWS CLI

```
aws help
aws ec2 help
aws ec2 describe-instances

aws s3 help
aws s3 cp file.txt s3://your_bucket_here
aws s3 ls
```

### AWS S3API
The aws `s3api` command provides direct access to the Amazon Simple Storage Service (Amazon S3) APIs,  enabling operations that are not exposed in the high-level s3 commands ❗❗❗

Example: Creating a Bucket

To create a new bucket in S3, you can use the create-bucket command.
```
aws s3api create-bucket --bucket my-new-bucket --region us-west-2
```

### 20. Assumig IAM Roles (CLI)
[Methods to assume a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage-assume.html)
[How to assume IAM role from CLI](https://www.youtube.com/watch?v=xmBA6cxZyJU)

1. create a role (by default it can be assumed by everyone user from your account)
2. add permissions to the role (can be done during the 1st step)
3. create a user
4. [Grant permissions to switch roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_permissions-to-switch.html). add an inline policy to the user or add user to a group that has the permission to assume a role

Thus when we create a role we create 3 objects❗❗❗:
- Role
- Permissions that's a provided by the role
- Policy that describes who can assume the role

Policy example:
```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "sts:AssumeRole",
    "Resource": "arn:aws:iam::account-id:role/role_name*"
  }
}
```

5. [How to setup a role for command line](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-cli.html#switch-role-cli-scenario-prod-env)
6. Execute a command using the profile
```
aws s3 ls --profile=your_profile_name
```


## Section 4 Amazon VPC, EC2 and ELB

📝 For additional information see the Slides

### 22. Amazon VPC, Security Group adn NACLs (Network ACLs)

[What Is Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
[VCP Network Access Control Lists NACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)

<img width="640" height="393" alt="image" src="https://github.com/user-attachments/assets/7ec95abc-988d-415a-b086-6309f7595fe0" />

- VPC is a logicl area within a Region. By default each VPC is isolated from other VPCs. It spans the entire AWS Region and might contain multiple subnets
- There are multiple VPCs within a region are allowed
- Subnets are exist within AZs, but they can be created within a single AZ as well.
- `VPC router` takes care about routing within a VPC and outside the VPC
- `route table` is used to configure `VPC router`
- `API Gateway` is used to get access to the Internet
- Each VPC has it's own CIDR block of IPs
- By default 5 VPCs are allowed wintin a region, but it can be extended
- By default a VPC is created within a region with subnets in each AZ


#### **VPC Components**
<img width="1016" height="540" alt="image" src="https://github.com/user-attachments/assets/d90fd925-e486-49b5-991c-f926a40a819c" />


**Security Groups and NALs**

- Security Groups are scoped to a specific VPC within a region.
- Security Groups only have allow rules. All the rest by default is denied.
- Security Group is stateful. That means if some traffic is allowed to out then it allows ANY inboud traffic associatied with that connection.
- Networks ACLs have inbound and outbound rules
- Each NACL has explicit allow and deny rule
<img width="1264" height="722" alt="image" src="https://github.com/user-attachments/assets/86097486-26b9-4791-b014-c90d8f1d792d" />


### 23. AWS EC2 Overivew

[EC2 Instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html)

ENI - Elastic Network Interface (attached to an EC2 instance)
- EC2 instances in `public networks` may have a private IP and a public IP (optional)
- you continue to pay for a provisioned EBS volume even if the EC2 instance it’s attached to is stopped.
- We pay for EBS voluem size (attached to EC2). If we provisioned 10GB we pay for 10 GB even we use 1 GB


3 types of IP addresses:
- Public IP
- Private IP
- Elastic IP (Public Static IP)


Network Interfaces:
- Elastic Network Interface (ENI) - standard. for all instances
- Elastic Network Adapter (ENA) - when need high bendwidth and low latency (inter instance connection) . NOT for all instances
- Elastic Fabric Adapter (EFA) - for high performcance computing . for all instances

#### EBS Volumes
[EBS volume types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html)

🛠️ io1, io2 support multi-attach
**!!!! find st1, sc1. They are in the course video. See the slides!!!!!**
<img width="1159" height="491" alt="image" src="https://github.com/user-attachments/assets/2f9216ed-a937-4d8c-84b3-b071b6b84247" />
<img width="1253" height="690" alt="image" src="https://github.com/user-attachments/assets/6030f940-fbb9-4ef2-9c94-04e107c027be" />
<img width="1268" height="623" alt="image" src="https://github.com/user-attachments/assets/1c9a0085-dc05-4107-837b-17c7fbe53383" />


### 24. Create a Custom VPC
[Creating a custom VPC github example](https://github.com/nealdct/aws-dva-code/blob/main/amazon-vpc/custom-vpc.md)

## 25. Amazon EBS ad Instance Stores

These are 2 types of storages:
- EBS
- Instance Storages

- A EBS volume is in the same AZ as an EC2 instance
- A EBS volume is replicated within an AZ
- EBS volumes are attached over the network
- EBS snapshots are sotred on S3 (outside the EBS volume AZ)
- EBS snapshots can be used to create an EBS volume in a different AZ
- Instance storages physically attached to the host server where EC2 instances are created
- Instance storages are ephemeral (deleted when an EC2 instance is shot down or power of the host server is turned off)

### 26. Create And Attach an EBS Volume
Create And Attach an EBS Volume [step by step manual github](https://github.com/nealdct/aws-dva-code/blob/main/amazon-ebs/amazon-ebs-volumes.md), including creating file system and monting a volume

list non-loop volumes attached to Linux
```
sudo lsblk -e7
```

### 27. Amazon Elastic File System (EFS)

[Working with EFS gihub](https://github.com/nealdct/aws-dva-code/blob/main/amazon-efs/working-with-efs.md)

- EFS supports Linux only. Uses NFS (network file system)
- EFS can be mount to multiple EC2 instances in multiple AZs
- EFS instances are mount to `mount points` in the local AZ
- `One zone` file system has one target in a single AZ
- Write operations of EFS volumes are consistancy stored across multiple AZs


[EFS Storages Classes](https://aws.amazon.com/efs/storage-classes/)
<img width="918" height="454" alt="image" src="https://github.com/user-attachments/assets/e91ead2d-7480-4f0d-9960-fd8e0aa2140e" />


### 29. Amazon EC2 User Data and Metadata

[GitHub examples](https://github.com/nealdct/aws-dva-code/blob/main/amazon-ec2/user-data-metadata.md)

(Instance Metadata Service) IMDSv1 vs IMDSv2 . V2 is newer. It requires a token and it's more secure . [How to get a token for IMDSv2](https://github.com/nealdct/aws-dva-code/blob/main/amazon-ec2/user-data-metadata.md)

[Access instance metadata for an EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html)
```
curl http://169.254.169.254/latest/meta-data

curl http://169.254.169.254/latest/meta-data/local-ipv4
curl http://169.254.169.254/latest/meta-data/public-ipv4
```

**`User Data`** a EC2 field. We can define code there which will be executed when the instance starts (supports Batch and PowerShell as well)
User data can be up to 16KB before it's encoded to base64. It's encoded by default by AWS Console and CLI

### 31. Access Keys and IAM Roles with EC2

- Access Keys are long live credentials and we should avoid to use it.
- An Access Key is associated with a user and use permissions assigned with the user
- We can use a user access key within EC2 when we use CLI, but that's a bad practice and instead it's better to use IAM Role (with a Policy) that's assigned to the EC2 instance

### 33 Amazon EC2 Auto Scaling

[Auto Scaling Groups (ASG)](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-groups.html)
[What is EC2 Auto Scaling?](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)

**Auto Scaling Group (ASG)** manages a fleet of EC2 instances, automatically launching or terminating instances based on defined policies, schedules, or health checks.

- Works with EC2, ECS and EKS
- Maintenan availability and scale capacity
- Integrated with CloudWatch (monitoring), Elastic Load Balancing (ELB), Amazon VPC to deploy EC2s across different AZs
- Auto Scaling Groups (ASG) replaces failed instances
- Scaling policies defines how to respond to changes on demand
- Supports auto scaling and scheduled scaling. 

[Auto Scaling Lunch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/launch-templates.html) define EC2 configurations
`Lunch Config` is replaced by `Lunch Templates` and has fewer settings

**Types of Auto Scaling**

Auto scaling dynamically adjusts resources to meet application demands, ensuring optimal performance and cost efficiency. Below are the primary types of auto scaling:

-** Manual Auto Scaling** This involves manually adjusting the number of instances based on observed workload changes. While it provides control, it requires constant monitoring and is less effective during rapid demand fluctuations.

- **Dynamic Auto Scaling** Dynamic scaling automatically adjusts resources in response to real-time metrics like CPU usage, memory, or network traffic. It ensures quick reactions to workload changes, making it ideal for unpredictable traffic patterns.

- **Predictive Auto Scaling** Predictive scaling uses historical data and machine learning to forecast future demand and proactively adjust resources. This prevents performance issues during anticipated traffic spikes.

-** Scheduled Auto Scaling** Scheduled scaling adjusts resources based on predefined schedules or patterns. For example, it can scale up during business hours and scale down during off-peak times, making it suitable for predictable workloads.

- **Target Tracking Auto Scaling** This method maintains a specific target metric, such as keeping CPU usage at a defined percentage. It continuously monitors and adjusts resources to meet the target, ensuring consistent performance.

- **Resource-Based Auto Scaling** Instead of scaling entire instances, this focuses on specific resources like databases or load balancers. It ensures that individual components meet demand without affecting the entire system.

-** Horizontal vs. Vertical Scaling Horizontal scaling** (scaling out/in) adds or removes instances to handle demand, while vertical scaling (scaling up/down) increases or decreases the capacity of existing resources. Horizontal scaling is more common in cloud environments due to its flexibility and minimal downtime.

Each type of auto scaling serves specific use cases, allowing organizations to optimize performance, reliability, and cost in their cloud infrastructure.


### 34. Create an Auto Scaling Group (ASG)
[Tutorial: Create your first Auto Scaling group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-your-first-auto-scaling-group.html)

[EC2 start script](https://github.com/nealdct/aws-dva-code/blob/main/amazon-ec2/user-data-web-server.sh)

### 35. Amazon Elastic Load Balancing (ELB)
[Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/)
[What is an Application Load Balancer?](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)

Provides high availability and fault tolerance

Targets:
- ec2 instances
- ECS containers
- lambdas
- IP addresses
- other load balancers

Elastic Load Balancing supports the following load balancers ([source doc](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)): 
- Application Load Balancers (HTTP(S))
- Network Load Balancers (TCP, UDP, TLS, etc). Can have Static and Elastic IP in each AZ
- Gateway Load Balancers,
- Classic Load Balancers.

**Gateway Load Balancer**

• Used in front of virtual appliances such as firewalls, IDS/IPS, and deep packet inspection systems.
• Operates at Layer 3 – listens for all packets on all ports
• Forwards traffic to the TG specified in the listener rules
• Exchanges traffic with appliances using the GENEVE protocol on port 6081

Gateway Load Balancer use cases:

• Load balance virtual appliances such as:
• Intrusion detection systems (IDS)
• Intrusion prevention systems (IPS)
• Next generation firewalls (NGFW)
• Web application firewalls (WAF)
• Distributed denial of service protection systems (DDoS)
• Integrate with Auto Scaling groups for elasticity
• Apply network monitoring and logging for analytics


## 9 AWS IAM

- Identity-based policy and Resoure-base policy
- User, User Groups, Role, Policy
- Identity-based policy is applied to user, groups and roles
- Roles are used for delegation and are assumed
- [IAM ARNs](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-arns). User example: arn:aws:iam::account:user/UserName
- Users gain the **permissions** applied to the group through the **policy**
- Authentication: AWS IAM, CLI, API
- The root user has Full unrestricted permissions. IAM Users are restricted by IAM Policies (by default everything is restricted)

Both Groups and Roles requires applied Policeis

⚖️ Groups vs Roles

| Feature       | Group                       | Role                                      |
| ------------- | --------------------------- | ----------------------------------------- |
| Type          | IAM entity (for users)      | IAM entity (assumed by users/services)    |
| Use case      | Assign permissions to users | Temporary permissions for access          |
| Assumable by  | IAM Users                   | IAM Users, Services, Federated Identities |
| Persistency   | Persistent permissions      | Temporary permissions                     |
| Identity tied | Yes (to users)              | No (standalone, assumed as needed)        |


## Authentication
- Console Management (login\pass + MFA)
- CLI, API (access key ID and secreat access key)
- AWS Security Token Service (STS)

When an app try to access a resource, credentials include:
- AccessKeyId
- Expiration
- SecretAccessKey
- SessionToken

Temporaary credentials are used with:
- identity federation
- delegation
- cross-account access
- IAM roles

Access control methods:
- Role-Based Access Control (RBAC)
- Attribute-Based Access Control (ABAC)

## 14. Access control methods. RBAC & ABAC
**RBAC** 

AWS job function policies. Are designed to closely align to common job functions in the IT industy.

- Administrator
- Billing
- DB administrator
- Data scientist
- Developer power user
- Network administrator
- sercurity auditor
- support user
- Sys. administrator
- View-only user


**ABAC** 

Key\Value tag is assigned to resources.

Example:

DBadmins group is allowed to start, strop and reboot DBs in production.

We have a user that has key\value tag as Departemtn\DBAdmins.
Our resources (Amazon RDS for example) have tags Environment\Production and Environment\Development.
Then we create an ABAC policy:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowDBAdminProdActions",
      "Effect": "Allow",
      "Action": [
        "rds:StartDBInstance",
        "rds:StopDBInstance",
        "rds:RebootDBInstance"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Environment": "Production",
          "aws:PrincipalTag/Department": "DBadmin"
        }
      }
    }
  ]
}
```

## 15. Swithing IAM Roles
It might be useful if we need to do critical operations time to time and should have the permission all the time by default. 

[Methods to assume a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage-assume.html)

[Grant a user permissions to switch roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_permissions-to-switch.html)


# Section 5. Amazon S3 and Cloud Front

[General purpose bucket naming rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html)

## 45. Permissions and Bucket Policies
Examples can be found [here](https://github.com/nealdct/aws-dva-code/tree/main/amazon-s3/permissions-lesson)

## 46. Versioning, Replication and Lifecycle Rules

- Cross-Region Replication (CRR)
- Same-Region Replication (SRR)

Also replication can be:
- live replication
- batch replication

CRR and SRR can be done not only within a single account, but also between 2 different accounts.

**S3 Lifecycle Management. 2 types of actions:**
- Transition actions - Define when objects transition to anoter storage class
- Expiration actions - Define when objects expire (deleted by s3)

 ⚠️  The sliedes have a schema how files can and cannot migrate between the storage classes.

## 47. Configure Replication and LIfecycle

[S3 Replication](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html)
[S3 Replication. How to setup](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-how-setup.html)
[S3 Replication. Replicate Data within and between AWS Regions](https://aws.amazon.com/getting-started/hands-on/replicate-data-using-amazon-s3-replication/?ref=docs_gateway/amazons3/replication-example-walkthroughs.html)


- Source and destinatin buckets were created. 
- Policies -> Create -> Custom Trust Policy. Policy name: SRR-S3 (Same Region Replication policy) created. [See example](https://github.com/nealdct/aws-dva-code/blob/main/amazon-s3/s3-trust-policy.json)
- Create a role for the policy. It will be used by the replication rule (see next step) Choose SRR-S3 policy -> add permisstions -> add [this example](https://github.com/nealdct/aws-dva-code/blob/main/amazon-s3/s3-replication-permissions.json). Role name the same as the policy name "SRR-S3"
- Creating a replication rule (name: SRR-S3) for the source bucket. Open the bucket -> Management -> create replication rule.

  At this point replication should work.

We also can create a `Lifecycle rule` to move files between storage classes. To do that open the source bucket -> Management -> Create `Lifecycle rule` and investigate the available settings there.

 ⚠️  If versioning is enabled for a bucket - you can see all updated or deleted fieles there (just turn on the toggle)


 ## 48. MFA with S3
 Usefult for:
 - chenging version state for a bucket
 - permanently deleting an object bersion (MFA Delete)

MFA-Protected API Access

📝 For additional information see the Slides


## 49. S3 Encryption

[How to protect you data using encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryption.html)

 ⚠️ 
```
!!!!!!!!! Important
Amazon S3 now applies server-side encryption with Amazon S3 managed keys (SSE-S3) as the base level of encryption for every bucket in Amazon S3. Starting January 5, 2023, all new object uploads to Amazon S3 are automatically encrypted at no additional cost and with no impact on performance. The automatic encryption status for S3 bucket default encryption configuration and for new object uploads is available in AWS CloudTrail logs, S3 Inventory, S3 Storage Lens, the Amazon S3 console, and as an additional Amazon S3 API response header in the AWS Command Line Interface and AWS SDKs. For more information, see Default encryption FAQ.
```

- Server side encryption with S3 managed keys (SSE-S3 )
- Server-side encryption with AWS KMS managed keys (SSE-KMS)
- Server-Side encryption with client provided keys (SSE-C)
- Client-side encryption

The policies can be chose when we upload a file on S3 (from UI)

📝 For additional information see the Slides

## 50 Enforce Encryption with AWS KMS..

[Deny bucket policy example](https://github.com/nealdct/aws-dva-code/blob/main/amazon-s3/s3-enforce-kms-encryption.json).Force to use `aws:kms`. Deny if `s3:x-am2-server-side-encryption` field value in PUT s3 object request is not `aws:kms`



## 51. S3 Event Notifications

[Amazon S3 Event Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html)

⚠️ S3 Events designed to be delivered at least once

Destinations:
- SNS
- SQS
- Lambdas

[Granting permissions to publish event notification messages to a destination](https://docs.aws.amazon.com/AmazonS3/latest/userguide/grant-destinations-permissions-to-s3.html)


## 52. S3 Presigned URLs

[Sharing objects with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html)

By default, all Amazon S3 objects are private, only the object owner has permission to access them. However, the object owner may share objects with others by creating a presigned URL. A presigned URL uses security credentials to grant time-limited permission to download objects. The URL can be entered in a browser or used by a program to download the object.

CLI example
```
aws s3 presign s3://amzn-s3-demo-bucket/mydoc.txt --expires-in 604800
```


## 53. Server Access Logging

[Enabling Amazon S3 server access logging](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-server-access-logging.html)

- Only pay for the storage space used
- most configs store events in a separate bucket
- Must grant write permissins to the `Amazon S3 Log Delivery` group on the destionation bucket


## 54 CORS (Cross-Origin Resource Sharing)

`origin` - DNS name you are connectiong to using your client

[Using cross-origin resource sharing (CORS)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/cors.html)


## 55. S2 Performance

⚠️ S3 Events designed to be delivered at least once
Amazon S3 automatically scales to high request rates. For example, your application can achieve at least 

- 3,500 PUT/COPY/POST/DELETE
-  or 5,500 GET/HEAD requests per second per partitioned Amazon S3 prefix.

There are no limits to the number of prefixes in a bucket. You can increase your read or write performance by using parallelization. For example, if you create 10 prefixes in an Amazon S3 bucket to parallelize reads, you could scale your read performance to 55,000 read requests per second. Similarly, you can scale write operations by writing to multiple prefixes. See 
[Best practices design patterns: optimizing Amazon S3 performance](https://docs.aws.amazon.com/AmazonS3/latest/userguide/optimizing-performance.html)

🎯 Use `Amazon S3 Transfer ASccelerator` to increase uploads over long distances.
⚠️ Buckets used with Amazon S3 Transfer Acceleration can't have periods (.) in their names.

[Byte-range fetches](https://docs.aws.amazon.com/AmazonS3/latest/userguide/optimizing-performance-guidelines.html) use the Range HTTP header to transer only specified byte-range from an object


## CloudFront Origins and Destinations

[CloudFront use cases](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/IntroductionUseCases.html)
[CloudFront](https://docs.aws.amazon.com/AmazonS3/latest/userguide/website-hosting-cloudfront-walkthrough.html)

Edge Locations - locations where the data is cached.

[Use various origins with CloudFront distributions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html)

When we create a CloudFront Distribution can have multiple origins. An `origin` (s3) and `custom origin` (EC2 for example).


Using Lambda@Edge with CloudFront enables a variety of ways to customize the content that CloudFront delivers. To learn more about Lambda@Edge and how to create and deploy functions with CloudFront, see Customize at the edge with [Lambda@Edge](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-at-the-edge.html). To see a number of code samples that you can customize for your own solutions, see Lambda@Edge example functions.


## 57. CloudFront Signed URL and OAI/OAC

[Signed URLs](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-signed-urls.html)

A signed URL includes additional information, for example, an expiration date and time, that gives you more control over access to your content. This additional information appears in a policy statement, which is based on either a canned policy or a custom policy. The differences between canned and custom policies are explained in the next two sections.

⚠️ Signed URLs should be used for `individual files` and clients that don't support cooies


[CloudFront Signed Cookies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-signed-cookies.html)

- Similar to Signed URLs
-  Use singed cookies when you don't want to change URLs
-  can also be used when you want to provide access to `multiple resticred files`


[OAI Origin Access Identity](https://docs.aws.amazon.com/config/latest/developerguide/cloudfront-origin-access-identity-enabled.html). ⚠️ OAI is Deprecated. OAC is recommended
OAI works only for S3. When it's configured (Policy applied to an 'origin' bucket) then user can only call the origin through CloudFront, and cannot call the bucket directly

[OAC Origin Access Control](https://aws.amazon.com/blogs/networking-and-content-delivery/amazon-cloudfront-introduces-origin-access-control-oac/)
OAC is an improvement version of OAI with additional use cases.


## 58. CloudFront Cache and Behavior Settings

[Amazon CloudFront Cache and Behavior Settings](https://github.com/nealdct/aws-dva-code/blob/main/amazon-cloudfront/cloudfront-cache-and-behavior.md)

CloudShell -> us-east-1 -> 
```
aws s3 mb s3://art-statitc-website-12345
aws s3 mb s3://art-pdf-bucket-12345
aws s3 mb s3://art-jpg-bucket-12345
```

Apply bucket policy to `your_bucket_name`

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Principal": "*",
			"Effect": "Allow",
			"Action": [
				"s3:GetObject"
			],
			"Resource": "arn:aws:s3:::your_bucket_name/*"
		}
	]
}
```


- Create a CloudFront distribution. 
- Add origins for all 3 buckets (main, jpeg, pdf). Where `main` is public and `jpeg` and `pdf` use OAC.
- When jpeg/pdf distribution is created -> copy the generated bucket policy and apply it to the corresponding bucket.

Generated policy example:
The policy says that Principal: CloudFront is allowed to GetObjects from S3 when  source ARN is equal to the destibution
```
{
        "Version": "2008-10-17",
        "Id": "PolicyForCloudFrontPrivateContent",
        "Statement": [
            {
                "Sid": "AllowCloudFrontServicePrincipal",
                "Effect": "Allow",
                "Principal": {
                    "Service": "cloudfront.amazonaws.com"
                },
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::your_bucket_name_here/*",
                "Condition": {
                    "StringEquals": {
                      "AWS:SourceArn": "arn:aws:cloudfront::123456789:distribution/GENERATED_DISTRIBUTiON_CODE"
                    }
                }
            }
        ]
      }
```

- Distribution -> Behaviors -> Create (additional) behavior. Path pattern: /*.jpg and Bucket: my jpeg bucket
- The same for the pdf bucket

Followeing the created rules requests will be forwarded to the corresponding buckets using the path patterns
<img width="440" height="274" alt="image" src="https://github.com/user-attachments/assets/1688dd17-cfff-4081-9f15-a6309282d72c" />

To open the site go to the disctribution and cope its "domain name' (available on the distributions page
⚠️ When you finish -> Disable the distribution, waith a little bit and delete it!!!


## 59. Amazone Route 53 DNS
- Domain registration (.com , .org)
- Hosted zones (example.com , example2.com - it's a set of records that belong to the domain)
- Health Check for Ec2 Instances
- Traffic Flow

[Route 53 Developer Guide](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html)
📝 For additional information see the Slides



## 60 S3 Examp Q&A
⚠️⚠️⚠️ See the slides

[SQS Standard queus](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html) vs [FIFO queus](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-fifo-queues.html

### FIFO 

- Supports up to 300 msg/sec or up to 3000 for a batch and multiple Groups 
- De-duplication interval is 5 minutes
- Order is guarantee in scope of the same Group ID
- requires `Message Group ID` and `Message Deduplication ID`

### Dead Letter Queue
- Is just a standard or FIFO queue specified as a dead-letter queue
- A message is moved to the queue if ReceiveCount exceeds maxReceiveCount for the original queue. Require to tick the `Use Redrive Policy` checkbox
- It breadks order of messages in the original FIFO queue

### SQS Delay Queue
[SQS Delay Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-delay-queues.html)

Delivery delay default: 0 seconds and Max 15 minutes
Visibility Timeout - time before a message will be visible in the queue again if a consumer picks the message and does not approve it was processed successfuly.
Default visibility timeout 30 sec. Max: 12 hours
There is a case when messages can be duplicated if a consumer does not send an approval within visibility period.


### Long polling and short polling
Long polling - has lower costs. Because in AWS SQS we pay for requests. Thus if we connect to a queue and wait for a message - it's chipper then make requests often and get empty responses

`Receive Message Wait Time` parameter - from 0 to 20 seconds.

Short polling - returns immediately even if the queue is empty


## Section 10. Containers on Amazon ECS\EKS

## 133. Amazon Elastic Containers Service (ECS)



## 11. CI\CD Tools

[Manual Part1 -Part5](https://github.com/nealdct/aws-dva-code/blob/main/fargate-blue-green-ci-cd/fargate-ci-cd-instructions.md)

### 155. AWS CodeDeploy

[Deployment Strategies](https://docs.aws.amazon.com/whitepapers/latest/overview-deployment-options/deployment-strategies.html)

[buildspec.yaml example](https://github.com/nealdct/aws-dva-code/blob/main/aws-developer-tools/buildspec.yml)

**CI\CD Workflow**

Developer Pushes Code → CodeCommit

      ↓
      
CodePipeline Starts

      ↓
      
CodeBuild Compiles & Tests

      ↓
      
Artifacts Stored in S3

      ↓
      
CloudFormation Deploys Infra (optional)

      ↓
      
CodeDeploy Deploys App to EC2 / Lambda / ECS

      ↓
      
CloudWatch Monitors Logs and Metrics




🚀  Main AWS Services Used in CI/CD
1. AWS CodeCommit – Source Control
What it is: A fully managed Git-based repository service.

Role in CI/CD: Stores your application source code, configuration files, and scripts. Triggers CI/CD pipelines on code changes.

2. AWS CodeBuild – Build & Test
What it is: A fully managed build service that compiles source code, runs tests, and produces software packages.

Role in CI/CD: Automates the build process. Can also run unit tests, static code analysis, or security scans.

3. AWS CodePipeline – Orchestration
What it is: A fully managed CI/CD service to orchestrate the end-to-end workflow (source → build → test → deploy).

Role in CI/CD: Connects all stages of your pipeline. Automatically triggers steps when changes are detected (e.g., in CodeCommit or S3).

4. AWS CodeDeploy – Deployment Automation
What it is: A service that automates application deployment to Amazon EC2, Lambda, on-premises servers, or ECS.

Role in CI/CD: Manages versioned and safe deployments using strategies like blue/green, canary, or in-place deployments.

5. Amazon S3 – Artifact Storage
What it is: A highly durable object storage service.

Role in CI/CD: Stores build artifacts, configuration files, or deployment packages between pipeline stages.

6. AWS CloudFormation – Infrastructure as Code
What it is: A service for modeling and provisioning AWS infrastructure using YAML or JSON templates.

Role in CI/CD: Automates the provisioning of infrastructure as part of your CI/CD pipeline. Used to create/update resources like EC2, Lambda, or VPC.

7. Amazon EC2 / AWS Lambda / Amazon ECS / Amazon EKS – Deployment Targets
These are runtime environments where your application gets deployed.

EC2: Virtual servers in the cloud.

Lambda: Serverless compute.

ECS/EKS: Container orchestration platforms for Docker and Kubernetes workloads.

8. Amazon CloudWatch – Monitoring & Logging
What it is: Monitoring service for logs, metrics, and events.

Role in CI/CD: Tracks pipeline performance, application logs, error rates, and metrics after deployment.

9. AWS Secrets Manager / AWS Systems Manager Parameter Store – Secrets Management
Role in CI/CD: Securely store and retrieve credentials, tokens, and configuration values during builds or deployments.

10. Amazon EventBridge – Event Triggers
What it is: A serverless event bus.

Role in CI/CD: Triggers CI/CD workflows based on events, such as a new image pushed to Amazon ECR or a change in CodeCommit.

Optional: Other Integrations
AWS CodeStar: A unified UI to manage your CI/CD pipeline.

AWS AppConfig: For feature flag deployments.

Third-Party CI/CD Tools (e.g., GitHub Actions, Jenkins): Can integrate seamlessly with AWS services using webhooks, IAM roles, and APIs.


### 156. AWS Fargate Blue\Greed Pipeline (Part 4)

 [See Part4](https://github.com/nealdct/aws-dva-code/blob/main/fargate-blue-green-ci-cd/fargate-ci-cd-instructions.md)

### 156. AWS Fargate Blue\Greed Pipeline (Part 5)

 [See Part5 manual](https://github.com/nealdct/aws-dva-code/blob/main/fargate-blue-green-ci-cd/fargate-ci-cd-instructions.md)

```
aws ecr create-repository --repository-name nginx --region us-east-1
docker tag nginx:latest <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/nginx:latest
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/nginx
docker push <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/nginx:latest
```

## 158. AWS Apmlify and AppSync
[AWS Amplify](https://aws.amazon.com/amplify/) Fully managed service to build full-stack applications.

AWS Amplify is a comprehensive development platform from Amazon Web Services (AWS) designed to simplify the process of building and deploying web and mobile applications. It provides a set of tools and services that enable developers to create full-stack applications with ease, without requiring extensive cloud expertise

[AWS AppSync](https://aws.amazon.com/appsync/) AWS AppSync is a service that lets you create and manage GraphQL APIs for web and mobile apps. It offers features such as data caching, API federation, real-time events, custom domains...


