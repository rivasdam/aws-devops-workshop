# DevOps Workshop AWS v1
AWS Workshop covering Devops Services

***Note: 
This version covers a Blue/Green Deployment scenario with AWS Fargate on Amazon ECS integrating the Code services (CodeCommit, CodeBuild, CodeDeploy and CodePipeline).
More updates to come. Stay tuned!***

*What we will cover*:
- Setup of a Cloud9 environment
- Deployment of required infrastructure including Amazon Elastic Container Service with Fargate and ECR repository
- Deploy and use a modernized pipeline using AWS CodePipeline, CodeBuild, and CodeBuild

### Setting up the lab environment
*Create a Workspace - AWS Cloud9 IDE - Setup*

AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes pre-packaged with essential tools for popular programming languages and the AWS Command Line Interface (CLI) pre-installed so you don't need to install files or configure your laptop for this workshop. Your Cloud9 environment will have access to the same AWS resources as the user with which you logged into the AWS Management Console.
Take a moment now and setup your Cloud9 development environment

*Step-by-step Instructions*

1. Go to the AWS Management Console, click Services then select Cloud9 under Developer Tools.
2. Click *Create environment*.
3. Enter MyDevEnvironment into *Name* and optionally provide a Description.
4. Click *Next step*.
5. You may leave Environment settings at their defaults except the instance type. Make sure of changing it to launch a new *t3.small* EC2 instance instead of the *t2.micro* selected by default. which will be paused after 30 minutes of inactivity.
6. Click *Next step*.
7. Review the environment settings and click *Create environment*. It will take several minutes for your environment to be provisioned and prepared.

[Image: image.png]

1. Once ready, your IDE will open to a welcome screen. Below that, you should see a terminal prompt similar to:
    
[Image: image.png]

You can run AWS CLI commands in here just like you would on your local computer. Verify that your user is logged in by running the following command.

````
aws sts get-caller-identity
````

You'll see output indicating your account and user information:

````
{    
     "Account": "123456789012",    
     "UserId": "AKIAI44QH8DHBEXAMPLE",    
     "Arn": "arn:aws:iam::123456789012:user/user"
}
````

Keep your AWS Cloud9 IDE opened in a tab throughout this workshop as we'll use it for activities like cloning, pushing changes to repository and using the AWS CLI.

*Deploy pre-requisites*


1. Download the setup files, unzip and navigate to the setup directory:

````
curl -o setup.zip https://d2u1mnqz9x7eh0.cloudfront.net/setup.zip
unzip setup.zip
cd setup/
````

Here we are going to execute a setup script which will do the following:

* Install required tools and set required environment variables (current Region, Account ID, Repository URI, etc.)
* Create initial IAM roles
* Create ECR Repository and push sample application container image
* Create the required infrastructure for the workshop:
    * ECS cluster with Fargate
    * Application Load Balancer
    * Task definition
    * Networking: VPC, Public and Private Subnets, NAT Gateway
    * Sample Application Deployed

2. Let's execute the setup script then!

````
chmod +x setup.sh
./setup.sh
````

This process will take several minutes as it need to deploy several resources which are dependent on each other in most cases, for example the ECS Service won't be created until the Load Balancer and it's Listener are created and configured.

Wait until *(STEP 9)* is set, meanwhile you can go to the CloudFormation console (https://console.aws.amazon.com/cloudformation) to see how is it going.  Just click on the cloud9 logo (upper left corner) and select Go to your dashboard.

Example screens below:
[Image: image.png]
Take a look at the Events tab to see the progress.
[Image: image.png]
[Image: image.png]
Once the devops-cluster stack reaches the CREATE_COMPLETE status, we can see that the template shows many outputs.
[Image: image.png]

3. Once it finishes we can review if everything was setup as expected. Just go to the CloudFormation Outputs tab and click on the ExternalUrl link. The resulting website should look something similar like this:
    

[Image: image.png]As we can see, it is a simple web page that we are going to use during this exercise.

***Note*** We also get the URL as a final output from the setup script as well in the Cloud9 console. Either way we should access the same page.

If you walk through the Resources tab in CloudFormation, you will see what has been deployed, the key components that we can observe are: ECS Cluster, the Task Definition, the Service running 2 Tasks based on the definition and the Load Balancer with one listener and 2 target groups (one of them is not being used right now). Feel free to explore the different configuration that were done during the setup.

4. Ok, now we are ready to move on with the workshop itself. Let's get started!




````
Testing code block
````
