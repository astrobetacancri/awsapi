# AWS API Gateway & DynamoDB 
Develop a simple RESTful API (using your language of choice) that interacts with a backend database of your choice as the backend permanent data store. The API shall have one endpoint supporting a POST and GET method allowing the user to create objects via POST and query for the objects via GETs. The object being created shall contain a mixture of primitive attributes (e.g., date/time, string, int, float, Boolean, etc) to support the REST API’s query capabilities.

## This practice follows the AWS blog: 
[Using Amazon API Gateway as a proxy for DynamoDB](https://aws.amazon.com/blogs/compute/using-amazon-api-gateway-as-a-proxy-for-dynamodb/)

## Create an API endpoint using AWS API Gateway connecting to Dynamo DB. 

1. Set up Dynamo DB: 
- Create Table 
- Primary Key 
- Create Index with Primary Key and Index Name 
- Create New Index (pageId-Index-copy) with added createdate column 
- DynamoDB support [Data type](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBMapper.DataTypes.html) restricted to All Number type, Boolean, String, Date (String)

2. Create IAM role with policies: 
- Execution role: 
    - IAM role with 
     - DynamoDB access and API Gateway Invoke Full Access
     - Trusted entities: 
      - dax.amazonaws.com
      - apigateway.amazonaws.com
       - [Trusted entities](trust_relationship)

3. Create API in API Gateway 
POST method 
- New Child Resource 
  - /comments/POST method
   - Integration Type : AWS Service Proxy
   - AWS Region: ca-central-1 
   - AWS Service: DynamoDB
   - HTTP method: POST
   - Action: PutItem 
   - Execution role: Attach IAM role ARN 
   - Apply [POST Mapping templates](POST_Mapping_Template) to Integration Request

- GET method     
    -  /comments/{pageid}/GET method    
    - GET Integration Request: 
     - Integration type: AWS Service 
     - AWS Region: ca-central-1 
     - AWS Service: DynamoDB
     - HTTP method: POST
     - Action: Scan 
     - Execution role: Attach IAM role ARN 
     - Apply [GET Mapping templates](GET_Mapping_Template) to Integration Request

    - GET Method Request: 
     - Request Paths: Name = pageid 

    - GET Integration Response:
     - Apply [GET Mapping Templates](GET_Response_Mapping_Template) to Integration Response


## Test API Gateway : 
- GET URL: 
 - https://67fy3si1z8.execute-api.ca-central-1.amazonaws.com/Live/comments/{pageid}
 - https://67fy3si1z8.execute-api.ca-central-1.amazonaws.com/test/comments/UrtheCast1

Use Insomnia to test POST, update to DynamoDB: 
- POST URL: 
 - https://67fy3si1z8.execute-api.ca-central-1.amazonaws.com/Live/comments/

Enter the sample [POST JSON Body](POST_JSON_body) in Insomia POST JSON body
(* with Insomnia Timestamp)

