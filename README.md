<div align="center">
  <h1>Conversational AI with Lex, Polly and Custom Models</h1>
  <p><strong>Step-by-step guide to building a multilingual conversational AI solution on AWS</strong></p>
</div>

---

## 📚 About This Project

This repository contains a Hugo-based documentation website for a hands-on lab on building a conversational AI solution using AWS services (Lex, Polly, Lambda, S3, IAM, API Gateway, Bedrock, v.v.) and custom NLP models. The guide is designed for the AWS First Cloud Journey program and supports both English and Vietnamese.

**Key Features:**
- Multilingual documentation (EN/VI)
- Step-by-step AWS resource provisioning (EC2, IAM, S3, Lambda, API Gateway, Lex, Polly, Bedrock)
- Slack integration for chatbot input/output
- Custom NLP and sentiment analysis
- Infrastructure as code and automation
- Security and cleanup best practices

## 🛠️ Tech Stack

- **Documentation Framework:** [Hugo](https://gohugo.io/)
- **Cloud Platform:** AWS (Lex, Polly, Lambda, S3, IAM, API Gateway, Bedrock, v.v.)
- **Deployment:** GitHub Pages, Netlify, or any static hosting

## 🚀 Lab Modules

1. **Giới thiệu / Introduction**
2. **Chuẩn bị / Prerequisites** (AWS Account, IAM, Lambda, API Gateway, Slack, Lex, Bedrock)
3. **Xử lý đầu vào / Processing Incoming Message**
4. **Xử lý ngôn ngữ tự nhiên / Generate Response**
5. **Gửi phản hồi / Send Output**
6. **Dọn dẹp / Cleanup**

## Getting Started
To view or contribute to this documentation locally:

1. **Install Hugo**
2. **Clone this repository:**
  ```sh
  git clone https://github.com/myuh250/lexbot.git
  cd lexbot
  ```
3. **Start the local server:**
  ```sh
  hugo serve
  ```
4. **Open** [http://localhost:1313](http://localhost:1313) in your browser.

## Deployment
This site deployed at myuh250.github.io/lexbot/

## Author
This guide and documentation site was written by Nguyen Tien Huy under AWS First Cloud Journey Training Program.

## License
This project is licensed under the MIT License.