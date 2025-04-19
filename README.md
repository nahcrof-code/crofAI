# [CrofAI](https://ai.nahcrof.com/)
API documentation for crofAI
## Donation page 
https://buymeacoffee.com/crofai
# API/SDK
CrofAI supports the OpenAI SDK for LLM inference. Below will be a python example.
## Python (No Streaming)
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ai.nahcrof.com/v2",
    api_key="optional-api-key"
)
response = client.chat.completions.create(
    model="MODEL-FROM-LIST",
    messages=[
        {"role": "user", "content": "Hello!"}
    ]
)
print(response.choices[0].message.content)
```
## Python (With Streaming)
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ai.nahcrof.com/v2",
    api_key="optional-api-key"
)

response = client.chat.completions.create(
    model="MODEL-FROM-LIST",
    messages=[
        {"role": "user", "content": "Howdy there! How are you?"}
    ],
    stream=True  # Enable streaming
)

for chunk in response:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

# AI models / API MODEL-NAME
```
llama3.1-8b
llama3.3-70b
llama3.1-405b
llama3.1-tulu3-405b
deepseek-r1
deepseek-v3
deepseek-v3-0324
deepseek-r1-distill-llama-70b
deepseek-r1-distill-qwen-32b
qwen-qwq-32b
gemma-3-27b-it
llama-4-scout
```
# LLM average speeds (tokens/second)
```

llama3.1-8b
Average speed: ~30 tokens/second

llama3.3-70b
Average speed: ~30 tokens/second

llama3.1-405b-Instruct
Average speed: ~30 tokens/second

llama3.1-tulu-3-405b
Average speed: ~30 tokens/second

deepseek-r1-distill-llama-70b
Average speed: ~30 tokens/second

deepseek-r1-distill-qwen-32b
Average speed: ~50 tokens/second

deepseek-r1
Average speed: ~25 tokens/second

deepseek-v3
Average speed: ~35 tokens/second

deepseek-v3-0324
Average speed: ~35 tokens/second

qwen-qwq-32b
Average speed: ~25 tokens/second

gemma-3-27b-it
Average speed: ~80 tokens/second

llama-4-scout
Average speed: ~65 tokens/second
```
# Specific API examples
## stable-diffusion-xl-base-1.0 (BETA)
### JSON
POST /v1/premium/stable_diffusion<br>
HEADERS:<br>
```
X-API-Key: api-token-here
```
BODY (application/json)
```json
{
    "prompt": "cat"
}
```
RESPONSE 200 OK
```json
{
    "response": "AI response"
}
```
### Python
```python
import requests
import json
headers = {"X-API-Key": "myapikey"}
payload = {
    "prompt": "cat"
}
r1 = requests.post(url=f'https://ai.nahcrof.com/v1/free/stable_diffusion', json=payload, headers=headers)
value = r1.json()
try:
    with open("generated_image.png", "wb") as f:
        f.write(eval(value["response"]))
except KeyError:
    print(value)
# good luck writing this in any other language, I'm so confused :)
```
