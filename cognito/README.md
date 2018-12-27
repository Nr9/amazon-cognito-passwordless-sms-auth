# Cognito E-Mail Auth Backend

This is an AWS Serverless Application. If you deploy it, this is what you get:

- An Amazon Cognito User Pool, pre-configured with AWS Lambda triggers to implement passwordless e-mail auth
- An Amazon Cognito User Pool Client, that you can use to integrate with the User Pool
- The needed Lambda functions that serve as User Pool triggers
- The permissions on the Lambda functions so that the User Pool may invoke them

An example client web app that is ready to run against this Serverless Application can be found at [../client](../client)

## Deployment instructions

Deploy either through the Serverless Application Repository or with the AWS SAM CLI

### Deployment trough Serverless Application Repository

This is the easiest path. Find the Serverless Application in the [Repository](https://console.aws.amazon.com/serverlessrepo/) using tags "cognito" and "passwordless" or navigate to it directly with [this link](https://serverlessrepo.aws.amazon.com/#/applications/arn:aws:serverlessrepo:us-east-1:520945424137:applications~cognito-passwordless-email-auth).

#### Pre-requisite

- You need to have a [verified E-Mail address in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses-procedure.html) to send the e-mails from. Also, if you don't want the mails to end-up in spam folders, [verify the domain as well](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-domain-procedure.html).

### Alternative Deployment with AWS SAM CLI

#### Pre-requisites

1. Download and install [Node.js](https://nodejs.org/en/download/)
2. Download and install [AWS SAM CLI](https://github.com/awslabs/aws-sam-cli)
3. Of course you need an AWS account and necessary permissions to create resources in it. Make sure your AWS credentials can be found during deployment, e.g. by making your AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY available as environment variables.
4. You need to have a [verified E-Mail address in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses-procedure.html) to send the e-mails from. Also, if you don't want the mails to end up in spam folders, [verify the domain as well](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-domain-procedure.html).
5. You need an existing S3 bucket to use for the SAM deployment. Create an empty bucket.

NOTE: To deploy this application _**please pick an AWS Region in which you can use Amazon Simple E-Mail Service (i.e. us-east-1, us-west-2 or eu-west-1)**_ and create all resources (including the S3 bucket) in that region. This is not a hard requirement for setting up e-mail auth in Cognito in general; but it is so in this demo application to keep things simple.

#### How to deploy the Serverless Application with AWS SAM CLI

1. Clone this repo: `git clone https://github.com/ottokruse/cognito-email-auth-backend`
2. Install dependencies: `cd cognito-email-auth-backend && npm install`
3. Set the following environment variables (all mandatory):
  - S3_BUCKET_NAME='the bucket name of the bucket you want to use for your SAM deployment'
  - SES_FROM_ADDRESS='the verfied e-mail address in SES the e-mails will be sent from'
  - STACK_NAME='the name you want the CloudFormation stack to be created with'
  - USER_POOL_NAME='the name you want your User Pool to be created with'
4. Build and deploy the application: `npm run bd` This runs AWS SAM CLI

if that succeeded, you have succesfully deployed your application. The outputs of the CloudFormation stack will contain the ID's of the User Pool and Client, that you can use in your client web app.