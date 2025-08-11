---
title : "2.2 Tạo các Lambda Function"
date: "2025-07-04" 
weight : 2
chapter : false
---
Ở đây chúng ta sẽ cùng tạo các lambda function sẽ sử dụng trong workshop này.

&nbsp;&nbsp;&nbsp;

### Function để nhận input từ Slack

#### Bước 1: Truy cập Lambda console

- Vào: https://console.aws.amazon.com/lambda
- Bấm nút **"Create function"**


#### Bước 2: Điền thông tin tạo function

| Trường | Giá trị nhập |
| --- | --- |
| Function name | `InputHandler` *(hoặc tên tùy chọn)* |
| Runtime | `Python 3.12` *(hoặc `Node.js 20.x`, nếu bạn thích JavaScript)* |
| Execution role | **Use an existing role** |
| Existing role | Chọn role bạn đã tạo ở **Mục 2.1** (`ChatbotLambdaExecutionRole`) |

> Nếu chưa thấy role, hãy chắc chắn bạn tạo role ở cùng Region và IAM role đã sẵn sàng.

#### Bước 3: Thêm code xử lý đơn giản để test

Sau khi tạo xong, AWS sẽ đưa bạn vào màn hình chỉnh sửa code. Bạn có thể thử với code Python như sau để test route Slack:

```python
import json

def lambda_handler(event, context):
    print("Event:", event)

    body = json.loads(event.get("body", "{}"))

    if body.get("type") == "url_verification":
        return {
            'statusCode': 200,
            'body': body.get("challenge", ""),
            'headers': {
                'Content-Type': 'text/plain'
            }
        }

    return {
        'statusCode': 200,
        'body': json.dumps({
            'text': 'Hello from Lambda! Your input has been received.',
            'response_type': 'in_channel'
        }),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

> Để Slack slash command không bị lỗi, bạn cần trả JSON với text + response_type.

#### Bước 4: Lưu và deploy

- Bấm **Deploy**
- Ghi chú lại **Lambda ARN** nếu cần

&nbsp;&nbsp;&nbsp;