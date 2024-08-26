<h1> Secure file sharing platform on AWS </h1>

This document details the architecture and deployment process for a serverless file sharing platform that allows users to securely upload and download files through a simple HTTP API.

<h2> Introduction </h2>

This project leverages AWS services to provide a scalable and secure solution for file sharing. It utilizes serverless functions for cost-effectiveness and eliminates the need to manage physical servers. Users can interact with the platform using any HTTP client, making it versatile for various file sharing and storage needs.

<h2> Use cases </h2>

- File Sharing: Users can securely upload and share documents, images, or any other digital assets through a simple API call.
- File Distribution: Content creators can distribute files (e.g., software updates, media files) by generating download links.
- Collaborative Work: Teams can securely share project resources, documents, and data, facilitating collaboration across locations.

<h2> Key components </h2>

The platform utilizes the following AWS services:

- Amazon S3: Provides durable and scalable object storage for uploaded files.
- AWS Lambda: Serverless compute service that executes code triggered by API requests. This project uses two Lambda functions:
- UploadFunction: Handles file uploads and stores them securely in the S3 bucket.
- DownloadFunction: Retrieves files from the S3 bucket based on user requests.
- Amazon API Gateway: Creates a RESTful API endpoint for users to interact with the platform.

<h2> Implementation guide </h2>

**Prerequisites**

- AWS Account with appropriate permissions to create Lambda functions, API Gateway, and S3 buckets.
- Familiarity with basic AWS concepts.

**Steps** 

1. Create an S3 Bucket:

- In the AWS Management Console, navigate to the S3 service.
- Create a new bucket to store uploaded files.


2. Develop Lambda Functions:

- Implement the UploadFunction and DownloadFunction logic in your preferred language (e.g., Python, Node.js). These functions should handle file uploads and downloads from the S3 bucket respectively.

3. Create Lambda Functions:

- In the AWS Management Console, navigate to the Lambda service.
- Create two Lambda functions:
    - Name: UploadFunction
    - Runtime: Select your preferred language runtime environment (e.g., Python 3.x)
    - Execution role: Create an IAM role with S3 PutObject permission for uploads.
- Upload your function code.
- Repeat for the DownloadFunction with S3 GetObject permission.

4. Create API Gateway:

- In the AWS Management Console, navigate to the API Gateway service.
- Create a new API and name it appropriately (e.g., "file-sharing-api").
- Create a resource named "files" with two methods:
    - Method: POST - integrates with UploadFunction for uploads.
    - Method: GET - integrates with DownloadFunction for downloads.


5. Configure API Methods:

For each method (POST and GET):

- Under "Integration Request," map request body and query parameters to function calls.
- For the POST method, configure the request body to pass the uploaded file content and filename to the UploadFunction.
- For the GET method, configure the query string parameter to pass the filename to the DownloadFunction for retrieval.


6. Deploy API Gateway:

Deploy the API to a stage (e.g., "dev") to make it publicly accessible.

7. Testing

Use an HTTP client like Postman or CURL to interact with the API.

Upload a file:
- Use a POST request to the "/files" endpoint.
- Specify the filename in the query string parameter and the file content in the request body.

Download a file:
- Use a GET request to the "/files" endpoint.
- Specify the filename in the query string parameter.


<h2> Security Considerations </h2>
Implement IAM policies to restrict access to Lambda functions and S3 buckets.
Consider using Amazon Cognito for user authentication and authorization (if applicable) for added security.


<h2> Conclusion </h2>
This serverless file sharing platform provides a cost-effective and scalable solution for secure file uploads and downloads. The outlined deployment guide simplifies the setup process, enabling developers to quickly build and deploy file sharing functionality within their applications.
