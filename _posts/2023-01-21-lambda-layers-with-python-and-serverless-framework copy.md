---
title: "Lambda Layers with Python and Serverless Framework"
date: 2023-01-20 13:15:00
categories: [aws]
tags: [aws, lambda, python]
---

## Preamble

[GroundSchool NZ](https://groundschool.co.nz) is currently deployed to AWS using Terraform, but I've recently moved some pieces to [Serverless Framework](https://www.serverless.com) (SLS) with the ultimate goal of a mix of Terraform and SLS. It's still a work in progress, but the idea is:
 - Terraform - infra layer:
   - IAM, Route53, CloudFront (maybe?), S3 deployment-related buckets
 - Serverless framework - app layer:
   - Lambda, S3, DynamoDB, API gateway

The existing Lambda functions were put together pretty roughly, and duplicate some  utility code for things like HTTP requests/responses and interacting with DynamoDB. It seemed like a good opportunity for a tidy-up.

After looking at options for sharing python code between Lambda functions, I realized that [Lambda Layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html) did what I wanted and is supported in SLS without requiring plugins.

## Folder structure

I have a `web-api` Serverless Framework service containing a `common` folder for shared functions and `functions` for the API endpoints. These should be able to access the layer we create from `common`, eg. `db_utils`, `list_utils`, etc.

**Important "`/common/python`":** in order for Lambda to be able to find functions in the layer, shared code needs to be in a folder called `python`. The root folder (eg. `common`) can be wherever you want (we define the path in the Serverless config), but the immediate child needs to be a directory called `python` which contains your shared functions.

```
web-api
|- serverless.yml
|- common
   |- python
      |- db_utils.py
      |- request_utils.py
      |- response_utils.py

|- functions
   |- exams
      |- exams.py
      |- serverless.yml
   |- questions
      |- questions.py
      |- serverless.yml
```

## Defining the layer in `serverless.yml`

### 1. Add a layers section to your root `serverless.yml` file
```
provider:
  ...

layers:
  common:
    path: common
    compatibleRuntimes:
      - python3.9
    description: 'Shared utilities'

functions:
  ...
```
 - `common:` - the name used to reference your layer - choose whatever makes sense
 - `path` - points to the root directory for your layer, which contains the `/python` directory with shared functions

More detail on the SLS layers configuration: [https://www.serverless.com/framework/docs/providers/aws/guide/layers](https://www.serverless.com/framework/docs/providers/aws/guide/layers)


### 2. Add the layer to your provider in `serverless.yml`
```
provider:
  name: aws
  ...
  layers:
   - !Ref CommonLambdaLayer
```

 - Replace `CommonLambdaLayer` with the layer name you chose in step 1, and a suffix 'LambdaLayer'. eg. if you named your layer `utils` instead of `common`, you'd change this to `- !Ref UtilsLambdaLayer`

## Writing the layer code

Nothing special here -- just add python functions and save as a `.py` file.

`response_utils.py`:
```python
import json

def make_200(response_body):
    return {
        'statusCode': 200,
        'body': json.dumps(response_body)
    }
```

## Using functions in the layer

Import the module that we created in the previous step, and use the function!
```python
import response_utils

def get(event, context):
    try:
        return response_utils.make_200({ 'availableExams': { 'name': 'PPL Air Law' } })
```
