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
llama3-8b
llama3.1-8b
llama3.3-70b
llama3.2-1b
llama3-70b
llama3.1-405b
llama3.1-tulu3-405b
deepseek-r1
deepseek-v3
deepseek-r1-distill-llama-70b
deepseek-r1-distill-qwen-32b
```
# LLM average speeds (tokens/second)
```
llama3-8b-Instruct - ~550 tokens/second
Low speed example: ~300 tokens/second
Highest speed seen: ~901 tokens/second

llama3.1-8b-Instruct - ~500 tokens/second
Low speed example: ~400 tokens/second
Highest speed seen: ~595 tokens/second

llama3.2-1b-Instruct - ~1,900 tokens/second
Low speed example: ~400 tokens/second
Highest speed seen: ~41,873 tokens/second

llama3-70b-Instruct - ~150 tokens/second
Low speed example: ~90 tokens/second
Highest speed seen: ~277 tokens/second

llama3.3-70b-Instruct - ~1000 tokens/second
Low speed example: ~500 tokens/second
Highest speed seen: ~15,620 tokens/second

llama3.1-405b-Instruct - ~10 tokens/second
Low speed example: ~7 tokens/second
Highest speed seen: ~156 tokens/second

llama3.1-tulu-3-405b - ~60 tokens/second
Low speed example: ~8 tokens/second
Highest speed seen: ~595 tokens/second

deepseek-r1-distill-llama-70b - ~700 tokens/second
Low speed example: ~250 tokens/second
Highest speed seen: ~5,547 tokens/second

deepseek-r1-distill-qwen-32b - ~800 tokens/second
Low speed example: ~300 tokens/second
Highest speed seen: ~7,849 tokens/second

deepseek-r1 - ~300 tokens/second
Low speed example: ~350 tokens/second
Highest speed seen: ~637 tokens/second

deepseek-v3 - ~60 tokens/second
Low speed example: ~30 tokens/second
Highest speed seen: ~314 tokens/second
```
Models with drastically higher "highest speed seen(s)" are testing a new method of inferencing, the lower speed is significantly more common, but there is a chance for inferences to see the highest speed, just don't bet on it.
Models Like mistral-7b and qwen0.5b see little to no usage, we will be implementing higher speeds on the least used models last.
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
# Important notes
models with asterisks in the AI models section were pointed out for their relatively low speeds. These are models we typically see low usage of. We would absolutely consider allocating more resources to such models, in the event that the community displayed an increase in desire for them. 
# Streaming API
(NOTE, this is a work in progress, python code is wacky, JS is designed for client but should be easily useable)
## Models with streaming
```
llama3-8b
llama3.1-8b
llama3.2-1b
llama3-70b
llama3.3-70b
llama3.1-405b
deepseek-r1
deepseek-r1-distill-llama-70b (NOT YET, next to add)
```
## Python example
```python
import socketio
import json
socket = socketio.Client()
socket.connect('https://ai.nahcrof.com/stream')
socket.emit("start_stream", "TOKEN_GOES_HERE")

@socket.on("chunk")
def handle_chunk(data):
    if data == "ENDOFMESSAGE": # disconnect at end of response
        socket.disconnect()
        print("")
    elif data == "||AI ERROR||": # disconnect on AI error
        socket.disconnect()
        raise RuntimeError("Improper AI response")
    else: # handle chunks
        print(data, end="") 

data = {
    "model": "MODEL", # AI model, same name as trailing /free/ endpoint
    "context": [
        {"role": "user", "content": "summarize the titanic"},
    ],
    "token": "TOKEN_GOES_HERE",
}
socket.emit("prompt", json.dumps(data))
socket.wait()
```

## JS example
```javascript
const socket = io();
socket.connect();
socket.emit("start_stream", "TOKEN");
socket.on("chunk", (data) => {
    console.log(data);
});

data = {
    model: "MODEL", // AI model, same name as trailing /free/ endpoint
    context: [{role: "user", content: "summarize the titanic"}],
    token: "TOKEN"
}
socket.emit("prompt", JSON.stringify(data));  
```
