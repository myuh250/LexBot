---
title : "3. Xử lý dữ liệu đầu vào"
date: "2025-07-04" 
weight : 2
chapter : false
---
Trong phần này, chúng ta sẽ mở rộng Lambda function **InputHandler** để:
- Nhận biết input từ Slack là **text** hay **audio** 
- Nếu là **text** → Tự động detect ngôn ngữ và dịch sang tiếng Anh (lưu ý, bạn phải dùng paid plan mới dùng được chức năng này)

#### Bước 1: Cập nhật IAM Role cho Lambda (optional nếu muốn gửi audio)
Để có thể truy cập được vào s3, chúng ta cần cập nhật thêm IAM role đã tạo trước đó. 
1. Di chuyển trở lại role **LambdaChatbotExecutionRole**, tại tab **Permissions policies**, nhấn vào **Attach Policies**.
![ConnectPrivate](/images/9.input/9modrole.png) 
2. Tìm kiếm và tick vào policy `AmazonS3FullAccess`. 
3. Tìm kiếm và tick vào policy `AmazonTranscribeFullAccess` để có thể sử dụng Transcribe.
   Vì bài workshop này không có tập trung khắc khe về việc quản lý các role cũng như hành vi họ được làm, nên chúng ta sẽ sử dụng full access để được tiện lợi nhất.
4. Nhấn **Add Permissions** để hoàn tất.
5. ![ConnectPrivate](/images/9.input/9donerole.png) 

#### Bước 2: Chuẩn bị S3 bucket (optional nếu muốn gửi audio)
1. Truy cập S3 console: https://s3.console.aws.amazon.com/s3
2. Nhấn **Create bucket**
3. Điền thông tin như sau:
   - Bucket name: `aws-chatbot-audio`
   - Region: Cùng region với các dịch vụ khác đã tạo.
   - Các tùy chọn khác để mặc định.
4. Nhấn **Create bucket** để hoàn tất.

#### Bước 3: Cập nhật Lambda function InputHandler
Vào Lambda console, mở lại function InputHandler, cập nhật code như dưới đây, trong đó thay đổi biến **BOT_ID** và **BOT_ALIAS_ID** với giá trị thực sự của bạn:

