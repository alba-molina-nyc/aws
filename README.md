# AWS Lambda Serverless Framework

- Basic Nodejs Python
- Basic AWS Cloud

## Lambdas are virtual functions

- lambda allows you to only manage functions and not have to manage the server
- lambdas have limited time - short executions
- they run on demand meaning they use what you need, if no one uses certain functions then you do not use the ones the users are not using, this allows for automated scaling

### Serverless just means that you do not need to manage the server anymore because AWS will do that

- AWS Cloud Watch allows you for to keep a pulse on your functions

AWS Lambda languages include python and nodejs

## AWS Lambda Integrations

they need to integrate with other items within AWS realm

- `Api Gateway` - provide REST API
- `Kinesis` - process data as it arrives
- `DynamoDB` - noSQL processing metadata
- `AWS S3` - simple storage for files like imgs, audio etc.
- `AWS IoT`
- `CloudWatch Event`
- `CloudWatch Logs` - to see how things are going
- `AWS SNS Simple Notification Service` - if you want to notify user for certain events like sending emails
- `AWS Cognito` - manages users in authorization - think logging and logging out

### Example

- Imagine you have website that users can upload img to S3
- in backend wanttrigger into AWS lambda function, to create a thumbnail
- Once thumbnail is created then it will push to S3 (but diff bc now it is a thumbnail)
- In addition to pushing the img, you can also remove some of the metadata of the img and save that metadata into dynamodb like img name, img size, creation date etc.

### Serverless Framework

- removes the pain of creating, deploying, managing, and debugging lambda functions
- it uses cloudformation to do the background work when pushing things to the cloud

### Installing Serverless

1. Install Node
2. Install Serverless
3. Create a user in IAM (do not share the credentials)
4. Open console & configure that user you just created:

   ```#!/bin/bash
    serverless config credentials --provider aws --key get_key_from_user_just_created  --secret get_secret_access_key --profile name_of_user
    ```

Create the first service, Deploy the service, make sure it works

### create serverless project

understanding yml files

![Understanding .yml file](tasks/01_nodejs_imgs/02.3understanding_.yml_files.png)

understanding handler.js

![understanding handler.js](tasks/01_nodejs_imgs/02.0create_serverless_project.png)

### When making changes to function

```#!/bin/bash
albamolina@Albas-Air aws-node-project % sls deploy
```

or can only push on that particular function instead of re-deploying the whole stack

```#!/bin/bash
 albamolina@Albas-Air aws-node-project % sls deploy function -f hello
 ```

## Fetching the Function Logs from the CLI

to invoke the function:
_albamolina@Albas-Air aws-node-project % sls invoke -f hello_

```#!/bin/bash
{
    "statusCode": 200,
    "body": "{\n  \"message\": \"hey there again!\",\n  \"input\": {}\n}"
}


```

to get the logs:

```#!/bin/bash
_albamolina@Albas-Air aws-node-project % sls invoke -f hello --log_
```

```#!/bin/bash
{
    "statusCode": 200,
    "body": "{\n  \"message\": \"hey there again!\",\n  \"input\": {}\n}"
}
--------------------------------------------------------------------
START
END Duration: 7.84 ms Memory Used: 65 MB
```

to get all of the logs ever: (same as going to CloudWatch to see the logs)

```#!/bin/bash
_albamolina@Albas-Air aws-node-project % sls logs -f hello
```

```#!/bin/bash
START
END Duration: 13.91 ms (init: 278.98 ms) Memory Used: 64 MB
START
END Duration: 7.84 ms Memory Used: 65 MB
```

### Deleting through Console

pros: you can make sure that all the objects and related objects are deleted rather than deleting from the AWS UI because you would be sacrificing deleting everything and could end up paying for stuff that should've been deleted a long time ago

- _up to this point I've learned how to deploy, run, stream logs and destroy Lambda functions both manually through AWS UI and through serverless framework aka programming_

-------

## Creating a Serverless Service with any Rutime

- up to this point I've created a serverless service via nodejs but now I will learn how to create a serverless service with any runtime

## Understanding Serverless Service with any Runtime

File patterns are the same whether Java, Python, Nodejs etc.

```text
Ex: if they use ApiGatewayResponse then they will have a file named: 
ApiGatewayResponse.java / ApiGatewayResponse.js / ApiGatewayResponse.py

Handler.java / Handler.js / Handler.py (where you can find the name of the function)

Serverless.yml remains constant accross the board for all of them and the .yml file structures the whole project basically is the metadata needed to configure the project so we can push to AWS
```

## Understanding YAML

- YAML is a markup language used to edit `serverles.yml` file
  
- Key- value (kind of a subset of json) idea creating / transforming structured object data in this but it is just a diff way to styling it

![understanding yml files](tasks/understanding_yml.png)

## AWS Functions and the Serverless Framework Core Concepts

`serverless`- allows to deploy, create, manage the AWS functions in the AWS console through the serverless CLI.

`Function`- capital "F" means AWS Lambda functions
![understanding yml files](tasks/02_python_imgs/02.2_python_lambda_function.png)

- An aws function is just code that can be deployed in the clooud
  
- it does a `single` job

`Events`- and triggers as it pertains to lambda function:

- anything that triggers an AWS lambda function to execute

ex1: AWS API GateWay HTTP endpoints (this is an event that triggers something)
  
ex2: upload an image an AWS bucket upload (this is an event that triggers something)

`Resources`- what is used by the AWS component AWS infrastructure components which your Functions use

- An AWS DynamoDB Table (i.e. saving a comment) you would save this after the event is triggered.
  
- An AWS S3 Bucket (for saving audio files) -- when we put it back to the bucket that is a resource (the result of an event)

- An AWS SNS (sending messages asynchronously)

`Services`- Framework's unit organization (project file) 

- aws-nodejs-project

- where we define our Functions, the events that trigger them and the resources our Functions use

- a service can be described in YAML or JSON
Ex:
![understanding yml files](tasks/02_python_imgs/aws_core_concepts.png)

### AWS Timeout and Memory

