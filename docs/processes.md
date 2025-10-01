---
title: Processes
description: Create and manage processes, including document signing, forms, and consents
---

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


⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃

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


⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃

## Download PDFs
Both **Signature Events** and **Form Fills** generate PDFs containing information about the document and the signatures of all participants. This endpoint returns a PDF encoded as a Base64 string, including all signatures collected up to the current moment.

- If **no signatures** have been collected, the API returns the original document without any signatures.
    
- If **some users have already signed**, their signatures will appear on the PDF.
    
- For **forms**, the PDF also includes the date of submission and the answers provided by each user.
    

Each PDF or form is assigned a unique **itemId**. This item ID is communicated to the organization via webhook whenever a relevant event occurs. Organizations can use the item ID to retrieve the corresponding PDFs and store them in their systems.
### Method: GET
>```
>{{base_url}}/process/pdf/:itemId
>```
### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|
|X-timestamp|1756197897518|


### Response: 200
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

```json
{
    "filecontent": "JVBERi0xLjcKJYGBgYEKCjcgMCBvYmoKPDwK...<truncated>...",
    "internalId": "mRjFS7PWuosi"
}
```
</details>

### Response: 404
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

```json
{
    "statusCode": 404,
    "message": "Process item not found",
    "error": "Not Found"
}
```
</details>


⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃

## List Items
Retrieve all available items that can be used to build **custom processes**. These items include **templates, forms, and consents**, which can be dynamically combined to define workflows.

This endpoint can be consumed not only by frontend applications but also by **AI agents** to **generate and deliver custom processes on demand** to end-users. For example, an AI system could automatically select the appropriate templates, forms, and consents for a given context and trigger a process in real time.
### Method: GET
>```
>{{base_url}}/process/items/:search
>```
### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|
|X-timestamp|1756197897518|


