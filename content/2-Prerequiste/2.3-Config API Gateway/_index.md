---
title : "2.3 Setup API Gateway"
date: "2025-07-04" 
weight : 2
chapter : false
---
#### Step 1: Open the API Gateway interface

- Go to: 👉 https://console.aws.amazon.com/apigateway
- Click **Create API**

#### Step 2: Select API type

- Choose: **HTTP API** (lighter and simpler for handling webhooks like Slack) → Click **Build**

#### Step 3: Configure routes
In the naming section, enter: `SlackChatbotAPI` (or any name you prefer)

1. Add integration
- Integration type: `Lambda function`
- Region: same region as your Lambda
- Function: `InputHandler` *(the function you created to handle Slack input)*
![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23integ.png) 

2. Add route
- Method: `POST`
- Resource path: `/slack` 
![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23route.png)

→ Click **Next**

#### Step 4: Review & create

You can leave the remaining settings as default. Review the information and then create the gateway.
![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23a.png)

#### Step 5: Get the URL to configure in Slack

After creation, if you click on this API Gateway, you will see the **Invoke URL in the stage section**, for example:

![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23url.png)

```
https://abc123xyz.execute-api.us-east-1.amazonaws.com
```

The final endpoint will be:
```
https://abc123xyz.execute-api.us-east-1.amazonaws.com/slack

```

📌 **Copy this URL.**