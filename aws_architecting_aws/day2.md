See notes from day 1 for all links. 

## Module 5: Making Your Environment Highly Available
_Event Driven Scaling and Serverless Architectures_

vertical scaling = give me a bigger/smaller box
horizontal scaling = give me more/less boxes

Use CloudWatch to handle auto-scaling. It monitors traffic and sends notifications and triggers auto scaling.

**Auto-Scaling**
Regional in scope. No cross-regional settings. But does work across AZs. 
You give it an AMI in the configuration from which to launch EC2 instances
Once up, it goes to the AutoScaling group and then to the VPC

Best Practices = Scale out early, scale in slowly

If using Docker images in Elastic Containers, AutoScaling will only take care of the EC2 instances. To scale the containers, use Lambda or ECS Services. 

**EC2 Auto Recovery**
Replace impaired Amazon E2 instances automatically.
* maintains same instance ID/metadata, IP addresses
Cannot use in-memory data from impaired instance (it is lost)
* Not available for every instance type. 
	- supported: C3, C4, M3, M4, R2, T2

You can do sharding in RDS but would need to do with some other software running in another EC2 instance.

You cannot deploy RDS across regions fully. You can create read-only copies across regions.
YOu can deploy DynamoDB across regions with a library. 

**AWS Lambda and Event-Driven Scaling**
1 million calls per month for free
Fully-managed compute servce that runs stateless code (Node, Java, Python, C# - but you can add others if you set it up)
Max run-time of 5 minutes. 
Manages OS and language updates
Lambda is per region and is available at the edge via Greengrass. 

Lambda is not necessary for auto-scaling to work, but you can use it to trigger auto-scaling events. 


## Module 6: Automating your Infrastructure
Best Practice: 
* Automate your environment - where possible, autoomate the provisioning, termination, and configuration or resources. 
* Use disposable resources - take advantage of the dynamically provisioned nature of cloud computing. Thiink of servers and other components as temporary resources. 

Example - use templates for Auto Scaling groups
Template docs are sent to CloudFormation engine in JSON or YAML format. Treat it like source code by checking it in your repository. 
You can get price quotes from templates. 
Stacks are a collection of resources created by CloudFormation. Stacks are tracked and reviewable in the AWS management console.

[VisualOps](https://ide.visualops.io) - import your JSON or YAML tempate into the IDE and see a visual representation. Can also do it from scratch and output the JSON.

[cloudcraft](https://cloudcraft.co) - similar to VisualOps. Can also give yoour CC account a role for AWS Console and it will visualize your setup and you can interact with the visual design too. 

AWS has one too, in the console. 

These labs are built with CloudFormation

Templates allow you to provide parameters, values that are filled in when you create a new stack. 
This is useful if you want to narrow the possibilities of what a user can create.

Recommended that you use Templates for your different layers/tiers:
* front-end services - (Consumer website, seller website, mobile back end)
* back-end services (Search, payments, reviews, recommendations)
* shared services (alarms, subnets, DBs)
* base network (VPCs, IGWs, VPNs, NATSs)
* Identity (IAM policies, Users, Groups, and Roles)

!! AMI ID#s are region specific. 
so are key pairs, security group names, subnet IDs, EBS Snapshot IDs. 

May need to tell template to wait until a certain resource has been created and checked before creating a new resource. THis is called a 'wait condition.'

**AWS Elastic Beanstalk**
Automated deployment and scaling service for web applications

Java, .NET, PHP, Node, Python, Ruby, Go, or Docker
Deploys on Apache, Nginx, Passenger, and IIS servers

Handles:
* load balancing
* health monitoring
* autoscaling
* application platform management
* code deployment

Creates security groups, auto-scaling policies, alarms...

It doesn't do anything differently - it just simplifies the setup process for you. Many people use thisin production and massage the variables according to their needs. 

"Blue-Green" Deployment assures that updates do not interrupt current users.
See slides 364-366

AWS OpsWorks - configuration management service that help you configure and operate applications of all shapes and sizes. 
* package installation
* software configuration
* resources (compute, storage, etc.)
See slides 366 

Convenience --> Control
Beanstalk -> OpsWorks > CloudFormation > EC2
You can use these in combination if you want. 

## MODULE 7: Decoupling your Infrastructure
Design SErvices, Not SErvers

**Service-Oriented Architecture**
An architectural approach in which application components provide services to other components via a communications protocol. 
SErvices are self-contained units of functionality.

Mircroservices - small, independent processes within an SOA. Each process is focused on doing one small task. Process communicate to each other using language-agnostic APIs.

Context - collection of related Microserves

?? How to reliably communicate between components? --> **Amazon Simple Queue Service (SQS)**

[ACloudGuru](https://acloud.guru/)

Going serverless
- if you need it on all the time churning out stuff, then EC2
- if it's on demand in/out, then def. Lambda

Lambda also handles load balancing. Totally feasible, even recommended, to do an entire webapp with Lambda and no server. See slide #419

Lambda events can now be chained, so the output of one can be sent as the input to another.

See streaming example on slide #423
Note: CloudFront takes care of streaming! Possible use with Kinograph?

## MODULE 8: Designing Web-Scale Storage
How should web-accessible content be stored?
1. Store static assets in Amazon S3
	- S3 eliminates the need to worry about network loads and data capacity on your web servers. 
	- CloudFront CDN can be leveraged as a global caching layer in front of Amazon S3 to accelerate content to your end users.
	- Performance depends on proximity to your users and to other services. Choose your region accordingly.
	- There is no limit to # of objects within a bucket. Dropbox uses only 2 buckets, total. 
	- See hashing best practice for bucket naming on slides 435-436
2. Serve frequently accessed assets from Amazon CloudFront
	- Use caching to minimize redundant data retrieval operations.
	- reduces costs since S3 is billed when taking data out.
	- CloudFront is also used as a CDN, caches content around the world.
	- keep-alive connections, SSl, POST/PUT optimazations, latency-based routing, and more...
	- Independent of Region
	- used for:
		* static websites
		* video and rich media
		* online gaming
		* interactive agencies
		* software downloads
3. Store non-relational data in a NoSQL database such as Amazon DynamoDB

How to choose between SQL and NoSQL? See slide 449-469

The managed DB services (RDS and DynamoDB) don't require an EC2 instance to work. That means you can build an entire web app with just S3 and a database if it's static. 

Amazon Aurora = MySQL 5.6 but managed like Dynamo. 
