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

![image](https://github.com/user-attachments/assets/9e770b76-b157-4b06-aa66-b3e5722222e3)


**Note:** 

- I kept the public access as blocked for now.

![image](https://github.com/user-attachments/assets/fd4b7571-0bdf-4acb-ab40-d949870cb93f)

2. Develop Lambda Functions:

- Implement the UploadFunction and DownloadFunction logic in your preferred language (e.g., Python, Node.js). These functions should handle file uploads and downloads from the S3 bucket respectively.

- You can check the code logic in the repository.

3. Create Lambda Functions:

- In the AWS Management Console, navigate to the Lambda service.
- Create two Lambda functions:
    - Name: UploadFunction
    - Runtime: Select your preferred language runtime environment (e.g., Python 3.x)
    - Execution role: Create an IAM role with S3 PutObject permission for uploads.

![image](https://github.com/user-attachments/assets/a2d55405-43c3-4686-b119-be3c21659d75)

**Trigger for upload function:**

![image](https://github.com/user-attachments/assets/33e1e3ed-bd56-4197-952e-ad9ca67c34df)


- Upload your function code.
- Repeat for the DownloadFunction with S3 GetObject permission.

![image](https://github.com/user-attachments/assets/627984d1-03e9-47c5-992a-01705e31b97d)

**Trigger for download function:**

![image](https://github.com/user-attachments/assets/6b1d9a31-baa6-4682-bb05-b5d1874b74dd)


**Note:** I attached to the lambda functions the followings:

- A role that will allow the lambda function to get, put and list objects from/to the S3 bucket.

![image](https://github.com/user-attachments/assets/9a1d15cd-1d93-4a72-ac83-c15c37246762)

- A trust relationship that will allow the lambda functions to assume the above role.

![image](https://github.com/user-attachments/assets/fdfd2620-c94f-4885-af8f-e2a5ad91cfd4)

4. Create API Gateway:

- In the AWS Management Console, navigate to the API Gateway service.
- Create a new API and name it appropriately (e.g., "file-sharing-api").

![image](https://github.com/user-attachments/assets/d19808fb-cd40-4e94-8f22-79870ac36b17)

- Create a resource named "files" with two methods:
    - Method: POST - integrates with UploadFunction for uploads.
    - Method: GET - integrates with DownloadFunction for downloads.

![image](https://github.com/user-attachments/assets/8002ba54-99e1-453b-8c22-ab471b3f3523)

5. Configure API Methods:

For each method (POST and GET):

- Under "Integration Request," map request body and query parameters to function calls.
- For the POST method, configure the request body to pass the uploaded file content and filename to the UploadFunction.
- For the GET method, configure the query string parameter to pass the filename to the DownloadFunction for retrieval.

**GET:**

Method request

![image](https://github.com/user-attachments/assets/0d292bb3-4b16-4036-976a-3001a537c9c0)

![image](https://github.com/user-attachments/assets/0c425d41-f94f-4cb6-a7f1-a403930ae3ae)

Integration request

![image](https://github.com/user-attachments/assets/a24e2545-cddf-4c7b-9ef9-5b38852f03c5)

Integration reponses

![image](https://github.com/user-attachments/assets/635e7ffb-0b3b-49bd-93c8-fb83e1d2aa9c)

**POST:**

Everything by default except the change below in Integration request:

![image](https://github.com/user-attachments/assets/c23ab193-9a2e-4b12-806f-29ca31288bd0)


6. Deploy API Gateway:

Deploy the API to a stage (e.g., "dev") to make it publicly accessible.

7. Testing

Use an HTTP client like Postman or CURL to interact with the API.

**Upload a file:**
- Use a POST request to the "/files" endpoint.
- Specify the filename in the query string parameter and the file content in the request body.

![image](https://github.com/user-attachments/assets/23f8d363-ad5c-4395-bb8c-e80a9403550a)

![image](https://github.com/user-attachments/assets/12bf33b1-0499-4cf7-afc4-047f51200f7a)

![image](https://github.com/user-attachments/assets/6d943213-e10d-492b-8000-7eb13835bde4)

Running the curl command in the command line: 

![image](https://github.com/user-attachments/assets/3ec0507f-3494-4fb9-9238-780b19f39051)

Checking the S3 bucket after running the above command:

![image](https://github.com/user-attachments/assets/3bef54fd-7d40-41fb-a273-4bc9736d82a6)

And the content of the file: 

![image](https://github.com/user-attachments/assets/31bf3de6-d4e7-4601-93ab-1b8c3b24a846)

**Download a file:**
- Use a GET request to the "/files" endpoint.
- Specify the filename in the query string parameter.

![image](https://github.com/user-attachments/assets/ae345678-b6f5-44fb-8a5b-83f14e58f888)

Running the curl command in the command line:

![image](https://github.com/user-attachments/assets/d4de39cb-48b8-4489-ac34-9b3466454e40)

Decoding from Base64 to make sure the result is as expected.

![image](https://github.com/user-attachments/assets/ca443f5d-5199-4707-a1ab-c6b13c2b8b0c)


<h2> Security Considerations </h2>
Implement IAM policies to restrict access to Lambda functions and S3 buckets.
Consider using Amazon Cognito for user authentication and authorization (if applicable) for added security.


<h2> Conclusion </h2>
This serverless file sharing platform provides a cost-effective and scalable solution for secure file uploads and downloads. The outlined deployment guide simplifies the setup process, enabling developers to quickly build and deploy file sharing functionality within their applications.
