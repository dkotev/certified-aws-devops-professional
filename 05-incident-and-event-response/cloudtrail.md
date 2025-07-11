# AWS CloudTrail

- Provides governance, compliance and audit for an AWS account
- CloudTrail is enabled by default!
- Provides a history of events/API calls made within an AWS account by:
    - console
    - sdk 
    - cli
    - aws services
- It can put logs into CloudWatch Logs or S3
- A trail can be applied to All Regions (default) or a single Region
- In case of a resource deletion, to investigate it (who did it), we have to look inside CloudTrail first
- The default UI only shows create, modify and delete events
- CloudTrail Trail features:
    - Provides detailed list of all events
    - Ability to store these events in S3 for further analysis
    - It can be region specific or global
- CloudTrail logs are encrypted with SSE-S3 encryption by default when they are stored into S3. There is a possibility to use SSE-KMS encryption
- A CloudTrail log entry contains information about:
    - Who made the request
    - When was the request made and from where
    - What was requested
    - What was the response
- CloudTrail may have a 15 minutes delay to deliver log files into the S3 bucket

## CloudTrail Events

- Management Events:
    - Operations that are performed on resources in our AWS account
    - Example: `AttachRolePolicy`, `CreateSubnet`, `CreateTrail`
    - By default trails are configured to log management events no mather what
    - We can separate Read Events (that don't modify resources) from Write Events (that modify resources)
- Data Events:
    - By default data events are not logged (because high volume of operations)
    - Data events are: 
        - S3 object-level activity (`GetObject`, `DeleteObject`, `PutObject`)
        - AWS Lambda function execution activity
- CloudTrail Insights Events:
    - If enabled, it will analyze events to detect unusual activity in our account. Examples:
        - Inaccurate resource provisioning
        - Hitting service limits
        - Bursts of AWS IAM actions
        - Gaps in periodic maintenance activity
    - CloudTrail Insights analyzes normal management events to create a baseline and then continuously analyzes write events to detect unusual 
    patterns
    - Insights events will appear in CloudTrail console
    - They are also sent to S3 (if enabled)
    - An EventBridge event is generated (for automation needs)
    ![alt text](image.png)


## CloudTrail Events Retention

- Events by default are stored for 90 days in CloudTrail
- To keep events beyond these period we can send these events to S3 and use *Athena* to analyze them

![alt text](image-1.png)

## Cloudtrail Intercept API calls 

e.g 
- recieve notification api call when deletetable api call in dynamodb -> logs in cloudtrail -> eventbridge -> sns and inform us when someone deletes table 
- recieve notification when user AssumeRole -> cloudtrail -> eventbridge -> sns
- user edit SG -> cloudtrain-> eventbridge -> sns 

## Log Integrity

- We can validate the integrity of the logs file using the AWS CLI
- The AWS CLI allows us to detect the following type of changes:
    - Modification or deletion of CloudTrail log files
    - Modification or deletion of CloudTrail digest files
    - Modification or deletion of both of the above
- Validate logs command:
    ```
    aws cloudtrail validate-logs --start-time <time> --trail-arn arn:aws:cloudtrail:us-east-2:123456:trail/my-trail-name --verbose --profile aws-devops
    ```

## Cross Account Logging

- Reference: [https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)
- Steps to enable cross account logging:
    1. Turn on CloudTrail in the account where the destination bucket will belong
    2. Update the bucket policy to grant cross-account permission to CloudTrail
    3. Turn on CloudTrail on the other accounts. Configure CloudTrail to use the same bucket from the destination account