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

<div align="center">
<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/7aca57fe077afa2efd7f31a8a21904a0bf72e3b3/1_uzSunZS144KT-DGHrIXSkg.png" width="350" hieght="350">
</div>

### Step 2: Creating an IAM Role for Lambda Execution

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
3. Copy and print the following policy document:


<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/4ec37f59ba8218a00ac87b7e572cd450e13da187/Annotation%202025-04-16%20083822.png" width="750" hieght="750">


4. Click "Review policy" and then "Create policy."

Attaching the S3 Policy to the IAM Role

Now that we have created the S3 policy, we need to attach it to the IAM role.

1. In the IAM dashboard, click on the role we created earlier (it should be named "lambda-execution-role").
2. Click on the "Permissions" tab.
3. Click on "Attach policies."
4. Search for the policy we created earlier (it should be named "S3AccessPolicy").
5. Click "Attach policy."

<div align="center">
<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/0be12053909708f589d00a9bc8145af209cbab69/aws-s3-logo.png" width="350" hieght="350"> 
</div>

### Step 3: Creating an S3 Bucket

Now that we have our IAM role and policy in place, let's create an S3 bucket to store our data.

1. Log in to the AWS Management Console.
2. Navigate to the S3 dashboard.
3. Click on "Create bucket."
4. Enter a unique name for your bucket (e.g., "my-lambda-bucket").
5. Choose a region for your bucket (e.g., "US East (N. Virginia)").
6. Click "Create bucket."

Configuring S3 Bucket Properties

Now that we have created our S3 bucket, let's configure some important properties.

1. Go to the S3 dashboard and select the bucket you just created.
2. Click on the "Properties" tab.
3. Click on "Edit" next to "Bucket policy."
4. Paste the following bucket policy:

<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/4c351234b3f4ff39541a4e2839b83ec392125229/Annotation%202025-04-16%20090854.png" width="750" hieght="750">


Replace "my-lambda-bucket" with the name of your S3 bucket.

5. Click "Save changes."

Enabling Versioning and Server-Side Encryption

To ensure data integrity and security, let's enable versioning and server-side encryption for our S3 bucket.

1. Go to the S3 dashboard and select the bucket you created.
2. Click on the "Properties" tab.
3. Click on "Edit" next to "Versioning."
4. Select "Enable versioning."
5. Click "Save changes."

6. Click on "Edit" next to "Server-side encryption."
7. Select "Enable server-side encryption."
8. Choose "AWS Key Management Service (KMS)" as the encryption type.
9. Click "Save changes."

Versioning:

Versioning is a feature in S3 that allows you to keep multiple versions of an object (file) in your bucket. When you enable versioning, S3 stores every version of an object, including the original version and any subsequent updates.

Here's an example:

1. You upload a file called "example.txt" to your S3 bucket.
2. Later, you update the file and upload a new version of "example.txt".
3. With versioning enabled, S3 stores both the original version and the updated version of the file.

Versioning provides several benefits:

- Data protection: You can recover previous versions of an object in case of accidental deletion or overwriting.
- Audit trail: You can track changes to objects over time.
- Compliance: Versioning can help you meet regulatory requirements for data retention and auditing.

#### Server-Side Encryption (SSE):

Server-Side Encryption (SSE) is a feature in S3 that encrypts your data at rest, meaning that your data is encrypted while it's stored in S3.

Here's how it works:

1. You upload data to your S3 bucket.
2. S3 encrypts the data using a encryption key.
3. The encrypted data is stored in your S3 bucket.

SSE provides several benefits:

- Data confidentiality: Your data is protected from unauthorized access.
- Compliance: SSE can help you meet regulatory requirements for data encryption.
- Security: SSE provides an additional layer of security for your data.

There are three types of SSE in S3:

- SSE-S3: S3 manages the encryption keys.
- SSE-KMS: You manage the encryption keys using AWS Key Management Service (KMS).
- SSE-C: You manage the encryption keys and provide them to S3 for encryption.

In our project, we enabled SSE-KMS, which means that AWS KMS manages the encryption keys for our S3 bucket.

