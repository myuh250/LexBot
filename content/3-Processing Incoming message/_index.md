---
title : "3. Processing Incoming Data"
date: "2025-07-04" 
weight : 2
chapter : false
---
In this section, we will extend the **InputHandler** Lambda function to:
- Detect whether the input from Slack is **text** or **audio**
- If it is **text** → Automatically detect the language and translate it to English (note: you need a paid plan to use this feature)

#### Step 1: Update IAM Role for Lambda (optional if you want to send audio)
To access S3, you need to update the IAM role you created earlier.
1. Go back to the **LambdaChatbotExecutionRole** role, in the **Permissions policies** tab, click **Attach Policies**.
![ConnectPrivate](/images/9.input/9modrole.png) 
2. Search for and check the `AmazonS3FullAccess` policy.
3. Search for and check the `AmazonTranscribeFullAccess` policy to use Transcribe.
   Since this workshop does not focus strictly on role management or permissions, we will use full access for convenience.
4. Click **Add Permissions** to complete.
5. ![ConnectPrivate](/images/9.input/9donerole.png)

#### Step 2: Prepare S3 bucket (optional if you want to send audio)
1. Go to the S3 console: https://s3.console.aws.amazon.com/s3
2. Click **Create bucket**
3. Enter the following information:
   - Bucket name: `aws-chatbot-audio`
   - Region: Same region as your other services.
   - Leave other options as default.
4. Click **Create bucket** to finish.

#### Step 3: Update the InputHandler Lambda function
Go to the Lambda console, open the InputHandler function, and update the code as below. Replace **BOT_ID** and **BOT_ALIAS_ID** with your actual values:

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

    # Translate text to English if needed
    original_language = 'en'
    english_text = text  # Default initialization

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

#### Step 4: Deploy & Test

1. **Test** and **Deploy** the Lambda with the code above.
   > You should increase the Lambda timeout to 15 seconds to avoid errors.
2. Test with Slack:
   - Send text: the bot will translate → Lex → respond
   - Send audio: the bot will transcribe → Lex → respond (currently not supported)
![ConnectPrivate](/images/9.input/9doneinput.png)

> After deploying and testing the function:
> 1. Go to **AWS Console** → **Lambda** → select your function
> 2. In the sidebar, click **Monitor** → **View logs in CloudWatch**
> 3. Search by execution time → open the latest log stream to see details

![ConnectPrivate](/images/9.input/9log.png) 