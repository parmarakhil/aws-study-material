# Well Architect framework

1. Reliability
    1. The ability of a system to recover from infra or service disruptions, dynamically acquire computing resources to meet demand and mitigate disruptions such as misconfigurations or transient network issues
    2. Test recovery procedures (disaster recovery)
    3. Auto recover from failure
    4. Scale horizontally
    5. Stop guessing capacity
    6. Manage change in automation
    7. Foundations - IAM, VPC, Service quotas, Trusted Advisor
    8. Change management - ASG, Cloudwatch, Cloudtrail, Config
    9. Failure management - Backups, Cloudformation, S3, S3 glacier, Route 53
2. Performance efficiency
    1. The ability to use computing resources efficiently to meet system requirements, and to maintain efficiency as demand changes and technologies evolve
    2. Democratise advanced technologies
    3. Go global in minutes
    4. Use serverless architectures 
    5. Experiment more often
    6. Mechanical sympathy (requirement determine tech to use)
    7. Selection - ASG, Lambda, EBS, S3, RDS
    8. Review - Cloudfromation, AWS News blog
    9. Monitoring - Cloudwatch, Lambda
    10. Tradeoffs - RDS, ElastiCache, Snowball, Cloudfront
3. Security
    1. Ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies
    2. Implement a strong identity foundation - IAM
    3. Enable traceability
    4. Apply security at all levels
    5. Automate security best practice
    6. Protect data in transit and rest
    7. Keep people away from data
    8. Prepare for security events
    9. IAM , STS, MFA token
    10. Detective control - config, Cloudtrail, CLoudwatch
    11. Infra protection - CLoudfront, VPC, Shield, WAF
    12. Data Protection - KMS, S3, ELB, EBS, RDS
    13. Incident response - IAM, Cloudfromation, Cloudwatch events
4. Cost optimisation
    1. Ability to run systems to deliver business value at lowest cost
    2. Adopt a consumption model
    3. Measure overall efficiency
    4. Stop spending money on data centre operations
    5. Analyze and attribute expenditure - use tags
    6. Use managed service to reduce total cost of ownership
    7. Expenditure awareness - Budgests, Cost and usage report, Cost explorer, Reserved instance reporting
    8. Cost effective resources - spot instance, reserved instance, S3 glacier
    9. Matching supply and demand - ASG, lambda
    10. Optimising Over time - Trusted advisor, Cost and usage report, AWS News Blog
5. Operational excellence 
	Ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedure
    1. Ability to run and monitor business process
    2. Perform operations as code - Infrastructure as code
    3. Annotated documentation
    4. Make frequent, small, reversible changes
    5. Refine operations procedures frequently
    6. Anticipate failure
    7. Learn from all operation failures
    8. Prepare - Cloudformation, config
    9. Operate - Cloudformation, config, cloudTrail, Cloudwatch, X-ray
    10. Evolve - Cloudformation, CodeBuild, CodeCommit, CodeDeploy, CodePipelin
6. Well Architect tool
7. AWS trusted Advisor
    1. High level AWS account assessment
    2. Analyse AWS accounts and provides recommendation on
        1. Cost optimisation
        2. Performance
        3. Security
        4. Fault tolerance
        5. Service limits
    3. Core checks and recommendations - all customers
    4. Can enable weekly email notification from the console
    5. Full trusted advisor - Available for business & Enterprise support plans
        1. Set cloudwatch alarms when reaching limits
        2. Programmatic access using AWS support API

