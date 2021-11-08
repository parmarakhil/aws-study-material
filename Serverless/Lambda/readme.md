# Lambda

1. Lambda
    1. Function as a service
    2. Virtual servers
    3. Limited by time - Short execution
    4. Run on demand
    5. Scaling is automated
    6. Benefits
        1. Easy Pricing
            1. Pay per request and compute time
            2. Free tier 1 Million lambda requests
        2. Integrated with many programming languages
        3. Easy monitoring through AWS cloud watch
        4. Easy to get meow resources per functions
        5. Increasing RAM will improve CPU and RAM
    7. Can run Lambda container image
        1. The container image must implement the lambda runtime API
        2. For arbitrary docker images use ECS or Fargate
    8. Integrated with
        1. API gateway
        2. Kinesis
        3. DynamoDB
        4. S3
        5. Cloudfront
        6. Cloudwatch events /EventBridge
        7. Cloudwatch logs
        8. SNS
        9. SQS
        10. Cognito
    9. Limits (Per regions)
        1. Executions
            1. Memory 128 MB to 10 GB
            2. 64MB increments
            3. Max execution time - 15 minutes
            4. Environment variables 4KB
            5. Disk capacity (/temp folder) -> 512MB
            6. Concurrent executions -> 1000 (can be increased)
        2. Deployments
            1. Lambda functions deployment size of compresses file -> 50MB
            2. Size of uncompressed deployment (code + dependencies) -> 250MB
            3. Can use tmp directory to load files at startup
            4. Size of env variables -> 4KB
    10. Lambda@Edge
        1. Deploy and run lambda functions alongside your CloudFront CDN
            1. Build more responsive application
            2. Lambda is deployed globally
            3. Customise CDN content
            4. Pay only for what you use
        2. To change cloudfront requests and response
        3. Can change
            1. Viewer request (user to cloudfront)
            2. Origin request (Clodfron to origin)
            3. Origin response (origin to cloudfront)
            4. Viewer response (cloudfront to use)
        4. You can also generate responses to viewers without ever sending the request to the origin
        5. Use case
            1. Website security and privacy
            2. Dynamic web app at edge
            3. SEO
            4. Intelligent route across origins and data centetrs
            5. Real time image transformation
            6. User authentication and authorisation
