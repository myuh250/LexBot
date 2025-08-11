
# Project Proposal: Multi-language AI Chatbot for SMEs using AWS

---

## 1. Executive Summary

Doanh nghiệp vừa và nhỏ (SME) cần một giải pháp AI chatbot thông minh, đa ngôn ngữ để nâng cao trải nghiệm khách hàng. Các hệ thống rule-based truyền thống quá cứng nhắc, thiếu tự nhiên, khiến khách hàng dễ bỏ cuộc. Đề xuất này xây dựng một chatbot serverless đa ngôn ngữ trên AWS (Translate, Lambda, Lex, Bedrock, Polly), xử lý đầu vào bất kỳ ngôn ngữ nào, chuẩn hóa sang English để xử lý, rồi trả lời lại ngôn ngữ gốc bằng văn bản hoặc âm thanh.

---

## 2. Problem Statement

- **Tính linh hoạt thấp:** Chatbot rule-based khó hiểu, dễ gây khó chịu cho khách hàng.
- **Hạn chế nguồn lực:** SME thiếu nhân sự và kiến thức AI/ML.
- **Rủi ro mất khách hàng:** Chatbot kém hiệu quả có thể làm giảm tỷ lệ chuyển đổi 20–40%.

---

## 3. Solution Architecture

![Solution Architecture](static/images/LexnPolly.png)

**Dịch vụ AWS sử dụng:**

- **Translate:** Hỗ trợ đa ngôn ngữ dễ dàng
- **Transcribe:** Chuyển đổi giọng nói thành văn bản
- **Lex:** Hiểu intent, xử lý hội thoại cơ bản
- **Bedrock:** Fallback AI mạnh mẽ (Claude, Titan…) cho câu hỏi phức tạp
- **Polly:** Trả lời âm thanh tự nhiên
- **Lambda + API Gateway:** Serverless, dễ mở rộng, tiết kiệm chi phí
- **IAM:** Bảo mật least privilege, CloudWatch giám sát

---

## 4. Technical Implementation & Timeline (4 tuần)

| Phase   | Nội dung |
|---------|---------------------------------------------------------------|
| Week 1  | Tạo IAM Role cho Lambda (least privilege), tạo Lambda functions (InputHandler, NLP Handler, OutputWrapper), cấu hình Translate, Lex, Polly |
| Week 2  | Tích hợp API Gateway, nhận dữ liệu từ Slack, xử lý dịch, setup Lex intent cơ bản |
| Week 3  | Thiết lập fallback Bedrock, tối ưu prompt, xử lý đa ngôn ngữ logic, xử lý audio (Polly) |
| Week 4  | Test toàn diện qua Slack, tối ưu performance, log (CloudWatch), viết documentation, bàn giao sản phẩm |

---

## 5. Budget Estimation (Chi phí ước tính hàng tháng)

- Translate / Polly / Lex / Lambda / API Gateway: tổng khoảng **$5–10/tháng** cho demo 1–2 người dùng.
- **Amazon Bedrock (Claude Instant):** Chi phí theo token, ví dụ Claude 3 Sonnet ~$0.003/1K input token + $0.015/1K output token.
- Với 10,000 tokens input + 20,000 tokens output/ngày → ~0.002 USD/request, tổng ~3 USD/tháng (dùng nhiều sẽ cao hơn).

---

## 6. Risk Assessment & Monitoring

- **Chi phí phát sinh:** Nếu người dùng gửi nhiều yêu cầu → Giới hạn tần suất, cache kết quả.
- **Lỗi hệ thống:** Lex không hiểu, Bedrock trả lỗi → Log CloudWatch, fallback rõ ràng, alert 5xx.
- **Bảo mật:** IAM least privilege, xác thực Slack signature (HMAC).

---

## 7. Expected Outcomes

- Tốc độ phản hồi trung bình **< 1 giây**
- Độ chính xác phản hồi **> 90%**
- Sẵn sàng hoạt động **≥ 99%**
- UX chatbot hài lòng người dùng **≥ 85%**, tỷ lệ dùng lại trong 7 ngày **> 60%**