### Step 4: Creating an AWS Lambda Function

Now that we have our S3 bucket and IAM role set up, let's create an AWS Lambda function that will query the API and store the data in our S3 bucket.

1. Log in to the AWS Management Console.
2. Navigate to the Lambda dashboard.
3. Click on "Create function."
4. Choose "Author from scratch."
5. Select "Python" as the runtime.
6. Choose the IAM role we created earlier (it should be named "lambda-execution-role").
7. Set the handler to "index.lambda_handler".
8. Click "Create function."

Configuring the Lambda Function

Now that we have created our Lambda function, let's configure it to query the API and store the data in our S3 bucket.

1. In the Lambda function configuration, click on the "Configuration" tab.
2. Scroll down to the "Environment variables" section.
3. Click on "Edit".
4. Add the following environment variables:

- API_URL: the URL of the API you want to query
- API_KEY: the API key for the API (if required)
- S3_BUCKET_NAME: the name of the S3 bucket we created earlier

5. Click "Save".

Writing the Lambda Function Code

Now that we have configured our Lambda function, let's write the code that will query the API and store the data in our S3 bucket.

I'll provide you with a sample Python code that you can use as a starting point:

<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/444f06f648765afedf779cd62547d3590bdd9c6c/Annotation%202025-04-16%20094330.png" width="1050" hieght="1100">

This code queries the API using the requests library and stores the data in our S3 bucket using the boto3 library.

### Step 5: Testing the Lambda Function

Now that we have written the Lambda function code, let's test it to make sure it's working as expected.

1. In the Lambda function configuration, click on the "Test" button.
2. Select "Hello World" as the test event.
3. Click "Save and test".

The Lambda function will execute and you should see the output in the "Function logs" section.

Verifying the S3 Bucket

Let's also verify that the Lambda function is storing the data in our S3 bucket.

1. Go to the S3 dashboard.
2. Select the bucket we created earlier.
3. Click on the "Objects" tab.

You should see a new object named "data.json" in the bucket. This is the data that the Lambda function retrieved from the API and stored in the bucket.

<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/32ac8c2fa6acd87e8f094da789c696c513e5b1ae/Annotation%202025-04-16%20101528.png">

Troubleshooting
If you encounter any errors during testing, you can troubleshoot by:

1. Checking the Lambda function logs for errors.
2. Verifying that the API is returning the expected data.
3. Checking the S3 bucket permissions to ensure that the Lambda function has the necessary permissions to write to the bucket.


<div align="centre">
<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/ddccbf59908e163d80e7f417b5091de666dca785/download%20(5).jfif">  
</div>   

### Step 6: Configuring API Gateway

Now that we have our Lambda function working, let's configure API Gateway to act as an entry point for our API.

1. Log in to the AWS Management Console.
2. Navigate to the API Gateway dashboard.
3. Click on "Create API".
4. Choose "REST API".
5. Click "Build".

Creating a Resource

Let's create a resource for our API.

1. In the API Gateway dashboard, click on the "Resources" tab.
2. Click on "Create resource".
3. Enter a name for the resource (e.g. "data").
4. Click "Create resource".

Creating a Method

Let's create a method for our API.

1. In the API Gateway dashboard, click on the "Resources" tab.
2. Select the resource we created earlier.
3. Click on "Create method".
4. Choose "GET" as the method type.
5. Click "Save".

Configuring the Integration

Let's configure the integration for our API.

1. In the API Gateway dashboard, click on the "Resources" tab.
2. Select the resource we created earlier.
3. Select the "GET" method we created earlier.
4. Click on the "Integration Request" tab.
5. Choose "Lambda Function" as the integration type.
6. Select the Lambda function we created earlier.
7. Click "Save".

Deploying the API

Let's deploy our API.

1. In the API Gateway dashboard, click on the "Actions" tab.
2. Click on "Deploy API".
3. Choose "New stage" as the deployment stage.
4. Enter a name for the stage (e.g. "prod").
5. Click "Deploy".







