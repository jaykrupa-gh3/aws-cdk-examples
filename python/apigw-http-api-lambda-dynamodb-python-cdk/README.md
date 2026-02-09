
# AWS API Gateway HTTP API to AWS Lambda in VPC to DynamoDB CDK Python Sample!


## Overview

Creates an [AWS Lambda](https://aws.amazon.com/lambda/) function writing to [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) and invoked by [Amazon API Gateway](https://aws.amazon.com/api-gateway/) REST API. 

This implementation includes **AWS X-Ray tracing** for end-to-end observability and **comprehensive security logging** for audit and compliance.

![architecture](docs/architecture.png)

## Features

- **End-to-End Tracing**: AWS X-Ray enabled for API Gateway, Lambda, and DynamoDB operations
- **Comprehensive Logging**: VPC Flow Logs, API Gateway access logs, CloudTrail, and structured Lambda logs
- **Security & Compliance**: CloudTrail for API activity, DynamoDB point-in-time recovery and streams
- **Centralized Log Storage**: S3 bucket with lifecycle policies for long-term log retention
- **VPC Isolation**: Lambda function runs in private isolated subnet
- **DynamoDB Integration**: VPC endpoint for secure DynamoDB access

## Setup

The `cdk.json` file tells the CDK Toolkit how to execute your app.

This project is set up like a standard Python project.  The initialization
process also creates a virtualenv within this project, stored under the `.venv`
directory.  To create the virtualenv it assumes that there is a `python3`
(or `python` for Windows) executable in your path with access to the `venv`
package. If for any reason the automatic creation of the virtualenv fails,
you can create the virtualenv manually.

To manually create a virtualenv on MacOS and Linux:

```
$ python3 -m venv .venv
```

After the init process completes and the virtualenv is created, you can use the following
step to activate your virtualenv.

```
$ source .venv/bin/activate
```

If you are a Windows platform, you would activate the virtualenv like this:

```
% .venv\Scripts\activate.bat
```

Once the virtualenv is activated, you can install the required dependencies.

```
$ pip install -r requirements.txt
```

At this point you can now synthesize the CloudFormation template for this code.

```
$ cdk synth
```

To add additional dependencies, for example other CDK libraries, just add
them to your `setup.py` file and rerun the `pip install -r requirements.txt`
command.

## Deploy
At this point you can deploy the stack. 

Using the default profile

```
$ cdk deploy
```

With specific profile

```
$ cdk deploy --profile test
```

## After Deploy
Navigate to AWS API Gateway console and test the API with below sample data 
```json
{
    "year":"2023", 
    "title":"kkkg",
    "id":"12"
}
```

You should get below response 

```json
{"message": "Successfully inserted data!"}
```

### Viewing X-Ray Traces

After making API requests, view end-to-end traces in the AWS X-Ray console:
1. Navigate to AWS X-Ray in the AWS Console
2. Select "Service Map" to see component interactions
3. Select "Traces" to view individual request traces with timing details

### Viewing Logs

The stack creates multiple log sources for comprehensive observability:

**CloudWatch Logs:**
- Lambda logs: `/aws/lambda/apigw_handler` (structured JSON format)
- VPC Flow Logs: Monitor network traffic patterns
- API Gateway access logs: Track all API requests with detailed metadata

**CloudTrail:**
- All AWS API calls are logged to the S3 log archive bucket
- Includes management events for security auditing

**DynamoDB:**
- Point-in-time recovery enabled for data protection
- DynamoDB Streams capture all data modifications

**Query logs using CloudWatch Logs Insights:**
```
fields @timestamp, message
| filter message like /Successfully inserted/
| sort @timestamp desc
```

## Cleanup 
Run below script to delete AWS resources created by this sample stack.
```
cdk destroy
```

**Note:** The S3 log archive bucket has a retention policy and will be retained after stack deletion for compliance purposes.

## Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

Enjoy!
