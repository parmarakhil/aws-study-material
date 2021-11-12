# CICD

1. Code Commit
    1. A fully-managed source control service that hosts secure Git-based repositories, similar to Github.
    2. You can create your own code repository and use Git commands to interact with your own repository and other repositories.
    3. You can store and version any kind of file, including application assets such as images and libraries alongside your code.
    4. The AWS CodeCommit Console lets you visualize your code, pull requests, commits, branches, tags and other settings.
    5. You can migrate a Git repository to a CodeCommit repository in a number of ways: by cloning it, mirroring it, or migrating all or just some of the branches.
    6. You can also migrate your local repository in your machine to CodeCommit.
    7. You can transfer your files to and from AWS CodeCommit using HTTPS or SSH. For HTTPS authentication, you use a static username and password. For SSH authentication, you use public keys and private keys.
    8. Your repositories are also automatically encrypted at rest through AWS KMS using customer-specific keys.
    9. You can give users temporary access to your AWS CodeCommit repositories through a number of methods: SAML, Federation, Cross-Account Access, or through third party authorizes with Amazon Cognito.
2. Code Build
    1. A fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy.
    2. A build project defines how CodeBuild will run a build. It includes information such as where to get the source code, which build environment to use, the build commands to run, and where to store the build output.
    3. A build environment is the combination of operating system, programming language runtime, and tools used by CodeBuild to run a build.
    4. The build specification is a YAML file that lets you choose the commands to run at each phase of the build and other settings. Without a build spec, CodeBuild cannot successfully convert your build input into build output or locate the build output artifact in the build environment to upload to your output bucket.
        1. If you include a build spec as part of the source code, by default, the build spec file must be named buildspec.yml and placed in the root of your source directory.
    5. A collection of input files is called build input artifacts or build input and a deployable version of a source code is called build output artifact or build output.
3. Code Deploy
    1. A fully managed deployment service that automates software deployments to a variety of compute services such as Amazon EC2, AWS Fargate, AWS Lambda, and your on-premises servers.
    2. An Application is a name that uniquely identifies the application you want to deploy. CodeDeploy uses this name, which functions as a container, to ensure the correct combination of revision, deployment configuration, and deployment group are referenced during a deployment.
    3. Compute platform is the platform on which CodeDeploy deploys an application (EC2, ECS, Lambda, On-premises servers).
    4. Deployment configuration is a set of deployment rules and deployment success and failure conditions used by CodeDeploy during a deployment.
    5. Deployment group contains individually tagged instances, Amazon EC2 instances in Amazon EC2 Auto Scaling groups, or both
    6. A Revision for an AWS Lambda deployment is a YAML- or JSON-formatted application specification file (AppSpec file) that specifies information about the Lambda function to deploy. The revision can be stored in Amazon S3 buckets.
    7. Target revision is the most recent version of the application revision that you have uploaded to your repository and want to deploy to the instances in a deployment group.
    8. A deployment goes through a set of predefined phases called deployment lifecycle events. A deployment lifecycle event gives you an opportunity to run code as part of the deployment.
        1. ApplicationStop
        2. DownloadBundle
        3. BeforeInstall
        4. Install
        5. AfterInstall
        6. ApplicationStart
        7. ValidateService
4. Code Pipeline
    1. A fully managed continuous delivery service that helps you automate your release pipelines for application and infrastructure updates.
    2. You can easily integrate AWS CodePipeline with third-party services such as GitHub or with your own custom plugin. 
    3. A pipeline defines your release process workflow, and describes how a new code change progresses through your release process.
    4. A pipeline comprises a series of stages (e.g., build, test, and deploy), which act as logical divisions in your workflow. Each stage is made up of a sequence of actions, which are tasks such as building code or deploying to test environments.
        1. Pipelines must have at least two stages. The first stage of a pipeline is required to be a source stage, and the pipeline is required to additionally have at least one other stage that is a build or deployment stage
    5. A revision is a change made to the source location defined for your pipeline. It can include source code, build output, configuration, or data. A pipeline can have multiple revisions flowing through it at the same time.
    6. A stage is a group of one or more actions. A pipeline can have two or more stages.
    7. An action is a task performed on a revision. Pipeline actions occur in a specified order, in serial or in parallel, as determined in the configuration of the stage
        1. You can add actions to your pipeline that are in an AWS Region different from your pipeline.
        2. There are six types of actions
            1. Source
            2. Build
            3. Test
            4. Deploy
            5. Approval
            6. Invoke
    8. An approval action prevents a pipeline from transitioning to the next action until permission is granted. This is useful when you are performing code reviews before code is deployed to the next stage.
5. Code Star
    1. A cloud‑based software development service that provides the tools you need to quickly develop, build, and deploy applications on AWS.
    2. CodeStar is commonly used along with CodeCommit, CodeBuild, CodeDeploy, and CodePipeline for a robust CI/CD toolchain.
    3. Each AWS CodeStar project comes with a project management dashboard, including an integrated issue tracking capability that uses Atlassian JIRA Software.
    4. AWS CodeStar provides you a variety of project templates to start developing applications on different compute platforms such as Amazon EC2, AWS Lambda, and AWS Elastic Beanstalk. Templates types include:
        1. Web service – A web service is used for tasks that run in the background, such as calling APIs.
        2. Web application – A web application features a UI. 
        3. Static web page – For projects on an HTML website.
        4. Alexa skill – For projects on an Alexa skill with an AWS Lambda function.
        5. Config rule – Choose this template if you want a project for an AWS Config rule that lets you automate rules across AWS resources in your account.
    5. AWS CodeStar projects supports the following programming languages:
        1. Java
        2. JavaScript
        3. PHP
        4. Ruby
        5. Python
        6. ASP.NET
    6. AWS CodeStar includes an integrated development toolchain for your project. Team members push code, and changes are automatically deployed.
    7. You can add users to your dashboard via IAM Users or email invite.
    8. CodeStar Roles
        1. Owner – Can add and remove team members, change the project dashboard, and delete the project.
        2. Contributor – Can change the project dashboard and contribute code if the code is stored in CodeCommit, but cannot add or remove team members or delete the project.
        3. Viewer – Can view the project dashboard, project code if the code is stored in CodeCommit, and the state of the project, but cannot move, add, or remove tiles from the project dashboard.