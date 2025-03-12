<div align="center">
<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/663839afa19c18648514dd0475289f70b3170b1a/1713907387571.png" width="550" hieght="550">
</div>

## Data bridge seurity using AWS IAM, Lambda and S3  (Securing data pipeline between cloud services)
This project demonstrates a secure serverless data ingestion pipeline using AWS Lambda. The pipeline retrieves data from a public API, stores it in an Amazon S3 bucket, and ensures data integrity throughout the process. The project showcases best practices for security, data protection, and compliance, making it an ideal reference architecture for organizations seeking to build secure and reliable data pipelines on AWS.

### Project Overview

Our project will involve:

1. Creating an AWS Lambda function that queries an external API. (OpenWeatherMap API: Provides current and forecasted weather data. (Free tier available) <a href="https://home.openweathermap.org/users/sign_in"> Click Here </a>    
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
7. Add the newly created IAM user to the "LambdaAdministrators" group.

By following these steps, we have created an IAM user with the necessary permissions to perform daily tasks, while also following best practices for security and access management.

Note: As a best practice, it's recommended to use the principle of **least privilege** when assigning permissions to IAM users and groups. This means granting only the necessary permissions required to perform specific tasks, rather than granting excessive permissions.

We've just created an IAM user with programmatic access. This user will be used to execute our Lambda function.


