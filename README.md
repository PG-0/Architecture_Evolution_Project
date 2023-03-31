# Architecture Evolution Project - Monolithic App Architecture to an Elastic & Scalable Architecture for a Wordpress App

This project is about the evolution of a monolithic application into an application that is both highly available, self-healing, and fault tolerant. 

Monolithic applications can be a good choice for small projects or applications with limited functionality. They are not suited for large applications that require scalability and reliability. This is solved by decoupling the underlying architecture and integrating the pieces together. 


Challenges & Risks of a Monolithic App:
1. The app and db are built manually which is time consuming and not automated
2. The db and app being on the same instance means neither can scale without the other
3. The app media/content is stored locally on the instance. Scaling in or out will risk the media 
4. User connects directly into instance via the IP. This poses a security risk 
* The IP address of the instance changes when it is stopped or terminated
5. Lack of load balancing, health checking and auto-healing

# Phase 1 - Manual Creation of Wordpress blog
Manually create the monolithic app. This app will be running the application itself, the database, and storing the content of all the blog posts. 

General steps perfomed: 
1. Setup the environment from 1-click deployment
2. Configure SSM Parameters
3. Perform manual install of the database and Wordpress

Challenges & Risks addressed of the Monolithic App:
* None. 


# Phase 2 - Automating Wordpress Blog Provisioning with the use of Launch Templates
Replicating the launch process using an AWS launch template. This will allow for automatic provisioning of the application. Drastically reducing provisioning time and decreases the risk of potential errors from user input

The launch template will be used at a later phase. 

General steps performed:
1. Create Launch Template

Challenges & Risks of a Monolithic App:
1. [Fixed/Addressed] The app and db are built manually which is time consuming and not automated
2. The db and app being on the same instance means neither can scale without the other
3. The app media/content is stored locally on the instance. Scaling in or out will risk the media 
4. User connects directly into instance via the IP. This poses a security risk 
* The IP address of the instance changes when it is stopped or terminated
5. Lack of load balancing, health checking and auto-healing


# Phase 3 - Database Migration
Migrating the MySQL Database off the EC2 instance and running it on a dedicated RDS instance. The data will exist outside of the application. This is the first step of moving toward an elastic architecture. 

In the event of an EC2/App failure, the data will persist. Thus increasing the availability of at least the data at  this stage

General steps Performed:
1. Create RDS instance
2. Take a backup and migrate Wordpress data from EC2 to RDS
3. Update Launch Template 

Challenges & Risks of a Monolithic App:
1. [Fixed/Addressed] The app and db are built manually which is time consuming and not automated
2. [Fixed/Addressed] The db and app being on the same instance means neither can scale without the other
3. The app media/content is stored locally on the instance. Scaling in or out will risk the media 
4. User connects directly into instance via the IP. This poses a security risk 
* The IP address of the instance changes when it is stopped or terminated
5. Lack of load balancing, health checking and auto-healing

# Phase 4 - Storing Content on EFS (Elastic File System)
EFS will be used to create a place to store content for the application. EFS is resilient network based shared file system. Thus increasing the elasticity in the application architecture. The content/media will persist even in the event of an EC2 failure. 

The architecture now has the components in place to be fully elastic based on load.

General steps Performed:
1. Create EFS File System
2. Install and mount EFS onto EC2
3. Update Launch Template 

Challenges & Risks of a Monolithic App:
1. [Fixed/Addressed] The app and db are built manually which is time consuming and not automated
2. [Fixed/Addressed] The db and app being on the same instance means neither can scale without the other
3. [Fixed/Addressed] The app media/content is stored locally on the instance. Scaling in or out will risk the media 
4. User connects directly into instance via the IP. This poses a security risk 
* The IP address of the instance changes when it is stopped or terminated
5. Lack of load balancing, health checking and auto-healing


# Phase 5 - Adding an ALB (App Load Balancer) & an ASG (Auto Scaling Group)
Adding the ALB will abstract the users away from the infrastucture. This is where the users will connect to rather than directly into an EC2 instance. The connection will happen directly onto the DNS of the ALB

The ASG will allow for resource provisioning based on load (Scaling in or out). 

System is now fully resilient, self-healing, and fully elastically scalable

General steps Performed:
1. Create ALB
2. Update Launch Template 
3. Create ASG (Capacity: Desired = 1, Min = 1, Max = 3)
4. Integrate ASG and ALB 
5. Add Simple Scaling:
* Scale out when CPU utilization > 40%
* Scale in when CPU utilizaiton < 40%

Challenges & Risks of a Monolithic App:
1. [Fixed/Addressed] The app and db are built manually which is time consuming and not automated
2. [Fixed/Addressed] The db and app being on the same instance means neither can scale without the other
3. [Fixed/Addressed] The app media/content is stored locally on the instance. Scaling in or out will risk the media 
4. [Fixed/Addressed] User connects directly into instance via the IP. This poses a security risk 
* The IP address of the instance changes when it is stopped or terminated
5. [Fixed/Addressed] Lack of load balancing, health checking and auto-healing

---
# Phase 6 [Optional] - Replacing RDS with Aurora 
Optional due to Aurora having a $$$ associated with it. 

By replacing Aurora with RDS, you increase the availability of your data in the event of a database failure. Aurora is fully managed by AWS and has auto scaling capabilities. It is reliable and fault tolerant by design. Aurora is much more cloud focused but limited to PostgreSQL and MySQL. 

RDS does have some form of elasticity if you enable the multi-AZ feature. It will synchornously replicate to another availability zone. You could stand up read replicas to reduce load on the primary DB but without multi-AZ enabled, a failure would affect the application's ability to write to the database. 

---
# Architecture Evolution Diagrams
## Phase 1
[Arch Diagram]
## Phase 2
[Arch Diagram]
## Phase 3
[Arch Diagram]
## Phase 4
[Arch Diagram]
## Phase 5
[Arch Diagram]

---
# Credits
Big shoutout to Adrian Cantrill for putting this lab together and teaching me about how to evolve and architecture. 