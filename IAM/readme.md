# IAM

1. IAM stands for Identity and access management
2. It is a global service
3. Users are people within your organisation and can be grouped
4. Groups only contain users not other groups
5. A user can either don’t belong to a group or can also be belong to multiple groups
6. IAM permissions
    1. User or groups can be assigned JSON documents called policies
    2. These policies define the permissions of the users
    3. You should apply the least privilege principle
    4.  Consists of
        1. Version —> Policy language version
        2. Id —> An identifier of the policy (Optional)
        3. Statement
            1. Sid —> An identifier for the statement (Optional)
            2. Effect —> Allow or deny (Effect)
            3. Principle —> Account/user/role tow which this policy applied tp
            4. Action —> list of actions the policy allows or denies
            5. Resource —> List of resources to which the actions applied to
            6. Condition —> Conditions for when this policy is in effect (optional)
7. IAM MFA (Multi factor authentication)
    1. Virtual MFA device
        1. Google authenticator -> Single device
        2. Authy -> multiple tokens on a single device
    2. Universal 2nf Factor (U2F) Security key
        1. Yubikey  -> Support multiple root &. User account
    3. Hardware key Fob MFA device
    4. Hardware key Fob MFA device for AWS GovCloud (US)
8. Options to access AWS
    1. AWS Management Console (password + MFA)
    2. AWS command line interface (CLI) (Access keys)
    3. AWS SDK - for code (Access keys)
9. AWS cloudshell
    1. It is a web based shell application
    2. We can run cli commands here
    3. The default region will be the region through which you started cloudshell
    4. Even if you closed and reopen cloudshell the data will still be present
    5. You can download and upload files also
    6. You can have multiple tabs also
    7. It is not available in all regions
10. IAM roles
    1. We can assign permissions to AWS services with IAM roles
    2. Common roles
        1. EC2 instance roles
        2. Lambda function roles
        3. Cloud formation roles
11. IAM security tools
    1. IAM credentials Report (Account level)
        1. A report that lists all your account’s users and the status of their credentials
    2. IAM Access advisor (user-level)
        1. Access advisor shows the service permissions granted to a user and when those services were last accessed
        2. You can use this info to revise your policies
12. Best practices
    1. Do not use the root account except for AWS account setup
    2. One physical user = one AWS user
    3. Assign users to groups and assign permissions to groups
    4. Create strong password policy and MFA
    5. Create and use roles to give permission to AWS services
    6. Use access keys for programmatic access (cli/sdk)
    7. Audit permissions of your account with IAM credentials report
    8. Groups can contain user only not other groups
13. You can use IAM policies defined by AWS or you can create your own policies (for stricter control)
14. You can add inline polices
    1. These policies cannot be added to other roles
    2. Not recommended
15. You can have versions of policies
16. Policy simulator
    1. To simulate actions that can be performed by user, group, role or different resources
17. Ec2 instance metadata
    1. It allows AWS ec2 instance to learn about themselves without using an IAM role for that purpose
    2. URL is http://169.254.169.254/latest/meta-data
    3. Will only work with EC2 instances
    4. You can retrieve the IAM role name from metadata but your cannot retrieve the IAM policy
18. AWS SDK
    1. To perform actions on AWS through code
    2. Official SDKs are
        1. Java
        2. .Net
        3. Node.js
        4. PHP
        5. Python (boto3)
        6. Go
    3. If you don’t specify or configure a region, then us-east-1 will be chosen by default

