## Troubleshooting

You can configure a **VPC endpoint** that allows you to access Amazon services nearby without hitting the public internet (like S3, etc.). 

See MODULE 14 for great questions that highlight real-world scenarios.

## MODULE 9: Design Patterns and Sample Architectures
Google Whitepaper on "AWS Well Architected Framework"
[link to page](https://aws.amazon.com/architecture/well-architected/)

Highly Available - ability to come back from downtime as quickly as possible
Fault Tolerant - avoiding going down at all

Glacier access policies cannot be changed once set. You say "don't delete for 7 years" and even you can't delete it for 7 years. o

### Security
**Monitoring**
* CloudTrail
	Enable in all regions, even if you don't have anything in them so that you can be aware of what's happening if something does. 
	DataDog allows you to take the regional outputs of CloudTrail and put them in a single dashboard
* CloudWatch
* AWS Config
* Enable S3 server logs
* Triggering Responses
	- Cloudwatch, SNS, Lambda
	* e.g. set alarm for ssh login failure, subscribe SNS to this alert and create a topic, Lambda listener for topic and hook notifications into topic

## Module 10: Well-Architected Pillar 1: Security
Have security measures at every level and on every service for every user
Enable Traceability - log and audit all actions and changes to your environment and access to your services
Automate Resonses to Security Events - use event-driven or condition-drive alerts
Automate Security BEst Practices
	- Create and save a custome baseline image of a virtual server and the use that image automatically on each new server you launch
	- create an entire infrastructure that is defined and maanaged on a template
**See slide 510** for table of available services for Security w/in AWS

[Bees with Machine Guns](https://github.com/newsapps/beeswithmachineguns) - test your own application's ability to handle loads


DDoS - how to avoid
* front your application with AWS Services (rely on built-in security)
	- ELB Security group, WAF/prox ("Web Application Firewall" AWS service), private subnets
* minimize attack surface
* learn normal behavior
* create a plan for attacks

[AWS Best Practices for DDos Resiliency](https://d0.awsstatic.com/whitepapers/DDos_White_Paper_June2015.pdf)

Use Amazon Inspector to test your app for security vulns. Will give you recommendations too. 
Tests for vulns and deviations from best practices. Produces detailed report with prioritized steps for remediation after performing the assessment.

**Securing Data**
Encryption
by default, contnetn delivered via CloudFront is delivered via HTTPS
CloudFront custom SSL certificate support available
Use alternate domain names with HTTPS (slide 522)
Restrict access to objects in S3
Requre that users use signed URLs
	- create CloudFront key pairs for your trusted signers
Restict access to Amazon S3 content by creating origin access identity (OAI)
Use IAM to Control API Access to CloudFront Resources

**Encryption**
Starts at slide 529
Use AWS Key Management Service - managed solution 

For test: how do you encrypt data at rest, in motion, server side, client side, what encryption does AWS use?
See slides 538-onward for answers.

**Authentiation**
Even if you're using ActiveDirectory, you still have to assign roles to your users.
Uses Window Server 2012 currently

!! I really don't understand this stuff very well. Need to learn it, though. 

RE: DBs inside vs outside a VPC.
* If you're within one region, you can keep it in a VPC or out. Just cover it with the right security group.
* If you're spaning more than one region, you need to put it outside a VPC so the other region can get to it, and so that the read-only copy can be made and put in the other region. 

Amazon Workspaces - a desktop environment in the cloud. 
Images can be saved and installed/pushed-out to other users. 
Can be linked to IAM,e tc. 
Not deployed with a VPC, it's a totally managed service that is separate. It's just a virtual desktop environment. 
How is security handled for that? NOT SURE. Needs more research. 

Can be run in a browser
priced per month.

ROUTE 53 also provides health checks for resources (like ELB)

### AWS Kinesis
Managed service for streaming data

For animation rendering at night: reserved scheduled instances.

AWS Shield is new offering for mitigating DDoS.

DynamoDB is replicable across regions using the cross-regional library

Once you have an API Gateway, you can publish (automagically) an SDK that allows other developers to build off of it.

You should backup your Lambda functions. You can use version control for this. Code could be in S3 if you're not using the inline editor

Trusted Advisor is always good to use. But it's just one opinion and doesn't mean you have to do anything. 

Reminder that roles can be assumed by people and/or resources. 

RTO = Recovery Time Objective (Route 53 health checks)
RPO = Recovery Point Objective (could be done through DB?, for Dynamo, see the cross-regional library documentation)


## Module 15: Design Patterns and Sample Architectures
See book/wiki "AWS CLoud Design Patterns" [link](http://en.clouddesignpattern.org/index.php/Main_Page), written by AWS Japan

ENI - elastic network interface, useful for keeping MAC address, etc. 

Security guy - "security is hampered by stateful stuff on the network level." 

**Kinesis**
For real-time apps and streaming
* streams
* firehose
* analytics

DEMO: <tiny.cc/kademo>

**Amazon Elastic Transcoder**
Transcode all the things, with notifications of job statuso

Test Prep Courses are offered. 

