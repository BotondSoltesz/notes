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