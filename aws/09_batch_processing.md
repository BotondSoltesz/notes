# Batch Processing
- services that facilitate batch processing
- create auto scaling groups triggered by items in a queue
- Amazon Simple Queue Service
    - delivery guarantees
    - performance and durability characteristics

## Example
- website that hosts freelance graphic artist' library of images
    - artists submit images which are
        - watermarked
        - thumbnails are automatically created
    - image processing requests are batched
        - stored in a queue
- analysis of large data sets
- scientific simulations
- analysis of financial markets
- batch processing
- item classification
- affiliate program - calculate monthly payments
- Nasa: process Mars rover images
- DigiChalk: SQS for managing & transcoding media files

### Characteristics
 - data is easily segmented
 - can be processed independently in parallel
 - generally consumes all available capacity
 - pay for used resources only

## Generalized architecture
1. Orchestration and Queuing
    - Guide flow of data through the system
    - services
        - SWF
        - SQS
2. Compute Services
    - worker components - each component processes an item from the batch
    - elastic map reduce - good candidate for worker component if the problem is a good fit for map reduce
    - services
        - EC2
        - Elastic Map Reduce
3. Notification Services
    - not always necessary
    - some batches may take a long time to process
    - notify observers when work is completed
    - services
        - SES
        - SNS
4. Data Storage Services
    - service depends on the kind of batch processing
        - RDS: relational data
        - DynamoDB: NoSql
        - Redshift: large petabyte scale data
        - S3: large files

## Simplified Architecture

        o 1.    .---.
        | ----> | J |---------+----------+----------+
        ^       '---'         |          |          |
        user    job manager   |2a.       |2b.       |2c.
                              V          V          V
                          .--------.    ___         O
                          |  |  |  |    \_/        |_|
                          '--------'                
                          input queue   S3 bucket   DB
                              |         |           |
                              |3.       |4.         |4.
                              V         |           |
                          .-----.       |           |
                          | EC2 |<------+-----------+
                          '-----'
                          EC2 instances
                          Autoscaling Group
                              |
                              |5.
                              V
                          .--------.
                    <-----|  |  |  |
                          '--------'
                          output queue
                          
1. user submits job to job manager
2. job manager queues work
    a. into queue
    b. if data is too big, store in S3
    c. not queriable data - store metadata in db
3. EC2 pulls job items from queue
4. grab work detail and work
5. output work
    - push original, processed images into storage & their url
    - delete original job request
    - send notification for job done

### Preparing the queue

### Autoscaling

