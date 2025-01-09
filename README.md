MyFirstServerlessAPI-
# AWS Lambda + API Gateway + DynamoDB Project

# Overview
This project demonstrates how to create a REST API using AWS Lambda and API Gateway to perform CRUD operations on a DynamoDB table. 
The architecture involves:
- **AWS Lambda**: Executes functions triggered by API Gateway requests.
- **API Gateway**: Acts as the front-facing REST API endpoint.
- **DynamoDB**: Stores data for CRUD operations.

## Architecture Diagram
```
Client -> API Gateway -> Lambda Function -> DynamoDB
```

## Prerequisites
1. **AWS Account**: Sign up if you don’t already have one.
2. **AWS CLI**: Installed and configured with IAM credentials.
3. **curl**: Installed for testing API requests.
4. **Text Editor/IDE**: To write the Lambda function.
5. **Permissions**: Ensure the IAM role has the necessary permissions for Lambda, API Gateway, and DynamoDB.

## Steps to Recreate

### 1. Create the DynamoDB Table
- Create a DynamoDB table named `YourTableName` with `id` as the partition key.
-  ![dynamodb table creation] (images/dynamodb table creation.png)

### 2. Create the Lambda Function
1. Write the Lambda function code (e.g., `LambdaFunctionOverHttps.py`).
2. Package it into a zip file:
   ```bash
   zip function.zip LambdaFunctionOverHttps.py
   ```
3. Use the AWS CLI to create the function:
   ```bash
   aws lambda create-function \
     --function-name LambdaFunctionOverHttps \
     --zip-file fileb://function.zip \
     --handler LambdaFunctionOverHttps.lambda_handler \
     --runtime python3.12 \
     --role arn:aws:iam::123456789012:role/service-role/Lambda-apigateway-role
   ```

### 3. Set Up API Gateway
- Create a new REST API in API Gateway.
- Define HTTP methods (GET, POST, PUT, DELETE) and link them to the Lambda function.
- Deploy the API and copy the `Invoke URL`.

### 4. Test the API
- Use `curl` commands to test CRUD operations:

**Create Operation:**
```bash
curl https://your-api-url/test -d '{"operation": "create", "payload": {"Item": {"id": "5678EFGH", "name": "example"}}}' --ssl-no-revoke
```

**Update Operation:**
```bash
curl https://your-api-url/test -d '{"operation": "update", "payload": {"Key": {"id": "5678EFGH"}, "UpdateExpression": "set #name = :name", "ExpressionAttributeNames": {"#name": "name"}, "ExpressionAttributeValues": {":name": "new_example"}}}' --ssl-no-revoke
```

**Delete Operation:**
```bash
curl https://your-api-url/test -d '{"operation": "delete", "payload": {"Key": {"id": "5678EFGH"}}}' --ssl-no-revoke
```

**Read Operation:**
```bash
curl https://your-api-url/test -d '{"operation": "read", "payload": {"Key": {"id": "5678EFGH"}}}' --ssl-no-revoke
```

## Challenges Faced and Solutions

### Challenge 1: SSL Certificate Revocation Issue
- **Error**: `curl: (35) schannel: next InitializeSecurityContext failed: CRYPT_E_NO_REVOCATION_CHECK`
- **Solution**: Add the `--ssl-no-revoke` flag to the `curl` command to bypass revocation checks.

### Challenge 2: Missing IAM Permissions
- **Error**: Access denied when creating the Lambda function.
- **Solution**: Attach the `AdministratorAccess` policy to the IAM role temporarily.

### Challenge 3: Syntax Differences in curl
- **Error**: Backslashes (`\`) used in Linux scripts caused issues in Windows.
- **Solution**: Remove backslashes and escape double quotes for JSON payload in Windows terminals.

## Screenshots
- **Error Messages**
  - Example: SSL certificate revocation error
- **AWS Console Views**
  - Lambda function creation
  - API Gateway setup
  - DynamoDB table with inserted data
- **Successful `curl` Requests**

## Usage
1. Deploy the API.
2. Use the `Invoke URL` for CRUD operations via `curl` or a REST client (e.g., Postman).
3. Ensure proper IAM permissions for long-term use.


