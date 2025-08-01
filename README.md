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

### 33. Amazon EC2 Auto Scaling

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
• Forwards traffic to the Target Group (TG) specified in the listener rules
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

### 36. Create an Application Load Balancer

- Create a Target group (dynamic auto scaling)
- Create an ApplicationLoad Balancer, Link Listener (default on port 80) to the Target group. After creating the ALB configuration is not completed(see next steps)
- Open the created Target Group (Targets tab is empty)
- Open the created ALB -> open "Integration tab" -> Load Balancing (Edit) -> Application, Network or Gateway Load Balancer target groups -> choose the created Target Group

Here is a list of objects that should be created for setting up a load-balanced environment for EC2 instances in AWS:

- VPC
- Subnets (at least two in different Availability Zones)
- Internet Gateway
- Route Tables
- Security Groups
- Network ACLs (optional)
- EC2 Launch Template or Launch Configuration
- Auto Scaling Group
- Target Group
- Application Load Balancer (or Network Load Balancer)
- Listener
- Listener Rules (if needed)
- IAM Role (for EC2 instance permissions)
- Elastic IPs (optional, typically for NLB)
- CloudWatch Alarms (for Auto Scaling policies)
- Auto Scaling Policies (optional, for dynamic scaling)
- Health Check Configuration


### 37. Create a Scaling Policy
[Step and simple scaling policies for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-simple-step.html)

- Open the created Auto Scaling Group
- Click "edit" on "[your ASG name] Capacity overview" (4 in our case)
- Set "max desired capasity"
- Click "edit" on "Network" and add more AZs (2 in our case)

Now we need to update the Load Balancer and add correspondigs sub-nets for the new AZs

- Open the Load Balancer
- Open "Network mapping", scroll to "Availability Zones and subnets" and click "edit". Add more subnets

Now we need to create a "Dynamic scaling policy" for the Auto Scaling Group
- open the ASG, open "Automatic scaling" tab and click "Create dynamic scaling policy"
- set "Metric type" to "Application Load Balancer request count per target" and set our Target Group as shown on the  screenshot below.

Check CloudWatch
- Open CloudWatch and check that alams were created (Alarms -> All Alarms)

<img width="776" height="522" alt="image" src="https://github.com/user-attachments/assets/71a845d1-57a5-4853-8e73-0755ded3c174" />


How to simulate workload. Run the command below from CloudShell
```

for i in {1..200}; do curl http://your-alb-address.com & done; wait
```


- After some time you should see an alarm in CloudWatch.
- And when new EC2 instances are created you can see those events in the Auto Scaling Group on "Activity" tab.
- And the new instances will be visible in the EC2 console.
- Calling the load balancer URL will show you pages with different AZs
<img width="930" height="175" alt="image" src="https://github.com/user-attachments/assets/b136beb0-7eea-489c-8c9a-89cf007901e6" />


After some time you should see an alarm in CloudWatch.
And when new EC2 instances are created you can see those events in the Auto Scaling Group on "Activity" tab.

⚠️ ⚠️ ⚠️ Do not foreget to remove the created resources:
- The Auto Scaling Group
- The Load Balancer
- The instances will be automatically terminated
- The target group doesn't cost us anything.


### 38. Create ASG and ALB using the AWS CLI

