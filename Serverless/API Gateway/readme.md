# API Gateway

1. API gateway
    1. Serverless API
    2. Invoke lambda using rest api
    3. APIs are public
    4. Lambda+gateway -> full server less application - no infra to manage
    5. Supports web socket protocol
    6. Handles API versioning
    7. Handle multiple environments dev, test, prod
    8. Handle security
    9. Create API keys, handles requests throttling
    10. Swagger/Open API import to quickly define APIs
    11. Transform and validate requests and responses
    12. Generate SDK and API specifications
    13. Cache API responses
    14. Integration 
        1. Lambda function
        2. HTTP
        3. AWS service
    15. Endpoint types
        1. Edge optimised 
            1. Default
            2. For global clients
            3. Requests are routed through cloudfront edge locations
            4. The API gateway still lives in only one region
        2. Regional
            1. For clients within same region
        3. Private
            1. Can only be accessed from within VPC using a interface VPC endpoint
            2. Use access policy
    16. In proxy mode you cannot transform the request and response
    17. Security
        1. IAM permissions
            1. Create a policy and attach to user/role
            2. Good to provide access within your own infra
            3. Leverages Sigv4 capability 
            4. Great for user/roles already within your AWS account
            5. Handle authentication + authorisation
        2. Lambda Authoriser
            1. Custom authoriser
            2. Uses AWS lambda to validate token in header being passed
            3. Option to cache the result of authentication
            4. Help to use OAuth/SAML/ 3rd party type of authentication 
            5. Lambda must return IAM policy of the user
            6. Handle authentication + authorisation
        3. Cognito User pools
            1. Manages user lifecycle
            2. API gateway verifies identity automatically from AWS cognate
            3. No implementation is required
            4. Cognito only helps with authentication and not authorisation
