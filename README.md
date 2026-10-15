# 🔗 Dub.co Shortener API Documentation
## For Domain: `pd.is-cool.dev`

This documentation outlines how to programmatically create and manage shortened links using your custom **is-cool.dev** subdomain connected to the **Dub.co** infrastructure.

---

## 🛠️ Step 1: DNS Setup (Open Domains)
Before calling the API, you must route your subdomain to Dub.co. In your `is-cool.dev` configuration (GitHub repository or Open Domains panel), update your record to point to Dub's servers:

* **Type:** `CNAME`
* **Name/Host:** `pd`
* **Target/Value:** `cname.dub.co`
* **TTL:** `Automatic` or `3600`

---

## 🔑 Step 2: Authentication
The Dub API relies on bearer tokens for authentication. 
1. Log in to your **Dub.co dashboard**.
2. Navigate to **Account Settings** -> **API Keys**.
3. Create a token and pass it inside your request header:

```http
Authorization: Bearer YOUR_DUB_API_KEY
```

---

## 🛰️ Step 3: Create a Short Link Endpoint

### HTTP Request
* **Method:** `POST`
* **URL:** `https://api.dub.co/links`
* **Content-Type:** `application/json`

### Request Body Parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `url` | String | **Yes** | The destination long URL you want to shorten. |
| `domain` | String | **Yes** | Your exact domain zone. Must be set to `"pd.is-cool.dev"`. |
| `key` | String | No | The custom path/slug (e.g., `my-link` results in `pd.is-cool.dev/my-link`). |

---

## 💻 Code Snippets

### 1. cURL
```bash
curl -X POST "https://api.dub.co/links" \
  -H "Authorization: Bearer YOUR_DUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://github.com/steven-tey/dub",
    "domain": "pd.is-cool.dev",
    "key": "repo"
  }'
```

### 2. JavaScript / Node.js
```javascript
const createShortLink = async () => {
  const response = await fetch("https://api.dub.co/links", {
    method: "POST",
    headers: {
      "Authorization": "Bearer YOUR_DUB_API_KEY",
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      url: "https://github.com/steven-tey/dub",
      domain: "pd.is-cool.dev",
      key: "repo"
    })
  });

  const data = await response.json();
  console.log("Short URL:", data.shortLink); 
  // Output: https://pd.is-cool.dev/repo
};

createShortLink();
```

### 3. Python
```python
import requests

url = "https://api.dub.co/links"
headers = {
    "Authorization": "Bearer YOUR_DUB_API_KEY",
    "Content-Type": "application/json"
}
payload = {
    "url": "https://github.com/steven-tey/dub",
    "domain": "pd.is-cool.dev",
    "key": "repo"
}

response = requests.post(url, json=payload, headers=headers)
print("Short URL:", response.json().get("shortLink"))
```

---

## 📋 Example JSON Response (`200 OK`)
```json
{
  "id": "cl_123456789",
  "domain": "pd.is-cool.dev",
  "key": "repo",
  "url": "https://github.com/steven-tey/dub",
  "shortLink": "https://pd.is-cool.dev/repo",
  "clicks": 0,
  "createdAt": "2026-10-15T16:15:00.000Z"
}
```
