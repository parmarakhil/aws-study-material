# IAM Advance
	
1. AWS STS
    1. Security Token service
    2. Allow to grant limited and temporary access to AWS resources
    3. Token is valid for 1 hour
    4. AssumeRole
        1. Within own account for enhanced security
        2. For cross account access
            1. We assume role in target account to perform actions
    5. AssumeRoleWithSAML
        1. Return credentials for users logged with SAML
    6. AssumeRoleWithWebIdentity
        1. Returns credentials for users logged with an IdP eg Facebook
        2. AWS recommends using Cognito instead
    7. GetSessionToken
        1. For MFA, from a user or AWS account root user
    8. Using STS to assume role
        1. Define IAM role within own or others account
        2. Define which principals can access the IAM role
        3. Use STS to retrieve credentials and Assume role you have access to using AssumeRole API
        4. Temporary credentials can be valid between 15 minutes to 1 hour
2. Identity Federation
    1. Lets users outside of AWS assume temporary role for accessing AWS resources
    2. The users assume identity provided access role
    3. You don’t need to create IAM users the user management is outside of AWS
    4. Federations can be through
        1. SAML 2.0
            1. To integrate with Active directory or ADFS with AWS
            2. No need to create IAM user for each employee
            3. Provide access to AWS console or CLI
            4. Uses the STS AssumeRoleWithSAML API
            5. It is old way, new service is AWS Single Sign On (SSO)
        2. Custom Federation
            1. Only if we don’t have compatibility with SAML 2.0
            2. Identity broker must determine the appropriate IAM role
            3. Use STS AssumeRole or GetFederationToken API
        3. Web identity federation with Cognito
            1. Provide direct access to AWS resource from client side
            2. Provide temporary access to upload file to S# using Facebook login
        4. Web identity federation without Cognito
            1. AssumeRoleWithWebIdentity
            2. Not recommended way. Instead use cognate
        5. Single sign on
        6. Non SAML with AWS Microsoft AD
            1. Database of objects like users, accounts, computers, printers
            2. Centralised security management
            3. AWS managed Microsoft AS
                1. Create own AS in AWS, manage users locally, supports MFS
                2. Establish trust connections with on-premise AD
            4. AD connector
                1. Direct gateway proxy to redirect to on-premise AD
                2. Users are managed on the on-premise AD
            5. Simple AD
                1. AD-compatible managed directory on AWS
                2. Cannot be joined with on-premise AD
3. Organisations
    1. Global service to manage multiple AWS accounts
    2. The main account is the master (Management) account which cannot be changed
    3. Other accounts are member accounts
    4. Member accounts can only be part of one organisation
        1. They can be moved to different organisation
    5. Consolidated billing for all accounts
    6. Pricing benefits for aggregated usage
    7. API is available to automate AWS account creation
    8. Multi account strategies
        1. Account per department, team (detest, prod),
        2. For better resource isolation
        3. Separate per account service limits
        4. Vs One account multi VPC
            1. Resource can talk to one another in multi VPC in 1 account
        5. Enable cloud trail on all accounts, send logs to central S3 account
        6. Send cloudwatch logs to central logging account
        7. Cross Account role for admin purpose
    9. Organisation Units (OU)
        1. To organise units
        2. Business unit
        3. Env lifecycle
        4. Project wise unit
    10. Service control policies
        1. Cannot be applied to master account
        2. Whitelist or blacklist IAM actions
        3. Applied at OU or account level
        4. SCP is applied to all users and roles of the account including root account
        5. The SCP does not affect service-link roles
            1. Service linked roles enable other AWS services to integrate with AWS organisations
            2. Can’t be restricted by SCPs
        6. SCP must have explicit allow
        7. Use
            1. Restrict access to certain services eg: S3
            2. Enforce PCI compliance by explicitly denying services
    11. To move account
        1. Remove account from old Org
        2. Send invite to new Org
        3. Accept invite from member account
        4. If you want to migrate master account 
            1. Remove all members of the old Org
            2. Delete the old Org
            3. Send invite from new Org to all members
            4. Accept the invite from all members
4. IAM
    1. IAM Conditions
        1. AWS:SourceIP
            1. Restrict the Clint IP from which the API calls are being made
        2. AWS:RequestedRegion
            1. Restricts the region the API cals are made to
        3. Restrict based on tags
        4. Force MFA
    2. IAM for S3
        1. ARN without “/“ applies to bucket level permission
        2. ARN with “/“ applies to object level permission
        3. IAM roles vs Resource based policies
            1. When you assume a role (user, service or application) you give up original permissions assigned to the role
            2. When using a resource based policy, the principal doesn’t have to give up his permission
                1. Eg: User in Account A needs to scan dynamoDB table in Account A and then dump it in S3 bucket in Account B
    3. IAM Permission boundaries
        1. IAM permission boundaries are supported for users and roles and not groups
        2. Advance feature to use a managed policy to set the maximum permissions an IAM entity can get
        3. Can be used in combinations of AWS Organisations SCP
        4. Use case
            1. To delegate responsibilities
            2. To allow developers to self-assign and manage permission
            3. Restrict a specific user
5. Resource Access Manager (RAM)
    1. Share AWS resources that you own with other AWS resources
    2. Share with any account or within your organisation
    3. Avoids resource duplication
    4. VPC Subnets
        1. Allow to have all the resources launched in the same subnets
        2. Must be from the same AWS Organisation
        3. Cannot share security groups and default VPC
        4. Participants can manage their own resources in there
        5. Participants can’t view, modify, delete resources that belong to other participants or the owner
        6. Each account is responsible for its resources
        7. Cannot view, modify or delete other resources in other account
        8. If network is shared the applications can talk to each other using private IPs
        9. Security groups can be referenced between accounts
        10. 
    5. AWS Transit gateway
    6. Route53
    7. License manager
6. AWS Single sign on (SSO)
    1. Centrally manage single sign on to access multiple accounts and 3rd party business applications
    2. Integrated with AWS organisations
    3. Supports SAML 2.0 Markup
    4. Integration with on-premise AD Centralised auditing with cloud trail
    5. Vs AssumeRoleWithSAML
        1. Need to setup 3rd party IDP login portal
