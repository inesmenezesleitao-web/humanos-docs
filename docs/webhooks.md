# Webhooks

Humanos provides real-time webhook notifications.

## Configuration
- **Organization level** webhooks (fallback).  
- **Resource level** webhooks (specific to forms, documents, consents, KYC).

## Example Webhook Handler
```javascript
const express = require("express");
const crypto = require("crypto");
const app = express();
app.use(express.json());
app.post("/webhook", (req, res) => {
  try {
    const signature = req.headers["x-signature"];
    const timestamp = req.headers["x-timestamp"];
    res.sendStatus(200);
  } catch (err) {
    res.status(500).send(err.message);
  }
});
app.listen(3000, () => console.log("Listening on port 3000"));
```

## Events
- `process.kyc.completed`
- `process.signature.completed`
- `process.consent.completed`
- `process.form.filled`
- `process.form.rejected`
