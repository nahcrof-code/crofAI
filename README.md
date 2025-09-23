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
## Python (embedding model)
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
```

# AI models / API MODEL-NAME
```
kimi-k2-0905
kimi-k2
kimi-k2-turbo
kimi-k2-eco
glm-4.5
gpt-oss-120b
qwen3-coder
qwen3-235b-a22b-2507-instruct
qwen3-235b-a22b
llama3.1-8b
llama3.3-70b
deepseek-v3.1-terminus
deepseek-v3.1-terminus-reasoner
deepseek-v3.1
deepseek-v3.1-reasoner
deepseek-r1-0528
deepseek-v3-0324
deepseek-v3-0324-turbo
deepseek-r1-distill-llama-70b
deepseek-r1-distill-qwen-32b
gemma-3-27b-it
gemma-3n-e4b-it
llama-4-scout
```
