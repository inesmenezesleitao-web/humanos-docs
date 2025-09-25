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



---

## Download PDFs

`http://localhost:9000/v0/process/pdf/:itemId`

Both Signature Events and Form Fills generate PDFs containing information about the document and the signatures of all participants. This endpoint returns a PDF encoded as a Base64 string, including all signatures collected up to the current moment.

If no signatures have been collected, the API returns the original document without any signatures.

If some users have already signed, their signatures will appear on the PDF.

For forms, the PDF also includes the date of submission and the answers provided by each user.

Each PDF or form is assigned a unique itemId. This item ID is communicated to the organization via webhook whenever a relevant event occurs. Organizations can use the item ID to retrieve the corresponding PDFs and store them in their systems.

### HEADERS

Authorization  
Bearer org1-key-1-12345678901234567890123456789012

X-Signature  
8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f

X-timestamp  
1756197897518

### PATH VARIABLES

itemId  
An item can be of type Signature Request or Form Fill. Each item is identified by a unique itemId. These IDs are generated and returned by the v0/process/generate endpoint whenever a new process is created. The webhook notification system also includes the corresponding itemId in every event, allowing organizations to track and retrieve the related documents.

---

## List Items

`http://localhost:9000/v0/process/items`

Retrieve all available items that can be used to build custom processes. These items include templates, forms, and consents, which can be dynamically combined to define workflows.

This endpoint can be consumed not only by frontend applications but also by AI agents to generate and deliver custom processes on demand to end-users. For example, an AI system could automatically select the appropriate templates, forms, and consents for a given context and trigger a process in real time.

### HEADERS

Authorization  
Bearer org1-key-1-12345678901234567890123456789012

X-Signature  
7a298fca1e2bcb15e807fa01af43d62cb7f9a56f9cb9f4377e5114c9a0f6e123

X-timestamp  
1756198000000

---

## Resend Process

`http://localhost:9000/v0/process/:processId/resend`

Resend invitations for users to complete pending items within a process. This is particularly useful in scenarios where users did not receive the initial notification, ignored it, or lost access to the original link.

Organizations can call this endpoint to ensure users are reminded to complete their tasks.

### HEADERS

Authorization  
Bearer org1-key-1-12345678901234567890123456789012

X-Signature  
c93f1d1ac72e3097fa38de38f0324b7432292e0fd26f07f3bc492e22a7b75e81

X-timestamp  
1756198999000

### PATH VARIABLES

processId  
Unique identifier of the process to resend.

---

## Cancel Process

`http://localhost:9000/v0/process/:processId/cancel`

Cancel a process, permanently stopping its execution. Once canceled, users will no longer be able to interact with the process. This operation cannot be reversed.

Use this endpoint if a process was started by mistake or is no longer required.

### HEADERS

Authorization  
Bearer org1-key-1-12345678901234567890123456789012

X-Signature  
d5c2b4c7e23d9e9b08e3f90e7c42a1a73b5a18f25dc39c0abdf5c2d7f6b3910f

X-timestamp  
1756199001000

### PATH VARIABLES

processId  
Unique identifier of the process to cancel.

---

## Generate Process

`http://localhost:9000/v0/process/generate`

Generate a new process instance from one or more preconfigured process templates. This endpoint allows organizations to quickly launch processes without manually assembling items.

### HEADERS

Authorization  
Bearer org1-key-1-12345678901234567890123456789012

X-Signature  
0fa27b2e8e61d6f437ec932947b1cfe01eeb2e7e60a904fa7f54e6c9e310d2d8

X-timestamp  
1756199123456

Content-Type  
application/json

### BODY

```json
{
  "contacts": ["+351912345678", "demo@humanos.tech"],
  "processIds": ["64c8e5f7d5a4f9123b7c9a1e"]
}
```

---

## Generate Custom Process

`http://localhost:9000/v0/process/generate/custom`

Generate a new process dynamically by specifying custom sets of documents, consents, and forms. This endpoint gives organizations maximum flexibility to assemble workflows at runtime instead of relying solely on preconfigured templates.

For example, an AI agent could automatically choose which consents and forms to include in a process based on the user’s profile, and then generate and send that process in real-time.

### HEADERS

Authorization  
Bearer org1-key-1-12345678901234567890123456789012

X-Signature  
3c8f7393f8c1b2d1a9d0f6b9e5a123f9d0c1b2a3f8c7d6e5a4b3c2d1e0f9a8b7

X-timestamp  
1756199234567

Content-Type  
application/json

### BODY

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
