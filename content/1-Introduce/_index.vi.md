---
title : "Giới thiệu"
date: "2025-07-04" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Trong phần này, chúng ta sẽ cùng nhau tìm hiểu nhanh về các service quan trọng nhất trong kiến trúc của giải pháp.

#### Amazon Lex

Amazon Lex là dịch vụ xử lý ngôn ngữ tự nhiên (NLU) và nhận dạng giọng nói (ASR) do AWS cung cấp. Cùng công nghệ đằng sau Alexa, Lex cho phép bạn tạo và triển khai các chatbot hiểu cả văn bản và giọng nói một cách dễ dàng, hội thoại tự nhiên như khi trò chuyện với con người. Lex hỗ trợ gắn sẵn intent, slot-filling và điều hướng hội thoại, giúp bot có thể thực thi các tác vụ như đặt lịch, tra cứu thông tin, hoặc trả lời câu hỏi khách hàng

Lex còn tích hợp sẵn với các kênh phổ biến như Slack, SMS, Amazon Connect, cho phép bot hoạt động đa kênh một cách liền mạch. Với việc kết hợp Lambda, Lex xử lý tốt backend logic và dễ dàng tương tác với các dịch vụ lưu trữ dữ liệu hoặc hệ thống quản lý khách hàng (CRM).

![ConnectPrivate](/lexbot/images/1.intro/lex.png) 

#### Amazon Polly

Amazon Polly là dịch vụ chuyển văn bản thành giọng nói (TTS) tích hợp công nghệ neural tiên tiến, cho phép tạo ra các giọng đọc tự nhiên, dễ nghe và biểu cảm tốt. Polly hỗ trợ hơn 100 giọng trong hơn 41 ngôn ngữ khác nhau, bao gồm cả các giọng long‑form và generative để đáp ứng các trường hợp yêu cầu âm thanh chất lượng cao.

Polly cũng cung cấp các tính năng tinh chỉnh như SSML (Speech Synthesis Markup Language) và custom lexicons để điều chỉnh ngữ điệu, phát âm từ chuyên ngành hoặc tên riêng. Điều này giúp bot phản hồi bằng âm thanh một cách chuyên nghiệp, phù hợp với các kịch bản như hướng dẫn audio, IVR, và chăm sóc khách hàng bằng giọng nói.

![ConnectPrivate](/lexbot/images/1.intro/polly.png) 

#### Amazon Bedrock

Amazon Bedrock là dịch vụ cung cấp đa dạng các foundation model (FM) từ nhiều nhà cung cấp hàng đầu như Anthropic, Cohere, Stability AI, và mô hình của AWS thông qua một API duy nhất. Đây là nền tảng serverless, cho phép bạn nhanh chóng tùy chỉnh mô hình với dữ liệu doanh nghiệp bằng kỹ thuật như fine-tuning hoặc Retrieval-Augmented Generation (RAG)

Trong kiến trúc chatbot, khi Lex không thể xác định intent hoặc confidence thấp, Bedrock đóng vai trò là lớp fallback thông minh, xử lý các câu hỏi phức tạp, ngữ cảnh ngoài training data hoặc yêu cầu chuyên sâu. Bạn có thể sử dụng QnAIntent kết hợp Knowledge Bases để truy vấn dữ liệu nội bộ và tạo phản hồi contextual và chính xác hơn.

![ConnectPrivate](/lexbot/images/1.intro/bedrock.png)
