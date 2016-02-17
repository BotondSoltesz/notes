# AWS Network Services
- Route53
- Amazon Private Cloud
- AWS Direct Connect

## Route53
- scalable DNS service
- pay for no. of queries answered

## VPC
- virtual private network: private, isolated part of AWS cloud
- specify ip range with public and private subnets
- network control access list - control in / out access
- store data in S3, set permissions so that it is accessible only from within the VPC
- attach elastic IP address to make it accessible from the internet
- bridge VPC and on-site infrastructure with VPN connections

## AWS Direct Connect
- create virtual interfaces directly to AWS bypassing ISP-s
- provides access to the **region** it's associated with
- establish connection with AWS Direct Connect locations in multiple regions
- a connection in one region does not provide connectivity to other regions