# Base URL
```
https://api.loopcommunications.com/v1/
```
# Authentication

All requests will require a valid API key in the form of a Bearer token in your request.

```
Authorization: Bearer YOUR_API_KEY
```

# SMS

## Send SMS
**Method:** `POST`

**URL:** `/sms/send_sms`

### Request Body

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| to | string | Y | DID to send message to |
| from | string | Y | DID to send message from |
| message | string | Y | Body of SMS message |

### Example Request

```json
{
  "to": "1112223333",
  "from": "3332221111",
  "message": "This is an SMS message"
}
```

## Fetch SMS Messages
**Method:** `GET`

**URL:** `/sms/fetch_messages`

### Parameters

| Parameter | Required | Description |
|-------|----------|-------------|
| did | Y | DID to fetch messages for |
| last_known_id | N | Latest known message ID, will return all messages newer than ID provided |

If last_known_id is not provided, all messages for the DID will be returned.

### Example Request

```http
GET /sms/fetch_messages?did=1112223333&last_known_id=111111
```

### Example Response

```json
{
    "success": true,
    "message": "",
    "data": [
      {
         "sms_id": 111112,
         "date": "2023-11-06 15:47:39",
         "sender": "1112223333",
         "recipient": "2221113333",
         "status": "delivered",
         "sms_text": "SMS message body",
         "error": "N",
         "error_messsage": ""
      },
      {
         "sms_id": 111113,
         "date": "2023-11-07 15:47:39",
         "sender": "1112223333",
         "recipient": "2221113333",
         "status": "delivered",
         "sms_text": "SMS message body",
         "error": "N",
         "error_messsage": ""
      }
}
```

If no messages with an ID greater than the last known ID provided are found, the following response will be returned:

```json
{
    "success": true,
    "message": "No new messages found",
    "data": []
}
```

# Error Responses

| Status | Description |
|--------|-------------|
| 401 | Unauthorized |
| 500 | Internal server error |

## Example Error

**HTTP 401 Unauthorized**

```json
{
    "success": false,
    "message": "Invalid API Key"
}
```