# AWS Storage Services
- S3
- Glacier
- EBS: Elastic Block Store (for use with EC2)

## S3
### Objects
- **Objects** are made up of
    - data (file)
    - default metadata (e.g. version)
    - standard http metadata
    - custom metadata defined when storing objects
- Every Object is stored in a **Bucket**
- Objects within a Bucket are uniqely identified by a key

### Buckets
- **Buckets** hold **Objects**
- Buckets are the unit of aggregation for usage reporting
- Name of a Bucket is unique accross all S3 accounts
- Access control is managed on a per Bucket basis
- Buckets can be configured to be region-specific
- An account may have multiple Buckets
- Useful for organizing Objects into namespaces

### Regions
Choose regions for buckets to
- optimize latency
- minimize costs
- address regulatory requirements

AWS Regions:
- US East
- US West - North California
- US West - Oregon
- Europe - Ireland
- Asia Pacific - Singapore
- Asia Pacific - Sydney
- Asia Pacific - Tokyo
- South America - Sao Paolo
- AWS Government Cloud (US)

### Access Control
- ACL: Access Control List defines permissions per bucket / object
- Policies: Collection of statements that define a user's permission to access S3 resources
- Policies can be attached to
    - users
    - groups
    - S3 Buckets
- Policies allow for centralized management

## Glacier
- pay-as-you-go service
- Offloads adminstrative burden of archival storage
- optiomized for infrequent access
- retrieve times are on the scale of hours
- store for decades for $0.01 / GB Months
- Use cases
    - offsite enterprise archiving
    - archival of media assets
    - archival of scientific and research data
    - digital preservation
- Stores data in **Archives** - single file or multiple files combined
- **Archives** are organized into **Vaults**
- To retrive an archive a **Job** is initiated - takes 3—5 hours to complete
- **Jobs** can send SMS notification when finished

### Vault
- container for unlimited number of archives
- to create: specify name and region
- a single AWS account is allowed to create up to 1000 Vaults per region
- to delete
    - must not contain anything according to inventory
    - must not have been written to since last inventory
- Vaults are created via API or AWS Console
- uploading data requires programming via REST Api or AWS SDK

## Lifecycle Management

## EBS: Elastic Block Storage

## AWS Import / Export