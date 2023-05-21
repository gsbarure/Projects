GO SERVERLESS WITH AWS!





Mega Project on AWS and Serverless

Future for Serverless

Serverless computing offers a number of advantages over traditional cloud-based or server-centric infrastructure. For many developers, serverless architectures offer greater scalability, more flexibility, and quicker time to release, all at a reduced cost. With serverless architectures, developers do not need to worry about purchasing, provisioning, and managing backend servers.

e.g.Developers are only charged for the server space they use, reducing cost

Installation Part:

Name itself says a serverless so we do not need any servers/manual intervention for the same once we install serverless.

Launch EC2 instance to install serverless.

![image](https://github.com/gsbarure/Projects/assets/125451289/c1ef4989-9cd1-465d-aab1-3cdf6908a12b)

Install Node.js 14 on Ubuntu 22.04|20.04|18.04

Update the Server:
                  sudo apt update
                  
Install Node.js 14 on Ubuntu 22.04|20.04|18.04

After system update, install Node.js 14 on Ubuntu 22.04|20.04|18.04 by first installing the required repository.

curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
sudo apt -y install nodejs

Verify the version of Node.js installed.

![image](https://github.com/gsbarure/Projects/assets/125451289/530efddb-ef99-475d-8e14-ccc249ff1bff)

Install serverless module via NPM:

npm install -g serverless

![image](https://github.com/gsbarure/Projects/assets/125451289/19308333-e5ff-450a-8f48-a3207013d1a4)

AWSCLI installation:

sudo apt install awscli

Setup serverless aacount:https://www.serverless.com/framework/docs/getting-started

![image](https://github.com/gsbarure/Projects/assets/125451289/0944c2b6-95e0-476e-a713-233fca841630)

Login serverless account and navigate to existing project,copy that URL and run it in server.

![image](https://github.com/gsbarure/Projects/assets/125451289/96d4d943-8d1f-4cac-bc79-6d352e70b2b7)

Run the following command

$serverless --org=gajananbarure

Select the services depends on project requirements here e.g. AWS/HTTP API

![image](https://github.com/gsbarure/Projects/assets/125451289/9e00de28-0f2b-427d-8640-9ea6874178f7)

Provide Project Name:

Login to AWS account and create IAM USER to connect serverless with AWS.

Policies for user:Administrator access.

Create access/Secret key.

![image](https://github.com/gsbarure/Projects/assets/125451289/ad2f4e3b-e10a-4924-8eb0-4527459181ed)

![image](https://github.com/gsbarure/Projects/assets/125451289/222009f3-80ce-4c57-a15e-724dfdbef654)

![image](https://github.com/gsbarure/Projects/assets/125451289/7af102f6-9e73-4ad6-ae05-9dec189e52ed)

![image](https://github.com/gsbarure/Projects/assets/125451289/5c4e8480-b30a-4459-8ae5-e6049e1be3a5)

![image](https://github.com/gsbarure/Projects/assets/125451289/6f11a4d1-e88b-4010-8736-0365f00b1314)

![image](https://github.com/gsbarure/Projects/assets/125451289/069538bb-6500-451f-a114-9e9d491ee0a9)

![image](https://github.com/gsbarure/Projects/assets/125451289/19807c26-a60e-4b64-8622-4015ada73fd2)

My first serverless Deployment:
                               serverless deploy / sls deploy
     
Here is the Simple serverless deployement for Hello!

![image](https://github.com/gsbarure/Projects/assets/125451289/e7000e43-c5bb-444e-a42f-248af81c0680)
![image](https://github.com/gsbarure/Projects/assets/125451289/bed1b77e-b5dd-478b-b3ac-0c2fc4546e98)

git clone :Project URLGithub ---https://github.com/gsbarure/serverless-taskmaster-aws-app.git

![image](https://github.com/gsbarure/Projects/assets/125451289/88f66956-2692-4da6-be8a-8814725f3920)

Write down the serverless.yml file for this app deployment

org: gajananbarure
app: testing-end-to-end
service: serverless-taskmaster-aws-app
frameworkVersion: '3'

provider:
  name: aws
  runtime: nodejs14.x
  region: us-east-1
  iamRoleStatements:
    - Effect: Allow
      Action:
        - dynamodb:*
      Resource:
        #    - arn:aws:dynamodb:us-east-1:626072240565:table/KaamKaro
       - arn:aws:dynamodb:us-east-1:310133246623:table/KaamKaro
functions:
  hello:
    handler: src/hello.handler
    events:
      - httpApi:
          path: /
          method: get
  kaamBharo:
    handler: src/kaamBharo.handler
    events:
      - httpApi:
          path: /kaam
          method: post
  kaamDikhao:
    handler: src/kaamDikhao.handler
    events:
      - httpApi:
          path: /kaam
          method: get
  kaamKhatamKaro:
    handler: src/kaamKhatamKaro.handler
    events:
      - httpApi:
          path: /kaam/{id}
          method: put

resources:
  Resources:
    KaamKaro:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: KaamKaro
        BillingMode: PAY_PER_REQUEST
 AttributeDefinitions:
          - AttributeName: id
            AttributeType: S
        KeySchema:
          - AttributeName: id
            KeyType: HASH
   
   
 Deployment:   serverless deploy / sls deploy
 
 It may throw error as to run package.json we need npm installer.

we can see these errors through cloudwatch loggroups.

![image](https://github.com/gsbarure/Projects/assets/125451289/d5effe6e-2232-4867-a736-90f8edc6f7c9)

![image](https://github.com/gsbarure/Projects/assets/125451289/6f5de05e-bfed-48b3-8e05-879f4575de6d)

Simple solution for above error run install npm install and deploy again

Below are the endpoints created for the functions which mentioned in above serverless.yml with get/post/put methods e.g.KaamBharo/KaamDikhao/KaamKhatamkaro

![image](https://github.com/gsbarure/Projects/assets/125451289/fdf96202-edff-4e0f-9f46-c972c9ba616f)

![image](https://github.com/gsbarure/Projects/assets/125451289/63ca999d-7270-48c8-b8f1-914bae7b8c31)

use strict";
const { v4 } = require("uuid");
const AWS = require("aws-sdk");

const kaamBharo = async (event) => {

  const dynamoDb = new AWS.DynamoDB.DocumentClient();

  const { kaam } = JSON.parse(event.body);
  const createdAt = new Date().toISOString();
  const id = v4();
  const newKaam = {
    id,
    kaam,
    createdAt,
    completed: false
  }
  await dynamoDb.put({
    TableName: "KaamKaro",
    Item: newKaam
  }).promise();

  return {
    statusCode: 200,
    body: JSON.stringify(newKaam),
    };
};

module.exports = {
    handler: kaamBharo,
};

serverless deploy / sls deploy

![image](https://github.com/gsbarure/Projects/assets/125451289/145adafb-5c15-4423-b00a-ce9c6f6f429b)

✔ Service deployed to stack serverless-taskmaster-app1-dev (56s)

dashboard: https://app.serverless.com/gajananbarure/apps/serverless-taskmaster-aws-app1/serverless-taskmaster-app1/dev/us-east-1 endpoint: GET - https://lq319zfrdk.execute-api.us-east-1.amazonaws.com/hello functions: hello: serverless-taskmaster-app1-dev-hello (232 kB)

Architecture in AWS which we did from serverless:Cloudformation stacks

![image](https://github.com/gsbarure/Projects/assets/125451289/5d657af1-2ad9-4c60-9e39-14224e343942)

![image](https://github.com/gsbarure/Projects/assets/125451289/6abff79b-5966-49ef-b175-06661338a53e)

Download Postman: to test this app and create account and login :https://www.postman.com/downloads

Postman is a tool for API Testing.

![image](https://github.com/gsbarure/Projects/assets/125451289/bf47f6a6-67aa-4b4b-98cd-99556ecaa70a)

![image](https://github.com/gsbarure/Projects/assets/125451289/8181f51c-4618-4f22-9599-f063f3191611)

Give App name under new collection and provide the names like below.

![image](https://github.com/gsbarure/Projects/assets/125451289/071007c6-a331-4acf-9e0a-29371724cf33)

Create environment and put url under that so that directly we can access like

{{url name}}/pathname

![image](https://github.com/gsbarure/Projects/assets/125451289/5e720c4e-7bde-4277-9ceb-067f2d2790e8)

To check functions/request add request

![image](https://github.com/gsbarure/Projects/assets/125451289/067de619-7780-408e-a7cb-11327f4d5a68)

dropdown and check your functions i.e.get/post/put etc.

![image](https://github.com/gsbarure/Projects/assets/125451289/bd592fe2-f41f-414b-9216-622059c08ecf)

![image](https://github.com/gsbarure/Projects/assets/125451289/e3f651a5-5b5f-4342-84df-c0604cbcc2da)

![image](https://github.com/gsbarure/Projects/assets/125451289/48b446ba-c804-4a00-bbc7-e3e0664e0034)

To write the text select below options:

![image](https://github.com/gsbarure/Projects/assets/125451289/6a986e9f-4ee2-4797-bb5f-e6e5514a641b)

![image](https://github.com/gsbarure/Projects/assets/125451289/fa1e6558-f999-4c09-aa59-f4e91246a211)

![image](https://github.com/gsbarure/Projects/assets/125451289/f00184af-1af6-4a58-88bb-130797264944)

Serverless Monitoring Integration setup:

![image](https://github.com/gsbarure/Projects/assets/125451289/f298e367-7960-4eb4-8ff6-6ac9c310fabd)

Logging into the Serverless Console via the browser If your browser does not open automatically, please open this URL: https://console.serverless.com?client=cli%3Aserverless&transactionId=8fc9b3be-005b-4621-9995-344d8f586343&clientVersion=3.30.1&clientOriginCommand=onboarding

![image](https://github.com/gsbarure/Projects/assets/125451289/75cd616e-ebf3-40b5-858d-88e145e155f1)

![image](https://github.com/gsbarure/Projects/assets/125451289/2866dbd2-02c1-4e2f-89f3-3dd04eac3075)

![image](https://github.com/gsbarure/Projects/assets/125451289/39db2c4f-4590-4a28-a4d9-ef1b49ebacd4)

![image](https://github.com/gsbarure/Projects/assets/125451289/1bc7c032-11fb-43fa-abbc-8fb11840721e)

![image](https://github.com/gsbarure/Projects/assets/125451289/dc668791-bf62-46af-968b-c2e167e95308)

![image](https://github.com/gsbarure/Projects/assets/125451289/70e8d697-ad26-4bd2-a0a3-731f90a337e5)

Here is integrated monitoring..!!!

![image](https://github.com/gsbarure/Projects/assets/125451289/af5bec77-5930-4bcf-a2a8-dcb0be4d09aa)

go to dev mode

![image](https://github.com/gsbarure/Projects/assets/125451289/800da987-224a-45b1-be43-ab6b2f7f6739)

Testing purpose take your URL and try accessing from browser it will start reflecting like below:

![image](https://github.com/gsbarure/Projects/assets/125451289/1f59e015-e653-496b-90b2-d762176ce896)

GIT integration with AWS for CI/CD deployments:

![image](https://github.com/gsbarure/Projects/assets/125451289/29b7c85e-6d09-4e9b-8e31-e7b6e604c2f2)

![image](https://github.com/gsbarure/Projects/assets/125451289/a4987b3c-1863-496d-8ecc-10b34cc0f119)

![image](https://github.com/gsbarure/Projects/assets/125451289/9f7673ed-9d84-49bc-9207-6bef725f603a)

![image](https://github.com/gsbarure/Projects/assets/125451289/8420dba6-8d83-41c7-8cea-f8bde9565a1f)

![image](https://github.com/gsbarure/Projects/assets/125451289/08f91c52-a919-4981-b74b-a79753c84560)

![image](https://github.com/gsbarure/Projects/assets/125451289/f2f99d5d-9ae0-4df7-81b7-09bf135e59b7)

![image](https://github.com/gsbarure/Projects/assets/125451289/6a960dfc-9f91-4dad-9513-57cc027b355b)

Serverless App deployment:

![image](https://github.com/gsbarure/Projects/assets/125451289/17400f76-7764-4b66-a70d-7c40c505c75b)

![image](https://github.com/gsbarure/Projects/assets/125451289/8b640a82-abde-4a2e-94d3-f92594c98f5a)

 sls remove
 
 This command is used to remove all the Services associated with this deployment.

![image](https://github.com/gsbarure/Projects/assets/125451289/5110e15b-d612-4e4f-80a9-063e7da8ed23)


Github project url:https://github.com/gsbarure/serverless-taskmaster-aws-app.git

Conclusion:

Deployed serverless AWS app,tested through Postman,set up Monitoring and Integrated CI/CD through GIT.

The future holds an exciting world for serverless. Abstractions will continue to get higher and higher and make our jobs continuously easier.

Applications could become “self-aware” in the sense that they know what the best infrastructure is given the use case and amount of traffic it sees. We’re already seeing progress with infrastructure from code, and it will continue to get better with time.

We have a long way to get there and of course, this is all theoretical. But not impossible.

The tools being developed today are truly revolutionary and integrate seamlessly into your cloud vendor environments. It’s not that far of a stretch to think some of these tools can perform usage analysis and modify the infrastructure.

We have many things to look forward to with serverless in the years to come. Keep learning, keep experimenting, keep innovating.

Happy Learning..!!

Thank you Shubham Londhe..!!

AWS Services created by this serverless App:(As we did everything from serverless)

Cloudformations.

![image](https://github.com/gsbarure/Projects/assets/125451289/f4efdfa8-ab21-4664-896f-bfcef689f16e)

Dynamodb.

![image](https://github.com/gsbarure/Projects/assets/125451289/85fe2c98-843e-497e-ba93-f86637dda72c)

Cloudwatch

![image](https://github.com/gsbarure/Projects/assets/125451289/13f828dc-6d99-46cd-b3e0-4a97cd487174)

AWS Lambda

![image](https://github.com/gsbarure/Projects/assets/125451289/655517d5-0c62-47a4-84cd-b1f67a3e7e97)

S3:

![image](https://github.com/gsbarure/Projects/assets/125451289/8133c257-e84a-4b76-b3be-141ac4bbbf87)


Thank you for reading!!

Always open for suggestions..!!




 




