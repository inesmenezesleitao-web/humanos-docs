# 📁 Processes 
This API section is fully dedicated to the creation and management of processes. 


## List Processes
This endpoint returns a list of processes saved by the organization, each including the process id, name and description. The id is required when calling the Generate Process endpoint. The response is limited to a maximum of 20 processes per request. Details regarding the items of the process can be included in the response by setting the withDetails param to true.
### Method: GET
>```
>{{base_url}}/process?search=<search_term>&withDetails=<bool>
>```
### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|
|X-timestamp|1756197897518|


### Query Params

|Param|value|
|---|---|
|search|<search_term>|
|withDetails|<bool>|


### Response: 200
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

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
  },
  {
    "id": "64f1a2c3b9d8e1f234abcd56",
    "name": "Marketing Consent",
    "description": "Collection of explicit client consent to receive communications and marketing campaigns."
  },
  {
    "id": "64f1a2c3b9d8e1f234abcd78",
    "name": "Legal Document Validation",
    "description": "Submission and validation of legal documents with electronic signature."
  },
  {
    "id": "64f1a2c3b9d8e1f234abcd9a",
    "name": "Service Agreement",
    "description": "Digital signing flow for a service contract between the client and the organization."
  }
]
```
</details>

### Response: 200
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

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
</details>

### Response: 200
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

```json
[]
```
</details>


## Process Status
This endpoint provides a real-time view of a process by passing its unique ID. **Response body includes:**

- **Basic process information:** name, reference, description, order, number of pending users, creation date, and cancellation date.
- **Item status:** for each item in the process (document, form, or consent), basic information is shown along with a list of signers and their actions (e.g., information provided, signature, rejection, etc.).

### Method: GET
>```
>{{base_url}}/process/status/:processId
>```

### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|
|X-Timestamp|1756197897518|

### Response: 200
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

```json
{
    "id": "68812e5ef69fa320d3b3fd86",
    "missingInvolved": 0,
    "reference": "mRjFS7PWuosi",
    "name": "Client Onboarding",
    "description": "Client Onboarding",
    "canceledAt": null,
    "createdAt": "2025-07-23T18:47:58.151Z",
    "order": [
        "DOCUMENT",
        "FORM"
    ],
    "users": [
        {
            "userOtpId": "68812e5ef69fa320d3b3fd87",
            "userId": "683e368d6ff0654f2c395289",
            "timesSent": 2,
            "sentAt": "2025-07-23T18:47:58.659Z",
            "maxAttemptsReached": false,
            "checkpoints": [
                true,
                true
            ],
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
                            "answer": {
                                "type": "text",
                                "text": "5th Avenue, 1000, NY"
                            }
                        },
                        {
                            "question": "Job occupation",
                            "answer": {
                                "type": "number",
                                "text": "99"
                            }
                        }
                        {
                            "question": "Yearly income",
                            "answer": {
                                "type": "text",
                                "text": "120K"
                            }
                        },
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
</details>

### Response: 404
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

```json
{
    "statusCode": 404,
    "message": "Process not found",
    "error": "Not Found"
}
```
</details>



---

## Download PDFs
This endpoint allows organizations to download the signed PDFs of a process once it has been completed. The returned response is a binary file containing the merged PDF documents.

### Method: GET
>```
>{{base_url}}/process/:processId/download
>```

### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|HMAC-SHA256 signature of the request|
|X-Timestamp|Current timestamp in milliseconds|

### Response: 200 (PDF binary)

---

## List Items
This endpoint lists all items belonging to a specific process, including forms, documents, and consents.

### Method: GET
>```
>{{base_url}}/process/:processId/items
>```

### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|HMAC-SHA256 signature of the request|
|X-Timestamp|Current timestamp in milliseconds|

### Response: 200

```json
[
  {
    "id": "68812e5ef69fa320d3b3fd88",
    "itemType": "FORM",
    "form": { "id": "68812ae5f69fa320d3b3fd7a", "name": "Client Information" },
    "required": true
  },
  {
    "id": "68812e5ef69fa320d3b3fd89",
    "itemType": "DOCUMENT",
    "template": { "id": "683de6b22d2267803376acb1", "name": "Terms and Conditions" }
  }
]
```

---

## Resend Process
This endpoint resends process invitations to the users who have not yet completed their tasks.

### Method: POST
>```
>{{base_url}}/process/:processId/resend
>```

### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|HMAC-SHA256 signature of the request|
|X-Timestamp|Current timestamp in milliseconds|

### Response: 200

```json
{ "status": "resent", "processId": "68812e5ef69fa320d3b3fd86" }
```

---

## Cancel Process
This endpoint cancels a process, preventing any further user actions.

### Method: POST
>```
>{{base_url}}/process/:processId/cancel
>```

### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|HMAC-SHA256 signature of the request|
|X-Timestamp|Current timestamp in milliseconds|

### Response: 200

```json
{ "status": "canceled", "processId": "68812e5ef69fa320d3b3fd86" }
```

---

## Generate Process
This endpoint generates a new process instance from a preconfigured template, sending it to the specified users.

### Method: POST
>```
>{{base_url}}/process/generate
>```

### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|HMAC-SHA256 signature of the request|
|X-Timestamp|Current timestamp in milliseconds|
|Content-Type|application/json|

### Body

```json
{
  "contacts": ["+351912345678", "demo@humanos.tech"],
  "processIds": ["64c8e5f7d5a4f9123b7c9a1e"]
}
```

### Response: 200

```json
{ "status": "generated", "processReference": "mRjFS7PWuosi" }
```

---

## Generate Custom Process
This endpoint generates a new process with custom-defined items (documents, consents, and forms).

### Method: POST
>```
>{{base_url}}/process/generate/custom
>```

### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|HMAC-SHA256 signature of the request|
|X-Timestamp|Current timestamp in milliseconds|
|Content-Type|application/json|

### Body

```json
{
  "contacts": ["+351912345678"],
  "documents": [
    {
      "templateId": "683de6b22d2267803376acb1",
      "internalId": "contract-001"
    }
  ],
  "forms": [
    {
      "formId": "68812ae5f69fa320d3b3fd7a",
      "internalId": "form-abc"
    }
  ],
  "consents": [
    {
      "consentId": "68812c92f69fa320d3b3fd7c",
      "internalId": "consent-data"
    }
  ]
}
```

### Response: 200

```json
{ "status": "generated", "processReference": "cPjTR7PWxyz" }
```

