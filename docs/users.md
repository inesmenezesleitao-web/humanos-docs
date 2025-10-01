---
title: Users
description: Manage users and import existing user data into Humanos
---

# 📁 Users

This section covers all endpoints related to user management within Humanos.

---

## Import Users

This endpoint allows organizations to import their existing users into Humanos.

Multiple users can be created in a single request.

A user can be created with just a contact (email or phone).

Optionally, additional identity information (KYC) can be included.

Optionally, organizations can attach an internal Id to allow mapping users within their database.

When passing an identity along with the contact the minimum required fields are:

- **fullName**: Full name of the user  
- **birth**: Birth date (ex: "dd-mm-yyyy")  
- **docId**: National document identifier  
- **countryAlpha3**: Must follow the [ISO 3166 international standard](https://www.iban.com/country-codes)

### Method: POST
>```
>{{base_url}}/users/import
>```

### Headers

| Header         | Value                |
|----------------|----------------------|
| Authorization  | Bearer {{api_key}}   |
| X-Signature    | HMAC-SHA256 signature of the request |
| X-Timestamp    | Current timestamp in milliseconds |
| Content-Type   | application/json     |

### Body

```json
{
  "users": [
    {
      "contact": "jane.doe@example.com",
      "internalId": "USR001",
      "identity": {
        "fullName": "Jane Doe",
        "birth": "21-12-1995",
        "docId": "AB1234567",
        "countryAlpha3": "PRT"
      }
    },
    {
      "contact": "+351912345678",
      "internalId": "USR002"
    }
  ]
}
```

### Response: 200

```json
{
  "status": "imported",
  "importedUsers": [
    { "id": "684ade7148a18e172af7cab1", "contact": "jane.doe@example.com" },
    { "id": "684ade7148a18e172af7cab2", "contact": "+351912345678" }
  ]
}
```
