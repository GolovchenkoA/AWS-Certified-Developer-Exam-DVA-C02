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

## 9 AWS IAM

- Identity-based policy and Resoure-base policy
- User, User Groups, Role, Policy
- Identity-based policy is applied to user, groups and roles
- Roles are used for delegation and are assumed
- [IAM ARNs](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-arns). User example: arn:aws:iam::account:user/UserName
- Users gain the **permissions** applied to the group through the **policy**
- Authentication: AWS IAM, CLI, API
- The root user has Full unrestricted permissions. IAM Users are restricted by IAM Policies (by default everything is restricted)

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


# Section 5. Amazon S3 and Cloud Front

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

- Server side encryption with S3 managed keys (SSE-S3 )
- Server-side encryption with AWS KMS managed keys (SSE-KMS)
- Server-Side encryption with client provided keys (SSE-C)
- Client-side encryption

📝 For additional information see the Slides
