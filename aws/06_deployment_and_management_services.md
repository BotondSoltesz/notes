# Deployment and Management Services
- AIM: Amazon IDentity and Access Management
- AWS Cloud Formation
- Amazon Elastic Beanstalk

## AIM
- manage users and permissions
- assign to users
    - individual security credentials, or
    - temporary security credentials
- manage permissions
- create roles
- define which entity (aws service or user) assumes which roles
- manage federated users and their permissions
    - enable identity federation, in other words
    - add permissions to existing corporate users without having to create additional AIM profiles

### Video demo notes
- Create new users
    - when creating new users, download security credentials (access key, secret key) when presented; after closing window there will be no access to them
    - credentials are required to access AWS API
- Create new user group
    - choose a group name
    - apply existing policy template, or
    - create custom policy
    - add users to group
- Add password to users

## AWS Cloud Formation
- scripting interface for creating / configuring aws resources
- framework for lifecycle management of resources created with scripts
- simple tamplates
- custom scripts
- provided at no additional cost

## AWS Cloud Watch
- Basic monitoring every 5 minutes
    - collects metrics on
        - CPU load
        - data transfer
        - disk usage
    - accessible via
        - management console
        - API
        - SDK
        - CLI
- Detailed monitoring every 1 minutes
    - basic monitoring PLUS
    - enables data aggregation by EC2 AMI ID & instance type

## AWS Elastic Beanstalk
- enables deployment to AWS cloud without worrying about the infrastructure
- reduces management complexity
- automatically handles
    - capacity provisioning
    - load ballancing
    - scaling
    - application health monitoring