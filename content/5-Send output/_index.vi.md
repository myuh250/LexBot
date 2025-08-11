---
title : "5. Gửi phản hồi"
date: "2025-07-04" 
weight : 2
chapter : false
---
Trong bước này, chúng ta sẽ tạo một AWS Lambda function dạng **async** để lấy output từ Lambda trước đó (thông qua API Gateway hoặc event) và gửi thẳng lên Slack qua Slack API.

#### Bước 1: Tạo Lambda function
- Tạo một lambda function với tên `WrapOutput`
  - Runtime: Python 3.x
  - Role: `LambdaChatbotExecutionRole`
  - Copy và paste code bên dưới, thay **SLACK_BOT_TOKEN** (ở trong Install App) và **SLACK_CHANNEL_ID** (Vào kênh Slack muốn gửi tin nhắn. Click vào tên kênh ở trên, View channel details. Ở cuối URL hoặc trong phần thông tin sẽ có ID) bằng giá trị thật của bạn:

```python
import json
import urllib.request

SLACK_BOT_TOKEN = "xoxb-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" # Replace with your Slack Bot Token
SLACK_CHANNEL_ID = "C1234567890" # Replace with your Slack Channel ID

def lambda_handler(event, context):
    try:
        # Nhận message từ NLPHandler
        message = event.get("message", "No message provided")

        # Tạo payload cho Slack
        payload = {
            "channel": SLACK_CHANNEL_ID,
            "text": message
        }

        # Gửi thẳng lên Slack API
        req = urllib.request.Request(
            "https://slack.com/api/chat.postMessage",
            data=json.dumps(payload).encode("utf-8"),
            headers={
                "Content-Type": "application/json",
                "Authorization": f"Bearer {SLACK_BOT_TOKEN}"
            }
        )

        with urllib.request.urlopen(req) as resp:
            slack_response = json.loads(resp.read().decode())

        return {
            "statusCode": 200,
            "body": json.dumps({
                "status": "Message sent to Slack successfully!",
                "slack_response": slack_response
            })
        }

    except Exception as e:
        print("=== ERROR trong WrapOutput ===")
        print(f"[SLACK] Error: {str(e)}")
        print(f"[SLACK] Event received: {json.dumps(event, indent=2)}")
        
        return {
            "statusCode": 500,
            "body": json.dumps({"error": str(e)})
        }

```

#### Bước 2: Kiểm tra tin nhắn output

Sau khi hoàn thành, chúng ta đã hoàn thiện tạo một luồng nhắn tin đơn giản bằng Lex và xử lý ngôn ngữ tự nhiên hơn bằng Bedrock
![ConnectPrivate](/images/5.output/5output.png) 