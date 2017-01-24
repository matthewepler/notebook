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

#### AWS IAM BEST PRACTICES










































