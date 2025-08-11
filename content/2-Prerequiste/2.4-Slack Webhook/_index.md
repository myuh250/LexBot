---
title : "2.4 Install Slack app"
date: "2025-07-04" 
weight : 2
chapter : false
---
Now we will create a Slack App with the Slash Command `/askfaq` so users in your workspace can send input to the API Gateway.

#### Step 1: Access Slack API Console

1. Go to: https://api.slack.com/apps
2. Click **Create New App**
3. Choose:
   - **From scratch**
   - App name: `AWSChatbot`
   - Workspace: select your workspace

#### Step 2: Create Slash Command

1. In the app dashboard, select **"Slash Commands"** > **Create New Command**

![ConnectPrivate](/images/2.pre/2.4.Slack/24slash.png) 

2. Fill in the following information:

| Field | Value |
| --- | --- |
| Command | `/askfaq` (or another name) |
| Request URL | `https://<your-api-id>.execute-api.<region>.amazonaws.com/slack` (the endpoint from **API Gateway** you created earlier.) |
| Short Description | "AWS chatbot" |
| Usage Hint | `[your question]` (e.g.: "`/askfaq What is aws?`") |

#### Step 3: Grant permissions to the app

1. Select **OAuth & Permissions** from the left menu
2. In the **Scopes** section, add:
   - `commands` 
   - `chat:write`
   - `files:read`
   - `calls:write` 
![ConnectPrivate](/images/2.pre/2.4.Slack/24oauth.png) 

#### Step 4: Enable Event Subscriptions

1. Go to `Event Subscriptions`
2. In this section, enable `Enable Events`
3. Enter Request URL: `https://<your-api-id>.execute-api.<region>.amazonaws.com/slack` (the endpoint from **API Gateway** you created earlier.)
4. Subscribe to bot events: file_shared
5. Save changes
![ConnectPrivate](/images/2.pre/2.4.Slack/24verify.png) 

#### Step 5: Install the app to your workspace

1. Go to **Install App**
2. Click **Install to Workspace**
3. Click `Allow`

✅ After installation, you will see:
- **Bot User OAuth Token**
- **Verification Token** (used to verify requests in Lambda if needed, but we will not use this for now)

#### Step 6: Test the Slash Command
Open Slack and try typing anything to see if you get a response. At this step, you just need to receive any response. If the API Gateway has an error, Slack will report a Timeout. Everything is set up correctly if you see a result like