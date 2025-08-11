---
title : "2.5 Tạo Amazon Lex Bot"
date: "2025-07-04" 
weight : 2
chapter : false
---
Trong phần này, chúng ta sẽ tạo một **Amazon Lex** bot đơn giản để nhận input từ người dùng và đưa ra phản hồi phù hợp. 

#### Bước 1: Chỉnh sửa trust policy

- Truy cập vào **IAM Roles**, thực hiện chỉnh sửa policy hiện tại để thực sự truy cập được vào **Lex V2**.
- Nhấn vào tab **Trust Relationships**, bấm **edit trust policy**
- Thay thế phần Service cũ bằng file mới dưới đây:
```json
"Service": [
    "lambda.amazonaws.com",
    "lexv2.amazonaws.com"
]
```
- Nhấn **[Update Trust Policy]** để lưu thay đổi.

#### Bước 2: Truy cập Lex Console

- Truy cập: [https://console.aws.amazon.com/lexv2](https://console.aws.amazon.com/lexv2)
- Chọn **Create bot**

#### Bước 3: Cấu hình cơ bản cho bot

| Trường | Giá trị |
|---|---|
| Method | Create a blank bot |
| Bot name | `askBot` |
| IAM role | chọn role đã tạo |
| Description | Lex bot to receive user questions and provide natural responses. |
| Bot error logging | Enabled |
| COPPA | No |

#### Bước 4: Cấu hình ngôn ngữ 

- Chọn ngôn ngữ là **English (US)**.
- **Voice interaction**: None, vì ta dùng Polly riêng.
- Bấm **Next** để hoàn thành.
![ConnectPrivate](/images/2.pre/2.5.Lex/25lexdone.png) 

### Bước 5: Tạo Intent và thêm câu nói mẫu (Utterances)

* Trong phần cấu hình bot → Chọn ngôn ngữ `English (US)` → vào mục **Intents**.
* Nhấn **+ Add intent** → chọn **Add empty intent**.
* Đặt tên cho intent, ví dụ: `GreetIntent`.
* Thêm Sample Utterances:

```
Hello
Hi
Hey there
Good morning
```

Thiết lập Response:
```
Hello! How can I help you today?
```

* Bấm **Save** để lưu intent.

#### Bước 6: Build bot language

* Quay lại trang chính của bot, tại tab `English (US)` → bấm nút **Build** để build ngôn ngữ.
* Đợi status chuyển sang **Built successfully**.
![ConnectPrivate](/images/2.pre/2.5.Lex/25en.png) 

#### Bước 7: Tạo Bot Version
- Trong menu bên trái, chọn **Bot versions**.
- Bấm **Create version**.
- (Tùy chọn) Nhập mô tả cho version.
- Bấm **Create**.

Sau khi tạo xong, bạn sẽ thấy version mới hiển thị như `Version 1`, `Version 2`, v.v. 
![ConnectPrivate](/images/2.pre/2.5.Lex/25ver.png) 

Tiếp theo chúng ta sẽ gán version cho alias:
- Chọn **Aliases** từ menu bên trái (trong mục Deployment).
- Chọn alias bạn muốn gán version (ví dụ `TestBotAlias`).
- Bấm **Associate version with alias**.
- Chọn version bạn vừa tạo từ dropdown.
- Bấm **Save**.
![ConnectPrivate](/images/2.pre/2.5.Lex/25alias.png) 

Bây giờ trong trang alias bạn sẽ thấy:
- **Alias name**: Tên alias bạn đã đặt.
- **Alias ID**: Dòng có ghi `ID: TSTALIASID` - đây chính là `Alias ID`, lưu giá trị này lại cho bước phía dưới.
