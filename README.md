# [CrofAI](https://ai.nahcrof.com/)
API documentation for crofAI
# API/SDK
CrofAI supports the OpenAI SDK for LLM inference. Below will be a python example.
## Python (No Streaming)
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://ai.nahcrof.com/v2",
    api_key="api-key"
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
    api_key="api-key"
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
llama3.1-8b
llama3.3-70b
llama3.1-405b
llama3.1-tulu3-405b
deepseek-r1
deepseek-r1-turbo
deepseek-r1-0528
deepseek-v3
deepseek-v3-0324
deepseek-r1-distill-llama-70b
deepseek-r1-distill-qwen-32b
qwen-qwq-32b
gemma-3-27b-it
llama-4-scout
qwen3-235b-a22b
multilingual-e5-large-instruct
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

deepseek-r1-turbo
Average speed: ~230 tokens/second

deepseek-r1-0528
Average speed: ~30 tokens/second

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

qwen3-235b-a22b
Average speed: ~60 tokens/second

multilingual-e5-large-instruct
average speed: ~75 tokens/second
```
