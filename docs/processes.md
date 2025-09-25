# Processes

This API section is fully dedicated to the creation and management of processes.

---

## List Processes

**Method:** `GET`  
**URL:** `{{base_url}}/process?search=<search_term>&withDetails=<bool>`

### Description
Returns a list of processes saved by the organization (max 20 per page). Use `withDetails=true` to include items (templates, forms, consents) in the response.

### Query Parameters
| Name | Type | Required | Description |
|---|---|---|---|
| `search` | string | no | Filter processes by name/title. |
| `withDetails` | boolean | no | Include process items in the response. |

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
```

### Responses

#### Processes without Details — 200 OK
```json
[
  {
    "id": "64f1a2c3b9d8e1f234abcd12",
    "name": "Account Opening",
    "description": "Onboarding flow for new clients with identity verification and mandatory form signing."
  },
  {
    "id": "64f1a2c3b9d8e1f234abcd34",
    "name": "Personal Data Update",
    "description": "Process for collecting and validating new documents and updated client information."
  }
]
```

#### Processes with Details — 200 OK
```json
[
  {
    "id": "68812be4f69fa320d3b3fd7b",
    "name": "Processo Onboarding Cliente",
    "description": "Processo Onboarding Cliente",
    "templates": [
      {
        "id": "683de6b22d2267803376acb1",
        "name": "Contrato de Serviço",
        "description": "Contrato de Serviço",
        "internalId": null
      }
    ],
    "forms": [
      {
        "id": "68812ae5f69fa320d3b3fd7a",
        "name": "Dados de Cliente",
        "description": "Atualização de dados de cliente",
        "required": true,
        "internalId": null
      }
    ],
    "consents": []
  },
  {
    "id": "68812e4ef69fa320d3b3fd85",
    "name": "Processo Onboarding Cliente com consentimento dados",
    "description": "Processo Onboarding Cliente com consentimento dados",
    "templates": [
      {
        "id": "683de6b22d2267803376acb1",
        "name": "Contrato de Serviço",
        "description": "Contrato de Serviço",
        "internalId": null
      },
      {
        "id": "683de6cb2d2267803376acb2",
        "name": "Contrato de Trabalho",
        "description": "Contrato de Trabalho",
        "internalId": null
      }
    ],
    "forms": [
      {
        "id": "68812ae5f69fa320d3b3fd7a",
        "name": "Dados de Cliente",
        "description": "Atualização de dados de cliente",
        "required": true,
        "internalId": null
      }
    ],
    "consents": [
      {
        "id": "68812c92f69fa320d3b3fd7c",
        "name": "Termos e Condições",
        "description": "",
        "required": true,
        "internalId": null
      }
    ]
  },
  {
    "id": "6883a63da1e72aa9fd23c512",
    "name": "Termos de responsabilidade",
    "description": "Termos de responsabilidade",
    "templates": [],
    "forms": [
      {
        "id": "68812ae5f69fa320d3b3fd7a",
        "name": "Dados de Cliente",
        "description": "Atualização de dados de cliente",
        "required": true,
        "internalId": null
      }
    ],
    "consents": [
      {
        "id": "68812c92f69fa320d3b3fd7c",
        "name": "Termos e Condições",
        "description": "",
        "required": true,
        "internalId": null
      }
    ]
  }
]
```

#### Empty Response — 200 OK
```json
[]
```

---

## Process Status

**Method:** `GET`  
**URL:** `{{base_url}}/process/status/:processId`

### Description
Provides a real-time view of a process by its unique ID, including basic information, item status, users and actions.

### Path Variables
| Name | Description |
|---|---|
| `processId` | The unique identifier of the process. |

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
```

