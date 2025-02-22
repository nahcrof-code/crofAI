# [CrofAI](https://ai.nahcrof.com/)
API documentation for crofAI
## Donation page 
https://buymeacoffee.com/crofai
# API format
## JSON
POST /v1/free/{MODEL-NAME}<br>
HEADERS:<br>
```
X-API-Key: api-token-here
```
BODY (application/json)
```json
{
    "messages": [
    {
        "role": "system",
        "content": "system message content"
    },
    {
        "role": "user",
        "content": "user message content"
    }],
    "max_tokens": 300
}
```
RESPONSE 200 OK
```json
{
    "response": "AI response"
}
```
Please note that model names are not always obvious, if your guess for the model name doesn't work, please return to the docs. Also note that speeds may vary between each model.

It is also important to note that this format does not work for stable-diffusion.
To use stable diffusion please refer to the bottom of the premium models section, or the bottom of the code examples section.
## Python
```python
import requests
import json
headers = {"X-API-Key": "myapikey"}
payload = {
    "messages": [
        {"role": "system", "content": "system message content"},
        {"role": "user", "content": "user message content"}
    ],
    "max_tokens": 500
}
r1 = requests.post(url=f'https://ai.nahcrof.com/v1/free/{MODEL-NAME}', json=payload, headers=headers)
value = r1.json()
try:
    print(value["response"])
except KeyError:
    print(value)
```
## TypeScript
```typescript
import axios, { AxiosResponse } from 'axios';
const headers = { 'X-API-Key': 'myapikey' };
const payload = {
    messages: [
        { role: 'system', content: 'system message content' },
        { role: 'user', content: 'user message content' }
    ],
        max_tokens: 500
    };
axios.post('https://ai.nahcrof.com/v1/free/{MODEL-NAME}', payload, { headers })
    .then((response: AxiosResponse<{ response: string }>) => {
        try {
            console.log(response.data.response);
        } catch (error) {
            if (error instanceof Error && error.name === 'TypeError') {
                console.log(response.data);
            } else {
                throw error;
            }
        }
    })
    .catch((error) => {
        console.error(error);
    });
```
# AI models / API MODEL-NAME
```
llama3-8b-Instruct - /free/llama3
llama3.1-8b-Instruct - /free/llama3.1-8b
llama3.2-1b-Instruct - /free/llama3.2-1b
llama3-70b-Instruct - /free/llama3_70b
llama3.3-70b-Instruct - /free/llama3.3-70b
llama3.1-405b - /free/llama3.1-405b
llama3.1-tulu-405b - /free/llama3.1-tulu-405b
tinyllama-1.1b-chat - /free/tinyllama
gemma-7b-it - /free/gemma_7b
stable-diffusion-xl-base-1.0 - /free/stable_diffusion
qwen1.5-0.5b-chat - /free/qwen0.5
mistral-7b-Instruct - /free/mistral-7b-instruct
deepseek-r1 - /free/deepseek-r1
deepseek-v3 - /free/deepseek-v3
deepseek-r1-distill-llama-70b - /free/deepseek-r1-distill-llama-70b
deepseek-r1-distill-qwen-32b - /free/deepseek-r1-distill-qwen-32b
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

tinyllama-1.1b-chat - ~100 tokens/second
Low speed example: ~50 tokens/second
Highest speed seen: ~130 tokens/second

gemma-7b-it - ~20 tokens/second
Low speed example: ~20 tokens/second
Highest speed seen: ~32 tokens/second

qwen1.5-0.5b-chat - ~100 tokens/second*
Low speed example: ~60 tokens/second
Highest speed seen: ~114 tokens/second

mistral-7b-Instruct - ~70 tokens/second*
Low speed example: ~50 tokens/second
Highest speed seen: ~83 tokens/second

deepseek-r1-distill-llama-70b - ~700 tokens/second
Low speed example: ~250 tokens/second
Highest speed seen: ~5,547 tokens/second

deepseek-r1-distill-qwen-32b - ~800 tokens/second
Low speed example: ~300 tokens/second
Highest speed seen: ~7,849 tokens/second

deepseek-r1 - ~16 tokens/second
Low speed example: ~5 tokens/second
Highest speed seen: ~113 tokens/second

deepseek-v3 - ~10 tokens/second
Low speed example: ~5 tokens/second
Highest speed seen: ~60 tokens/second
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
