---
title : "2.4 Cài đặt Slack app"
date: "2025-07-04" 
weight : 2
chapter : false
---
Bây giờ chúng ta sẽ tạo một Slack App với Slash Command `/askfaq` để người dùng trong workspace có thể gửi input đến API Gateway.

#### Bước 1: Truy cập Slack API Console

1. Truy cập: https://api.slack.com/apps
2. Nhấn **Create New App**
3. Chọn:
   - **From scratch**
   - App name: `AWSChatbot`
   - Workspace: chọn workspace của bạn

#### Bước 2: Tạo Slash Command

1. Trong app dashboard, chọn **"Slash Commands"** > **Create New Command**

![ConnectPrivate](/images/2.pre/2.4.Slack/24slash.png) 

2. Điền thông tin như sau:

| Trường | Giá trị |
| --- | --- |
| Command | `/askfaq` (hoặc tên khác) |
| Request URL | `https://<your-api-id>.execute-api.<region>.amazonaws.com/slack` (endpoint từ **API Gateway** mà bạn đã tạo ở bước trước.) |
| Short Description | "AWS chatbot" |
| Usage Hint | `[your question]` (ví dụ: "`/askfaq What is aws?`") |

#### Bước 3: Cấp quyền cho app

1. Chọn **OAuth & Permissions** từ thanh menu bên trái
2. Trong mục **Scopes**:
   - `commands` 
   - `chat:write`
   - `files:read`
   - `calls:write` 
![ConnectPrivate](/images/2.pre/2.4.Slack/24oauth.png) 

#### Bước 4: Bật Event Subscriptions

1. Truy cập mục `Event Subscriptions`
2. Trong mục này, bật `Enable Events`
3. Nhập Request URL: `https://<your-api-id>.execute-api.<region>.amazonaws.com/slack` (endpoint từ **API Gateway** mà bạn đã tạo ở bước trước.)
4. Subscribe to bot events: file_shared
5. Save changes
![ConnectPrivate](/images/2.pre/2.4.Slack/24verify.png) 

#### Bước 5: Cài đặt app vào workspace

1. Truy cập mục **Install App**
2. Nhấn **Install to Workspace**
3. Chọn `Allow`

✅ Sau khi cài đặt, bạn sẽ thấy:
- **Bot User OAuth Token**
- **Verification Token** (dùng để xác minh request trong Lambda nếu muốn, tuy nhiên hiện tại chúng ta sẽ không làm điều này)

#### Bước 5: Kiểm tra Slash Command
Mở Slack và thử gõ bất kì để xem có nhận được phản hồi không. Hiện tại tới bước này chỉ cần có phản hồi là được. Nếu API Gateway bị lỗi, Slack sẽ báo lỗi Timeout. Mọi thứ sẽ được setup thành công nếu kết quả hiển thị như ảnh:
![ConnectPrivate](/images/2.pre/2.4.Slack/24test.png) 