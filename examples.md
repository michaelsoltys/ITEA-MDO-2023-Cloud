# ITEA MDO - Intro to Cloud - Part 2

This folder is a git repository, on public GitHub:

- https://github.com/michaelsoltys/ITEA-MDO-2023-Cloud

This is the demonstrationa part of the tutorial. To display Markdown in terminal install `mdless` as follows:
```
sudo yum install gem
gem install mdless
```
Notes on `mdless` can be found here:

- https://github.com/ttscoff/mdless

## Example of AWS CLI

Example of starting an ec2 instance:
```
aws ec2 run-instances \
    --image-id ami-0507f77897697c4ba \
    --count 1 \
    --instance-type t2.micro \
    --key-name itea-mdo-2023-demo \
    --security-group-ids sg-04d6aede25999117f \
    --subnet-id subnet-25c56342 \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=itea-mdo-2023}]'
```
For complete documentation see:

- https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html

Then, to connect to the instance use the following command:
```
ssh -i itea-mdo-2023-demo.pem ec2-user@IP
```
where the IP is the the public IP of the instance, which in turn can be found with:
```
aws ec2 describe-instances --filters Name=instance-state-name,Values=running | grep PublicIpAddress
```

## Example of AWS SDK

SDK, stands for _Software Development Kit_, and it is a way to programmatically access AWS resources. For example, to implement the previous command to describe instances as Python code, use
```
import boto3
from pprint import pprint

ec2 = boto3.client('ec2')
response = ec2.describe_instances()
pprint(response)
```
with more information here:

- https://boto3.amazonaws.com/v1/documentation/api/latest/guide/ec2-example-managing-instances.html

## Example of "raw access"

Curl:

- http://bit.ly/3wzSFgM

Example:

- https://github.com/michaelsoltys/CloudProgrammatic/blob/main/lambda.sh

## Example of Synthetic Data Generator

List the available Lambda functions:
```
aws lambda list-functions
```
and to only show the name of functions:
```
aws lambda list-functions | grep FunctionName
```
List the available REST APIs:
```
aws apigateway get-rest-apis
```
and just as in the case of listing lambda functions, in order to get just the id, use the `grep` function:
```
aws apigateway get-rest-apis | grep id
```
Invoke the synthetic data generator:
```
python3 invoke_endpoint.py --api-id jfexyyj8w5 --path test.json --plot histogram 
```