```python
import json
import boto3
import urllib.request
import uuid
import time
import base64
from urllib.parse import parse_qs

s3 = boto3.client('s3')
translate = boto3.client('translate')
transcribe = boto3.client('transcribe')
lex = boto3.client('lexv2-runtime')

BUCKET_NAME = 'aws-chatbot-audio'
BOT_ID = '<YourLexBotId>' # replace with your actual Bot ID
BOT_ALIAS_ID = '<YourAliasBotId>' #   replace with your actual Bot Alias ID
LOCALE_ID = 'en_US'

def is_audio(event):
    files = event.get('files', [])
    return any(f.get('mimetype', '').startswith('audio/') for f in files)

def download_audio_file(url, headers):
    req = urllib.request.Request(url, headers=headers)
    return urllib.request.urlopen(req).read()

def upload_to_s3(data, key):
    s3.put_object(Bucket=BUCKET_NAME, Key=key, Body=data)
    return f's3://{BUCKET_NAME}/{key}'

def start_transcribe(s3_uri):
    job = f"job-{uuid.uuid4()}"
    transcribe.start_transcription_job(
        TranscriptionJobName=job,
        Media={'MediaFileUri': s3_uri},
        MediaFormat='mp3',
        LanguageCode='en-US',
        OutputBucketName=BUCKET_NAME
    )
    return job

def poll_transcribe(job, timeout=30):
    elapsed = 0
    while elapsed < timeout:
        res = transcribe.get_transcription_job(TranscriptionJobName=job)
        status = res['TranscriptionJob']['TranscriptionJobStatus']
        if status == 'COMPLETED':
            return res['TranscriptionJob']['Transcript']['TranscriptFileUri']
        elif status == 'FAILED':
            raise Exception("Transcribe job failed")
        time.sleep(3)
        elapsed += 3
    raise TimeoutError("Transcribe job timed out")

def fetch_text(uri):
    with urllib.request.urlopen(uri) as r:
        return json.loads(r.read())['results']['transcripts'][0]['transcript']

def send_to_lex(text, session_id):
    res = lex.recognize_text(
        botId=BOT_ID,
        botAliasId=BOT_ALIAS_ID,
        localeId=LOCALE_ID,
        sessionId=session_id,
        text=text
    )
    return res['messages'][0]['content'] if res.get('messages') else "No reply from Lex."

def lambda_handler(event, context):
    print("Incoming event:", event)
    
    # Handle Slack URL verification challenge
    if 'body' in event:
        body_str = event['body']
        if event.get('isBase64Encoded', False):
            body_str = base64.b64decode(body_str).decode('utf-8')
        
        try:
            # Try to parse as JSON first (for Slack events)
            body_json = json.loads(body_str)
            if body_json.get('type') == 'url_verification':
                print("Handling Slack URL verification challenge")
                return {
                    'statusCode': 200,
                    'body': body_json.get('challenge', ''),
                    'headers': {'Content-Type': 'text/plain'}
                }
        except json.JSONDecodeError:
            # If not JSON, continue with existing logic (form data)
            pass
    
    text = ""

    if 'body' in event and 'headers' in event:
        body_str = event['body']
        if event.get('isBase64Encoded', False):
            body_str = base64.b64decode(body_str).decode('utf-8')
            print(f"Decoded body: {body_str}")
        
        # Try to parse as form data (for slash commands)
        try:
            body = parse_qs(body_str)
            text = body.get('text', [''])[0].strip()
            print("Input type: TEXT (via Slack slash command)")
            print(f"Received text: '{text}'")
        except:
            print("Could not parse as form data")

    if not text or not text.strip():
        return {
            'statusCode': 200,
            'body': json.dumps({
                'text': 'Please provide some text.',
                'response_type': 'ephemeral'
            }),
            'headers': {'Content-Type': 'application/json'}
        }

    # Translate text sang tiếng Anh nếu cần
    original_language = 'en'
    english_text = text  # Khởi tạo mặc định

    try:
        translate_result = translate.translate_text(
            Text=text,
            SourceLanguageCode='auto',
            TargetLanguageCode='en'
        )
        original_language = translate_result['SourceLanguageCode']
        english_text = translate_result['TranslatedText']
        print(f"Detected language: {original_language}")
        print(f"Translated to English: '{english_text}'")
    except Exception as e:
        print(f"Translation error: {e}")
        original_language = 'en'
        english_text = text  # Fallback to original text

    print("Sending text to Lex...")
    lex_msg = send_to_lex(english_text, session_id="test-session-123")  
    print(f"Lex replied: {lex_msg}")

    return {
        'statusCode': 200,
        'body': json.dumps({
            'text': lex_msg,
            'response_type': 'in_channel'
        }),
        'headers': {'Content-Type': 'application/json'}
    }
```

#### Bước 4: Triển khai & Test

1. **Test** và **Deploy** Lambda có code trên.
   > Bạn nên tăng timeout time cho lanbda lleen 15 giây để tránh lỗi
2. Test Slack:
   - Gửi text: bot sẽ translate → Lex → phản hồi
   - Gửi audio: bot sẽ transcribe → Lex → phản hồi (hien tai chua su dung duoc)
![ConnectPrivate](/images/9.input/9doneinput.png) 

> Sau khi deploy và test function:
> 1. Vào **AWS Console** → **Lambda** → chọn function của bạn
> 2. Trong sidebar, click **Monitor** → **View logs in CloudWatch**
> 3. Tìm theo thời gian thực thi → mở log stream gần nhất để xem chi tiết

![ConnectPrivate](/images/9.input/9log.png) 