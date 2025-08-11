---
title : "2.1 Create IAM Role"
date: "2025-07-04" 
weight : 2
chapter : false
---
The goal of this section is to create an IAM role to assign permissions for workshop accounts. We will grant the **minimum required permissions** so participants can use services such as:
- Lambda 
- Translate
- Polly
- Lex
- Bedrock
- Transcribe 
- CloudWatch 

#### Step 1: Open AWS IAM Console

1. Access IAM at: 👉 https://console.aws.amazon.com/iam/
    
2. In the left sidebar, select **“Roles”** → click the **Create role** button

![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21role.png) 

#### Step 2: Select trusted entity

1. In the **“Trusted entity type”** section, choose: **AWS service**
    
2. In the **“Use case”** section, select the service: **Lambda**
    
3. Click the **Next** button

![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21step1.png) 

#### Step 3: Attach permissions policies
Find and **tick the following policies**:

| Policy Name | Purpose |
| --- | --- |
| `TranslateFullAccess` | Allows use of Amazon Translate to **automatically translate text** between languages. |
| `AmazonLexFullAccess` | Grants access to Amazon Lex to deploy and **invoke NLP chatbots** using voice or text. |
| `AmazonPollyFullAccess` | Enables use of Amazon Polly to **convert text to speech**. |
| `AmazonBedrockFullAccess` | Grants access to Amazon Bedrock to **invoke large language models**. |
| `CloudWatchLogsFullAccess` | Allows **writing and reading logs** from all AWS services to Amazon CloudWatch Logs for system monitoring. |
| `AWSLambdaBasicExecutionRole` | Allows Lambda functions to write logs to CloudWatch Logs. |

After ticking all, review the information, then click **Next**
![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21a.png) 

#### Step 4: Name the Role

- Role name: `LambdaChatbotExecutionRole`
- Review the information, then create the role.

#### Step 5: Complete
![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21b.png) 