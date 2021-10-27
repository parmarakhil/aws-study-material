# Security

1. Encryption
    1. Encryption in flight is done through using SSL certificate
        1. Ensures MITM (Man in the middle) attack cannot happen
    2. Server side encryption at rest
        1. Data is encrypted after being received by the server
        2. Data is decrypted before being sent
        3. The encryption/decryption keys are managed using some management service
    3. Client side encryption
        1. Data is encrypted by client and never decrypted by the server
        2. Data will be decrypted by a receiving client
        3. Could leverage envelope encryption
2. KMS
    1. Key management service
    2. AWS KMS manages keys for us
    3. Integrated with IAM for authorisation
    4. Can be used by SDK and CLI
    5. Customer Master Key (CMK)
        1. Symmetric (AES-256 keys)
            1. Single encryption key that is used to encrypt and decrypt
            2. Necessary for envelope encryption
            3. AWS services that are integrated with KMS use symmetric CMKs
            4. You never get access to the key unencrypted
                1. Must call KMS API to use
        2. Asymmetric (RSA & ECC key pairs)
            1. Public  key for encryption
            2. Private key for decryption
            3. Used for sign/verify options
            4. The public key is downloadable
            5. Cannot access the private key unencrypted
            6. Use
                1. Encryption outside of AWS
        3. Types
            1. AWS managed service default CMK - free
            2. User keys created in KMS
            3. User keys imported (must be 256 bit symmetric keys)
    6. KMS gives ability to fully manage keys & policies
        1. Create
        2. Rotation policies
        3. Diable
        4. Enable
    7. Using Cloudtrail we can audit key usage
    8. Use
        1. To share sensitive information
    9. Benefit of KMS is that CMK cannot be retrieved by the user and CMK can be rotated for extra security
    10. Encrypted secrets can be stored in code/environment variables but not in plaint text
    11. KMS can only help in encrypting upto 4KB of data per call
        1. If data > 4KB, use envelope encryption
    12. To give access to KMS
        1. Make sure the key policy allows the user
        2. Make sure the IAM policy allows the API calls
    13. KMS keys are bound to specific region
    14. KMS key policies
        1. They are resource policies
        2. You cannot control access without them
        3. Default policy
            1. Created if you don’t provide a specific KMS key policy
            2. Complete access to the key to the root user - entire AWS account
            3. Give access to the IAM policy to the key
        4. Customer KMS key policy
            1. Define users, roles that can access the KMS key
            2. Define who can administer the key
            3. Useful for cross-account access of your KMS key
    15. Copy snapshot across accounts
        1. Create a snapshot that is encrypted with your own CMK
        2. Attach a KMS key policy to authorise cross-account access
        3. Share the encrypted snapshot
        4. In the target create a copy of snapshots encrypt it with KMS key in account
        5. Create a volume from the snapshot
    16. KMS key rotation
        1. Automatic rotation
            1. For customer managed CMK
                1. If enabled: automatic key rotation happens every 1 year
                2. Previous key is kept active to decrypt old data
                3. New key has same CMK ID only the backing key is changed
            2. Cannot rotate AWS managed CMK
        2. For manual key rotation
            1. When you want to rotate key according to your time period eg 90 days
            2. New key created has different CMK ID
            3. You should keep the previous key active to decrypt old data
            4. It is better to use alias to hide the change of key for the application
                1. Update Alias to hide the change of key application
3. SSM parameter store
    1. Secure storage for configurations and secrets
    2. Optionals seamless encryption using KMS
    3. Serverless, scalable, durable, easy SDK
    4. Version tracking of configurations
    5. Integration with cloudformation and has cloudwatch events feature
    6. Can store plain text configuration
    7. Can create hierarchy to store parameters
    8. We can even reference secrets from secrets manager and even reference an AWS asset
    9. Can integrate with lambda function
        1. Lambda function should have permissions
        2. Boto3 client is used to get connection with SSM
    10. Max size of parameter value is 4KB
    11. Parameters policies
        1. eg: Allow to assign TTL to a parameter
        2. Can assign multiple policies at a time
    12. Can get parameters by path, also recursively
    13. To get encrypted configurations in plaintext you need to provide withDecryption Flag
        1. You /lambda should have access to decrypt the configuration
4. AWS Secrets Manager
    1. Used for storing secrets - encrypted using KMS
    2. Can force rotation of secrets every X days
    3. Automate generation of secrets on rotation using lambda
    4. Integration with RDS
5. Cloud HSM
    1. Dedicated hardware where you manages your own encryption keys entirely
    2. AWS will provision the hardware
    3. HSM is tamper resistant
    4. Supports both symmetric and asymmetric keys
    5. You need to use CloudHSM client Software
    6. Redshift supports cloudHSM for database encryption
    7. Good option to use with SSE-C encryption
    8. Have high availability, spread across Multi AZ
    9. They are single tenant (KMS is multi tenant)
    10. Can import asymmetric keys (Not possible in KMS)
    11. Can be shared across VPCs using VPC peering
6. AWS Shield
    1. To protect against DDoS attack
    2. AWS Shield Standard
        1. Free service
        2. Provides protection from attacks such as SYN/UDP floods and other layer 3/4 attacks
    3. AWS shield advance
        1. Optional DDoS migration service
        2. Protects against sophisticated attacks on EC2, ELB,CloudFront, Global accelerator and route 53
        3. 24/7 access to DDoS response team (DRP)
    4. Protect against higher fees during usage spike due to DDoS
7. Web Application Firewall (WAF)
    1. Protects web applications from common web exploits (layer 7 -HTTP)
    2. Deployed on ALB, API gateway, CloudFront
    3. Define Web ACL to use WAF
        1. Rules on IP, HTTP headers, HTTP body
        2. Protect from common attacks like SQL injection and cross site scripting (XSS)
        3. Size constraints, geo-match to block specific countries
        4. Rate based rules (DDoS protection)
    4. Firewall manager
        1. Manages rules in all accounts of an AWS Organisation
        2. Common set of security rules
8. AWS GuardDuty
    1. Intelligent threat discovery to protect AWS Account using ML
    2. Detects anomaly detection, 3rd party data
    3. Input data
        1. CloudTrail logs
            1. To determine unusual API calls, unauthorised deployments
        2. VPC flow logs
            1. To determine unusual internal traffic, unusual IP address
        3. DNS logs
            1. Compromised EC2 instances, sending encoded data with DNS queries
    4. Can setup cloudwath event rules to be notified inc are of findings
        1. Can trigger SNS, Lambda
    5. Can protect against CryptoCurrency attacks
9. AWS inspector
    1. Automated Security Assessments for EC2 Instances
    2. Analyse running OS against known vulnerabilities
    3. Need to install AWS Inspector agents
        1. No need to install for network assessments
    4. Analyse against unintended network accessibility 
10. Macie
    1. It is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS
    2. Uses ML in backend
    3. Helps identify and alert you. Sensitive data eg Personal identifiable information (PII)
11. Shared Responsibility Model
    1. AWS responsibility - Security of the cloud
        1. Protecting infrastructure
        2. Managed services
    2. Customer responsibility - Security in cloud
        1. Usage of resources/services
        2. Encrypting application data
    3. Shared controls
        1. Patch management
        2. Control management

 
