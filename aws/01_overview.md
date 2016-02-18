# Aws Platform
Foundation Services:
- Compute
- Storage
- Nertwork
- Database

## Compute
### EC2
- scalable: pay as you go
- autoscale with user defined conditions
- elastic load ballancing

### EMR
- Elastic Map Reduce
- hosted HADOOP framework
- tightly integrated with EC2 and S3

## Storage
### S3
- fully redundant
- retrive any amount at any time from anywhere
- high volume storage
- natively online, http access
- objects are stored in *buckets*
- objects are retrieved via user defined keys

### Glacier
- low cost archive / backup
- secure
- durable
- optimized for infrequent access

### EBS
- Elastic Block Store
- block level storage
- for use with EC2 instances

### Aws Storage Gateway
- Connects on-premise hardware with Amazon cloud
- Gateway cached volume: local cache, data in the cloud
- Gateway stored volume: all data stored locally, backed up in S3
- Mirror: on-premise data is mirronred on EC2 instances

### Aws Import / Export
- Mail in data storage media
- have it uploaded through Amazon's high speed network

## Network
### Route 53
- highly available, scalable DNS web service
- manage IP addresses listed for own domain names
- translate domain names - IPs

### Direct Connect
- dedicated network connection from enterprise infrastructure to AWS

### VPC
- Amazon Virtual Private Cloud
- isolated part of the cloud with private virtual network
- launch aws resources in the user defined network
- allows complete control over own virtual network environment
    - IP range
    - subnets
    - routing tables
    - gateways

### ELB
- Elastic Load Ballancing
- Http, Https, Tcp traffic routing to EC2 instances
- health checks; detect/remove failing instances
- dynamically grow/shrink resources based on traffic
- seamlessly integrates with autoscaling
- single CName (entry point for DNS config)

## Database
### RDS
- web service
- set up, operate, scale relational db in the cloud
- cost efficient
- resizable cpapcity
- engines
    - MySql
    - Oracle
    - MS
- automatic backup with user defined retention

### Dynamo DB
- fully managed NoSQL db service
- removes burden of operation from customer
- automatically replicate over 3 availability zones
- fast & predictable; data stored on ssd-s

### ElastiCache
- web servcie
- in memory cache in the cloud
- useful for improving webapp performance
- seamlessly caches in front of RDS

### RedShift
- fully managed petabyte scale data warehousing - reliable db service
- reduced IO
- parallelized query, load, backup, restore, resize

# Application Services
## Amazon Cloud Search
- Fully managed search service in teh cloud
- allows users to integrate fast/scalable search into their applications

## SWS: Simple Workflow Service
- coordinate processing steps
- manage distributed execution state

## SQS: Simple Queue Service
- hosted message queue

## SNS: Simple Notification Service
- notifications from the cloud

## SES: Simple Email Service
- bulk & transactional email sending from the cloud

# Deployment Management Services
## IAM: Identity and Account Management
- AWS identity and Access management
- securely control access to resources & services
- create and manage users
- grant access to AWS resources for users managed outside of AWS in corporate directory

## Amazon Cloud Watch
- monitor cloud resources starting with EC2

## Amazon Elastic Beanstalk
- quickly deploy & manage apps in the cloud
- upload app - automatiocally handle:
    - deployment details
    - provisioning
    - load ballancing
    - auto scaling
    - app health monitoring

## Amazon Cloud Formation
- Service that allows the creation of collections of AWS resources and their provisioning

# Interacting with the AWS platform
- account setup: username + password + payment method, verify & confirm
- access via
    - AWS management console web interface
    - Command Line Interface
    - SDK
    - IDE integration (VS, Eclipse)
- Use CLI when service is not available via web console

# Global Infrastructure
## Regions
- 9 regions in total
- independent collection of Aws resources in a geography
- location-dependent requirements

## Availability zones
- Within each Region there are multiple Availability Zones
- automatically store data on multiple devoces accross multiple facilities within a region

## Edge locations
- multiple edge locations within a single availability zone
- a complete content delivery network
- requests are automatically routed to the nearest edge location
