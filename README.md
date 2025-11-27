# [CrofAI](https://ai.nahcrof.com/)
API documentation for crofAI
# API/SDK
CrofAI supports the OpenAI SDK for LLM inference. Below will be a python example.
## Python (No Streaming)
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ai.nahcrof.com/v2",
    api_key="api-key-here"
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
    api_key="api-key-here"
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
<!--## Python (embedding model)
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ai.nahcrof.com/v2",
    api_key="api-key"
)

response = client.embeddings.create(
    input="The quick brown fox jumps over the lazy dog",
    model="multilingual-e5-large-instruct",
)

print("Embedding:", response.data[0].embedding[:5])  # Print first 5
print("Total tokens:", response.usage.total_tokens)
```-->

# Optional endpoints:
### OpenAI Compatible
```
base_url: https://ai.nahcrof.com/v2
OR
base_url: https://ai.nahcrof.com/v1 # available for tools that don't support /v2

EXAMPLE
https://ai.nahcrof.com/v2/chat/completions
https://ai.nahcrof.com/v1/chat/completions
```
### Anthropic endpoint:
```
base_url: https://anthropic.nahcrof.com/v1

EXAMPLE
https://anthropic.nahcrof.com/v1/messages
```

# Supported Parameters
- max_tokens
- temperature
- top_p
- stop
- seed
- tools

# AI models / API MODEL-NAME
These are available on the /pricing page

# 429 Error codes
By default, CrofAI does not serve 429 error codes because we do not intend on rate limiting users. However, in the event that a user is registered as a likely bot account (this is unrelated to the amount of usage you handle) your account will only receive 429 error codes. If this is happening to you, contact support and we will unblock your account
