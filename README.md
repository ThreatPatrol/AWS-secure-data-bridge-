<div align="center">
<img src="https://github.com/ThreatPatrol/AWS-secure-data-bridge-/blob/663839afa19c18648514dd0475289f70b3170b1a/1713907387571.png" width="550" hieght="550">
</div>

## Data bridge seurity using AWS IAM, Lambda and S3  (Securing data pipeline between cloud services)
This project demonstrates a secure serverless data ingestion pipeline using AWS Lambda. The pipeline retrieves data from a public API, stores it in an Amazon S3 bucket, and ensures data integrity throughout the process. The project showcases best practices for security, data protection, and compliance, making it an ideal reference architecture for organizations seeking to build secure and reliable data pipelines on AWS.

### Project Overview

Our project will involve:

1. Creating an AWS Lambda function that queries an external API. (OpenWeatherMap API: Provides current and forecasted weather data. (Free tier available) <src href="https://home.openweathermap.org/users/sign_in"/> ) 
2. Storing the API response data in an Amazon S3 bucket.
3. Ensuring the security and integrity of our data and resources.

