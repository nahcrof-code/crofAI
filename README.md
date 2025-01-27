# crofAI
API documentation for crofAI

# API format
## JSON
POST /v1/{MODEL-TIER}/{MODEL-NAME}<br>
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
r1 = requests.post(url=f'https://ai.nahcrof.com/v1/{MODEL-TIER}/{MODEL-NAME}', json=payload, headers=headers)
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
axios.post('https://ai.nahcrof.com/v1/{MODEL-TIER}/{MODEL-NAME}', payload, { headers })
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
# Specific API JSON examples
## stable-diffusion-xl-base-1.0 (BETA)
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
