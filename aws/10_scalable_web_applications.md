# Scalable Web Applications
## Traditional web hosting
- web tier
- app server tier
- data tier

scheme:

                (o) 
                 |
                 V
                 O load balancer
                 |
             +---+---+
             |       |
             V       V
            .-.     .-.
            | |     | | web servers
            '-'     '-'
             |       |
             +---+---+
                 |
                 V
                 O load ballancer
                 |
             +---+---+     
             |   |   |
             V   V   V
            .-. .-. .-.
            | | | | | | app servers
            '-' '-' '-'
             |   |   |
             +---+---+
                 |
                 V
                 -       -
             db |_|<--->|_| slave db

### Challenges
- requires acurate forecasts
- sudden traffic peaks
- low utilization rate of expensive hardware
- high maintenance cost for idle hardware
- idle hardware - parallel fleets for
    - prod
    - pre prod
    - testing
    - staging
    - ...

## AWS Hosting
- scalable
- cost effective
- elastic capacity / capacity on demand
- on demand provisioning means no idle hardware
- new hosts can be launched / stoppped in minutes - responds well to traffic peaks
- components
    - content delivery
    - managing public dns
    - host security
    - load ballancing

scheme:

                    (o)
                     |
                     V
                     o Route53 DNS
                     |
                     V
                     O Elastic Load Ballancer
                     |
                     +----------------------------------+----------------+
                     |                                  |                |
                     V                                  V                |
            +------------------------------------+    +------+           |
            |A      .---.                        |    |A     |           |
            |       |   | Autoscaling group      |  +-|      |           |
            |       '---' Web Server Instances   |  | +------+           |
            |         |                          |  | availability       |
            |         V                          |  | zone               |
            |         O Elastic Load Ballancer   |  | RDS is a read      |
            |         |                          |  |   only replica     |
            |         V                          |  |   of master        |
            |       .---.                        |  |                    |
            |       |   | Autoscaling group      |  |                    |
            |       '---' App Server Instances   |  |                    |
            |         |                          |  |                    |
            |     +---+---+                      |  |                    |
            |     |       |                      |  |                    |
            |     V       V                      |  |                    |
            |    .-.     .-.                     |  |                    |
            |    | |EC   | |RDS master           |  |                    |
            |    '-'     '-'                     |  |                    |
            +-------------|----------------------+  |                    |
            availability  |                         |                    |
            zone          +-------------------------+                    |
                          |                                              |
                          V                                              |
                         ___                                             |
                         \_/ S3                                          |
                          |                                              |
                          +----------------------------------------------+
                          |                                              
                          V
                          _ .
                        (  _ )_  Amazon Cloud Front
                      (_  _(_ ,) edge caching

### Notes
- EC: ElastiCache removes load from db
- multiple availability zones within a region
- S3: backup and static content

### Amazon Cloud Front
- content delivery
- requests are routed to nearest edge location
- optimized to work seamlessly with other Amazon services: S3, EC2
- also works well with non-amazon origin servers

### managing dns routing: Route53
- queries for domains are routed to nearest dns server
- resolves domain requests to Elastic Load Ballancer, zone apex record

### Host Security
- security groups: network security at host level (by EC2 instances) by network traffic filtering
- each EC2 instance can have multiple security groups assigned
- can limit access to EC2 instances to specific subnets, ip ranges

example:

                             EC2 Security group
                             Firewall
                                     |
                +------------+       |
                |            |       |     Allow 80,443
             +--| Web Server | <---------- from internet
             |  |            |       |
             |  +------------+       |
             |                       |
             |  +------------+       |
             +->|            |       |     Allow 22 (ssh)
                | App Server | <---------- to developers in
             +--|            |       |     corporate net
             |  +------------+       |
             |                       |
             |  +------------+       |
             |  |            |       |
             +->| Db Server  |       | x-- No outside access
                |            |       |
                +------------+       |
                      |
                      O
                     |_| Db

### Load Ballancing
- Elastic load ballancers
- health checks on hosts
- distribution of traffic accross multiple availability zones
- dynamically removes / adds EC2 hosts from / to load balancing rotation
- load ballancing can also grow / shrink to match traffic, while providing a predictable entry point by using a persistent cname
- for advanced load ballancing, run software based load ballancers on EC2 instances; assign elastic IP addresses - requires minimal dns change

### Auto scaling
- traditional hosting
    - traffic forcasting used to provision hardware ahead of time
- Aws hosting
    - instance creation is triggered on demand
    - Auto scaling can create capacity groups - grow / shrink on demand
    - stateless design: allows on demand scaling
- Autoscaling works with metrics from Amazon Cloud Watch
- Each layer (web, app, db, ...) can scale independently with multiple autoscaling groups
- Autoscaling up or down is either triggered automatically
- or manually via Api

### Persistence
- either use AWS infrastructure
- or host own db service on EC2 instances

| type | relational db | NoSql |
| --- | --- | --- |
| managed db service | RDS: mySql, Oracle, MS | Dynamo Db |
| self-managed | host any relational db on EC2 | host any NoSql db on EC2 |

#### RDS
- hosted db service
- multiple engines to choose from
    - MySql
    - Oracle
    - MS
- existing tools can be used out of the box with RDS
- Automatically patches db software for
    - backups
    - resores
    - backup with user defined retention policy
- point in time recovery
- multi-availability zone deployment with read only replicas

#### Self managed db
- run on EC2 instances
- together with Elastic Block Storage (EBS) data remains available even if host fails
- EBS snapshots
- fault tolerant

#### Dynamo Db
- handles burden of operation
- scale capacity without downtime or performance loss

### Data Storage
- static resources: S3
- dynamic content: deploy on EC2 instance running app server
- user data: RDS
- session data: Dynamo Db
- db files: EBS volumes
- shared storage: distributed network of file systems

### Failover
With multiple availability zones
- redundant deployment locations
- insulated from each other
- low latency connection to other availability zones in the same region