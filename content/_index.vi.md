---
title : "Session Management"
date: "2025-07-04" 
weight : 1 
chapter : false
---
# Conversational AI với Lex, Polly và Custom Models

### Tổng quan

Muc tiêu của chúng ta trong bài lab này là phát triển một giải pháp AI hội thoại nâng cao kết hợp giữa các dịch vụ AWS và mô hình NLP tùy chỉnh, hỗ trợ tương tác đa ngôn ngữ và hiểu cảm xúc con người. Chúng ta sẽ tập trung chủ yếu vào việc giao tiếp bằng văn bản. 

### Mục tiêu hệ thống

- Nhận input từ Slack (qua Webhook hoặc Slash Command)
- Phân tích và hiểu ngôn ngữ tự nhiên với Lex + custom NLP
- Tạo phản hồi phù hợp (có thể là text hoặc audio)
- Hỗ trợ tương tác đa ngôn ngữ
- Giám sát và phân tích dữ liệu được truyền qua suốt quá trình làm việc

### Sơ đồ hệ thống

![ConnectPrivate](/images/LexnPolly.png) 

### Nội dung

 1. [Giới thiệu](1-introduce/)
 2. [Các bước chuẩn bị](2-Prerequiste/)
 3. [Xử lý đầu vào](3-Processing%20Incoming%20message/)
 4. [Xử lý ngôn ngữ tự nhiên](4-Generate%20response/)
 5. [Gửi phản hồi](5-Send%20output)
 6. [Dọn dẹp](6-Cleanup/)


