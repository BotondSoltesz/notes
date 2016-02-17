# AWS Database Services
## Use Cases
A number of db alternatives exists, chose the one appropriate for the use case

| use case | appropriate service |
| --- | --- |
| relational db service with minimal administration | Amazon RDS |
| fast highly scalable NoSQL db service | Amazon Dynamo DB |
| a self managed relational db | EC2 + EBS + AMI image |

## Relational vs Non-relational

| aspect | Relational | Non-Relational |
| --- | --- | --- |
| App Type | existing & business process centric apps | new web scale apps with lots of small reads / writes |
| App Characteristics | relational data model with complex queries, joins, updates | simple data model / transactions, range queries, simple updates |
| Scaling | architected | seamless on-demand scaling per app needs | 
| QOS | performance depends on data model / indexing / query / storage optimization; managed durability | automatically optimized performance; managed reliability & durability |
| Skills | sql + programming | web-style programming |

## Amazon RDS
- from aws console set up, operate, scale relational db in the cloud
- cost efficient
- scalable capacity
- manages time consuming admin tasks
- works with MySQL, Oracle, MS engines
- automatic backups

To set up:
1. Authorize access
2. create db security group - what ip or ec2 instances have access
3. launch db instance
    - db instance is specific to db engine
4. connect to db instance

## Dynamo BD
- fully managed NoSQL db-service
- ideal for low latency, scalable applications, with simple queries
- store any amount of data
- fast & predictable performance - stored on ssd-s
- elastic map reduce integration
- throughput alarm: sends email notification if provisions are exceeded
- NoSQL: rows are not required to have the same structure

## RedShift
- manages all the work required to
    - set up
    - operate
    - scale
- data warehouse cluster
- no downtime when scaling warehouse cluster
- continuously monitors cluster health
- automatically replaces failing components
- built in security
    - in transit
    - at rest
    - when backed up
- backup to S3 is
    - continuous
    - incremental
    - automatic
- disk failures are transparent, nodes recover automatically
- streaming restores
- integrated with other Amazon tools