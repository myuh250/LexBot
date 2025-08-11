---
title : "2.3 Cài đặt API Gateway"
date: "2025-07-04" 
weight : 2
chapter : false
---
#### Bước 1: Mở giao diện API Gateway

- Truy cập: 👉 https://console.aws.amazon.com/apigateway
- Chọn **Create API**

#### Bước 2: Chọn loại API

- Chọn: **HTTP API** (nhẹ hơn, đơn giản hơn để xử lý các webhook như Slack) → Bấm **Build**

#### Bước 3: Thiết lập routes
Ở mục đặt tên, hãy nhập: `SlackChatbotAPI` (hoặc bất cứ tên nào bạn thích)

1. Add integration
- Integration type: `Lambda function`
- Region: cùng region với Lambda bạn tạo
- Function: `InputHandler` *(function bạn đã tạo để xử lý Slack input)*
![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23integ.png) 

2. Add route
- Method: `POST`
- Resource path: `/slack` 
![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23route.png)

→ Bấm **Next**

#### Bước 4: Kiểm tra & khởi tạo

Các thông tin còn lại có thể để ở mặc định. Review lại các thông tin sau đó khởi tạo gateway.
![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23a.png)

---

#### Bước 5: Lấy URL để cấu hình vào Slack

Sau khi tạo xong, nếu click vào API Gateway này, bạn sẽ thấy **Invoke URL tại phần stage**, ví dụ:

![ConnectPrivate](/lexbot/images/2.pre/2.3.API/23url.png)

```
https://abc123xyz.execute-api.us-east-1.amazonaws.com
```

Đường dẫn cuối cùng sẽ là:
```
https://abc123xyz.execute-api.us-east-1.amazonaws.com/slack

```

📌 **Copy URL này lại.**