### Example Success Response — 200 OK
```json
{
  "id": "68812e5ef69fa320d3b3fd86",
  "missingInvolved": 0,
  "reference": "mRjFS7PWuosi",
  "name": "Client Onboarding",
  "description": "Client Onboarding",
  "canceledAt": null,
  "createdAt": "2025-07-23T18:47:58.151Z",
  "order": ["DOCUMENT", "FORM"],
  "users": [
    {
      "userOtpId": "68812e5ef69fa320d3b3fd87",
      "userId": "683e368d6ff0654f2c395289",
      "timesSent": 2,
      "sentAt": "2025-07-23T18:47:58.659Z",
      "maxAttemptsReached": false,
      "checkpoints": [true, true],
      "contact": "+351919197958"
    }
  ],
  "items": [
    {
      "id": "68812e5ef69fa320d3b3fd88",
      "itemType": "FORM",
      "template": null,
      "form": {
        "id": "68812ae5f69fa320d3b3fd7a",
        "name": "Client Information",
        "required": true
      },
      "consent": null,
      "signers": [
        {
          "userId": "683e368d6ff0654f2c395289",
          "responsible": null,
          "metadata": [
            {
              "question": "Address",
              "answer": { "type": "text", "text": "5th Avenue, 1000, NY" }
            },
            {
              "question": "Job occupation",
              "answer": { "type": "number", "text": "99" }
            },
            {
              "question": "Yearly income",
              "answer": { "type": "text", "text": "120K" }
            }
          ],
          "signature": "0x58d9afcb8f8b96b6351e48b37163792544ee177a90abc9a7b961de3e2789dfaf0324a71903a36439235c1db880941ec79845a357c2bd7bf87213714fc39739e01c",
          "decisionDate": "2025-07-23T18:53:49.548Z",
          "rejected": false
        }
      ]
    },
    {
      "id": "68812e5ef69fa320d3b3fd89",
      "itemType": "DOCUMENT",
      "template": {
        "id": "683de6b22d2267803376acb1",
        "name": "Terms and Conditions"
      },
      "form": null,
      "consent": null,
      "signers": [
        {
          "userId": "683e368d6ff0654f2c395289",
          "responsible": null,
          "metadata": {},
          "signature": "0xbf5f6846b6b105c07a702747248a8cc07c9c9221ac37f4ff835f8cdce81802d72f2bd4f8d27c661aa431cc86c22303af32c57ced91d49766aa3af1cdfdfd83af1b",
          "decisionDate": "2025-07-23T18:53:12.554Z",
          "rejected": false
        }
      ]
    }
  ]
}
```

### Example Error — 404 Not Found
```json
{
  "statusCode": 404,
  "message": "Process not found",
  "error": "Not Found"
}
```

---

## Generate Process

**Method:** `POST`  
**URL:** `{{base_url}}/process/generate`

### Description
Creates and sends unique process links so clients can complete the requested items. You may send one or more `processIds` to one or more contacts.  
If multiple contacts are provided in one request, all are attached to the same process instance. To send separately, call the endpoint per contact.

### Security Levels
- **Level 0** – Verified contact via OTP only.  
- **Level 1 (default)** – Humanos Verified Identity _or_ organization KYC.  
- **Level 2** – Humanos Verified Identity required.  
- **Level 3** – Full KYC (document scan + selfie) per process.

> Contacts not yet registered will be created automatically.

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
Content-Type: application/json
```

### Request Body
```json
{
  "contacts": ["+351917210770", "+351916724141", "+351964152121"],
  "processIds": ["68812be4f69fa320d3b3fd7b", "68812e4ef69fa320d3b3fd85"],
  "securityLevel": 1
}
```

### Example Error — 404
```json
{
  "statusCode": 404,
  "message": "Process not found",
  "error": "Not Found"
}
```

---

## Generate Custom Process

**Method:** `POST`  
**URL:** `{{base_url}}/process/generate-custom`

### Description
Creates and sends a **custom process** on demand. Useful for frontend apps and AI agents to dynamically assemble processes.

### Requirements
- Identifiers of items to include (**templates, forms, consents, documents**).  
- The `order` the items should be presented.  
- *(Optional)* `pdfs[]` with `name` and `fileContent` (base64). You may also pass `internalId` and `extraFields`.  
- *(Optional)* `securityLevel` (defaults to **Level 1**).

### Security Levels
- **Level 0** – OTP-verified contact only.  
- **Level 1 (default)** – Humanos Verified Identity or KYC.  
- **Level 2** – Humanos Verified Identity required.  
- **Level 3** – Full KYC (document scan + selfie).

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
Content-Type: application/json
```

