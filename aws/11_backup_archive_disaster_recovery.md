# Backup - Archive - Disaster recovery
## traditional approach
- agent based
- entire content is backed up over LAN or SAN
- complex to manage
- resource intensive to operate
- requires extra effprt to avoid duplicates

## aws 
- lightweight in comparison
- backup: making copies of data to restore originals after data loss or recover data from an earlier point in time
- archiving: moving data that is no longer used actively for long term retention; archives are indexed to enable searching, restoring of files or parts of files
- disaster recovery: process + policies + procedures for recovery and continuation of tech infrastructure after a dsiaster

## AWS for Backup and Arhive
### traditional approach
- data to tape
- transfer tapes off-site
- store in tape museums
- long recovery times (days)
- low durability
- expensive

### aws
- S3 + Glacier
- 99.999999999% durability
- S3: available instanty
- Glacier: available in hours

### Lifecycle management
- real time access for how long
- retain for how long
- evacuate after what time

### Data transfer to Aws Network
- over the internet to S3 and Galcier
- Aws Direct Connect
    - dedicated connection to region
    - low latency
    - privacy
- AWS Import / Export
    - ship physical media to / from Amazon
    - for large data sets
        - faster than over the internet
        - cheaper than AWS Direct Connect
- AWS Storage Gateway
    - gateway cached: minimal infrastructure, only cache is local
    - gatway stored: data stored locally, backed up to the cloud
- EC2 AMI copy - copy images to other regions
- Elastic Block Storage (EBS) - snapshots backed up to S3, used by EC2 instances

### Data replication
- consider
    - distance between sites
    - bandwidth
    - required data rate - should be less than available
    - data replication technology - should be parallel

#### synchronous data replication
- data is automatically updated in  multiple locations
- puts dependency on network performance and availability
- availability zones: well connected, ohysically separated 
- ex: RDS deployed in Multi-AZ mode replicates db synchronously to a second Availability Zone

#### asynchronous data replication
- data is not automatically updated in multiple locations
- transfered when network performance and availability allows
- some data written is not fully replicated at that moment
- replica is not fully in sync - may be acceptable when read only

#### 3rd party custom replication
- build or
- buy

## Disaster Recovery
- preparing and recovering from a disaster
- need to establish a point in time for recovery

### Traditional approach
- duplicate infrastructure
    - procured
    - installed
    - maintained
- underutilezed or overprovisioned resources
- different levels of off-site duplication
- critical business services set up, maintened, tested reguarly
    - required
        - facilities
        - HVAC
        - Security
        - capacity to scale
        - support
        - contract with isp
        - network infrastructure
        - server capacity

### Aws
- scale up / down on demand
- access to global aws infrastructure
- change / optimize resources
- tools for segregation of duties
- automate deployment of entire environments
- test config changes in a duplicate environments
- available in multiple regions
- AZ - distinct insulated locations with low latency connectivity to other AZ-s within the region
- regions are completely independent
    - no difference in how they are accessed
    - create disaster recovery process that span continents whithout the costs & challanges
    - backup to 2 or more regions: service restoration
    - use regions to serve end customers around the globe: low latency of operational processes

### Metrics
- Recovery Time Objectives: time to restore functions to an acceptable level
- Recovery Point Objective: acceptable amount of data loss measured in time
    - ex: RPO is 1 hour
    - disaster struck at 12:00
    - all data is restored that was recorded before 11:00
- RTO, RDO is determinde based on costs
    - how much does it cost to implement
    - how much business damage does an outage cause
    - cost is
        - production traffic handled by aws in normal operation
        - pay for what you use - pay for Disaster Recovery environment only when used at full scale, when required
        - reserved always-on instances

## Pilot Light for Quick Recovery into Aws
- pilot light
    - critical core elements of the system are already configured and running in Aws
    - when rewuired for recovery, a full scale production environment is provisioned rapidly around the critical core
- pilot light infrastructure
    - critical core: data + db servers replicating datat to EC2

example schema:

        +---------------+                 +-------------+ 
        | Corporate     |                 | AWS   .--.  |
        | Data Center   |                 |       |  |---- reverse proxy - stopped
        |               |                 |       '--'  |
        |               |                 |       .--.  |
        |               |                 |       |  |---- app server - stopped
        |               |                 |       '--'  |
        |  O            | data mirroring  |       .--.  |
        | |_| ------------------------------>     |  |---- db server - pilot light system
        | master db     | application     |       '--'  |  data volume
        |               |                 |             |
        +---------------+                 +-------------+
        
- use either elastic ip address associated with instances
- or elastic load ballancing
    - distribute traffic to multiple instances
    - update record to point at EC2 instances, or
    - point to elastic load ballancing using a cname

### EBS Snapshot
- for less critical systems it is enough to have install packages and config ready
    - for example in the form of EBS snapshots
    - stored in S3
    - by regions

### Pilot Light Method
- quicker recovery than backup and restore
- core pieces of the system is kept runnning
- kept up to date 
- some install and config is still necessary
    - with AWS automated provisioning / configuration of infrastructure resources

### Prep Phase for Pilot Light
- set up EC2 instances to replicate & mirror data
- ensure that custom software packages are available in AWS
- create and maintain AMI of key servers
- regular run-test-update cycle
- automate AWS resource provisioning

### Recovery Phase with Pilot Light
- start EC2 instances from AMIs
- scale instances if necessary
- change DNS to point at EC2 instances
- install and config non-AMI based systems; automate if possible

## Warm Standby solutions in AWS
- **Standby** extends Pilot Light elements
    - more systems are always on
- fully functional but
- not scaled to bear production load
    - minimal fleet
- in case of a disaster
    - horizontal scaling preferred to vertical scaling
    - add more instances to the load ballancer
    - resize small capacity servers to run large EC2 instance types

### Prep phase
- set up EC2 instances to replicate or mirror data
- create and maintain AMIs
- run application with minimal footprint EC2 instances
- patch & update software and config files in line with live environment

example schema:

                                {O} Route53
                                 |
                +----------------+ - - - - - - - + not active for prod traffic
                |                                |
                V                                V 
        +---------------+                 +--------------+ 
        | Live          |                 | AWS     |    |
        | Env           |                 |        (O)----- elastic load ballancer
        |               |                 |         |    |
        |               |                 |       .---.  |
        |               |                 |       |   |---- reverse proxy
        |               |                 |       '---'  |
        |               |                 |         |    |
        |               |                 |       .---.  |
        |               |                 |       |   |---- app server
        |               |                 |       '---'  |
        |               |                 |         |    |
        |  O            | data mirroring  |    O  .---.  |
        | |_| ------------------------------> |_|-|   |---- slave db server
        | master db     | application     |       '---'  |  with data volume
        |               |                 |              |
        +---------------+                 +--------------+

### Recovery Phase
- start apps on larger EC2 instance types - vertical scaling
- increase no of EC2 instances in service with elastic load ballancer - horizontal scaling
- change dns record to point at AWS environment
- autoscaling

## Multi-site solution
- both on-site & AWS environments are live
- Route53 distributes traffic in a given %
- adjust DNS weighting in case of failure
- autoscale to accomodate load
- programm logic to detect failure

### Prep phase
- duplicate prod environment in AWS
- set up DNS weighting to distribute traffic between sites

### Recovery phase
- change weighting to redirect all traffic to AWS
- have app logic for failover to use local AWS db servers
- consider autoscaling