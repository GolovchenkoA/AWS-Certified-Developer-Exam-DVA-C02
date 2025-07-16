# AWS-Certified-Developer-Exam-DVA-C02
[AWS Certified Developer Exam with this AWS Developer Udemy Course [DVA-C02]](https://www.udemy.com/course/aws-certified-developer-associate-exam-training/) 

[DVA-CO2 Exam link](https://aws.amazon.com/certification/certified-developer-associate/)

[DigitalCloud AWS Hands-On Challenge Labs](https://digitalcloud.training/aws-hands-on-challenge-labs) - Optional. Additional labs using an AWS Sandbox.

[Course Downloads GitHub](https://github.com/nealdct/aws-dva-code)

[Course Downloads](https://digitalcloud.training/aws-certified-developer-course-downloads/)


👉 All additional files (including slides) are stored on Google Disc in the folder `Certification Courses\Kubernetes\AWS\DVA-CO2 AWS Certified Developer Exam\` and there is an email from ...@digitalcloud.training


# Section 2 AWS Accounts and IAM

## 8 AWS Account and Budget
- Account. Add alias
- enable MFA
- Account. Activate AM user and role access to Billing information
- Billing preferences. Alert preferences. Recieve AWS Free Tier Alerts
-  Budgets. create a monthly budget
- Cost Explorer

## Section 3. Command Line Interfica (CLI)


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


## 52. S2 Presigned URLs

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


### 156. AWS Fargate Blue\Greed Pipeline

 [See Part4-5 manual](https://github.com/nealdct/aws-dva-code/blob/main/fargate-blue-green-ci-cd/fargate-ci-cd-instructions.md)



## 158. AWS Apmlify and AppSync
