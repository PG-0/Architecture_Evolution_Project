# Architecture_Evolution_Project

This project is about the evolution of a monolithic application into an application that is both highly available and fault tolerant. 

# Phase 1 - Manual Creation of Wordpress blog
Manually create the monolithic app. This app will be running the application itself, the database, and storing the content of all the blog posts. 

Risks of a Monolithic App:
* Not fault tolerent or highly available
* Security concerns due to user needing to connect directly to the EC2 instance
* Data Loss if application fails

# Phase 2 - Automating Wordpress Blog Provisioning with the use of Launch Templates
Replicating the launch process using an AWS launch template. This will allow for automatic provisioning of the application. Drastically reducing provisioning time and decreases the risk of potential errors from user input

The launch template will be used at a later phase. 

Risks:
* See above Phase 1 risks. 

# Phase 3 - Database Migration
Migrating the MySQL Database off the EC2 instance and running it on a dedicated RDS instance. The data will exist outside of the application. This is the first step of moving toward an elastic architecture. 

In the event of an EC2/App failure, the data will persist. Thus increasing the availability of at least the data at  this stage

Risks:
* A
* B

# Phase 4 - Storing Content on EFS (Elastic File System)
EFS will be used to create a place to store content for the application. EFS is resilient network based shared file system. Thus increasing the elasticity in the application architecture. 

The architecture now has the components in place to be fully elastic based on load. 

Risks:
* A
* B


# Phase 5 - Adding an ALB (App Load Balancer) & an ASG (Auto Scaling Group)
Adding the ALB will abstract the users away from the infrastucture. This is where the users will connect to rather than directly into an EC2 instance. 

The ASG will allow for resource provisioning based on load (Scaling in or out). 

System is now fully resilient, self-healing, and fully elastically scalable

---
# Phase 6 [Optional] - Replacing RDS with Aurora
Optional due to Aurora having a $$$ associated with it. 

By replacing Aurora with RDS, you increase the availability of your data in the event of a database failure. Aurora is fully managed by AWS and has auto scaling capabilities. It is reliable and fault tolerant by design. Aurora is much more cloud focused but limited to PostgreSQL and MySQL. 

RDS does have some form of elasticity if you enable the multi-AZ feature. It will synchornously replicate to another availability zone. You could stand up read replicas to reduce load on the primary DB but without multi-AZ enabled, a failure would affect the application's ability to write to the database. 

---
# Architecture Evolution Images
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
## [Optional] Phase 6


---
# Guide 
## Pre Step:

1 Click Deployment Link: https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbkE3T3JidGRuNlJFNmVvNVBYY2VDTGhzdVo0QXxBQ3Jtc0trZXZqbmk4MlZmUTN6N3huWVRISkVsOFVRYWNLYnNhQWM1ajlQVmRxRHI0S3RNUmFIcmVVcGZwWmh3SVY5dm91UDZpV2xCelFadV9MYlZnMHh2RUkwZE16bXVtUGNUYlJqbjhoWXY5UHl3c3Q1MXU3Yw&q=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudformation%2Fhome%3Fregion%3Dus-east-1%23%2Fstacks%2Fquickcreate%3FtemplateURL%3Dhttps%3A%2F%2Flearn-cantrill-labs.s3.amazonaws.com%2Faws-elastic-wordpress-evolution%2FA4LVPC.yaml%26stackName%3DA4LVPC&v=G40apgj66Cw

The one click deployment will setup the base infrastructure for this project. It will provision:
* Internet Gateway
* VPC
* CW Agent Config
* Roles for Wordpress and IPv6 

Time to provision this infrastructure is ~ 5 mins. 

