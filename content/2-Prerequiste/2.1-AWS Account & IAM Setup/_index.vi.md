---
title : "2.1 Tạo IAM Role"
date: "2025-07-04" 
weight : 2
chapter : false
---
Mục tiêu của chúng ta ở phần này là tạo IAM role để phân quyền sử dụng cho các account. Chúng ta sẽ cấp quyền **tối thiểu cần thiết** để người tham gia workshop có thể dùng các dịch vụ như:
- Lambda 
- Translate
- Polly
- Lex
- Bedrock
- Transcribe 
- CloudWatch 

#### Bước 1: Mở AWS IAM Console

1. Truy cập IAM tại: 👉 https://console.aws.amazon.com/iam/
    
2. Ở thanh bên trái, chọn **“Roles”** → bấm nút **Create role**

![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21role.png) 

#### Bước 2: Chọn trusted entity

1. Ở mục **“Trusted entity type”**, chọn: **AWS service**
    
2. Ở mục **“Use case”**, chọn service là: **Lambda**
    
3. Bấm nút **Next**

![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21step1.png) 

#### Bước 3: Attach permissions policies
Tìm và **tick vào các policy sau**:

| Policy Name | Mục đích |
| --- | --- |
| `TranslateFullAccess` | Cho phép sử dụng dịch vụ Amazon Translate để **dịch văn bản** tự động giữa các ngôn ngữ. |
| `AmazonLexFullAccess` | Cho phép truy cập Amazon Lex để triển khai và **gọi chatbot NLP** sử dụng giọng nói hoặc văn bản. |
| `AmazonPollyFullAccess` | Cho phép sử dụng Amazon Polly để **chuyển văn bản thành giọng nói**. |
| `AmazonBedrockFullAccess` | Cho phép truy cập Amazon Bedrock để **gọi các mô hình ngôn ngữ lớn**. |
| `CloudWatchLogsFullAccess` | Cho phép **ghi và đọc log** từ tất cả dịch vụ AWS vào Amazon CloudWatch Logs để theo dõi hệ thống. |
| `AWSLambdaBasicExecutionRole` | Cho phép Lambda function ghi log ra CloudWatch Logs. |

Sau khi tick hết, kiểm tra lại các thông tin, sau đó bấm **Next**
![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21a.png) 

#### Bước 4: Đặt tên Role

- Role name: `LambdaChatbotExecutionRole`
- Kiểm tra lại các thông tin, sau đó tạo role.

#### Bước 5: Hoàn tất
![ConnectPrivate](/lexbot/images/2.pre/2.1.IAM/21b.png) 