### Response: 200
<details open style="width: fit-content; max-height: 600px; overflow: auto">
<summary>Response example:</summary>

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
    "id": "6842be61c51450fcf80877fd",
    "name": "Life Insurance Policy",
    "description": "Template for life insurance agreement.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "68824b3f655b773fda3d0ed9",
    "name": "ISO/IEC 27001 Audit Checklist",
    "description": "Audit checklist for ISO/IEC 27001 compliance.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "6883a92ca1e72aa9fd23c513",
    "name": "Employment Contract - Rodrigo Sarroeira",
    "description": "Personalized employment contract for Rodrigo Sarroeira.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "6899d06e00bec34b6d3fd9b3",
    "name": "Service Agreement 2025",
    "description": "Standard service agreement for 2025.",
    "internalId": "TPL-SERVICE-2025",
    "type": "TEMPLATE"
  },
  {
    "id": "6845a69f48a18e172af7ca74",
    "name": "Marketing Consent - Single Document",
    "description": "Unified consent document for marketing activities.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "683de70f2d2267803376acb4",
    "name": "Medical Liability Agreement",
    "description": "Agreement outlining clinical liability responsibilities.",
    "internalId": "TPL-CLINICAL-001",
    "type": "TEMPLATE"
  },
  {
    "id": "683de6b22d2267803376acb1",
    "name": "Service Contract",
    "description": "General service contract template.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "683de6cb2d2267803376acb2",
    "name": "Employment Contract",
    "description": "Standard employment contract template.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "683eb6254b4ddc288c3edd8a",
    "name": "Medical Liability Agreement 2025",
    "description": "Updated version of the clinical liability agreement for 2025.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "683ebdb54b4ddc288c3edd8f",
    "name": "Pet Inn Liability Agreement 2025",
    "description": "Liability agreement for Pet Inn services, 2025 edition.",
    "internalId": null,
    "type": "TEMPLATE"
  },
  {
    "id": "6883a855308d6b9931bdd444",
    "name": "Contrast Administration Consent",
    "description": "Consent form for medical contrast administration.",
    "internalId": "TPL-MEDICAL-002",
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
    "id": "689620e18e06c3561fd49f0d",
    "name": "Personal Information Form",
    "description": "Form to collect or update personal information.",
    "required": true,
    "internalId": null,
    "type": "FORM"
  },
  {
    "id": "68812c92f69fa320d3b3fd7c",
    "name": "Terms and Conditions",
    "description": "Consent to terms and conditions of service.",
    "required": true,
    "internalId": "CONSENT-TC-001",
    "type": "CONSENT"
  },
  {
    "id": "6894b96e8aecc042e54184cc",
    "name": "Webhook Test Consent",
    "description": "Test consent item for webhook validation.",
    "required": false,
    "internalId": null,
    "type": "CONSENT"
  },
  {
    "id": "6894b97a8aecc042e54184cd",
    "name": "New Consent Agreement",
    "description": "Generic consent document for new use cases.",
    "required": true,
    "internalId": "CONSENT-NEW-001",
    "type": "CONSENT"
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


⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃

## Resend Process
This endpoint is used when the initial OTP delivery fails. If the first attempt is unsuccessful, Humanos will send a failure notification through a webhook, and this endpoint can then be called to resend a new OTP.
### Method: PATCH
>```
>{{base_url}}/process/resend/:userOtpId
>```
### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|
|X-Timestamp|1756197897518|


### Body (**raw**)

```json

```


⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃

## Cancel Process
This endpoint cancels a previously sent process.

- Once cancelled, the unique process link becomes inactive and no further user interactions are possible.
    
- Cancellation is only permitted if the process has **not yet been completed by all participants**.
    
- Any data already collected from users **remains stored** and is not deleted.
### Method: PATCH
>```
>{{base_url}}/process/cancel/:processId
>```
### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|
|X-Timestamp|1756197897518|


### Body (**raw**)

```json

```


⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃

## Generate Process
**Processes** can be created via the organization dashboard and may include **Identity Validations**, **Signature Requests**, **Consent Collection**, and **Form Filling**.

This endpoint triggers the creation and delivery of unique process links, enabling clients to complete the tasks requested by the organization.

- You can specify a list of processIds to be sent, along with a list of contacts.
    
- When multiple contacts are provided, all users are attached to the same process instance.
    
- To send processes to users separately, make multiple calls, each including only the intended contact.
    

### **Security Levels**

The securityLevel parameter defines the authentication required from the client:

- **Level 0** – End-user only needs a verified contact via OTP (no identity required).
    
- **Level 1 (default)** – End-user must have a Humanos Verified Identity **or** a KYC performed by the requesting organization.
    
- **Level 2** – End-user must have a Humanos Verified Identity.
    
- **Level 3** – End-user must perform a full KYC (document scan + selfie) for each process.
    

> **Note:** Contacts not yet registered in the Humanos API will be created automatically.
### Method: POST
>```
>{{base_url}}/process/generate
>```
### Headers

|Header|Value|
|---|---|
|Authorization|Bearer {{api_key}}|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|
|X-Timestamp|1756197897518|


### Body (**raw**)

```json
{
    "contacts": ["+351917210770", "+351916724141", "+351964152121"],
    "processIds": ["68812be4f69fa320d3b3fd7b", "68812e4ef69fa320d3b3fd85"],
    "securityLevel": 1
}
```


⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃ ⁃

## Generate Custom Process
This endpoint **creates and sends a custom process on demand**.

It can be used by:

- **Frontend applications** – allowing users to build processes dynamically.
    
- **AI agents** – automatically generating and delivering processes to users in real time.
    

To generate a process, the request body must include:

- The identifiers of the items to be used (e.g., **templates, forms, consents, documents**).
    
- The **order** in which these items should be presented to the user.
    
- (Optional) An array of PDFs containing the name and fileContent as a base64 string. Optionally, an internalId and extraFields (Json) can be passed. The extra fields will be displayed in the signature page.
    
- (Optional) The securityLevel parameter, which controls the authentication required from the client. If not provided, it defaults to **Level 1**.
    

**Security Levels**

The securityLevel parameter accepts the following values:

- **Level 0** – End-user only needs a verified contact via OTP (no identity required).
    
- **Level 1 (default)** – End-user must have a Humanos Verified Identity **or** a KYC performed by the requesting organization.
    
- **Level 2** – End-user must have a Humanos Verified Identity.
    
- **Level 3** – End-user must perform a full KYC (document scan + selfie) for each process.
### Method: POST
>```
>{{base_url}}/process/generate-custom
>```
### Headers

|Content-Type|Value|
|---|---|
|Authorization|Bearer {{api_key}}|


### Headers

|Content-Type|Value|
|---|---|
|X-Signature|8c7695c48907e84f5f6b1ec94362a2798ca640621ed051e95a152f6ab9d2ec4f|


### Headers

|Content-Type|Value|
|---|---|
|X-Timestamp|1756197897518|


### Body (**raw**)

```json
{
  "contacts": [
    "+573178901234",
    "+573178901235"
  ],
  "securityLevel": 1,
  "order": [
    "CONSENT",
    "FORM",
    "DOCUMENT"
  ],
  "templateIds": [
    "507f1f77bcf86cd799439011"
  ],
  "formIds": [
    "507f1f77bcf86cd799439012"
  ],
  "consentIds": [
    "507f1f77bcf86cd799439013"
  ],
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