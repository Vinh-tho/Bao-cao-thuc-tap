---
title: "Week 2 Worklog"
date: 2026-08-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Objectives Week 2:

- Learn AWS Core Services: EC2, S3 and IAM.
- Practice launching a virtual machine, storing objects and hosting a static website on S3.
- Understand IAM Users, Roles and Policies and the least-privilege principle.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Sat – Sun | Study EC2: instance types, AMI, EBS, key pair, Security Group basics, and the Free Tier option (t2.micro). | 08/08/2026 | 09/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Mon – Tue | Launch a t2.micro EC2 instance, connect via SSH, install a simple web server, then stop/terminate the instance to save cost. | 10/08/2026 | 11/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wed | Study S3: buckets, objects, storage classes, Block Public Access and Bucket Policy. | 12/08/2026 | 12/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thu | Create an S3 bucket, upload files, enable Static Website Hosting (index/error document) and publish a sample page; fix the 403 error by turning off Block Public Access and adding a GetObject policy. | 13/08/2026 | 13/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Fri | Study IAM Users, Groups, Roles and Policies; create an IAM Role for an AWS service and attach a managed policy following least privilege. | 14/08/2026 | 14/08/2026 | <https://docs.aws.amazon.com/IAM/latest/UserGuide/> |

### Technical configuration details:

| Component | Configuration / value |
| --- | --- |
| EC2 instance | t2.micro (Free Tier eligible), SSH access with a key pair for the lab |
| S3 website bucket | Block Public Access off; Static website hosting: index document index.html, error document error.html; Object Ownership: ACLs disabled |
| Bucket Policy | Effect Allow, Principal *, Action s3:GetObject, Resource arn:aws:s3:::<bucket>/* |
| Eshop-EC2-Instance-Role | Trusted entity EC2; policy AmazonEC2ContainerServiceforEC2Role |
| Eshop-ECS-Task-Execution-Role | Trusted entity ECS Task; policy AmazonECSTaskExecutionRolePolicy |
| Eshop-Lambda-Image-Role | Trusted entity Lambda; policies AWSLambdaBasicExecutionRole and AmazonS3FullAccess (simplified for practice) |

### Knowledge gained:

- EC2 basics: AMI (image of the OS), instance type (CPU/RAM), key pair (SSH authentication) and Security Group (virtual firewall).
- S3 is object storage: bucket names are globally unique, lowercase and without spaces; objects are addressed by key; static hosting serves files over HTTP from a website endpoint.
- A bucket policy is JSON with Version, Statement, Effect, Principal, Action and Resource; the /* suffix is required to cover objects inside the bucket.
- IAM Users represent people with long-term credentials; IAM Roles are assumed temporarily by services, so services never need static access keys.
- Least privilege: grant only the actions a component needs (the Lambda role uses S3FullAccess only to simplify the lab and should be narrowed in production).

### Commands and code used:

**Example SSH command used for the EC2 lab**

```bash
ssh -i <key-pair>.pem ec2-user@<public-ip>
```

**Bucket policy that allows public read of website files**

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::<bucket-name>/*"
  }]
}
```

### Difficulties and solutions:

- S3 website returned 403 Forbidden. → Disabled Block Public Access and added a Bucket Policy with Resource ending in /* .

### Achievements Week 2:

- Launched and connected to an EC2 instance successfully.
- Hosted a static website on S3 and understood why a bucket returns 403 without a proper policy.
- Understood the difference between IAM User and IAM Role, and why services should use roles instead of static access keys.
- Laid the foundation for the E-shop frontend bucket and IAM roles built in the final project.

### Deliverables and evidence:

- Working EC2 SSH session.
- S3 static website reachable over its website endpoint.
- Three IAM roles defined for later use (EC2 instance, ECS task execution, Lambda).

### Plan for next week:

- Learn VPC networking and build public/private subnets.
