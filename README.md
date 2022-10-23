README
=========

# Introduction
This is deployable API using AWS API Gateway and Lambdas to allow interfacing with Tesla API.
Main usecase is to use this with IOS Shortcuts app.

# Deployment instructions to AWS
1. `yarn global add  serverless` - install dependencies
2. Create AWS user with the [following policies](./aws_deployer_policies.json)
3. Export `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` from the newly created user
4. `serverless deploy` - deploy

# Testing / Usage
1. Obtain access token: [Auth for Tesla](https://apps.apple.com/us/app/auth-app-for-tesla/id1552058613)
2. Obtain vehicle ID:
```bash
  curl 'https://<api gateway id>.execute-api.us-east-1.amazonaws.com/dev/command-tesla-api' \
    -XPOST  -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'x-api-key: <api key>' -i \
    --data '{"TOKEN": "<tesla acccess token>"}'
}
```

3. Open / Close trunk:
```bash
  curl 'https://<api gateway id>.execute-api.us-east-1.amazonaws.com/dev/command-tesla-api' \
    -XPOST  -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'x-api-key: <api key>' -i \
    --data '{"TOKEN": "<tesla acccess token>", "INPUT_CMD": "actuate_trunk", "VEHICLE_ID": "<vehicle_id>"}'
}
```

Typically this is meant to be used from IOS shortcuts app and [Auth for Tesla](https://apps.apple.com/us/app/auth-app-for-tesla/id1552058613) provides proper API via shortcuts app to obtain access token to perform various actions via Tesla API

# Basic architecture
IOS Shortcut / curl -> AWS API Gateway -> Front-end AWS Lambda Function -> AWS SNS -> Back-end AWS Lambda Function -> Tesla API Service

# References
Basd on https://github.com/dburkland/tesla_cmds_aws_lambda_functions
