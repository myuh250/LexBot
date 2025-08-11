---
title : "Introduction"
date: "2025-07-04" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
In this section, we will quickly explore the most important services in the solution's architecture.

#### Amazon Lex

Amazon Lex is a natural language understanding (NLU) and automatic speech recognition (ASR) service provided by AWS. Powered by the same technology behind Alexa, Lex allows you to easily build and deploy chatbots that understand both text and voice, enabling natural conversations similar to human interactions. Lex supports built-in intents, slot-filling, and conversation flow, allowing bots to perform tasks like scheduling appointments, retrieving information, or answering customer inquiries.

Lex also integrates natively with popular channels such as Slack, SMS, and Amazon Connect, enabling seamless multi-channel operation. When combined with AWS Lambda, Lex handles backend logic effectively and can easily interact with data storage services or customer relationship management (CRM) systems.

![ConnectPrivate](/lexbot/images/1.intro/lex.png) 

#### Amazon Polly

Amazon Polly is a text-to-speech (TTS) service that uses advanced neural technology to generate natural-sounding, expressive, and easy-to-understand speech. Polly supports over 100 voices in more than 41 languages, including long-form and generative voices to meet high-quality audio requirements.

Polly also provides customization features such as SSML (Speech Synthesis Markup Language) and custom lexicons to control intonation, pronunciation of technical terms, or proper names. This enables bots to deliver professional audio responses, suitable for use cases like voice-guided tutorials, IVR systems, and voice-based customer service.

![ConnectPrivate](/lexbot/images/1.intro/polly.png) 

#### Amazon Bedrock

Amazon Bedrock is a service that offers a wide range of foundation models (FMs) from leading providers such as Anthropic, Cohere, Stability AI, and AWS—accessible through a single API. It is a serverless platform that allows you to quickly customize models with your business data using techniques like fine-tuning or Retrieval-Augmented Generation (RAG).

In a chatbot architecture, when Lex cannot determine an intent or has low confidence, Bedrock acts as an intelligent fallback layer. It handles complex questions, out-of-training data context, or specialized queries. You can use QnAIntent together with Knowledge Bases to query internal data and generate more contextual and accurate responses.

![ConnectPrivate](/lexbot/images/1.intro/bedrock.png) 

