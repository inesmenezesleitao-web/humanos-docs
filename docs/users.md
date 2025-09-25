# Users

This API section allows organizations to manage and create their users along with their identities. 

## KYC

### Method: POST
```
{{base_url}}/user/kyc
```

### Headers
| Header | Value |
|---|---|
| Authorization | Bearer {{api_key}} |
| X-Signature | HMAC-SHA256 signature |
| X-Timestamp | Current timestamp in ms |

### Request Body
```json
{
  "users": [
    {
      "contact": "john.doe@example.com",
      "identity": {
        "fullName": "John Doe",
        "birth": "01-01-1990",
        "docId": "1234567890",
        "countryAlpha3": "PRT"
      },
      "internalId": "12312312312"
    }
  ]
}
```
