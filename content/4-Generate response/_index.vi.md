---
title : "4. Xử lý ngôn ngữ tự nhiên"
date: "2025-07-04" 
weight : 2
chapter : false
---
Trong phần này, chúng ta sẽ tạo một Lambda function mới có tên là `NLPHandler` để xử lý và cải thiện phản hồi từ Lex bằng cách sử dụng Amazon Bedrock.

#### Bước 1: Tạo lambda function NLP handler

- Truy cập AWS Lambda console và tạo một function mới tên `NLPHandler` với runtime là Python 3.x. 
  - Chọn role là `LambdaChatbotExecutionRole`
- Thêm mã nguồn sau vào function:

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
        FunctionName="WrapOutput",  # Tên Lambda gửi Slack
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

#### Bước 2: Kết nối Lambda với output của Lex

- Truy cap vao Lambda console va chon `InputHandler`
- Sua `InputHandler` de goi den `NLPHandler`:
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

#### Bước 3: Tạo policy để invoke lambda

- Truy cập lại role `LambdaChatbotExecutionRole`, chọn **Add permissions** > **Create inline policy**
- Service: Lambda
- Chọn tab JSON và dán này vào (best practice la nen gan resources cu the thay vi *)
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
- Đặt tên là `InvokeLambda` và tạo.

#### Bước 4: Kiểm tra văn bản đã được xử lý
![ConnectPrivate](/images/4.nlp/4log.png) 