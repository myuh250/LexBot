---
title : "4. Natural Language Processing"
date: "2025-07-04" 
weight : 2
chapter : false
---
In this section, we will create a new Lambda function called `NLPHandler` to process and enhance Lex responses using Amazon Bedrock.

#### Step 1: Create the NLP handler Lambda function

- Go to the AWS Lambda console and create a new function named `NLPHandler` with Python 3.x runtime.
  - Choose the role `LambdaChatbotExecutionRole`
- Add the following source code to the function:

```python
import boto3
import json
import os

bedrock = boto3.client(service_name='bedrock-runtime', region_name='ap-southeast-2')

def enhance_with_bedrock(text):
    prompt = f"Rewrite the following response in a natural, fun, and Gen Z vibe — playful, trendy, and a little humorous, but still keeping the original meaning:\n\n{text}"
    
    body = {
        "inputText": prompt,
        "textGenerationConfig": {
            "temperature": 0.7,
            "maxTokenCount": 512
        }
    }

    try:
        res = bedrock.invoke_model(
            modelId="amazon.titan-text-lite-v1",
            body=json.dumps(body)
        )
        payload = json.loads(res['body'].read())
        output_text = payload.get('results', [{}])[0].get('outputText', '')
        return output_text
    except Exception as e:
        return "[Error] Could not enhance text"

def lambda_handler(event, context):
    lex_output = event.get("lex_output", "")
    if not lex_output:
        return {"statusCode": 400, "body": "Missing Lex output"}

    enhanced_text = enhance_with_bedrock(lex_output)

    lambda_client = boto3.client("lambda")
    lambda_client.invoke(
        FunctionName="WrapOutput",  # Lambda function to send to Slack
        InvocationType="Event",  # async
        Payload=json.dumps({
            "message": enhanced_text
        })
    )

    return {
        "statusCode": 200,
        "body": json.dumps({"enhanced_text": enhanced_text})
    }
```

#### Step 2: Connect Lambda with Lex output

- Go to the Lambda console and select `InputHandler`
- Edit `InputHandler` to call `NLPHandler`:
```python
    print("Sending text to Lex...")
    lex_msg = send_to_lex(english_text, session_id="test-session-123")  
    print(f"Lex replied: {lex_msg}")

    lambda_client = boto3.client('lambda')
    nlp_payload = {
        "lex_output": lex_msg,
        "original_language": original_language,
        "original_text": text
    }

    lambda_client.invoke(
        FunctionName='NLPHandler',
        InvocationType='Event',  # Async call
        Payload=json.dumps(nlp_payload)
    )

    return {
        'statusCode': 200,
        'body': json.dumps({
            'text': lex_msg,
            'response_type': 'in_channel'
        }),
        'headers': {'Content-Type': 'application/json'}
    }
```

#### Step 3: Create a policy to invoke Lambda

- Go back to the `LambdaChatbotExecutionRole` role, select **Add permissions** > **Create inline policy**
- Service: Lambda
- Choose the JSON tab and paste the following (best practice is to specify resources instead of *):
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "lambda:InvokeFunction",
            "Resource": "*"
        }
    ]
}
```
- Name it `InvokeLambda` and create the policy.

#### Step 4: Check
![ConnectPrivate](/images/4.nlp/4log.png) 