# Amazon Compute Services
- launch & connect to an EC2 instances
    - EC2: web service that provides resizabe compute capacity in the cloud
    - Boot new instances in minutes
    - quickly scale capacity
- attach autoscaling group to EC2
    - scale EC2 capacity according to user defined conditions
    - well suited for hourly / daily / weekly variability in usage
- Elastic Map Reduce
    - hosted Hadoop framework
    - instantly provision arbitrary capacity

## AMI: Amazon Machine Images
- EC2 instances are created from AMI
- template for root volume
- launch permissions
- block device mapping - which volumes to attach when launching
- to create an instance: launch a public AMI & customize

## Steps to use EC2
EC2 management console > launch instance > wizard

1. select preconfigured AMI or create AMI with applications, libs, data,...
2. configure network access & security on EC2 instance
3. choose instance type: micro (free)< small < medium < large
    - cpu
    - memory
    - local storage
4. determine whether to run instances 
    - at multiple locations
    - with static IPs
    - with attached persistent storage
5. advanced settings
    - encryption keys
    - firawall settings (with security groups)

## Autoscaling
- enabled by cloud watch
- no additional fees besides regular cloud watch fees
- number of EC2 instances by demand

## EMR: Elastic Map Reduce
- uses Apache Hadoop framework
- EC2 -> EMR -> Cloud Watch, S3
