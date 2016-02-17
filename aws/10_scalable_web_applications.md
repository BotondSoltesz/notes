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
                  



- EC: ElastiCache removes load from db
- multiple availability zones within a region
- Amazon Cloud Front: content delivery
- S3: backup and static content