### Request Body
```json
{
  "contacts": ["+573178901234", "+573178901235"],
  "securityLevel": 1,
  "order": ["CONSENT", "FORM", "DOCUMENT"],
  "templateIds": ["507f1f77bcf86cd799439011"],
  "formIds": ["507f1f77bcf86cd799439012"],
  "consentIds": ["507f1f77bcf86cd799439013"],
  "pdfs": [
    {
      "name": "contract.pdf",
      "fileContent": "base64EncodedPdfContent...",
      "internalId": "hddf78d78r43gid8fdnj",
      "extraFields": {
        "extraLabel1": "extraValue1",
        "extraLabel2": "extraValue2",
        "extraLabel3": "extraValue3"
      }
    }
  ]
}
```

---

## Download PDFs

**Method:** `GET`  
**URL:** `{{base_url}}/process/pdf/:itemId`

### Description
Returns the generated PDF (signature events and form fills) as a Base64 string, with all signatures and/or submitted answers to date.  
If no signatures exist yet, returns the original document.

### Path Variables
| Name | Description |
|---|---|
| `itemId` | Unique ID of the item (Signature Request or Form Fill). Returned by `process/generate` and sent in webhook events. |

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
```

### Example Success — 200 OK
```json
{
  "filecontent": "JVBERi0xLjcKJYGBgYEKCjcgMCBvYmoKPDwK...<truncated>...",
  "internalId": "mRjFS7PWuosi"
}
```

### Example Error — 404 Not Found
```json
{
  "statusCode": 404,
  "message": "Process item not found",
  "error": "Not Found"
}
```

---

## List Items

**Method:** `GET`  
**URL:** `{{base_url}}/process/items/:search`

### Description
Retrieves all available **items** that can be used to build custom processes (templates, forms, consents). Can be consumed by frontend apps or AI agents to generate processes on demand.

### Path Variables
| Name | Description |
|---|---|
| `search` | String used to filter items by name/description. |

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
```

### Example Success — 200 OK
```json
[
  {
    "id": "683e3c6c6ff0654f2c3952b5",
    "name": "Real Estate Contract Draft - Rua Professor Dr. Jorge Mineiro, 15",
    "description": "Draft contract for property purchase agreement (CPCV).",
    "internalId": "TPL-REALESTATE-001",
    "type": "TEMPLATE"
  },
  {
    "id": "6845a6d348a18e172af7ca7a",
    "name": "Employment Contract",
    "description": "Standard employment contract template.",
    "internalId": "TPL-HR-001",
    "type": "TEMPLATE"
  },
  {
    "id": "68551ab04f05fa7b6e74a5a7",
    "name": "Direct Debit Authorization",
    "description": "Authorization form for direct debit payments.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "6899d08a00bec34b6d3fd9b4",
    "name": "Airbnb Business Plan 2025",
    "description": "Business plan draft for Airbnb operations, 2025 edition.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "683de6fd2d2267803376acb3",
    "name": "Personal Data Processing Consent",
    "description": "Consent form for the processing of personal data for marketing purposes.",
    "internalId": "TPL-DATA-001",
    "type": "TEMPLATE"
  },
  {
    "id": "68812ae5f69fa320d3b3fd7a",
    "name": "Customer Information Form",
    "description": "Form for updating customer details.",
    "required": true,
    "internalId": "FORM-CUSTOMER-001",
    "type": "FORM"
  },
  {
    "id": "68812c92f69fa320d3b3fd7c",
    "name": "Terms and Conditions",
    "description": "Consent to terms and conditions of service.",
    "required": true,
    "internalId": "CONSENT-TC-001",
    "type": "CONSENT"
  }
]
```

### Example Empty — 200 OK
```json
[]
```

---

## Resend OTP

**Method:** `PATCH`  
**URL:** `{{base_url}}/process/resend/:userOtpId`

### Description
Used when initial OTP delivery fails. Humanos sends a webhook on failure; use this endpoint to resend a new OTP.

### Path Variables
| Name | Description |
|---|---|
| `userOtpId` | The OTP identifier for the user in the process. |

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
```

---

## Cancel Process

**Method:** `PATCH`  
**URL:** `{{base_url}}/process/cancel/:processId`

### Description
Cancels a previously sent process. Once cancelled, the process link becomes inactive. Cancellation is only allowed if the process is not completed. Data already collected remains stored.

### Path Variables
| Name | Description |
|---|---|
| `processId` | The unique process identifier. |

### Headers
```http
Authorization: Bearer {{api_key}}
X-Signature: 8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f
X-Timestamp: 1756197897518
```
