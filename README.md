# Exam Tips AWS Certified Developer Associate
Author: Yvo van Zee
Source: ACloud.guru 

## AWS Concepts and Components:
- AWS Global Infrastructure
- Compute
- Storage
- Databases
- Migration
- Networking & Content Delivery
- Developer Tools
- Management Tools
- Media services
- Machine Learning
- Analytics
- Security & Identity & Compliance
- Mobile services
- Agumented Reality/Virtual Reality
- Application Integration
- Customer Engagement
- Business Productivity 
- Desktop & App streaming
- Internet of Things
- Game Development

## What is needed for the exam according to A Cloud Guru:
- AWS Global Infrastructure
- [Compute](#compute)
- Storage
- Databases
- Networking & Content Delivery
- Management Tools
- Analytics
- [Security & Identity & Compliance](#security)
- Application Integration

---

## Security
### IAM
Identity and Access Management (IAM) is universal, it does not apply to regions at this time.
The "root account" is simply the account created when first setup your AWS accont. It has complete Admin access.
New users have _NO_ permissions when first created. The new users are assigned an Access Key ID & Secret Access Keys when first created. These are not the same as a password, and you cannot use the Access Key ID & Secret Access Key to login in to the consile. You can use this to access AWS via the API's and Command Line however. You only get to view these keys once. If you lose them, you have to regenerate them.
Also always set Multifactor Authentication on your Root account. It is also possible to create own password rotation policies. 

- Centralised Control of your AWS account
- Shared Access to your AWS account
- Granular Permissions
- Identity Federation
- Multifactor Authentication
- Provide Temporary access for users/devices and services where necessary
- Allows you to set up your own password rotation policy
- Integrates with may different AWS services
- Supports PCI DSS Compliance

#### Critical terms IAM
Users = End Users (think people)
Groups = A collection of users under one set of Permissions
Roles = You create roles and can then assign them to AWS resources
Policies = A document that defines one or more Permissions

### Security Token Service (STS)
Grants users limited and temporary access to AWS resources. Users can come from three sources:
- Federation (typically Active Directory)
	- Users Security Assertion Markup Language (SAML)
	- Grants temporary access based off the users Active Directory credentials/
	  Does not need to be a user in IAM
	- Single sign on allows users to log in to AWS console without assigning IAM credentials
- Federation with Mobile apps
	- Use FB/Amazon/Google or other OpenID providers to log in.
- Cross Account Access
	- Lets users from one AWS account access resources in another.

##### Federation:
Combining or joining a list of users in one domain (such as IAM) with a list of users in another domain (such as Active Directory or FB)
##### Identity Broker: 
a service that allows you to take an identity from point A and join it (federate it) to point B
##### Identity Store:
Services like Active Directory, Facebook, Google
##### Identities:
a user of a service like AD or FB 

Remember the following steps how identity communication works:
1. Develop an Identity Broker to communicate with LDAP and AWS STS
2. Identity Broker always authenticates with LDAP first, then with AWS STS
3. Application then gets temporary access to AWS resources (assumerole)

---
## Compute
### EC2
Read EC2 FAQ before Exams

Differences between:
- On Demand
- Spot
- Reserved
- Dedicated Hosts

Remember with Spot instances
- if you terminate the instance, you pay for the hour
- if AWS terminates the spot instance, you get the hour it was terminated for free.

Exam Tips EBS
Ebs consist of:
- SSD, General Purpose - GP2 up to 10k iops
- SSD, Provisioned IOPS, I01, more then 10k iops
- HDD, througput optimized, ST1, frequently accessed workloads, no boot volume
- HDD, Cold, SC1, less frequently accessed data, no boot volume
- HDD, Magnetic, Standard, cheep, infrequently accessed storage

You cannot mount 1 EBS volume to multiple EC2 instances, instead use EFS for shared storage service

EC2 Instance Types
How to remember them:
	- D for Density
	- R for Ram
	- M Main choise for general purpuse apps
	- C for Compute
	- G for Graphics
	- I for iops
	- F for FPGA
	- T cheap general purpose, think of T2 Micro
	- P for graphics (think pics)
	- X for Extreme Memory

EC2
Termination protection is turned off by default, you must turn it on
Firewall Nacl vs Security Groups:
	- Security Groups are stateful, so when port 80 is opened, traffic is allowed back as well to the client, even when an outbound rule is nog configured
	- Nacl are stateless, So you always need to configure inbound and outbound rules
	- Nacl's you can block traffic, Security groups, everything is blocked by default, you can only grant access

EBS volume and EC2 instance are always in the same Availability Zone!
Snapshots are stored on S3, you can restore to another AZ or Region

EBS vs Instance Store
Instance store is temporary storage. You can't poweroff or stop an instance on instance store. If the underlying hypervisor failes, you will lose data. It is also called Ephemeral Storage.
Ebs can be stopped.
You can reboot both systems, you will not lose data.
By default both root volumes will be deleted on termination, however with EBS volumes you can tell AWS to keep the root device volume.

Application loadbalancer is a level 7 loadbalancer (application layer)
Classic loadbalancer is a level 4 loadbalancer (transport layer)

Cloudwatch
	- standard monitoring = 5 min, detailed monitoring = 1 min (cost extra)
	- Dashboards, see what is happening
	- Alarms, set alarms to get notified when thresholds are hit
	- Events, respond to state change within your AWS resources
	- Logs, aggregate, monitor and store logs
	- Difference between CloudWatch and CloudTrail
		- Cloudtrail is auditing
		- Cloudwatch is performance monitoring

Instance Meta-data
Used to get information about aan instance, such as public ip
curl http://169.254.169.254/latest/meta-data/

Placement Groups
	Logical grouping of instances within a single AZ. Mostly used for high performance compute.

EFS
Elastic file system, file storage service for Amazon Elastic Compute Cloud (ec2)
EBS can only be mounted to 1 instance, where EFS can be mounted to multiple instances
Supports NFSv4, and has read after write consistency

Lambda
Lambda scales out, not up, automatically
Lambda functions are independent, 1 event is 1 function
Lambda is serverless
Know what services are serverless.
Lambda functions can trigger other lambda functions.

DNS Examtips
Route 53
Always 2 standard Records sets: NS and SOA(start of authority)
Routing Policies:
	- Simple routing policy – Use for a single resource that performs a given function for your domain, for example, a web server that serves content for the example.com website.
	- Failover routing policy – Use when you want to configure active-passive failover.
	- Geolocation routing policy – Use when you want to route traffic based on the location of your users.
	- *Geoproximity routing policy – Use when you want to route traffic based on the location of your resources and, optionally, shift traffic from one resources in one location to resources in another.
	- Latency routing policy – Use when you have resources in multiple locations and you want to route traffic to the resource that provides the best latency.
	-	*Multivalue answer routing policy – Use when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.
	- Weighted routing policy – Use to route traffic to multiple resources in proportions that you specify.
ELB's never have an ipv4 address, you resolve them using DNS name
Diffenece between Alias Record and a CNAME, Alias can resolve AWS instances such as elb

Database
Sql server, Oracle, Mysql Server, Postgresql, aurora and MariaDB
RDS- Online transaction processing

VPC
Logical Datacenter within AWS
consist of IGWs (or Virtual Private Gateways), Route tables, NACLs, subnets and security groups.
1 subnet = 1 Availability Zone
Security Groups are Statefull, Network Access Control Lists (NACLs) are stateless.
No transitive peering

When creating an VPC, automatically AWS will make a NACL, Security Group and Routing Table

Exam tips NAT instance
- when creating a NAT instance, Disable Source/Destination check on  the instance
- NAT instances must be in a public subnet
- There must be a route out of the private subnet to the NAT instance, in order for this to work (updates etc)
- The amount of traffic that NAT instances can support depends on the instance size, if you are bottlenecking, increase the instance size
- You can create high Availability using autoscaling groups, multiple subnets in different AZs, and a script to automate failover.
- Behind a security groups

Exam tips NAT Gateways
- Preffered by the enterprise
- Scale automatically up to 10Gbps
- No need to patch
- Not associated with security Groups
- Automatically assigned a public ip address
- Remember to update your route tables
- No need to disable Source/Destination checks
- More secure than a NAT instance

Exam tips Network ACLs
- Your VPC automatically comes a default network ACL, and by default it allows all outbound and inbound traffic
- You can create custom network ACL's. By default, each custom network ACL denies all inbound and outbound traffic until you add rules
- Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet wit a network ACL, the subnet is automatically associated with the default network ACL
- You can associate a network ACL with multiple subnets; however, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed
- Network ACLs contain a numbered list of rules that is evaluated in order, starting with the lowest numbered line
- Network ACLs have separate inbound and outbound rules, and each rule can either allow or deny traffic
- Network ACLs are stateless; responses to allowed inbound traffic are subject to the rules for outbound traffic(and vica versa)
- with NACLs you can block ip addresses, with security groups you can not.

Application Loadbalancers
- You will need at least 2 public subnets in order to deploy an application loadbalancer

VPC Flowlogs
- you cannot enable flow logs for VPC's that are peered with your VPC unless the peer VPC is in your account
- You cannot tag a flow logs
- After you have created a flow log, you cannot change its configuration; for example, you can't associate a different IAM role with the flow log

Not all IP traffic is monitored
- Traffic generated by instances when they contact the Amazon DNS server. If you use your own DNS server, then all traffic to that DNS server is logged
- Traffic generated by a windows instance for Amazon windows license activation
- traffic to and from 169.254.169.254 for instance metadata
- DHCP traffic
- Traffic to the reserved ip addresses for the default VPC router

SQS - Simple queue service
- SQS is pull based
- messages are 256KB in size
- Messages can be kept in the queue from 1 minute to 14 days, default is 4 days.
- SQS guarentees that your message will be processed at least once
SQS long polling is a way to retrieve messages from your Amazon SQS queues, without constantly polling, but only polling when a message is in the queue.
Visabilty time out is the amount of time a message is invisibel for the SQS queue, after it has been picked up. When the job is processed before visibility time out expires, the message will be deleted from the queue, otherwise it will be processed again.
fifo queue vs standard queue

SWF - Simple Workflow service
Workers are programs that interact with amazon swf to get tasks, process received tasks and return results.
Decider is a program that controls the coordination of tasks, i.e. heir ordering, concurrency and scheduling according the application logic.

Main difference between SQS an SWF is that SWF only assign a tasks once. SQS can duplicate or a tasks can be executed more than once.

Elastic Transcoder
Media converter.

API Gateway
Remember what API gateway is at a high level
API Gateway has caching capabilities to increase performance
API Gateway is low cost and scale automatically
You can trottle API gateway to prevent attacks
You can log results to CloudWatch
if you are using javascript/ajax that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway
