---
title: "6. Clean Up AWS Resources"
date: "2025-07-04"
weight: 3
chapter: false
---After completing the workshop, you should delete AWS resources to avoid unnecessary costs.

### Cleanup Steps:

1. **Delete Lambda Functions**
   - Go to the AWS Lambda Console
   - Delete the functions created during the workshop (e.g., NLPHandler, WrapOutput, ...)

2. **Delete API Gateway**
   - Go to the API Gateway Console
   - Delete the APIs created for the chatbot

3. **Delete Lex Resources**
   - Go to the Amazon Lex Console
   - Delete related bots, intents, and slot types

4. **Delete S3 Buckets (if created for logs or temporary files)**
   - Go to the S3 Console
   - Delete any unused buckets

5. **Delete IAM Roles and Policies**
   - Go to the IAM Console
   - Delete any roles/policies used only for the workshop (e.g., LambdaChatbotExecutionRole)

6. **Delete Other Resources (if any):**
   - Bedrock, Translate, CloudWatch log groups, etc.

### Notes
- Double-check before deleting to avoid affecting resources used by other systems.
- After deletion, check your billing to ensure there are no remaining charges.

Good luck completing the workshop and optimizing your
