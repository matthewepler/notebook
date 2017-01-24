# Day 1
[quiklab](http://aws.quiklab.com)
[slides](https://evantage.gilmoreglobal.com) - credentials under "gilmore.ca" in Last pass
Instructor: Roger Davis <rdavisjr@amazon.com>

NOTE: Labs go down after a week. The lab book is still accessible, but will not get you 100% there in the real AWS console. This is because the labs are built witih templates that take care of some setup for you. You can interpolate from the book and try to build it in AWS, though and it would get you close. Meh. 

**Certification**
Recommend "AWS Official Study Guide" for Architect Associate. Has exam questions, domains, etc. Available in hardcopy and Kindle editions. There is also a certification for Developers!

For the associate level, it is supposed to prove you have been using the system on a day-to-day level for 6-months to a year. Don't just take the course and then the test. 

TL;DR take the certification after you've been using AWS for a few months.

**My Goals**
1. Deploy Web and IoT Apps
2. Set up a virtual lab for pentesting 
3. Pentest existing deployments

Recommended viewing: "A day in the Life of a Billion Packets" and "Another Day in the Life of a Billion Packets." Search online

Regions are made up of multiple Availability Regions (AZ).
You enable and control data replication across regions.

**Managed vs Unmanaged**
Unmanaged = you are responsible (e.g. EC2)
Managed = Amazon is responsible (e.g. RDS)

Together, this constitutes the "shared responsibility" model. AWS goes from the concrete to the hypervisor. You take it from there.

Security Responsibilities of the user
Instance, Application, Groups, OS, network, account access.

**EFS vs EBS**
EBS can only be attached to one EC2 instance at a time
EFS (Elastic File System) is a storage drive that can be used across instances. 

**Security and State**
Security Groups are stateful (like a doorman who remembers who you are) 
NACLs are stateless (like passport control, just cause you got in doesn't mean we'll let you out))

### Best Practices
See slides in Module 1: Core AWS Knowledge (link at head of doc), #63-

1. Enable scalability - ensure that your architecture can handle cahnges in demand.
2. Automate your environment - removing manual processes to improve your system's stability and consistency, and the efficiency of your organization.
3. Use Disposable Resources - Think of servers and other components as temporary resources. __e.g.__ is the job comopleted? Shut the instance down.
4. Loosely Couple Your Components - Reduce interdependencies so that the change or failure of one component does not affect other components. __e.g.__ Load balancer reduces dependency between instances.
5. Design Services, Not Servers - Managed services ands erverless architectures can provide greater reliability and efficiency in your environment. __e.g__ use Lamda for running simple functions, SQS for message queing, SNS for push notifications.
6. Choose the Right Database Solutions - Match the tech to the workload: choose from an array of relational database engines, NoSQL solutions, data warehousing options, and search-optimized data stores. __e.g.__ Redshift for data warehouseing, RDS for relational databases, DynamoDB for NoSQL, Kinesis for streaming data, Elasticsearch for search.
7. Avoid single points of failure - implement redundancy where possible so that single failures don't bring down an entire system. __e.g.__ if one instance goes down, another is available
8. Optimize for Cost - ensure that your resources are sized appropriately, that they scale in and out based onneed, and that you're taking advantage of different pricing options. __e.g.__ start with a size too small and then work your way up only if you need it. 
9. Use Caching - use caching to minimize redundant data retrieval operations. __e.g.__ caching with CloudFront, tied to an S3 bucket.
10. Secure Your Infrastructure Everywhere - AWS enables you to implement security both at the perimeter and within/between your resources. __e.g.__ between security groups


## Module 2: AWS Core Services
VPC: you can have __up to 5 per region__
Do not use your default VPC in production and do not delete it. 
Each VPC lives in a region, but can include resources in more than one AZ. It has access to all AZs in the region. 

See slides for more detail

EC2 now supports FPGAs, and GPU instances.

AWS runs on Intel Xenon processors. Good to know the architecture of the chip in many cases.  
they can handle encryption and have many other features. 

EC2 pricing tiers are tied to their performance.
* On-Demand - short term, spiky, unpredictable workloads __e.g.__ app dev/testing
* Reserved Instances - steady or predictable workloads, apps that require reserved capacity, including disaster relief
* Spot - Bid on unused capacity. Applications with flexible start and end times, very very low cost. __e.g.__ users with urgent computing needs for large amounts of additional capacity

! If you're using an EC2 more than 55% of the year, you should use a Reserved instance. You're losing money after that with on-demand instances. 
NTS: business setting up render pipelines for post-houses and departments? GPU services, Spot instances, etc...

EBS are good for dtabase hosts. Auto-replication within the AZ. 

**Block storage vs. object storage.**
Example case: if you change one character, only that block of memore is updated in Block Storage. If Object storage, you have toupdate the entire object. 

S3 - because data is stored redundantly, deleting/updating objects can take a little time. If you do a 'read' on a db right after a 'delete,' you may still see it. Test with different object sizes. This is not true for 'put'... for some reason I don't understand.

S3 pricing depends on the transaction. See slide #97-98

You can create S3 lifecycle policies that will move your files into other services based on use. S3 Standard -> S3 Infrequent Access -> Glacier -> Delete

Retrieval of deleted files in S3 is only possible when using versioning. 
You can set a policy that requires MFA (multi-factor authentication) for deleting anything.
?? but what if versioning is on and I want to __really__ delete something (esp. if I don't want to pay for it being there)
IOW - versioning creates a new copy of a file every time you update it, and that will increase overall costs. 

RDS is fully managed all the way up to the data layer itself. CPU, security, updates, patches, etc. 
Aurora is MySQL refactored for the cloud. They're working on the same thing for Postgre

RDS is for complex queries and transactions. High durability. NOT good for massive read/writes, sharding, simple GET/PUT and queries. 

If you need fine grain control, you can create your own DB in EC2. 

DynamoDB - also fully managed up to data layer. Replication, scaling, etc.

For RDS and DynamoDB, you cannot login directly to the service. IOW you can't just log into an EC2 running your db. It's a managed service, therefore you only get the access points offered by AWS.

**IAM and Security**
See slide 111 for types of Security Credentials available on AWS
On slide 116, the "Deny" policy says deny for anything BUT the resource described. So the first one gives them access, and the second denies them from anything but that. 

Everything is __implicitly__ denied. You must __explicity__ allow anything you want to give access for. 
Diagram from slide 114. 

Denied				 Allowed
Explicity? -------> explicity? -------> Deny
	|					|
   Yes				   Yes
	|					|
   Deny				  Allow

There are slides expalining policies vs. groups vs. users, etc. starting at 117.
You can set limits for access for a role, i.e. date, time, duration, etc.

### Setting up a new AWS account
1. With the root account, create an IAM user for yourself.
2. Create an IAM group and give it full administrator permissions. 
3. Sign-in with your IAM user credentials.
4. Store your root account credentials in a very secure place. Disable and remove your root account access keys, if you have them.  
5. Require MFA for access (for root and all IAM users). Alternatively, you can also do this for AWS Service API.
	* software MFA options: AWS virtual MFA, Google Auth, SMS notification
	* hardware MFA options: key fob or display card offered by Gemalto <http://onlineoram.gemalto.com> 
6. Enable AWS CloudTrail for logging all API requests. 
	a. Via CloudTrail console: create a trail, give it a name, apply it to all regions, and enter a name for the new Amazon S3 bucket that the logs will be stored in. 
	b. Ensure that the Amazon S3 bucket you use for CloudTrail has its access restricted to only those who should have access, such as admins. 
7. Track changes to resources with AWS Config (see below)
8. Enable a billing report, such as the AWS Cost and Usage Report. These are delivered to an Amazon S3 bucket at least once a day (you specify the bucket).

AWS Config: resource inventory, config history, and config change notifications. Combines with CloudTrail. Enables compliance auditing, security analysis, resource change tracking, and troubleshooting.

Inline Policies are used to allow an IAM Role to be assumed by a resource.  This can be added in a User's settings at the bottom fo the "permissions tab," and to the right @ "Add inline policy."


## Lab 1 Notes
Every subnet in your VPC must be associeate with a route table. A subnet can be associated with only one route table at a time, but you can associate multiple subnets with the same route table.
When you create a VPC, it automatically has a main route table with a single route that enables communication with the VPC. If you don't explicity associate a subnet with a route table, the subnet is implicity assoc. with the main route table. 



