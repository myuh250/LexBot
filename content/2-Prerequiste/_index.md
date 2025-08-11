---
title : "2. Prerequisite Steps"
date: "2025-07-04" 
weight : 2
chapter : false
---
### Services Required for the Lab

To complete this lab, you will need the following services:

- **API Gateway**: HTTP endpoint to receive requests from Slack
- **Lambda Functions**: Orchestrate logic: handle input, routes
- **Amazon S3**: Temporarily store audio files from Slack
- **Amazon Transcribe**: Convert audio to text
- **Amazon Translate**: Automatically detect and translate language to English, and translate back to the user's input language
- **Amazon Lex**: Detect intent from user input
- **Amazon Bedrock**: Fallback to handle queries with unclear intent (FAQ style)
- **Amazon Polly**: Convert output to speech if requested by the user
- **Amazon CloudWatch**: Log the entire processing flow

---

⚠️ **Cost Warning**  
This lab uses several AWS AI services such as Amazon Lex, Bedrock, and Polly.  
If not carefully monitored, these services **may incur significant costs**, especially when processing large amounts of data or using advanced features like generative models.  
You **should** take the following steps to control costs:

- Set limits and budgets for the services
- Monitor usage via the AWS Billing Dashboard
- Clean up unused resources after completing the lab

---

💡 **Slack Workspace Required**  
You need access to a Slack workspace.  
This lab uses Slack as the main interface