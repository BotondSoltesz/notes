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
- new queue in SQS management console
- if necessary, change region in console before creating the queue
- name should be unique for account
- visibility timeout
    - when item is read from queue by worker, it becomes hidden, so that it is not delivered to multiple workers
    - worker must remove work item from queue after processing is done
    - if item is not removed within *visibility timeout* time from queue, it becomes visible again
- use SGS console to view details of the queue
- note url of queue - this is what workers will use to communicate with queue via the API

### Autoscaling
1. launch configuration via command line tool
    - `as-create-launch-config myLaunchConfig --image-id ami_1234 --instance-type m1.small`
2. create autoscaling group
    - `as-create-auto-scaling-group myAutoGroup --launch-configuration myLaunchConfig --min-size 1 --max-size 20`
3. autoscaling policy
    - no logic is added as to when it should be executed
    - it is more like a button
    - it is good practice to have at least 2 policies:
        - scale up
            - `as-put-scaling-policy myScaleUpPolicy --type PercentChangeInCapacity --auto-scaling-group myAutoGroup --adjustment 50`
        - scale down
            - `as-put-scaling-policy myScaleDownPolicy --type ChangeInCapacity --autos-caling-group myAutoGroup --adjustment -1`
    - a new instance is time consuming
        - up in large increments
        - down in small steps
4. triggering autoscaling
    - manually via command line or API
    - scheduled
    - as a response to **Amazon Cloud Watch** alarm
        - created from Cloud Watch console
        - by default metrics are read every 5 minutes - not instantaneous