[Full Example](https://github.com/nealdct/aws-dva-code/blob/main/amazon-ec2/create-asg-alb-cli.md)
[create-auto-scaling-group docs](https://awscli.amazonaws.com/v2/documentation/api/2.1.29/reference/autoscaling/create-auto-scaling-group.html)

### 39. Exam Questions Overview
See the slides

⚠️ ⚠️ ⚠️ Do not foreget to remove the created resources

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


## Section 6. Infrastructure as Code and PaaS. IaC
**See the slides with detailed descriptions!!**

[CloudFormation template snippets (examples) ](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-snippets.html)


### 62. Infrastructure as Code with AWS CloudFromation

Components:
- [Templates](https://aws.amazon.com/cloudformation/resources/templates/)
- [Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudformation-stack.html) - It's a deployed environment
- [StackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudformation-stackset.html) - extends Stack and deploy an environment across multiple regions and accounts within a single operation
- Change Sets -Updates a stack. Also provides a summary of proposed changes. Useful to see what will be changed

### 63. Creating and updating Stacks
[CloudFormation template example. EC2 instance with SSH rule]([https://github.com/nealdct/aws-dva-code/tree/main/aws-cloudformation](https://github.com/nealdct/aws-dva-code/blob/main/aws-cloudformation/3-ec2-template.yml))

[CloudFormation template Mappings syntax](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html) The optional Mappings section helps you create key-value pairs that can be used to specify values based on certain conditions or dependencies. One common use case for the Mappings section is to set values based on the AWS Region where the stack is deployed. This can be achieved by using the AWS::Region pseudo parameter. The AWS::Region pseudo parameter is a value that CloudFormation resolves to the region where the stack is created. Pseudo parameters are resolved by CloudFormation when you create the stack.

### 64. Creating nested Stacks using the AWS CLI
[CloudFormation nested stack example. Manual](https://github.com/nealdct/aws-dva-code/blob/main/aws-cloudformation/Create%20Nested%20Stack%20using%20the%20AWS%20CLI.md)

### 65. CloudFormation template Deep Dive
[Intristic function](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/intrinsic-function-reference.html)
[Condition functions](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/intrinsic-function-reference-conditions.html)

### 66. Complex VPC Stack
[Example](https://github.com/nealdct/aws-dva-code/blob/main/aws-cloudformation/create-vpc-with-cloudformation.yaml)

### 68. Advanced configuration and SSL/TLS
[Elastic Beanstalk. Advanced environment customization with configuration files (.ebextensions)](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html).

[QuickStart: Deploy a Java application to Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/java-quickstart.html)

[AWS Elastic Beanstalk security](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/security.html)

[Configuring a secure listener using a configuration file](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/configuring-https-elb.html#configuring-https-elb.configurationfile)

⚠️ **See the slides for more detailed security information!!**

[General options](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options-general.html) see SSLCertificateArns


[Beanstalk deployment strategies](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html)

**Supported deployment policies:**

| Method                          | Impact of failed deployment                                           | Deploy time                       | Zero downtime | No DNS change | Rollback process                    | Code deployed to          |
|--------------------------------|------------------------------------------------------------------------|------------------------------------|----------------|----------------|-------------------------------------|----------------------------|
| All at once                    | Downtime                                                               | 🕒                                 | No             | Yes            | Manual redeploy                     | Existing instances         |
| Rolling                        | Single batch out of service; successful batches running new version   | 🕒🕒†                              | Yes            | Yes            | Manual redeploy                     | Existing instances         |
| Rolling with an additional batch | Minimal if first batch fails; otherwise, similar to Rolling           | 🕒🕒🕒†                            | Yes            | Yes            | Manual redeploy                     | New and existing instances |
| Immutable                      | Minimal                                                                | 🕒🕒🕒🕒                          | Yes            | Yes            | Terminate new instances             | New instances              |
| Traffic splitting              | Percentage of traffic temporarily impacted                            | 🕒🕒🕒🕒††                        | Yes            | Yes            | Reroute traffic and terminate       | New instances              |
| Blue/green                     | Minimal                                                                | 🕒🕒🕒🕒                          | Yes            | No             | Swap URL                            | New instances              |


## Section 7. AWS Lambda and SAM (AWS Serverless Application Model)
[AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/)

[AWS Lambda Quotas. Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)

[Lambda sample applications (including java)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-samples.html)

[Official guide. aws-lambda-developer-guide with examples. Github](https://github.com/awsdocs/aws-lambda-developer-guide/tree/main)


### 72. Serverless Services and Event-Driven Architecture

[Git. Lambda examples](https://github.com/nealdct/aws-dva-code/tree/main/aws-lambda)
[Git. Trigger lambda from the CLI](https://github.com/nealdct/aws-dva-code/blob/main/aws-lambda/invoking-functions.md)

[Serverless Computing - AWS Lambda](https://aws.amazon.com/pm/lambda/#Learn-more-about-building-with-AWS-Lambda)

[Introduction to AWS Lambda & Serverless Applications](https://youtu.be/EBSdyoO3goc)

[CloudFormation lambda template](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-lambda.html)

[Lambda quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)

[Understanding Lambda function scaling](https://docs.aws.amazon.com/lambda/latest/dg/lambda-concurrency.html)


After you deploy your Lambda function, you can invoke it in several ways [Understanding Lambda function invocation methods](https://docs.aws.amazon.com/lambda/latest/dg/lambda-invocation.html):
- The Lambda console – Use the Lambda console to quickly create a test event to invoke your function.
- The AWS SDK – Use the AWS SDK to programmatically invoke your function.
- The Invoke API – Use the Lambda Invoke API to directly invoke your function.
- The AWS Command Line Interface (AWS CLI) – Use the aws lambda invoke AWS CLI command to directly invoke your function from the command line.
- A function URL HTTP(S) endpoint – Use function URLs to create a dedicated HTTP(S) endpoint that you can use to invoke your function.
- or [AWS Toolkit](https://aws.amazon.com/intellij/) (in this case it's for Intellij Idea)


[Create your first Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html). Also aws services provide documentation for its "aws lambda blueprints (examples)"


### 76. Create Event Source Mapping
[Git example. Lambad & SQS. Event Source Mapping](https://github.com/nealdct/aws-dva-code/blob/main/aws-lambda/event-source-mapping.md)

### 77. Lambda Versions and Aliases
[Versioning](https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html) - you can have multiple versions for a lambda function

[Create an alias for Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html)

- Every lambda version has its own ARN
- if we do not set a version (suffix) for a lambda it implicitly uses `$LATEST`
- An alias can be created for a lambda with a version. For $LATEST an alias cannot be created
- An alias can point to two versions at the same time and distribute traffic (for example 20%/80%)
- Aliases implement blue\green deployment using wighted traffic distribution (see [Implement Lambda canary deployments using a weighted alias](https://docs.aws.amazon.com/lambda/latest/dg/configuring-alias-routing.html))

### 78. Using Versions and Aliases
[Implement Lambda canary deployments using a weighted alias](https://docs.aws.amazon.com/lambda/latest/dg/configuring-alias-routing.html))

### 79. Deployment Packages and and Environment Variables
Lamda supports 2 types of deployment packages:

- [.zip file archives](https://docs.aws.amazon.com/lambda/latest/dg/configuration-function-zip.html) (limit 50 MB for zipped file and 250 MB for unzipped, 3 MB for console aditor)
- [Container Images](https://docs.aws.amazon.com/lambda/latest/dg/images-create.html)


To keep your deployment package size small, package your function's `dependencies in layers`. Layers enable you to manage your dependencies independently, can be used by multiple functions, and can be shared with other accounts. For more information, see [Managing Lambda dependencies with layers](https://docs.aws.amazon.com/lambda/latest/dg/chapter-layers.html). See also: [Deploy Java Lambda functions with .zip or JAR file archives](https://docs.aws.amazon.com/lambda/latest/dg/java-package.html)


A Lambda layer is a .zip file archive that contains supplementary code or data. Layers usually contain library dependencies, a custom runtime, or configuration files.

There are multiple reasons why you might consider using layers:
- To reduce the size of your deployment packages. Instead of including all of your function dependencies along with your function code in your deployment package, put them in a layer. This keeps deployment packages small and organized.
- To separate core function logic from dependencies. With layers, you can update your function dependencies independent of your function code, and vice versa. This promotes separation of concerns and helps you focus on your function logic.
- To share dependencies across multiple functions. After you create a layer, you can apply it to any number of functions in your account. Without layers, you need to include the same dependencies in each individual deployment package.
- To use the Lambda console code editor. The code editor is a useful tool for testing minor function code updates quickly. However, you can’t use the editor if your deployment package size is too large. Using layers reduces your package size and can unlock usage of the code editor.
- To lock an embedded SDK version.The embedded SDKs may change without notice as AWS releases new services and features. You can lock a version of the SDK by creating a Lambda layer with the specific version needed. The function then always uses the version in the layer, even if the version embedded in the service changes.



**Lambda and CloudFormation**

- zip-file should be stored in an S3 bucket.
- The bucket should be at the same region where CloudFormation is run
- CloudFormation template file should set `package type` to `Image` or `Zip`
- A Lambda can have up to 5 layers
- Layers are extracted to the /opt directory. Different languages look for depedencies in different locations under /opt


**AWS CLI lambda update-function-configuration and get-function-configuration**

[aws lambda update-function-configuration](https://docs.aws.amazon.com/cli/latest/reference/lambda/update-function-configuration.html)

[aws lambda get-function-configuration](https://docs.aws.amazon.com/cli/latest/reference/lambda/get-function-configuration.html)

[Working with lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html)


### 80. Using Environment Variables
[Git. Lambda environment variables example](https://github.com/nealdct/aws-dva-code/blob/main/aws-lambda/lambda-environ-test.md)

[Encrypting environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars-encryption.html) Passwords, for example.


### 81. Destinations and Dead-Letter Queues
Destinations and settings for a Deal-Letter Queue are 2 different things that are configured in different places

- [AWS Lambda Destinations](https://aws.amazon.com/blogs/compute/introducing-aws-lambda-destinations/)
- [Adding a dead-letter queue](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async-retain-records.html#invocation-dlq)
- [How Lambda handles errors and retries with asynchronous invocation](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async-error-handling.html)

<img width="838" height="472" alt="image" src="https://github.com/user-attachments/assets/cacac1f9-79d8-4215-acb0-d8ad49d17a05" />


Dead-Letter Queue (DLQ):

- Is applied to asynchronous invocations
- The DLQ can be SNS topic or SQS queue
- We can specify numbers of re-tryes and amount of time before it's sent to a DQL


### 83. Reserved and Provisioned Concurrency
[Configuring Reserved Concurrency For A Function](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html)

In Lambda, concurrency is the number of in-flight requests that your function is currently handling. There are two types of concurrency controls available:

- Reserved concurrency – This sets both the maximum and minimum number of concurrent instances allocated to your function. When a function has reserved concurrency, no other function can use that concurrency. Reserved concurrency is useful for ensuring that your most critical functions always have enough concurrency to handle incoming requests. Additionally, reserved concurrency can be used for limiting concurrency to prevent overwhelming downstream resources, like database connections. Reserved concurrency acts as both a lower and upper bound - it reserves the specified capacity exclusively for your function while also preventing it from scaling beyond that limit. Configuring reserved concurrency for a function incurs no additional charges.

- Provisioned concurrency – This is the number of pre-initialized execution environments allocated to your function. These execution environments are ready to respond immediately to incoming function requests. Provisioned concurrency is useful for reducing cold start latencies for functions and designed to make functions available with double-digit millisecond response times. Generally, interactive workloads benefit the most from the feature. Those are applications with users initiating requests, such as web and mobile applications, and are the most sensitive to latency. Asynchronous workloads, such as data processing pipelines, are often less latency sensitive and so do not usually need provisioned concurrency. Configuring provisioned concurrency incurs additional charges to your AWS account.

[CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)

<img width="1151" height="364" alt="image" src="https://github.com/user-attachments/assets/b8e90682-90d3-4742-90ba-05e1b8c972d2" />


[Working with Lambda function logs](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-logs.html) CloudWatch Logs, S3, Firehouse.

[Visualize Lambda function invocations using AWS X-Ray](https://docs.aws.amazon.com/lambda/latest/dg/services-xray.html)

Burst concurrency quotas depend on a region. Different regions support different quotas.

If the concurrency limit is exceeded the client will get  HTTP 429 `TooManyRequestsException`

[Service Quotas dashboard](https://console.aws.amazon.com/servicequotas/home) Requires to be logged in.

- The default concurrency limit is 1000 invocations per/sec
- The Default burst concurrency quota per Region is between 500 to 3000
- There is not limit of concurrency. You need to ask AWS (at least 2 weeks ahead of time) to increase the quote.
   

### 85. Lambda in a VPC	 and ALB Targets

[Enable internet access for VPC-connected Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc-internet.html)

[Process Application Load Balancer (ALB) requests with Lambda](https://docs.aws.amazon.com/lambda/latest/dg/services-alb.html)


You can register your [Lambda functions as targets of an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/lambda-functions.html) and configure a listener rule to forward requests to the target group for your Lambda function. When the load balancer forwards the request to a target group with a Lambda function as a target, it invokes your Lambda function and passes the content of the request to the Lambda function, in JSON format.

Limits: 
- The Lambda function and target group must be in the same account and in the same Region.
- The maximum size of the request body that you can send to a Lambda function is 1 MB. For related size limits, see HTTP header limits.
- The maximum size of the response JSON that the Lambda function can send is 1 MB.
- WebSockets are not supported. Upgrade requests are rejected with an HTTP 400 code.
- Local Zones are not supported.
- Automatic Target Weights (ATW) is not supported.


### 86. Security of Lambada Functions

[Lambda code signing with AWS Signer](https://docs.aws.amazon.com/lambda/latest/dg/governance-code-signing.html)  
AWS SIgner is a fully managed code-signing service that allows you to validate your code against a digital signature to confirm that code is unaltered and from a trusted publisher. AWS Signer can be used in conjunction with AWS Lambda to verify that functions and layers are unaltered prior to deployment into your AWS environments. This protects your organization from malicious actors who might have gained credentials to create new or update existing functions.


### 87. AWS Lambda best practices
### 88. AWS Serverless Application Model (SAM)

AWS SAM templates are an extension of CloudFormation Templates

[What is the AWS Serverless Application Model (AWS SAM)?](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html). AWS Serverless Application Model (AWS SAM) is an open-source framework for building serverless applications using infrastructure as code (IaC). With AWS SAM’s shorthand syntax, developers declare AWS CloudFormation resources and specialized serverless resources that are transformed to infrastructure during deployment. This framework includes two main components: the AWS SAM CLI and the AWS SAM project. The AWS SAM project is the application project directory that is created when you run sam init. The AWS SAM project includes files like the AWS SAM template, which includes the template specification (the shorthand syntax you use to declare resources).

[Using AWS SAM with layers](https://docs.aws.amazon.com/lambda/latest/dg/layers-sam.html)
[Tutorial: Deploy a Hello World application with AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html) and [Git example](https://github.com/nealdct/aws-dva-code/blob/main/aws-lambda/sam-cli-commands.md)

```
sam deploy --template-file /var/folders/45/5ct135bx3fn2551_ptl5g6_80000gr/T/tmpq3x9vh63 --stack-name <YOUR STACK NAME>
```


### 89. Run SAM App using CloudShell
[Tutorial: Deploy a Hello World application with AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html) and [Git example](https://github.com/nealdct/aws-dva-code/blob/main/aws-lambda/sam-cli-commands.md)


### 90. The Serverless Application Repository

[What Is the AWS Serverless Application Repository?](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/what-is-serverlessrepo.html)

The AWS Serverless Application Repository makes it easy for developers and enterprises to quickly find, deploy, and publish serverless applications in the AWS Cloud.
You can easily publish applications, sharing them publicly with the community at large, or privately within your team or across your organization. To publish a serverless application (or app), you can use the AWS Management Console, the AWS SAM command line interface (AWS SAM CLI), or AWS SDKs to upload your code. Along with your code, you upload a simple manifest file, also known as an AWS Serverless Application Model (AWS SAM) template. For more information about AWS SAM, see the AWS Serverless Application Model Developer Guide.


### 91. Lambda Exam 
Summary or this chapter is available in the slides on Google Drive.


## Section 8. Amazon DynamoDB

[What is DynamoDb?](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html). See what fetures are provided by the DB.

- NoSQL DB
- Fully-mnaged scalable DB with milliseconds latency
- Key\value and document DB. Supports:
   1. Scalars (string, number, boolean, etc)
   2. Document Types (Json, List, Map)
   3.  Set Types.
- Transactions. eventually or strong consystency. Supports ACID transactions for strong consystency
- DynamoDB Streams. Stores item-based modification up to 24 hours. Usually used by with Lambdas and Kinesis Client Library (KCL)
- DAX DynamoDB Axelerator - DB cache. Supports latency for microseconds
- DB Backup. Point-in-time recovery down to the second in last 35 days
- GlobalTables (multi-regions support)


### 93. DynamoDB API
[DynamoDB API](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.API.html). For a full list of the API operations, see the Amazon DynamoDB API Reference.

- Control plane
- Data plane
- DynamoDB Streams
- Transactions

You can use PartiQL - a SQL-compatible query language for Amazon DynamoDB, to perform these CRUD operations or you can use DynamoDB’s classic CRUD APIs that separates each operation into a distinct API call.

PartiQL - A SQL-compatible query language:
- `ExecuteStatement` – Reads multiple items from a table. You can also write or update a single item from a table. When writing or updating a single item, you must specify the primary key attributes.
- `BatchExecuteStatement` – Writes, updates or reads multiple items from a table. ⚠️ This is more efficient than ExecuteStatement because your application only needs a single network round trip to write or read the items.


**[DynamoDB Table Classes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.TableClasses.html):**
DynamoDB offers two table classes designed to help you optimize for cost. 
- The DynamoDB Standard table class is the default, and is recommended for the vast majority of workloads.
- The DynamoDB Standard-Infrequent Access (DynamoDB Standard-IA) table class is optimized for tables where storage is the dominant cost. For example tables that store infrequently accessed data, such as:
  1. application logs
  2.  old social media posts
  3.   e-commerce order history, and past gaming achievements

 are good candidates for the Standard-IA table class. See Amazon DynamoDB Pricing for pricing details.


DynamoDB sypports IAM roles and [resource-based policies](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/access-control-resource-based.html). Policies can be applied to:
- table
- index
- stream

Max policy size per resource is 20 KB.


### DynamoDB Partitions and Primary Keys
There are 2 types of Primary Key:
- partition keys - unique keys per table
- composite keys - partitin key + sort key. 2 items may have the same partition key but they must have a different Sort key

[Partitions and data distribution in DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.Partitions.html)


[DynamoDB throughput capacity](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/capacity-mode.html).
DynamoDB evenly distributes provisioned throughput among partitions:
- `read capacity units` (RCUs) . Limit 3000
-`wrtie capacity units` (WCUs) . Limit 1000

⚠️ All itmes with the same Partition Key are stored togeter. A Primary Key should be choosen wisely. If you choose a wrong partition key you might exceed a partition limit read\write because:
- hot key. Frequent access to the same key
- uneven distributed data
- request rate greater then a provisioned throughput

Best Practices:
- use high cardinality primary key (user email, order id, session id, ...)
- use composite attibutes to get a unique key: for example, customer id + product id + country code and order date as a Sort key.
- use DAX
- for write-heavy use cases use random numbers from a predetermined range (to distribute items evenly) and use it as a prefix. Example: Invoice123+343253463 whre 343253463 is a random number

### 95. Practice Creating DynamoDB Tables



### 96. DynamoDB Consystency Models and Transactions

- Supports [strong consystency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html) and eventually consystancy reads
- For strong consystency DB may return HTTP 500 in case of network outage or latency
- strong consystency takes more throughput then eventually consystency
- strong consystency has higher latency


[Amazon DynamoDB Transactions: How it works](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transaction-apis.html)

- Transactions provide all-or-nothing chages to multiple items both within and across tables 
- There are 2 reads or 2 writes when we use transactions. 1 for prepear a transaction and the 2nd one to commit the transaction.


### 97. DynamoDB Capacity Units (RCU/WCU)
RCU

<img width="615" height="388" alt="image" src="https://github.com/user-attachments/assets/43c00918-bcbd-415e-a821-d03ec03f0039" />

<img width="677" height="335" alt="image" src="https://github.com/user-attachments/assets/5ccc4497-a946-493b-b78b-89c2228a4274" />

WCU

<img width="629" height="341" alt="image" src="https://github.com/user-attachments/assets/5d719c6e-019c-4d75-ae54-b7d640731364" />
<img width="682" height="324" alt="image" src="https://github.com/user-attachments/assets/aac8b2d8-9097-4aaf-a42e-fc801203ab11" />



### 98. DynamoDB Performance and Throttling

- You might get `ProvisionedThroughputExceededException` if read/write request rate is too high. See: [Troubleshooting throttling issues for provisioned mode](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TroubleshootingThrottling-common-issues.html)
- AWS SDKs for DynamoDB automatically retry requests that recieve  `ProvisionedThroughputExceededException`

⚠️ For additinal information see the slides.


### 99. DynamoDB Scans and Query API
[Scan API](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Scan.html)

[Query API](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Query.html)

[Git. Scan and Query examples](https://github.com/nealdct/aws-dva-code/blob/main/amazon-dynamodb/DynamoDB%20CLI%20Commands.sh)


Compare Scan and Query API
| Feature          | **Scan**                         | **Query**                      |
| ---------------- | -------------------------------- | ------------------------------ |
| Reads All Items? | Yes                              | No – only items with given key |
| Requires Key?    | No                               | Yes – partition key required   |
| Performance      | Poor on large tables             | High performance               |
| Cost             | Higher (reads everything)        | Lower (targeted reads)         |
| Use Case         | Full-table analysis or dev tools | Production lookups & filtering |


### 100. DynamoDB LSI and GSI

Local Secondary Index (LSI):
- provides an alternative sort key to use for scans and queries
- Up to 5 LSIs per table
- Must be created at table creation time
- cannot be added/midified/removed later
- has the same partition key as the original table (different sort key)

Global Secondary Index (GSI):
- Used to speed up queries on non-key attributes
- can be created at any time
- can use a different partition key and a sort key
- provides completle different view of data


### 104. DynamoDB Optimistic Locking and Conditional Updates
[Optimistic locking](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBMapper.OptimisticLocking.html)

DynamoDB supports [TTL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html)


### 106. DynamoDB Streams
[DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/streamsmain.html)


| Properties                    | Kinesis Data Streams for DynamoDB                                                                 | DynamoDB Streams                                                                                   |
|------------------------------|----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| **Data retention**           | Up to 1 year.                                                                                      | 24 hours.                                                                                           |
| **Kinesis Client Library (KCL) support** | Supports KCL versions 1.X, 2.X, and 3.X.                                                        | Supports KCL versions 1.X and 2.X.                                                                  |
| **Number of consumers**      | Up to 5 simultaneous consumers per shard, or up to 20 with enhanced fan-out.                      | Up to 2 simultaneous consumers per shard.                                                           |
| **Throughput quotas**        | Unlimited.                                                                                        | Subject to throughput quotas by DynamoDB table and AWS Region.                                     |
| **Record delivery model**    | Pull model over HTTP using `GetRecords`, and push with enhanced fan-out using `SubscribeToShard`. | Pull model over HTTP using `GetRecords`.                                                           |
| **Ordering of records**      | Timestamp attribute can be used to identify the actual order of changes.                          | Stream records appear in the same sequence as modifications to the item.                           |
| **Duplicate records**        | Duplicate records might occasionally appear.                                                      | No duplicate records appear.                                                                       |
| **Stream processing options**| AWS Lambda, Amazon Managed Service for Apache Flink, Kinesis Data Firehose, AWS Glue streaming ETL.| AWS Lambda or DynamoDB Streams Kinesis adapter.                                                    |
| **Durability level**         | Availability zones to provide automatic failover without interruption.                            | Availability zones to provide automatic failover without interruption.                             |

[Working with DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html)

You can also use the CreateTable or UpdateTable API operations to enable or modify a stream. The StreamSpecification parameter determines how the stream is configured:

- StreamEnabled — Specifies whether a stream is enabled (true) or disabled (false) for the table.
- StreamViewType — Specifies the information that will be written to the stream whenever data in the table is modified:
- KEYS_ONLY — Only the key attributes of the modified item.
- NEW_IMAGE — The entire item, as it appears after it was modified.
- OLD_IMAGE — The entire item, as it appeared before it was modified.
- NEW_AND_OLD_IMAGES — Both the new and the old images of the item.


### 107. DynamoDB Accelerator (DAX)
[DAX](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.html)
- It's a DynamoDB cache.
- Improves reads throughput
- from miliseconds to microseconds

### 109. Enabling Global Table
[How DynamoDB global tables work](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/V2globaltables_HowItWorks.html)

There are 2 table types:
- tables (replicated within a region)
- global tables (master-master table replicated between multiple regions)

Global tables support:
- multi-region strong consistancy
- multi-region eventual consistancy


### 110. DynamoDB Examp Questions

See the slides!!!!



## Section 9. Application Integration and APIs

### 112. Amazon Simple Queue Service (SQS)

[What is Amazon Simple Queue Service?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)

[Using JMS with Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-java-message-service-jms-client.html)

### 113. Working with SQS Queries
[Git SQS CLI queries examples](https://github.com/nealdct/aws-dva-code/blob/main/amazon-sqs/aws-sqs-cli-commands.md)

### 114. Amazon SQS API and Client Library
[Use ChangeMessageVisibility with an AWS SDK or CLI](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/example_sqs_ChangeMessageVisibility_section.html)



- ChangeMessageVisibility: Default 30 sec. Min: 0 sec. Max: 12 hours!
- GetQueueAttributes
- SetQueueAttributes
- [All other Actions for Amazon SQS using AWS SDKs](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/service_code_examples_actions.html)

[Configuring visibility timeouts in Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/working-with-visibility-timeouts.html)

**ReceiveMessage**

[Receive Message and VisibilityTimeout](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html)

- ReceiveMessage polls up to 10 messages at time
- `WaitTimeSeconds` - enables long-poll support [Amazon SQS short and long polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-short-and-long-polling.html#sqs-long-polling)

**SendMessage**

[Send Message](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/example_sqs_SendMessage_section.html)

[SendMessage API](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html) with request parameters: 
- DelaySeconds
- MessageDeduplicationId
- MessageBody. The message to send. The minimum size is one character. The maximum size is 256 KiB.
- and other ...


⚠️ SQS Extended Library for Java supports:
- specify if a message always should be stored in an S3 bucket or only messages that exceed 256 KiB
- send a message that references a single message object stored in an S3 bucket
- get a message stored in an S3 bucket
- delete a message stored in an S3 bucket once it's processed


[SQS delay queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-delay-queues.html)

Delay queues let you postpone the delivery of new messages to consumers for a number of seconds, for example, when your consumer application needs additional time to process messages. If you create a delay queue, any messages that you send to the queue remain invisible to consumers for the duration of the delay period. The default (minimum) delay for a queue is 0 seconds. The maximum is 15 minutes. For information about configuring delay queues using the console see Configuring queue parameters using the Amazon SQS console.


### 115. Amazon Simple Notifcication Sevice (SNS)
Amazon Simple Notification Service (Amazon SNS) is a fully managed service that provides message delivery from publishers (producers) to subscribers (consumers). Publishers communicate asynchronously with subscribers by sending messages to a topic, which is a logical access point and communication channel.


[Amazon Simple Notification Service Documentation](https://docs.aws.amazon.com/sns/)

[SNS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/index.html)

In SNS, publishers send messages to a topic, which acts as a communication channel. The topic acts as a logical access point, ensuring messages are delivered to multiple subscribers across different platforms.

Subscribers to an SNS topic can receive messages through different endpoints, depending on their use case, such as:

- Amazon SQS
- Lambda
- HTTP(S) endpoints
- Email
- Mobile push notifications
- Mobile text messages (SMS)
- Amazon Data Firehose
- Service providers (For example, Datadog, MongoDB, Splunk)

SNS supports both Application-to-Application (A2A) and Application-to-Person (A2P) messaging, giving flexibility to send messages between different applications or directly to mobile phones, email addresses, and more.


[Amazon SNS API Reference](https://docs.aws.amazon.com/sns/latest/api/welcome.html)

Amazon SNS is a web service that enables you to build distributed web-enabled applications. Applications can use Amazon SNS to easily push real-time notification messages to interested subscribers over multiple delivery protocols. For more information about this product see the Amazon SNS product page. For detailed information about Amazon SNS features and their associated API calls, see the Amazon SNS Developer Guide.

For information on the permissions you need to use this API, see Identity and access management in Amazon SNS in the Amazon SNS Developer Guide.

We also provide SDKs that enable you to access Amazon SNS from your preferred programming language. The SDKs contain functionality that automatically takes care of tasks such as: cryptographically signing your service requests, retrying requests, and handling error responses. For a list of available SDKs, go to Tools for Amazon Web Services.


### 116. Simple Serverless Application
Example how to setup SNS+SQS+Lambda

### 117. Step Functions
[AWS Step Functions Documentation](https://docs.aws.amazon.com/step-functions/)

AWS Step Functions makes it easy to coordinate the components of distributed applications as a series of steps in a visual workflow. You can quickly build and run state machines to execute the steps of your application in a reliable and scalable fashion.


[Amazon States Language](https://states-language.net/spec.html)

This document describes a JSON-based language used to describe state machines declaratively. The state machines thus defined may be executed by software. In this document, the software is referred to as "the interpreter".

Step Functions has ready to use State Machine template that can be choosen.


### 119. Amazon EventBridge

## Amazon EventBridge

[Amazon EventBridge](https://aws.amazon.com/eventbridge/) is a serverless event bus service that makes it easy to connect applications using data from your own services, integrated SaaS applications, and AWS services. It enables you to build event-driven architectures by routing events based on patterns, transforming event data, and delivering events to targets like AWS Lambda, Step Functions, or Kinesis.

- 📘 [Product page](https://aws.amazon.com/eventbridge/)
- 📚 [Developer guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)
- 📗 [API reference](https://docs.aws.amazon.com/eventbridge/latest/APIReference/)

### 🔄 Sources and Target services:

- **Event Sources**: AWS services (e.g., EC2, S3, Lambda), integrated SaaS applications (e.g., Zendesk, Auth0), and custom applications that publish events to a custom event bus.
- **Event Targets**: AWS services like [AWS Lambda](https://aws.amazon.com/lambda/), [Step Functions](https://aws.amazon.com/step-functions/), [Amazon SNS](https://aws.amazon.com/sns/), [Amazon SQS](https://aws.amazon.com/sqs/), [Kinesis Data Streams](https://aws.amazon.com/kinesis/data-streams/), and more.

You can define routing rules to filter, transform, and deliver events in near real time to multiple targets.

EventBridge includes [two ways to process and deliver events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html): event buses and pipes:
- Event buses are routers that receive events and delivers them to zero or more targets. 
- Pipes EventBridge Pipes is intended for point-to-point integrations; 
- In addition, EventBridge provides EventBridge Scheduler, a serverless scheduler that allows you to create, run, and manage tasks from one central, managed service.



### 120. Create EventBus and Rule
[Amazon EventBridge Documentation](https://docs.aws.amazon.com/eventbridge/)
Amazon EventBridge is a serverless event bus service that makes it easy to connect your applications with data from a variety of sources. EventBridge delivers a stream of real-time data from your own applications, software-as-a-service (SaaS) applications, and AWS services and routes that data to targets such as AWS Lambda. **You can set up routing rules to determine where to send your data to build application architectures that react in real time to all of your data sources**. EventBridge enables you to build event-driven architectures that are loosely coupled and distributed.

Chances are you won't want to process every single event that gets delivered to a given event bus or pipe. Rather, you'll likely want to select a subset of all the events delivered, based on the source of the event, the event type, and/or attributes of those events.

To specify which events to send to a target, you create an [event pattern](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html). An event pattern defines the data EventBridge uses to determine whether to send the event to the target. If the event pattern matches the event, EventBridge sends the event to the target. Event patterns have the same structure as the events they match. An event pattern either matches an event or it doesn't.

### 121. Amazon API Gateway
[Amazon API Gateway](https://aws.amazon.com/api-gateway/)

[Amazon API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)

[What is Amazon Gateway API? Architecture and features of API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)


- REST API
- HTTP API
- WebSockets API

[Choose between REST APIs and HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html)


Supports import:
- OpenAPI
- Swagger

<img width="1012" height="575" alt="image" src="https://github.com/user-attachments/assets/f7bbadc8-3b40-4dad-809a-786872cdfc09" />


[API endpoint types for REST APIs in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-endpoint-types.html)

- Edge-optimized API endpoints
- Regional API endpoints
- Private API endpoints


### 122. Methods, Integrations, and Mapping Templates

## Amazon API Gateway – REST API Integrations

Amazon API Gateway REST APIs support several integration types that determine how your API methods interact with backend endpoints. You can choose the integration that best suits your use case, from simple HTTP backends to AWS services and mock responses.

### 🔗 Supported Integration Types
[Choose an API Gateway API integration type](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-integration-types.html)

- **HTTP** - Forward requests to any HTTP/HTTPS endpoint.
- **HTTP_PROXY** - Pass-through proxy to an HTTP/HTTPS backend with minimal transformation.
- **AWS** – Invoke AWS service actions (e.g., DynamoDB, S3, Lambda) using service-specific integration.
- **AWS_PROXY** – Pass-through to AWS Lambda using Lambda proxy integration.
- **MOCK** – Return a static response configured in API Gateway without calling a backend.

Each method in your REST API can use a different integration type, allowing flexibility in how requests are processed and routed.

- 📘 [Learn more – API Gateway REST API Integration Types](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-integration-types.html)

<img width="1028" height="517" alt="image" src="https://github.com/user-attachments/assets/26d5d7fc-57d7-4795-8db6-8a96f4110351" />


[Mapping template transformations for REST APIs in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/models-mappings.html)

A mapping template transformation uses a mapping template to modify your integration request or integration response. A mapping template is a script expressed in [Velocity Template Language (VTL)](https://velocity.apache.org/engine/devel/vtl-reference.html) and applied to a payload using JSONPath  based on the Content-type header. You use mapping templates when you use mapping template transformations. This section describes conceptual information related to mapping templates.


### 123. API Gateway Stages and Deployments

In Amazon API Gateway, a **deployment** is a snapshot of your API configuration—including resources, methods, integrations, and settings—that you can publish to make your API callable. 

A **stage** is a named reference to a deployment, allowing you to manage different environments (such as `dev`, `test`, or `prod`). You can configure settings per stage, including logging, throttling, and stage variables.

This separation enables versioning, testing, and controlled rollout of your APIs across environments.

- 📘 [Learn more – Deployments and Stages](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-deploy-api.html)

**Stage variables** in Amazon API Gateway are key-value pairs associated with a specific stage of your API. They act like environment variables and can be used to:

- Customize integration endpoints per stage (e.g., point to different backend URLs for `dev` and `prod`)
- Pass configuration data to Lambda functions via mapping templates
- Reuse a single API definition across multiple environments

Stage variables can be referenced in integration URIs and mapping templates using the `${stageVariables.<variableName>}` syntax.

- 📘 [Learn more – Stage Variables](https://docs.aws.amazon.com/apigateway/latest/developerguide/stage-variables.html)

<img width="801" height="388" alt="image" src="https://github.com/user-attachments/assets/3560df55-6d04-4101-b21d-8e99254455c3" />



### 124. Build an API with Lambda Proxy
[Git examples](https://github.com/nealdct/aws-dva-code/tree/main/amazon-api-gateway)

[Java sample applications for AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/java-samples.html)

### 125. API Gateway Caching and Throttling
[Cache settings for REST APIs in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-caching.html)

- The default TTL value for API caching is 300 seconds. The maximum TTL value is 3600 seconds.
- cache capacity is between 0.5 GB to  237 GB
- cache can be invalidate if it's required
- clients can invalidate cache with the header: Cache-Control: max-age=0

[Throttle requests to your REST APIs for better throughput in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-request-throttling.html)

You can configure throttling and quotas for your APIs to help protect them from being overwhelmed by too many requests. Both throttles and quotas are applied on a best-effort basis and should be thought of as targets rather than guaranteed request ceilings.

When request submissions exceed the steady-state request rate and burst limits, API Gateway begins to throttle requests. Clients may receive 429 Too Many Requests error responses at this point. Upon catching such exceptions, the client can resubmit the failed requests in a way that is rate limiting.

As an API developer, you can set the target limits for individual API stages or methods to improve overall performance across all APIs in your account. Alternatively, you can enable usage plans to set throttles on client request submissions based on specified requests rates and quotas.

API Gateway applies your throttling-related settings in the following order:
1. Per-client or per-method throttling limits that you set for an API stage in a usage plan
2. Per-method throttling limits that you set for an API stage
3. Account-level throttling per Region
4. AWS Regional throttling


### 127. API Gateway Usage Plans and API Keys
[Usage plans and API keys for REST APIs in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-usage-plans.html)	

**A usage plan** specifies who can access one or more deployed API stages and methods—and optionally sets the target request rate to start throttling requests. The plan uses API keys to identify API clients and who can access the associated API stages for each key.

**API keys** are alphanumeric string values that you distribute to application developer customers to grant access to your API. You can use API keys together with Lambda authorizers, IAM roles, or Amazon Cognito to control access to your APIs. API Gateway can generate API keys on your behalf, or you can import them from a CSV file. You can generate an API key in API Gateway, or import it into API Gateway from an external source. For more information, see Set up API keys for REST APIs in API Gateway.

### 129. Microservies with API, Lambda, and DynamoDB
[Step-by-step guide. Microservies with API, Lambda, and DynamoDB. Git example](https://github.com/nealdct/aws-dva-code/blob/main/amazon-api-gateway/api-lambda-dynamodb-hol.md)


### 130. API Gateway Access Control

AWS API Gateway offers multiple mechanisms to control access to your APIs, ensuring secure and authorized usage. Below are the primary methods for managing access:

1. IAM Authorization
Use AWS Identity and Access Management (IAM) to authorize requests from AWS services or users within your AWS account. This is ideal for internal services or trusted applications.

Use Case: Internal service-to-service communication

How: Clients sign requests using AWS Signature Version 4 (SigV4)

Docs: [Controlling access to API Gateway using IAM permissions](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-control-access-to-api.html)


2. IAM Tags for Attribute-Based Access Control (ABAC)
IAM Tags enable attribute-based access control (ABAC), allowing you to manage permissions based on tags attached to IAM users, roles, or resources. This approach is flexible for scaling access control in dynamic environments.

Use Case: Environments where users, roles, or APIs are dynamically created and managed (e.g., multi-tenant systems)

How:

Tag your IAM entities (e.g., roles, users) and API Gateway resources (e.g., restApi or stage)

Create IAM policies that grant access only when tags match

Example: Allow developers to access only APIs tagged with Project = Alpha

Docs: 
[Use IAM tags for ABAC in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-resource-policies-with-iam.html)

[ABAC using IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)

IAM Tags example:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "apigateway:GET",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Project": "Alpha"
        }
      }
    }
  ]
}

```


3. Lambda Authorizer (formerly Custom Authorizer)
A Lambda function evaluates incoming requests and returns an IAM policy (Allow/Deny). Useful for implementing custom authentication logic (e.g., API keys, tokens, OAuth).

There are two types of Lambda authorizers:

- Request parameter-based Lambda authorizer (REQUEST authorizer)
- Token-based Lambda authorizer (TOKEN authorizer)

Use Case: Custom authentication (e.g., JWT, third-party identity providers)

How: Pass headers, query strings, or context to a Lambda function for verification

Docs: [Use Lambda authorizers with a REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html)

4. Amazon Cognito User Pools
Integrate with Amazon Cognito to add user sign-up, sign-in, and identity management to your API. API Gateway verifies tokens issued by Cognito.

Use Case: User authentication for web/mobile apps

How: Use OAuth 2.0 access tokens or ID tokens in Authorization headers

Docs: [Using Amazon Cognito user pools for authorization](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)

5. API Keys
Simple static keys to identify callers. API keys are not secure on their own and are best combined with other methods. You can throttle or meter requests based on keys.

Use Case: Rate limiting or monetizing APIs

How: [Require API keys for method access; assign keys to usage plans](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-key-source.html)

Docs: Use API keys in API Gateway

6. Resource Policies
Attach resource policies directly to your API to allow or deny access based on source IPs, VPCs, AWS accounts, or AWS organizations.

Use Case: Restricting access by network or account

How: Define JSON policy statements at the API level

Docs: [Use resource policies to control access](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-resource-policies.html)

✅ Best Practices
- Combine multiple methods for layered security (e.g., IAM + API Key + Lambda authorizer)
- Use HTTPS endpoints to encrypt data in transit
- Apply throttling and quota limits to prevent abuse



## Section 10. Containers on Amazon ECS\EKS

### 133. Amazon Elastic Containers Service (ECS)

🧩 Core Components of Amazon ECS
1. Clusters
[Amazon ECS Clusters](https://docs.aws.amazon.com/AmazonECS/latest/userguide/clusters.html) A cluster is a logical grouping of tasks or services. You can run ECS clusters using either EC2 or AWS Fargate as the compute backend.

2. Task Definitions
[Amazon ECS Task Definitions](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task_definitions.html) A task definition is a blueprint that describes one or more containers (e.g., image, CPU, memory, ports, environment variables). You register task definitions and use them to run tasks or services.

3. Tasks
[Amazon ECS Tasks](https://docs.aws.amazon.com/AmazonECS/latest/userguide/ecs-using-tasks.html) A task is a running instance of a task definition. Tasks can be run manually or as part of a service.

4. Services
[Amazon ECS Services](https://docs.aws.amazon.com/AmazonECS/latest/userguide/ecs_services.html) A service enables you to run and maintain a specified number of instances of a task definition simultaneously in a cluster. ECS services support integration with load balancers, autoscaling, and rolling updates.

5. Launch Types
You can run ECS workloads on different compute options:
- [Fargate](https://docs.aws.amazon.com/AmazonECS/latest/userguide/what-is-fargate.html) - Run containers without managing servers
- [EC2](https://docs.aws.amazon.com/AmazonECS/latest/userguide/launch_types.html) -Run containers on a user-managed EC2 cluster


6. Container Agent
[Amazon ECS Container Agent](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent.html) The ECS container agent runs on EC2 instances and allows ECS to interact with the Docker daemon. With Fargate, the agent is managed for you.

7. Amazon ECR (Elastic Container Registry)
[Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) Fully managed Docker container registry to store, manage, and deploy container images.

8. Service Discovery
[Service Discovery in ECS](https://docs.aws.amazon.com/AmazonECS/latest/userguide/service-discovery.html) Use Cloud Map for DNS-based service discovery between ECS tasks, especially for microservices architectures.

9. Auto Scaling
[ECS Service Auto Scaling](https://docs.aws.amazon.com/AmazonECS/latest/userguide/service-auto-scaling.html) ECS integrates with Application Auto Scaling to adjust the number of tasks in a service or EC2 instances in a cluster.

10. Security & IAM
[Amazon ECS Security](https://docs.aws.amazon.com/AmazonECS/latest/userguide/security.html) Use IAM roles and policies to control access to ECS resources. ECS supports task roles, execution roles, and encrypted environment variables.


🆚 ECS Launch Types: Fargate vs EC2

| Feature                       | Fargate                                                                                           | EC2                                                                                    |
| ----------------------------- | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **Infrastructure Management** | Fully managed by AWS (serverless)                                                                 | You manage EC2 instances and capacity                                                  |
| **Use Case**                  | Simple, serverless deployments with less operational overhead                                     | Fine-grained control over infrastructure, required for custom AMIs or GPUs             |
| **Pricing**                   | Pay per vCPU and memory per second                                                                | Pay for EC2 instances, regardless of task usage                                        |
| **Scaling**                   | Scales automatically with task demand                                                             | Requires manual instance management or autoscaling configuration                       |
| **Startup Time**              | Fast startup times for containers                                                                 | Slower due to EC2 instance provisioning                                                |
| **Operating System Access**   | No direct access (managed environment)                                                            | Full SSH access to EC2 instances                                                       |
| **Custom AMIs**               | Not supported                                                                                     | Supported                                                                              |
| **Persistent Storage**        | Supported via [EFS](https://docs.aws.amazon.com/AmazonECS/latest/userguide/efs-volumes.html)      | Supported via EBS or EFS                                                               |
| **Networking**                | Each task has its own ENI (awsvpc mode only)                                                      | Supports multiple network modes (bridge, host, awsvpc)                                 |
| **Resource Allocation**       | Must define exact CPU and memory per task                                                         | Shares EC2 resources; more flexibility for over-provisioning                           |
| **Security**                  | Task IAM roles, encryption, and isolation at the task level                                       | EC2 instance roles, security groups, less granular isolation                           |
| **Integration**               | Native with AWS services (CloudWatch, IAM, CloudFormation, etc.)                                  | Also integrates natively, but requires more configuration                              |
| **Documentation**             | [AWS Fargate on ECS](https://docs.aws.amazon.com/AmazonECS/latest/userguide/what-is-fargate.html) | [ECS on EC2](https://docs.aws.amazon.com/AmazonECS/latest/userguide/launch_types.html) |


🌍 Amazon ECS Anywhere
[Amazon ECS Anywhere](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-anywhere.html) extends the capabilities of Amazon ECS to run container workloads on customer-managed infrastructure, such as on-premises servers, edge devices, or virtual machines in other cloud environments — all while using the same ECS APIs, tools, and control plane.

| Feature                   | ECS (Fargate/EC2 on AWS)                                                                                                                                                       | ECS Anywhere                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| **Compute Location**      | AWS-managed infrastructure (Fargate or EC2)                                                                                                                                    | Customer-managed infrastructure (on-prem, edge, or other clouds)                         |
| **Control Plane**         | AWS ECS (fully managed)                                                                                                                                                        | AWS ECS (fully managed)                                                                  |
| **Data Plane**            | Runs in AWS                                                                                                                                                                    | Runs outside AWS                                                                         |
| **Installation Required** | No installation needed (AWS manages it)                                                                                                                                        | You install the ECS agent + SSM agent on your own servers                                |
| **IAM Integration**       | Native IAM roles, task roles                                                                                                                                                   | Requires IAM credentials on external instances                                           |
| **Networking**            | Uses VPC networking in AWS                                                                                                                                                     | Uses your local network configuration                                                    |
| **Task Definitions**      | Same ECS task definitions                                                                                                                                                      | Same ECS task definitions                                                                |
| **Use Cases**             | Cloud-native container workloads, autoscaling apps                                                                                                                             | Edge computing, hybrid cloud, data residency, low-latency local processing               |
| **Pricing**               | Based on AWS compute (Fargate/EC2 pricing)                                                                                                                                     | Pay only for ECS control plane usage + AWS Systems Manager usage                         |
| **Supported OS**          | Amazon Linux, Bottlerocket, etc.                                                                                                                                               | Amazon Linux 2, Ubuntu 20.04, CentOS 7, RHEL 7+ (with ECS agent + SSM agent)             |
| **Docs**                  | [ECS on EC2](https://docs.aws.amazon.com/AmazonECS/latest/userguide/launch_types.html), [Fargate](https://docs.aws.amazon.com/AmazonECS/latest/userguide/what-is-fargate.html) | [ECS Anywhere](https://docs.aws.amazon.com/AmazonECS/latest/userguide/ecs-anywhere.html) |



### 135. Amazon ECS and IAM Roles

<img width="1013" height="575" alt="image" src="https://github.com/user-attachments/assets/05568b38-6fab-4800-abf4-b656278abbf9" />

<img width="1000" height="394" alt="image" src="https://github.com/user-attachments/assets/3627e5d1-dbce-4c4e-bc21-1235c92fa63d" />



### 136. Task Placement Strategies

[Use strategies to define Amazon ECS task placement](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-placement-strategies.html)

For tasks that use the EC2 launch type, Amazon ECS must determine where to place the task based on the requirements specified in the task definition, such as CPU and memory. Similarly, when you scale down the task count, Amazon ECS must determine which tasks to terminate. You can apply task placement strategies and constraints to customize how Amazon ECS places and terminates tasks.

<img width="1108" height="515" alt="image" src="https://github.com/user-attachments/assets/950245be-8f7c-4b4e-9c0b-e4ee98d75f49" />


**[Cluster Query Language](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-query-language.html?utm_source=chatgpt.com)** Cluster queries are expressions that allow you to group objects. For example, you can group container instances by attributes such as Availability Zone, instance type, or custom metadata. For more information, see Amazon ECS container instance attributes.


### 137. Scaling Amazon ECS
[ECS Service Auto Scaling](https://docs.aws.amazon.com/AmazonECS/latest/userguide/service-auto-scaling.html)

2 types of scaling:
- Cluster auto scaling
- Service auto scaling

Amazon ECS Service Auto Scaling supports the following types of automatic scaling:
- Use a target metric to scale Amazon ECS services — Increase or decrease the number of tasks that your service runs based on a target value for a specific metric. This is similar to the way that your thermostat maintains the temperature of your home. You select temperature and the thermostat does the rest.
- Use predefined increments based on CloudWatch alarms to scale Amazon ECS services— Increase or decrease the number of tasks that your service runs based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach.
- Use scheduled actions to scale Amazon ECS services—Increase or decrease the number of tasks that your service runs based on the date and time.
- Use historical patterns to scale Amazon ECS services with predictive scaling—Increase or decrease the number of tasks that your service runs based on historical load data analytics to detect daily or weekly patterns in traffic flows.


[ECS Capacity Provider](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-capacity-provider-console-v2.html)

Auto Scaling Group can automaticall scale using:

| Feature                                     | Description                                                                                                                                              | Docs                                                                                                                            |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Managed Scaling**                         | ECS automatically adjusts the number of EC2 instances in your Auto Scaling group based on the **resource needs of running tasks**.                       | [Managed Scaling Docs](https://docs.aws.amazon.com/AmazonECS/latest/userguide/managed-scaling.html)                             |
| **Managed Instance Termination Protection** | Prevents ECS from terminating EC2 instances that are running **essential tasks** when scaling in. Ensures tasks aren’t disrupted during scale-in events. | [Instance Termination Protection Docs](https://docs.aws.amazon.com/AmazonECS/latest/userguide/managed-instance-protection.html) |


| Concept             | Managed Scaling                                 | Managed Instance Termination Protection           |
| ------------------- | ----------------------------------------------- | ------------------------------------------------- |
| **What it does**    | Adjusts EC2 instance count automatically        | Prevents task-bearing instances from being killed |
| **When it's used**  | During scale-out/scale-in decisions             | During instance termination (scale-in only)       |
| **Who controls it** | ECS (based on task demand)                      | ECS (with protection flags on tasks)              |
| **Applies to**      | Auto Scaling Group managed by Capacity Provider | EC2 instances with running ECS tasks              |



### 138. Amazon ECS with ALB

<img width="1209" height="676" alt="image" src="https://github.com/user-attachments/assets/dce31821-adf1-4c38-a4eb-5aac6bdaaa72" />

### 139. Create an ECS Cluster with EC2 Lunch Type

Create a role with [AmazonEC2ContainerServiceforEC2Role](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AmazonEC2ContainerServiceforEC2Role.html) permissions

[Amazon ECS container agent configuration](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-config.html)


### 141. Amazon Elastic Container Registry (ECR)
[Amazon Elastic Container Registry Documentation](https://docs.aws.amazon.com/ecr/)

Amazon Elastic Container Registry (Amazon ECR) is a fully managed container registry offering high-performance hosting, so you can reliably deploy application images and artifacts anywhere.

[Amazon Elastic Container Registry Identity-based policy examples](https://docs.aws.amazon.com/AmazonECR/latest/userguide/security_iam_id-based-policy-examples.html)


### 142. AWS Fargate Blue-Green CI\CD Pipeline - Part1

[Fargate CI\CD instructions. Step-by-step Github Manual](https://github.com/nealdct/aws-dva-code/blob/main/fargate-blue-green-ci-cd/fargate-ci-cd-instructions.md)

Tasks we’ll perform:
• Create an nginx container image using Docker and push toAmazon ECR
• Create an ALB with multiple listeners for blue/green updates
• Create an AWS Fargate cluster and service fronted by the ALB
• Create a pipeline for pushing code updates via GitHub using CodePipeline and CodeDeploy
• Implement a blue/green updates for the ECS application



### 145. Amazon Elastic Kubernetes Service (EKS)

[Amazon Elastic Kubernetes Service Documentation](https://docs.aws.amazon.com/eks/) 
Amazon Elastic Kubernetes Service (Amazon EKS) is a managed service that makes it easy for you to run Kubernetes on AWS without needing to install and operate your own Kubernetes clusters.


[Amazon EKS Distro](https://aws.amazon.com/eks/eks-distro/) An open-source distribution of Kubernetes from AWS

[Amazon EKS Anywhere](https://aws.amazon.com/eks/eks-anywhere/?nc2=type_a) Create and operate Kubernetes clusters on your own infrastructure

Use cases
- Deploy across hybrid environments
- Deploy high-performing large language models (LLMs)
- Maintain data sovereignty


[EKS Use Cases](https://docs.aws.amazon.com/eks/latest/userguide/common-use-cases.html)

Amazon EKS offers robust managed Kubernetes services on AWS, designed to optimize containerized applications. The following are a few of the most common use cases of Amazon EKS, helping you leverage its strengths for your specific needs.

- Deploying high-availability applications
- Building microservices architectures
- Automating software release process
- Running serverless applications
- Executing machine learning workloads
- Deploying consistently on premises and in the cloud
- Running cost-effective batch processing and big data workloads
- Securing application and ensuring compliance

EKS supports
- ALB, NLB, CLB
- Horizontal and Vertical Scaling


### 146. AWS Copilot

[Deploy demo app using AWS Copilot. Step-by-step Github exmaple](https://github.com/nealdct/aws-dva-code/blob/main/aws-copilot/copilot-commands.md)	

[AWS Copilot ](https://aws.github.io/copilot-cli/) is an open source command line interface that makes it easy for developers to build, release, and operate production ready containerized applications on AWS App Runner, Amazon ECS, and AWS Fargate.


[AWS App Runner](https://aws.amazon.com/apprunner/) Deploy web applications and APIs at scale. AWS App Runner builds and deploys web applications automatically, load balances traffic with encryption, scales to meet your traffic needs, and allows for the configuration of how services are accessed and communicate with other AWS applications in a private Amazon VPC.


### 147. ECS EKS Exam Questions
See the slides.



## Section 11. CI\CD Tools

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


### 157.  AWS Fargate Blue-Green CI\CD Pipeline - Part 4


## 158. AWS Apmlify and AppSync
[AWS Amplify](https://aws.amazon.com/amplify/) Fully managed service to build full-stack applications.

AWS Amplify is a comprehensive development platform from Amazon Web Services (AWS) designed to simplify the process of building and deploying web and mobile applications. It provides a set of tools and services that enable developers to create full-stack applications with ease, without requiring extensive cloud expertise

[AWS AppSync](https://aws.amazon.com/appsync/) AWS AppSync is a service that lets you create and manage GraphQL APIs for web and mobile apps. It offers features such as data caching, API federation, real-time events, custom domains...


## Section 12. Databases and Analytics
[Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) is a fully managed service for setting up, operating, and scaling relational databases in the cloud. It automates common tasks such as provisioning, patching, backup, recovery, and replication.


✅ Amazon RDS supports the following engines:
- Amazon Aurora (MySQL/PostgreSQL-compatible)
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server


🛠️ Key Features

📘 [Amazon RDS Features](https://aws.amazon.com/rds/features/)

- Automated Backups and Point-in-Time Recovery
- Multi-AZ Deployments for high availability
- Read Replicas for read scaling (MySQL, PostgreSQL, Aurora, MariaDB)
- Automatic Software Patching & Upgrades
- Encryption at Rest & in Transit (TLS/SSL, KMS)
- Monitoring via Amazon CloudWatch
- IAM Authentication for database access
- Performance Insights for real-time analysis
- Database Migration Support with DMS

### 165. Amazon Aurora
See the slides!!!!!

[Replication with Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Replication.html):
- Aurora Replicas
- Replication with Aurora MySQL
- Replication with Aurora PostgreSQL

- Amazon Aurora is an AWS database offering in the RDS family
-  Amazon Aurora is a MySQL and PostgreSQL-compatible relational database built for the cloud
- Amazon Aurora is up to five times faster than standard MySQL databases and three times faster than standard PostgreSQL databases
- Amazon Aurora features a distributed, fault-tolerant, self-healing storage system that auto-scales up to 128TB per database instance

[Using Amazon Aurora Global Database](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-global-database.html)

With the Amazon Aurora Global Database feature, you set up multiple Aurora DB clusters that span multiple AWS Regions. Aurora automatically synchronizes all changes made in the primary DB cluster to one or more secondary clusters. An Aurora global database has a primary DB cluster in one Region, and up to 10 secondary DB clusters in different Regions. This multi-Region configuration provides fast recovery from the rare outage that might affect an entire AWS Region. Having a full copy of all your data in multiple geographic locations also enables low-latency read operations for applications that connect from widely separated locations around the world.


[Using Amazon Aurora PostgreSQL Limitless Database](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/limitless.html) provides automated horizontal scaling to process millions of write transactions per second and manages petabytes of data while maintaining the simplicity of operating inside a single database. With Aurora PostgreSQL Limitless Database, you can focus on building high-scale applications without having to build and maintain complex solutions for scaling your data across multiple DB instances to support your workloads.


### 165. Amazon RDS Security
See the slides!!!

[Encrypting Amazon Aurora resources](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Overview.Encryption.html)

- Supports encryption at rest, but it can be only enabled when a DB instance is creating
- encryption cannot be changed or disabled after a DB is created
- DB Backup and replicas has the same encryption (or absence of encryption) as the original DB.  for example, you cannot, make a backup of unencrypted DB and restore an encrypted DB from the backup
- But there is a way to make an unecnrypted snapshot of a DB and copy it to an encrypted snapshot and restore the DB from the encrypted snapshot. In this case it will be not the same DB, but a copy of the original DB

### 166. Amazon ElastiCache

[Amazon ElastiCache Documentation](https://docs.aws.amazon.com/elasticache/)

Welcome to the Amazon ElastiCache User Guide. Amazon ElastiCache is a web service that makes it easy to set up, manage, and scale a distributed in-memory data store or cache environment in the cloud. It provides a high-performance, scalable, and cost-effective caching solution. At the same time, it helps remove the complexity associated with deploying and managing a distributed cache environment.

You can operate Amazon ElastiCache in two formats. You can get started with a serverless cache or choose to design your own cache cluster.

Amazon ElastiCache works with:
- Valkey
- Memcached
- Redis OSS engines.

If you're unsure which engine you want to use, see [Comparing Valkey, Memcached, and Redis OSS self-designed caches](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/SelectEngine.html) in this guide.


### 171. Amazon MemoryDB for Redis

The slides contain comparison of MemoryDB and ElastiCache

[Amazon MemoryDB](https://aws.amazon.com/memorydb/)

Use cases
- Real-time generative AI applications
- Build web and mobile applications
- Develop online games
- Stream media and entertainment


### 172. Amazon Kinesis Core Knowledge
[Amazon Kinesis](https://aws.amazon.com/kinesis/)

Use cases
- Create real-time applications
- Evolve from batch to real-time analytics
- Analyze IoT device data
- Build video analytics applications

<img width="1002" height="582" alt="image" src="https://github.com/user-attachments/assets/af6b6b4b-22b5-4d64-b74f-d79292740657" />



[Amazon Kinesis Data Firehose](https://aws.amazon.com/firehose/)

[Amazon Kinesis Data Firehouse Docs](https://docs.aws.amazon.com/firehose/)

[Amazon Kinesis Data Firehouse supported surces and destinations](https://docs.aws.amazon.com/firehose/latest/dev/create-name.html)

Amazon Data Firehose provides the easiest way to acquire, transform, and deliver data streams within seconds to data lakes, data warehouses, and analytics services. To use Amazon Data Firehose, you set up a stream with a source, destination, and required transformations. Amazon Data Firehose continuously processes the stream, automatically scales based on the amount of data available, and delivers it within seconds.

[Analytics on AWS](https://aws.amazon.com/big-data/datalakes-and-analytics/)

### 173. Amazon Kinesis Client Library (KCL)
[Kinesis Client Library (KCL)](https://docs.aws.amazon.com/streams/latest/dev/kcl.html)

Kinesis Client Library (KCL) is a standalone Java software library designed to simplify the process of consuming and processing data from Amazon Kinesis Data Streams. KCL handles many of the complex tasks associated with distributed computing, letting developers focus on implementing their business logic for processing data. It manages activities such as load balancing across multiple workers, responding to worker failures, checkpointing processed records, and responding to changes in the number of shards in the stream.


- The Kinesis Client Library (KCL) helps you consume and process data from a Kinesis data stream
- The KCL is different from the Kinesis Data Streams API that is available in the AWS SDKs
	- The Kinesis Data Streams API helps you manage many aspects of Kinesis Data Streams (including creating streams, resharding, and putting and getting records)
	- The KCL provides a layer of abstraction specifically for processing data in a consumer role

### 174. Amazon OpenSearch Service
Based on the popular open source ElasticSearch.

[What is OpenSearch](https://aws.amazon.com/what-is/opensearch/)

### 175. Amazon Athena and AWS Glue

<img width="1018" height="576" alt="image" src="https://github.com/user-attachments/assets/1831b375-a7c2-42f9-b966-65ac41940709" />



[Available data source connectors](https://docs.aws.amazon.com/athena/latest/ug/connectors-available.html) lists prebuilt Athena data source connectors that you can use to query a variety of data sources external to Amazon S3. To use a connector in your Athena queries, configure it and deploy it to your account. Athena uses Lambdas to make requests to the datasources (but not to S3)

[AWS Glue](https://docs.aws.amazon.com/glue/) is a fully-managed, scalable, serverless data integration service that makes it easy to discover, prepare, and combine data for analytics, machine learning, and application development.

### 176. Exam questions



## Seection 13. Management and Security

### 178. AWS AppConfig
[AWS AppConfig Documentation](https://docs.aws.amazon.com/appconfig/)  Use AWS AppConfig to quickly deploy **application configurations** to applications of any size. AWS AppConfig supports controlled deployments and includes built-in validation checks and monitoring.


### 179. Amazon CloudWatch Overview

[Feature flags and supported configuration data stores](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-profile.html)

[non-native data sources](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-profile-other-data-sources.html)

CloudWatch Core Features:
- CloudWatch Metrics – services send time-ordered data points to CloudWatch
- CloudWatch Alarms – monitor metrics and initiate actions
- CloudWatch Logs – centralized collection of system and application logs
- CloudWatch Events – stream of system events describing changes to AWS resources and can trigger actions

[CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) is a software component that collects metrics, logs, and traces from your Amazon EC2 instances, on-premises servers, and containerized applications. It enables you to monitor your infrastructure and applications more comprehensively than the basic monitoring provided by default.

Key benefits:
- Collect system-level metrics (CPU, memory, disk, network)
- Gather custom metrics from your applications
- Collect and centralize logs from various sources
- Monitor both AWS and on-premises environments with a single tool
- Set up alarms and notifications based on collected data


[Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) You can create both `metric` and `composite` alarms in Amazon CloudWatch.

### 180. Create a Custom Metric and Alarm
[CloudWatch custom metrics. Github step-by-step mainual.](https://github.com/nealdct/aws-dva-code/blob/main/amazon-cloudwatch/custom-cloudwatch-metrics.md)



### 183. Unified CloudWatch agent

(Use the unified CloudWatch agent to get started with CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/UseCloudWatchUnifiedAgent.html)

### 184. AWS CloudTrail
[AWS CloudTrail Documentation](https://docs.aws.amazon.com/cloudtrail/)

With AWS CloudTrail, you can monitor your AWS deployments in the cloud by getting a history of AWS API calls for your account, including API calls made by using the AWS Management Console, the AWS SDKs, the command line tools, and higher-level AWS services. You can also identify which users and accounts called AWS APIs for services that support CloudTrail, the source IP address from which the calls were made, and when the calls occurred. You can integrate CloudTrail into applications using the API, automate trail creation for your organization, check the status of your trails, and control how administrators turn CloudTrail logging on and off.


[Understanding CloudTrail events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-events.html)

There are four types of CloudTrail events:
- Management events
- Data events
- Network activity events
- Insights events


### 186 KMS
[AWS Key Management Service Documentation](https://docs.aws.amazon.com/kms/) 
AWS Key Management Service (AWS KMS) is an encryption and key management service scaled for the cloud. AWS KMS keys and functionality are used by other AWS services, and you can use them to protect data in your own applications that use AWS.

[Data keys ](https://docs.aws.amazon.com/kms/latest/developerguide/data-keys.html) are symmetric keys you can use to encrypt data, **including large amounts of data** (> 4KB) and other data encryption keys. Unlike symmetric KMS keys, which can't be downloaded, data keys are returned to you for use outside of AWS KMS.


### 188 AWS KMS API and CLI

### 189 AWS Certificate Manager

### 190 AWS System Manager

[AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html) is a fully managed AWS Systems Manager tool. With Session Manager, you can manage your Amazon Elastic Compute Cloud (Amazon EC2) instances, edge devices, on-premises servers, and virtual machines (VMs). You can use either an interactive one-click browser-based shell or the AWS Command Line Interface (AWS CLI). Session Manager provides secure node management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys. Session Manager also allows you to comply with corporate policies that require controlled access to managed nodes, strict security practices, and logs with node access details, while providing end users with simple one-click cross-platform access to your managed nodes. To get started with Session Manager, open the Systems Manager console. In the navigation pane, choose Session Manager.


### 191 AWS System Manager Parameter Store

Does not rotate keys automatically (differene with AWS Secrets Manager)

### 192. AWS Secrets Manager

Rotates keys automatically

[AWS Secrets Manager CLI commands. Github](https://github.com/nealdct/aws-dva-code/blob/main/aws-secrets-manager/secrets-manager-cli-commands.md)

### 193. Working with Secrets

### 194. Amazon Cognito

### 195. Exam Questions
