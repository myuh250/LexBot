---
title: "6. Dọn dẹp tài nguyên AWS"
date: "2025-07-04"
weight: 3
chapter: false
---

## Bước 6: Dọn dẹp tài nguyên sau workshop

Sau khi hoàn thành workshop, bạn nên xóa các tài nguyên AWS để tránh phát sinh chi phí không cần thiết.

### Các bước dọn dẹp:

1. **Xóa các Lambda Function**
   - Vào AWS Lambda Console
   - Xóa các function đã tạo trong workshop (ví dụ: NLPHandler, WrapOutput, ...)

2. **Xóa API Gateway**
   - Vào API Gateway Console
   - Xóa các API đã tạo cho chatbot

3. **Xóa các tài nguyên Lex**
   - Vào Amazon Lex Console
   - Xóa bot, intents, slot types liên quan

4. **Xóa S3 bucket (nếu có tạo để lưu log hoặc file tạm)**
   - Vào S3 Console
   - Xóa các bucket không còn sử dụng

5. **Xóa IAM Role và Policy**
   - Vào IAM Console
   - Xóa các role/policy chỉ dùng cho workshop (ví dụ: LambdaChatbotExecutionRole)

6. **Xóa các tài nguyên khác (nếu có):**
   - Bedrock, Translate, CloudWatch log group...

### Lưu ý
- Kiểm tra kỹ trước khi xóa để tránh ảnh hưởng tài nguyên đang dùng cho hệ thống khác.
- Sau khi xóa, kiểm tra lại billing để đảm bảo không còn chi phí phát sinh.

Chúc bạn hoàn thành workshop thành công và tối ưu chi phí AWS!
