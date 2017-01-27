# AWS TECHNICAL ESSENTIALS
Taken through AWS Activate for Startups (free)
See welcome email from AWS Activate for links. 
[Training link](https://awstraining.csod.com)

## Module 1: Intro & History of AWS

## Module 2: AWS Infrastructure
### EC2 - Elastic Computer Cloud
* virtualized server
* Linux or Windows
* pay only for what you use

1. Choose region
2. Choose AMI (Amazon Machine Image)
3. Configure network

AMI - template for a root volume. You can launch as many instances as you want from a single AMI.

To create a custom AMI you save an instance of a template. These can be shared, sold, and bought.

Storage inside an EC2 instance is temporary and is destroyed when the instance is terminated. Use EBS volumes or backup to S3.

Only EBS-backed instances can be stopped and started again. Stop/Starting will cause loss of local storage on the instance.

You are billed for all runniing instances, even if they're idle or not being connected to. 

REbooting - recommendation is to use AWS API or console for rebooting instead of from within the OS of the instance. This will preserve data and IP provisioning. 

Terminated instances will still appear in console for a while. You cannot recover a terminated instance.

### Instance MEtadata vs. User MEtadata
**Instance Metadata**
No protection. Anyone can access through public URL. Do not store keys or keep passwords here.

From inside the instance, run `curl` or use a GET request:
`curl http://169.254.169.254/latest/meta-data`

**User Metadata**
Private. Like a Dockerfile, it is a config text file that is used at runtime. 

From inside the instance, run `curl` or use a GET request:
`curl http://169.254.169.254/latest/user-data`

### Key pairs
If you lose your key pairs for EC2, you're screwed. You have to contact AWS support to get back in. 

### AWS Storage
#### S3
Protected by permissions set for users through AWS Console. 
1. IAM (users within your AWS account)
2. ACL - Access Control Lists (users from other Amazon accounts)

**Encryption**
Data can be encrypted on the client or the server. 
Can set rules for buckets to only accepted encrypted data. 
Can encrypt incoming data automatically if you choose.

Support for versioning is available for files with same name uploaded multiple times to same location. 

**Storage Types**
Standard = frequent access
IA = Infrequent Access (long-lived data that still requires performance, like backups)
Glacier = long-term, slow availability of data (hours)
RRS = Reduced Redundancy Storage (? type)

#### Amazon EBS
EBS = Elastic Block Storage. It's like hard drives. Used as primary storage with frequent updates. It uses a file system, unlike S3. 

S3 is used across facilities in a region (made up of several AZs). EBS is used across servers in a single AZ.

Must be attached to an EC2 instance in the same zone. Can be attached to more than one EC2 instance. 

Auto-replicated within Availability Zones (AZ)

Snapshots are stored in S3.

Billed whether you use it or not. You pay for what you provision.

Can save snapshot in one AZ and auto-link it to another storage instance in another AZ.


u can create a volume from a snapshot. This also gives you the ability to change its type, size, and other settings. This is useful for restoring a backup, or as a way to change the capacity of an existing drive without interruption. 

### Networking
NTS - need to learn more about basic and intermediate Networking

Amazon VPC (Virtual Private Cloud). Groups of instances, databases, and services all sharing the same virtual space. Subnets, exposure to internet, security groups, port access, instance linking can all be controlled. 

NAT = network address translation. 1 public IP address is translated into several local private IP addresses and vice-versa. 

All US accounts have a VPC already in your account. **DO NOT DELETE IT.**

Public IP = an IP that changes 
Elastic IP = persists for lifetime of an instance, used for static public IP addresses. 

**Route Tables**
'igw' in the links  = internet gateway (public)
'eni' = elastic NAT instance (private)

See labs for more detail. 

All EC2 instances run inside a VPC. 

No billing for VPC, only the components within them. 

More free training on VPC's in the self-paced labs (free) via Amazon's training site. 

ACL (Network Control Lists) offer additional security at the subnet level by acting like a firewall, controlling both inbound and outbound traffic at the subnet level. 


## Module 3: Security & Identity & Access Management
! AWS is monitoring for pentesting activity like port-scanning. See <http://aws.amazon.com/security> for prerequisites if pentesting.

IAM is free. 

**Always create users, even for admins. Never use 'root'**

### Policies
Written as a JSON object
Explicitly lists permissions
Assigned to Users, Groups, and Roles

**Roles**
Are an Identity + Policy. Not unique to a single user. Does not require access keys.
You can "assume" a role and be given temporary access to something. 
Resources can also assume roles. 
For example: a Python app on EC2 needs to interact with S3. AWS credentials are required for this. 
* option 1: store credentials in the app (running in EC2 instance)
	! what if you accidentaly publish to the web, forget to include it in your .gitignore, etc?
* option 2 (recommended): code inherits permissions by assuming a role assigned to the instance through the metadata service.

Note: you can only assign a role at the creation of an instance. Changes to roles will require creation of a new instance. 

### IAM
Not for OS or applications. It's for:
* AWS management console, CLI, SDK API
* Authorization and Policies

**AWS IAM BEST PRACTICES**
1. Delete AWS account root access keys
2. Create individual IAM users
3. Use groups to assign permissions to IAM users
4. Grant least privelage
5. Configure strong password policy
6. Enable MFA (Multi-factor Authentication) for privelaged users or all users
7. Use roles for apps that run on EC2
8. Delegate by using roles instead of sharing credentials
9. Rotate credentials regularly
10. Remove unecessary users and credentials
11. Use policy conditions for extra security
12. Monitor activity in your AWS account (see AWS CloudTrail)

Alt to AIM is Resource-based Policies
* offered by some services, not all
* grant across-account access
* examples include S3, Glacier

Note: when configuring IAM in console, region will appear as "Global."

**Admin Roles**
Best Practice is to not even give yourself admin access by default. Create an admin role you can assume manually so you don't make a stupid mistake. 

Examples of typical security group names:
* Administrators
* Developers
* Security Auditors

Default privelages should be ZERO!

Note: Assigning someone to a group does not give them sign-in access to AWS. Assign that in their "Security Credentials" tab.

You can have 2 access keys, for rotation. One is active, the other inactive and pending. 
__Setting Up__
1. Just give them a single key
2. When time to rotate, come back and see the following:

__Rotating Keys__
1. If only one key, add another one. It will be inactive.
2. Turn off the active one
3. Make the other one active
4. Verify a sign-on and that it's working for the user
5. Delete the old one. 


## Module 4: Storage
### RDS 
Relational Database Service
* scales 'vertically' (adds more hardware)
* resizable capacity
* diff. flavors: MySQL, PostreSQL, etc...
* autobackus w/ up to 35 days retention
* capable of snapshots saved to S3 & cross-regional snapshots

There is always a Standby copy instance of the RDS database. In the event of failure of the original instance, the Standby is swapped in and a new Standby is created. 

**RDS Best Practices**
1. Monitor memory, CPU, storage
2. Use multi AZ deployments to auto provision and maintain a synchronous standby in a different AZ
3. Enable auto backups
4. Set the backup window to occur during the daily low in write IOPS
5. Set a TTL of < 30 seconds if your client app is caching the DNS data of your DB in stances.
6. Test failover for your DB instance. Don't just assume it will work the way you think it will. 

### DynamoDB (NoSQL)
* no storage limits
* SSD storage, so it's fast
* easy to change capacity

Each tabe requires that every item needs a unique ID (called the primary id)
There are 2 different types of Primary ID's
1. Partition - uses a value from the entry to hash a value. That value becomes where it will be stored. No 2 items in a table can have the same partition key value. 
2. Composite - Partition + Sort. ??? __Unclear to me.__

**Supported Operations**
1. Query - uses keys (faster)
2. Scan - reads every item (slower). Do not use if avoidable.

You can use RDS and Dynamo in the same application and/or instance.

You can run other DB types, you just have to set them up yourself in your instances. There are also AMIs build to handle other DB types.


## Elasticity and Management Tools
**Auto-Scaling**
good for apps that experience variability in usage
Service is free, but you pay for the EC2 instances

Need to know:
* What - launch configuration - a template used to launch new EC2 instances
* Where - Auto-scaling Grouops - A collection of EC2 instances that share similar characteristincs. You set a min, max, and target quantity and auto-scaling attempts to stay at the target. 
* When - Auto-scaling lifecycles

**Load Balancers**
distributek traffic across instances. Supports HTTP/S, TCP to EC2 instances

continually checks helath of every instance. Can tie the health w/ auto-scaling to create a self-healing system.

Can also be used for internal networks as well as internet-facing ones.

Can also be used to force SSL by encrypting incoming HTTP to HTTPS

**CloudWatch**
monitoring, logging
can add custom metrics from apps if desired
default refresh is every 5 minutes. You can pay addit'l for every 1 min

Alarms can trigger actions like SMS, email, and auto-scaling

**Trusted Advisor**
A reccomendation engine for best practices
* cost
* security
* fault tolerances
* performance improvement



































