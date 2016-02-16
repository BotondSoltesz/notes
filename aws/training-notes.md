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

### Direct Connect
- dedicated network infrastructure

### VPC
- Virtual Private Network

## Database
### RDS
- web service
- set up, operate, scale relational db in the cloud

### Dynamo DB
- fully managed NoSQL db service

### ElastiCache
- web servcie
- in memory cache in the cloud

### RedShift
- data warehousing - reliable db service

# Application Services
- Content delivery
- Networking
- Search
- Distributed computing
- Libraries and SDK

## Products
- Amazon Cloud Search
- Amazon Simple Workflow Service
- Amazon Simple Query Service
- Amazon Simple Notification Service
- Amazon Simple Email Service

# Deployment Management Services
- web interface
- deployment and administration
- identity & access
- monitoring

## Products
- IAM: Identity and Account Management
- Amazon Cloud Watch
- Amazon Elastic Beanstalk
- Amazon Cloud Formation

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
