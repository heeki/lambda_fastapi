# Overview
This repository implements example controls as Config rules with AWS Lambda functions.

## Pre-requisites
Copy `etc/environment.template` to `etc/environment.sh` and update accordingly.
* `PROFILE`: your AWS CLI profile with the appropriate credentials to deploy
* `REGION`: your AWS region
* `BUCKET`: your configuration bucket

For the API Gateway an dLambda stack, update the following accordingly.
* `P_API_STAGE`: the deployment stage for API Gateway
* `P_FN_MEMORY`: amount of memory in MB for the Lambda function
* `P_FN_TIMEOUT`: timeout in seconds for the Lambda function

## Deployment
Deploy the Lambda resources: `make sam`

After completing the deployment, update the following outputs:
* `O_FN`: output of the function name for invoke testing

## Testing
To start a local API Gateway endpoint: `make sam.local.api`  
To perform a dice roll: `curl -s -XPOST -d @etc/payload.json http://127.0.0.1:3000/ | jq`
To get dice rolls: `curl -s -XGET http://127.0.0.1:3000/?name=heeki | jq`

In order to test against the deployed API Gateway endpoint, update the URL to your FQDN.

## Troubleshooting
To test request validation (missing payload): `curl -s -XPOST http://127.0.0.1:3000/ -v`  
(local test doesn't seem to do request validation -> error code: 403 (from the function))

To test request validation (headers): `curl -s -XPOST -d @etc/payload.json -H 'content-type: application/json' -H 'accept: application/json' ${ENDPOINT}/dev/| jq`  
(the content-type header is required for request validation to work properly via both curl and postman; without it curl defaulted to "content-type": "application/x-www-form-urlencoded")
