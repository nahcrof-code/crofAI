# [CrofAI](https://ai.nahcrof.com/)
API documentation for crofAI
# API/SDK
CrofAI supports the OpenAI SDK for LLM inference. Below will be a python example.
## Python (No Streaming)
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ai.nahcrof.com/v1",
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
    base_url="https://ai.nahcrof.com/v1",
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
## Python (tool use)
```python
from openai import OpenAI
import json

client = OpenAI(
    base_url="https://ai.nahcrof.com/v1",
    api_key="api-key-here"
)

tools = [{ # tootally original example
    "type": "function",
    "function": {
        "name": "get_horoscope",
        "description": "Get today's horoscope for an astrological sign.",
        "parameters": {
            "type": "object",
            "properties": {
                "sign": {
                    "type": "string",
                    "description": "An astrological sign like Taurus or Aquarius",
                },
            },
            "required": ["sign"],
            "additionalProperties": False,
        },
        "strict": True,
    },
}]

def get_horoscope(sign):
    return f"{sign}: Next Tuesday you will befriend a baby otter."

messages = [
    {"role": "user", "content": "What is my horoscope? I am an Aquarius."}
]

stream = client.chat.completions.create(
    model="MODEL-FROM-LIST",
    messages=messages,
    tools=tools,
    stream=True
)

for chunk in stream:
    delta = chunk.choices[0].delta

    if delta.content:
        print(delta.content, end="", flush=True)

    if delta.tool_calls:
        for tc in delta.tool_calls:
            print(f"\nTool call: {tc.function.name}")
            if tc.function.arguments:
                print(f"Args: {tc.function.arguments}")
```

## /models API
when visiting `/v1/models` you will receive a standard JSON list containing each model. Each model should look as follows
```json
{
    "context_length": 163840,
    "created": 1755799640,
    "id": "deepseek-v3.2",
    "max_completion_tokens": 163840,
    "name": "DeepSeek: DeepSeek V3.2",
    "pricing": {
        "completion": "0.00000038", // $0.38/m output
        "prompt": "0.00000028" // $0.28/m input
    },
    "quantization": "Q4_0",
    "speed": 50 // rough estimate
},
```

<!--## Python (embedding model)
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ai.nahcrof.com/v1",
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
base_url: ```https://ai.nahcrof.com/v2``` (the original endpoint)<br>
OR<br>
base_url: ```https://ai.nahcrof.com/v1``` (same thing, just more standard)
```
EXAMPLE
https://ai.nahcrof.com/v2/chat/completions
https://ai.nahcrof.com/v1/chat/completions
```
### Anthropic endpoint:
base_url: ```https://anthropic.nahcrof.com```
```
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

<!--
This is technically still true but is commented out since we have changed anti bot measures and **should** be impossible to encounter
# 429 Error codes
By default, CrofAI does not serve 429 error codes because we do not intend on rate limiting users. However, in the event that a user is registered as a likely bot account (this is unrelated to the amount of usage you handle) your account will only receive 429 error codes. If this is happening to you, contact support and we will unblock your account
-->
