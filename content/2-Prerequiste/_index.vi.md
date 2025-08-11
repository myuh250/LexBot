---
title : "2. Các bước chuẩn bị"
date: "2025-07-04" 
weight : 2
chapter : false
---
### Những dịch vụ cần cho bài lab

Để thực hiện bài lab này, chúng ta sẽ cần những services sau:

- **API Gateway**: Endpoint HTTP để tiếp nhận yêu cầu từ Slack
- **Lambda Functions**: Điều phối logic: xử lý input, routes.
- **Amazon S3**: Lưu trữ tạm thời các file audio từ Slack
- **Amazon Transcribe**: Chuyển đổi audio sang văn bản
- **Amazon Translate**: Dịch ngôn ngữ (auto-detect) về tiếng Anh và dịch ngược về ngôn ngữ input của người dùng
- **Amazon Lex**: Nhận diện intent từ input người dùng
- **Amazon Bedrock**: Fallback để xử lý query không rõ intent (FAQ style)
- **Amazon Polly**: Chuyển output thành giọng nói nếu người dùng yêu cầu
- **Amazon CloudWatch**: Ghi log toàn bộ quá trình xử lý

---

⚠️ **Cảnh Báo Chi Phí**  
Bài lab này sử dụng nhiều dịch vụ AI của AWS như Amazon Lex, Bedrock, và Polly.  
Nếu không được giám sát cẩn thận, các dịch vụ này **có thể phát sinh chi phí đáng kể**, đặc biệt khi xử lý khối lượng dữ liệu lớn hoặc sử dụng các tính năng nâng cao như mô hình sinh văn bản (generative models).  
Bạn **nên** thực hiện các bước sau để kiểm soát chi phí:

- Thiết lập hạn mức và ngân sách cho các dịch vụ
- Theo dõi mức sử dụng qua AWS Billing Dashboard
- Dọn dẹp tài nguyên không sử dụng sau khi hoàn thành bài lab

---

💡 **Yêu Cầu Có Slack Workspace**  
Bạn cần có quyền truy cập vào một Slack workspace.  
Bài lab này sử dụng Slack làm giao diện chính để nhận input từ người dùng.
