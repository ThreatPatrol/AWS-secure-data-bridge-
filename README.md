<div align="center">
<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/663839afa19c18648514dd0475289f70b3170b1a/1713907387571.png" width="550" hieght="550">
</div>

## Data bridge seurity using AWS IAM, Lambda and S3  (Securing data pipeline between cloud services)
This project demonstrates a secure serverless data ingestion pipeline using AWS Lambda. The pipeline retrieves data from a public API, stores it in an Amazon S3 bucket, and ensures data integrity throughout the process. The project showcases best practices for security, data protection, and compliance, making it an ideal reference architecture for organizations seeking to build secure and reliable data pipelines on AWS.

### Project Overview

Our project will involve:

1. Creating an AWS Lambda function that queries an external API. (OpenWeatherMap API: Provides current and forecasted weather data) (Free tier available) <a href="https://home.openweathermap.org/users/sign_in"> Click Here </a>    
2. Storing the API response data in an Amazon S3 bucket.
3. Ensuring the security and integrity of our data and resources.

### Step 1: Creating an AWS Account and Setting up IAM (as a security professional this step is high priority) 

Before we can start building our project, we need to create an AWS account and set up IAM (Identity and Access Management). <a href="https://aws.amazon.com/free/compute/"> Click Here <a/>

Instead of using the root user, we will create an IAM user with the necessary permissions to perform daily tasks. (best practice) 

1. Log in to the AWS Management Console using your root account credentials.
2. Navigate to the IAM dashboard and click on "Users" in the left-hand menu.
3. Click on "Create user" and enter a username, such as "lambda-admin".
4. Select "Programmatic access" as the access type, as we will be using this user to interact with AWS services via the CLI and SDKs.
5. Click "Next: Review" and then "Create user".
6. Create a new group, such as "LambdaAdministrators", and attach the following policies:
   
    - "AWSLambdaBasicExecutionRole"
    - "AmazonS3ReadOnlyAccess"
    - "CloudWatchLogsReadOnlyAccess"
      
8. Add the newly created IAM user to the "LambdaAdministrators" group.

In addition to creating an IAM user, we also need to define roles that will be assumed by the Lambda function.

1. Navigate to the IAM dashboard and click on "Roles" in the left-hand menu.
2. Click on "Create role" and select "AWS service" as the trusted entity type.
3. Choose "Lambda" as the service that will use the role.
4. Attach the following policies to the role:
    
    - "AWSLambdaBasicExecutionRole"
    - "AmazonS3ReadOnlyAccess"
    - "CloudWatchLogsReadOnlyAccess"
      
5. Name the role, such as "LambdaExecutionRole".
6. Note the ARN of the role, as we will need it later when creating the Lambda function.

By defining roles and creating an IAM user, we have established a secure and scalable way to manage access to our AWS resources.

By following these steps, we have created an IAM user with the necessary permissions to perform daily tasks, while also following best practices for security and access management.

Note: As a best practice, it's recommended to use the principle of <a href="https://www.beyondtrust.com/blog/entry/what-is-least-privilege#:~:text=Least%20Privilege%20Access%20&%20Zero%20Trust,Download%20the%20Guide">**least privilege**<a/>  when assigning permissions to IAM users and groups. This means granting only the necessary permissions required to perform specific tasks, rather than granting excessive permissions.

We've just created an IAM user with programmatic access. This user will be used to execute our Lambda function.

Step 2: Creating an IAM Role for Lambda Execution

Now that we have an IAM user, we need to create an IAM role that will be used to execute our Lambda function.

1. In the AWS Management Console, navigate to the IAM dashboard.
2. Click on "Roles" and then "Create role."
3. Choose "AWS service" as the trusted entity type.
4. Select "Lambda" as the service that will use the role.
5. Click "Next: Review" and then "Create role."

Configuring the IAM Role

Now that we have created the IAM role, we need to configure it with the necessary permissions.

1. In the IAM dashboard, click on the role we just created (it should be named "lambda-execution-role").
2. Click on the "Permissions" tab.
3. Click on "Attach policies."
4. Search for the "AWSLambdaExecute" policy and select it.
5. Click "Attach policy."

Creating a Policy for S3 Access

We also need to create a policy that will allow our Lambda function to access our S3 bucket.

1. In the IAM dashboard, click on "Policies" and then "Create policy."
2. Choose "Custom policy" and then "JSON."
3. Paste the following policy document:


{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowS3Access",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
        }
    ]
}


Replace "YOUR_BUCKET_NAME" with the name of the S3 bucket we will create later.

4. Click "Review policy" and then "Create policy."

Attaching the S3 Policy to the IAM Role

Now that we have created the S3 policy, we need to attach it to the IAM role.

1. In the IAM dashboard, click on the role we created earlier (it should be named "lambda-execution-role").
2. Click on the "Permissions" tab.
3. Click on "Attach policies."
4. Search for the policy we created earlier (it should be named "S3AccessPolicy").
5. Click "Attach policy."


