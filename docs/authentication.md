---
title: Authentication
description: Learn how to authenticate with the Humanos API using API keys and request signing
---

# Authentication

## 🔑 API Key

The **Humanos API** uses multiple layers of authentication and request validation. The first being the API key.

---

### 1. Generate an API Key
Go to [Humanos Admin – API Keys](https://app.humanos.id/admin/api-keys) and click **“+ API Key”** to create a new key.

### 2. Configure API Key
- **Name** _(required)_ – A label to identify the API key.  
- **Description** _(optional)_ – A short note to document the purpose of the API key.  
- **Allowed domain list** (Not yet available) – A list of whitelisted domains.  
- **Allowed IP list** (Not yet available) – A list of whitelisted IP addresses.  
- **Expiration (days)** _(optional)_ – The number of days until the key expires.

### 3. Retrieve Your Credentials
Once the API key is created, copy and securely store the following values:
- API Key  
- Signature Secret  

### 4. Rotate Secrets
It is possible to rotate the secrets associated with an API key.  
Only the **Signature Secret** changes; the API Key remains the same.

---

## 🔒 Request Signing

Each request must be signed using **HMAC-SHA256** with a timestamp.

### Signature Generation Example
```javascript
import crypto from "crypto";

function generateSignature(data, secret, timestamp) {
  const hmac = crypto.createHmac("sha256", secret);
  hmac.update(data ? `${timestamp}.${data}` : timestamp.toString());
  return hmac.digest("hex");
}
```

### Example: GET Request (no body)
```javascript
import fetch from "node-fetch";

const apiKey = "YOUR_API_KEY";
const signatureSecret = "YOUR_SIGNATURE_SECRET";
const timestamp = Date.now();
const signature = generateSignature("", signatureSecret, timestamp);

const response = await fetch("https://api.humanos.id/v0/processes", {
  method: "GET",
  headers: {
    "Authorization": `Bearer ${apiKey}`,
    "X-Timestamp": timestamp.toString(),
    "X-Signature": signature
  }
});
console.log(await response.json());
```

### Example: POST Request (with body)
```javascript
import fetch from "node-fetch";

const apiKey = "YOUR_API_KEY";
const signatureSecret = "YOUR_SIGNATURE_SECRET";
const body = JSON.stringify({
  "contacts": ["+351912345678"],
  "processIds": ["64c8e5f7d5a4f9123b7c9a1e"]
});

const timestamp = Date.now();
const signature = generateSignature(body, signatureSecret, timestamp);

const response = await fetch("https://api.humanos.id/v0/process/generate", {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${apiKey}`,
    "Content-Type": "application/json",
    "X-Timestamp": timestamp.toString(),
    "X-Signature": signature
  },
  body
});
console.log(await response.json());
```
