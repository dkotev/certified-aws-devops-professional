# Exam Notes by sections


## sdlc


## configiuration magagment
- static web site -> s3
- dynamic web site -> ec2, elastic beanstalk 

## resilient cloud solution 
- read replica cannot be configured in another region 

## monitor and logging 
- AWS config can't automatically remediate the accounts rules 
- cloudtrail-enabled  rule is only available for the periodic trigger type and not configuration changes 
- PII and intellectual property -> hints to macie 
- eventbridge monitor changes of a state and can inform and send notifications (Use a Lambda function to pass a notification to a Slack channel whenever deployments fail) can set sns,lambda, kinesis, sqs and build-in target(CW alarms)
- create a cloudwatch alarm for every change to PUT or DELETE bucket policy, bucket lifecycle, bucket replication, or to PUT a bucket ACL, don't use eventbridge 
- trust advisor can terminate machines with the help of eventbridge (not ssm)


## Security 
- restict/authorized.... template or only specific region  - Service catalog 
- AWS RDS don't support Oracle RAC or Aurora
- if we need to check if all of the ec2 instances are using and approved ami-   AWS Config manage rule and specify a list of approved AMI id, then send notifiaction for non-compliant instances 
- if we need to use a service for 3rd party tool detection use guardduty 
