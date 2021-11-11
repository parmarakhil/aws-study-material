# Cognito & SAM

1. AWS cognito
    1. To give users an identity to interact with application
    2. Cognito user pools
        1. Sign in functionality for app users
        2. Integrate with API gateway
        3. Serverless database of users for mobile apps
        4. Can enable federated identities (Facebook, google, SAMl)
        5. Sends back JQT
        6. Can be integrated with API gateway for authentication
        7. Simple login
    3. Cognito Identity pools (Federated identity)
        1. Provide AWS credentials to user so they can access AWS resources
        2. Integrate with Cognito user pools as identity provider
        3. Get temporary AWS credentials back from federated identity pool
        4. Eg: to upload to S3 by login to Facebook
    4. Cognito Sync
        1. Synchronise data from device to cognate
        2. May be deprecated and replaced by AppSync
        3. Store preferences, configuration, state of app
        4. Offline capability (sync when online)
        5. Requires Federated identity pool in cognite (not user pool)
2. SAM
    1. Serverless application model
    2. Framework for developing and deploying serverless application
    3. All configuration is YAML code
    4. Can help run lambda, API gateway and DynamoDB in local
    5. Can use CodeDeploy to deploy lambda functions
