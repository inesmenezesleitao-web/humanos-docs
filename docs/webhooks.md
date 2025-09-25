# 📁 Webhooks 
Humanos provides a comprehensive webhook system that allows organizations to receive real-time notifications when specific events occur in their processes. The webhook system supports two levels of granularity to provide maximum flexibility. Organizations have full flexibility to specify the exact URL paths where webhooks should be sent to.

1. **Organization Level** (Fallback): Default webhooks for the entire organization
    
2. **Resource Level**: Specific webhooks for different types of resources (forms, documents, consents, KYC)
    

To configure the webhook system, organization administrators can navigate to [Humanos Admin – Webhooks](https://app.humanos.id/admin/webhooks). The following image illustrates an example configuration:

- An **Organization-Level Webhook** is defined, which will receive all notifications for **KYC** and **Signature** events, since no resource-specific webhooks have been configured for those event types.
- **Form events** will be delivered to [https://your.domain.com/webhook/forms](http://your.domain.com/webhook/forms).
- **Consent events** will be delivered to [https://your.domain.com/webhook/consents](http://your.domain.com/webhook/consents).
- **Signature Secret** is used for validating that the webhooks were sent by Humanos.
- **Encryption Secret** is used for decrypting the payloads, ensuring security on transit.
- **Encryption Salt** is used together with the Encryption Secret to derive the final encryption key. The salt adds uniqueness and protects against dictionary or pre-computed attacks, ensuring stronger security for each payload.
    

<img src="https://content.pstmn.io/819d9046-b424-4c70-9fd8-7dd0286227ee/U2NyZWVuc2hvdCAyMDI1LTA5LTE0IGF0IDIwLjIyLjAwLnBuZw==">

---

The following code snippet implements an express API containing an endpoint to deal with Humanos Webhook notifications. The request should be handeled as follows:

1. **Receive the request** → Your endpoint will be called with an encrypted payload.
2. **Verify authenticity** → Check the x-signature header using your **Signature Secret**.
3. **Decrypt the payload** → Use the **Encryption Secret** to decrypt the message and read the event data.
4. **Process the event** → Store it in your system, update statuses, or trigger business logic.
5. **Respond quickly** → Always return **200 OK** after successful processing. Humanos retries failed deliveries automatically.

``` javascript
const express = require("express");
const crypto = require("crypto");
const app = express();
app.use(express.json());
const SIGNATURE_SECRET = "your-signature-secret";
const ENCRYPTION_SECRET = "your-encryption-secret";
const ENCRYPTION_SALT = "your-encryption-salt"
app.post("/webhook", (req, res) => {
  try {
    const signature = req.headers["x-signature"];
    const timestamp = req.headers["x-timestamp"];
    // 1. Verify signature
    const expected = generateSignature(req.body, SIGNATURE_SECRET, timestamp);
    if (signature !== expected) return res.status(401).send("Invalid signature");
    // 2. Decrypt payload
    const { iv, data, tag } = req.body;
    const key = crypto.pbkdf2Sync(
      Buffer.from(ENCRYPTION_SECRET, "base64"),
      ENCRYPTION_SALT,
      10000,
      32,
      "sha256"
    );
    const decipher = crypto.createDecipheriv(
      "aes-256-gcm",
      key,
      Buffer.from(iv, "base64")
    );
    decipher.setAuthTag(Buffer.from(tag, "base64"));
    const decrypted = Buffer.concat([
      decipher.update(Buffer.from(data, "base64")),
      decipher.final(),
    ]).toString("utf8");
    const webhook = JSON.parse(decrypted);
    console.log("Webhook received:", webhook);
    res.sendStatus(200);
  } catch (err) {
    res.status(500).send(err.message);
  }
});
app.listen(3000, () => console.log("Listening on port 3000"));
```


#### **process.kyc.completed**

This webhook is triggered whenever a client successfully completes a **Know Your Customer (KYC)** verification process. Its purpose is to notify the organization and provide verified identity data for the user.

``` json
{
    "event": "process.kyc.completed",
    "organizationId": "680a65a4da4a16c0ea64face",
    "processId": "689a037727b93cfe15f82230",
    "userId": "684ade7148a18e172af7cab1",
    "completedAt": "2025-08-11T14:52:43.613Z",
    "metadata": {
        "kycId": "689a028f8bec20beaa53f23e",
        "kycType": "PROFESSIONAL",
        "verificationStatus": "COMPLETED",
        "documentsVerified": [
            "IDENTITY_DOCUMENT",
            "LIVENESS_CHECK",
            "FACE_MATCH"
        ],
        "verificationDate": "2025-08-11T14:52:43.613Z",
        "identityData": {
            "fullName": "Rodrigo Carvalheda Duarte da Fonseca Sarroeira",
            "gender": "M",
            "birth": "21-12-2001",
            "docId": "14499760",
            "countryAlpha3": "PRT",
            "expiresAt": "2026-12-23T00:00:00.000Z",
            "issueDate": null,
            "taxNumber": null,
            "healthNumber": null,
            "socialSecurityNumber": null,
            "photo": null,
            "height": "1,79",
            "documentDiscriminator": "6ZWS",
            "placeOfBirth": null,
            "mrzText": null,
        }
    },
    "timestamp": 1754923963854
}
```


---

#### **process.signature.user.completed**

This webhook is triggered whenever a client completes the signing process for one or more documents. The payload contains information about the organization, the process, and the user who signed. Within metadata.items, it lists each document involved, including its identifiers, name, cryptographic hash, the client’s signature, decision date, and whether the signature was accepted or rejected.

``` json
{
  "eventType": "process.signature.completed",
  "organizationId": "680a65a4da4a16c0ea64face",
  "processId": "68c4292cfcf19b66d3d200b3",
  "userId": "684ade7148a18e172af7cab1",
  "completedAt": "2025-09-12T14:27:51.571Z",
  "metadata": {
    "items": [
      {
        "itemId": "68c4292cfcf19b66d3d200b5",
        "internalId": "3nvf9fdnb3fk",
        "documentId": "683de6b22d2267803376acb1",
        "documentName": "Service Agreement",
        "hash": "0x47f18c9c0c8119fb96582105c9e06a973bc384f0bc842290cbe38cdba2ec669d",
        "signature": "0x6cf9d8dbe1ed9c2eabe373b66c83495a684263065d2ad88acfe7b5ad95a4ec8f174a471e81f76e9c917d3e1f300f7d142f58a20bf91dddc42bc9276c6bde1c371b",
        "decisionDate": "2025-09-12T14:27:51.571Z",
        "rejected": false
      },
      {
        "itemId": "68c4292cfcf19b66d3d200b6",
        "internalId": "7hd83ksla92p",
        "documentId": "683de6b22d2267803376acb2",
        "documentName": "Contract Amendment",
        "hash": "0xa21f3e5c7b62f90e6c8e1e5d74d13a8f47f45bbf332e14db4b39a7a1c5e63f9a",
        "signature": "0xd9f8c8f0b0a6c1e5e94e7b42f9c50d1c43db7a441c5b0de8b3eaa4b8d6dbb7eecdf41d12a3b5c7d6f9a842e16a6f4c5bc64a890cd56b50d4c43c61e94d2b3f1c2",
        "decisionDate": "2025-09-12T14:32:10.842Z",
        "rejected": true
      },
      {
        "itemId": "68c4292cfcf19b66d3d200b7",
        "internalId": "9plm4sdqk71r",
        "documentId": "683de6b22d2267803376acb3",
        "documentName": "Non-Disclosure Agreement",
        "hash": "0xc9f71a3e6b94e0a4f6ad982c1d9e3721d8472e67af5d456e91bda63ac5d3cf17",
        "signature": "0xa8bfa7c3c2e5f6b471d01b0dbf73d6c9e0b5d1f8a6c84e3a7f1e6c9b52b7c8d1a92e03db0e9c5f3b0a72d3a1c6e7f814c82b3d9f27b8e5a0f3b4c5e6a7d8f9b2c",
        "decisionDate": "2025-09-12T14:35:22.197Z",
        "rejected": false
      }
    ]
  }
}
```

---

#### **process.signature.professional.completed**

This webhook is triggered whenever a professional completes the signing process for one or more documents. The payload contains information about the organization, the process, and the professional who signed. Within metadata.items, it lists each document involved, including its identifiers, name, cryptographic hash, the client’s signature, decision date, and whether the signature was accepted or rejected.

``` json
{
  "eventType": "process.signature.completed",
  "organizationId": "680a65a4da4a16c0ea64face",
  "processId": "68c3f4a9275a5464513815e5",
  "professionalId": "680a6d31a601995047471150",
  "professionalName": "Rodrigo Sarroeira",
  "professionalInternalId": "CA435",
  "completedAt": "2025-09-12T14:46:43.320Z",
  "metadata": {
    "items": [
      {
        "itemId": "68c3f4a9275a5464513815e7",
        "documentId": "683de6b22d2267803376acb1",
        "documentName": "Service Agreement",
        "hash": "0x47f18c9c0c8119fb96582105c9e06a973bc384f0bc842290cbe38cdba2ec669d",
        "signature": "0x3147a17ccd944eef40dc3ebe36bbff08e42d80b5cd97972f8e821deaa1cb12933053bd31399470846a86aa5b9f2c72b5109084fae2e8a57a93fb1687d84918bf1c",
        "decisionDate": "2025-09-12T14:46:43.320Z",
        "rejected": false
      },
      {
        "itemId": "68c3f4a9275a5464513815e8",
        "documentId": "683de6b22d2267803376acb2",
        "documentName": "Non-Disclosure Agreement",
        "hash": "0xb8d71e4a6f9b2c83e40f9d81d7d4e6a1c2f9b37a6d28e47e12c4b5c3e7f0d918",
        "signature": "0xa17d6c9f1e3f92a5c8b7d41a2e9f04c5b6c1d8f3a2b7e4f8c9a5d3e2b1f6a47c90e1f3a5d7c2b8e4f1a9d3e7f0c5b2a9d",
        "decisionDate": "2025-09-12T14:49:15.500Z",
        "rejected": true
      }
    ]
  }
}
```

---

#### **process.consent.completed**

This webhook is triggered when a client completes the consent process for one or more consents. The payload includes details about the organization, process, and user. Inside metadata.items, each consent entry contains its identifiers, name, text, link, the client’s signature, decision date, and whether the consent was accepted or rejected.

``` json
{
  "eventType": "process.consent.completed",
  "organizationId": "680a65a4da4a16c0ea64face",
  "processId": "68c42ec3e47c9a7f9241e0ba",
  "userId": "684ade7148a18e172af7cab1",
  "completedAt": "2025-09-12T14:37:31.236Z",
  "metadata": {
    "items": [
      {
        "itemId": "68c42ec4e47c9a7f9241e0bc",
        "consentId": "68c2d5b3ad6376134938a601",
        "internalId": "7hd83ksla92p",
        "consentName": "Data Privacy Policy",
        "text": "By agreeing, you confirm that you have read and understood our Data Privacy Policy, which explains how your personal data will be collected, stored, and used.",
        "link": "https://example.com/privacy-policy",
        "required": true,
        "decisionDate": "2025-09-12T14:37:31.236Z",
        "rejected": false
      },
      {
        "itemId": "68c42ec4e47c9a7f9241e0bd",
        "consentId": "6894b96e8aecc042e54184cc",
        "internalId": null,
        "consentName": "Marketing Communications",
        "text": "I agree to receive promotional emails, newsletters, and updates about new products and services. You can unsubscribe at any time.",
        "link": "https://example.com/marketing-consent",
        "required": false,
        "decisionDate": "2025-09-12T14:37:31.236Z",
        "rejected": true
      },
      {
        "itemId": "68c42ec4e47c9a7f9241e0be",
        "consentId": "68812c92f69fa320d3b3fd7c",
        "internalId": null,
        "consentName": "Terms and Conditions",
        "text": "By continuing, you confirm that you have read, understood, and agree to the Terms and Conditions. Please review them carefully as they outline your rights and obligations when using our services.",
        "link": "https://example.com/terms-and-conditions",
        "required": true,
        "decisionDate": "2025-09-12T14:37:31.236Z",
        "rejected": false
      }
    ]
  }
}
```


---

#### **process.form.filled**

This webhook is triggered when a client completes and submits a form. The payload contains details about the organization, process, and user, along with the form information. Within the metadata, it includes the fields the user filled in, the form identifiers and metadata, and any associated signature data.

``` json
{
  "event": "process.form.filled",
  "processId": "68c88aaa6002536da855c049",
  "organizationId": "680a65a4da4a16c0ea64face",
  "userId": "684ade7148a18e172af7cab1",
  "completedAt": "2025-09-15T21:54:36.637Z",
  "metadata": {
    "items": [
      {
        "itemId": "68c88aaa6002536da855c04b",
        "formId": "fo67jWVc",
        "submittedFields": [
          {
            "question": "What is your full name?",
            "answer": {
              "type": "text",
              "text": "John Doe"
            }
          },
          {
            "question": "Select your gender",
            "answer": {
              "type": "choice",
              "text": "Male"
            }
          },
          {
            "question": "Do you currently study?",
            "answer": {
              "type": "boolean",
              "text": "true"
            }
          },
          {
            "question": "Which fruits do you like?",
            "answer": {
              "type": "choices",
              "text": "Apple, Banana, Orange"
            }
          },
          {
            "question": "How old are you?",
            "answer": {
              "type": "number",
              "text": "29"
            }
          },
          {
            "question": "What is your email?",
            "answer": {
              "type": "email",
              "text": "john.doe@example.com"
            }
          },
          {
            "question": "What is your phone number?",
            "answer": {
              "type": "phone_number",
              "text": "+351912345678"
            }
          }
        ]
      }
    ]
  }
}
```

---

#### **process.form.rejected**

This webhook is triggered when a client rejects a form or chooses not to provide the required information. The payload includes the process, organization, and user identifiers, as well as the specific form details.

``` json
{
  "event": "process.form.rejected",
  "processId": "68c88aaa6002536da855c049",
  "organizationId": "680a65a4da4a16c0ea64face",
  "userId": "684ade7148a18e172af7cab1",
  "rejectedAt": "2025-09-15T22:10:12.421Z",
  "metadata": {
    "items": [
      {
        "itemId": "68c88aaa6002536da855c04b",
        "formId": "fo67jWVc",
        "reason": "User opted out of completing this form."
      }
    ]
  }
}
```
