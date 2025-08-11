---
title : "2.2 Create Lambda Functions"
date: "2025-07-04" 
weight : 2
chapter : false
---
Here we will create the Lambda functions used in this workshop.

&nbsp;&nbsp;&nbsp;

### Function to receive input from Slack

#### Step 1: Access the Lambda console

- Go to: https://console.aws.amazon.com/lambda
- Click the **"Create function"** button

#### Step 2: Enter function creation details

| Field | Value to enter |
| --- | --- |
| Function name | `InputHandler` *(or any name you prefer)* |
| Runtime | `Python 3.12` *(or `Node.js 20.x` if you prefer JavaScript)* |
| Execution role | **Use an existing role** |
| Existing role | Select the role you created in **Section 2.1** (`ChatbotLambdaExecutionRole`) |

> If you don't see the role, make sure you created it in the same Region and the IAM role is ready.

#### Step 3: Add simple handler code for testing

After creation, AWS will take you to the code editor screen. You can test with the following Python code for the Slack route:

```python
import json

def lambda_handler(event, context):
    print("Event:", event)

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

> To avoid errors with Slack slash commands, you need to return JSON with both text and response_type.

#### Step 4: Save and deploy

- Click **Deploy**
- Note the **Lambda ARN** if needed

&nbsp;&nbsp;&nbsp;