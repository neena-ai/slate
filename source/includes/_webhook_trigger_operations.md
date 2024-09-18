# Webhook Trigger Operations

Webhook trigger operations allow you to define actions that are initiated by external events through webhooks. These operations are tied to specific webhook trigger definitions and enable your workflows to react to real-time events from various integrations.

When an external event occurs and a webhook is received by Neena, a **trigger run** is created for each event. This trigger run effectively becomes a [flow request](https://docs.neena.io/#flow-requests), which then generates a [flow](https://docs.neena.io/#flows) and starts a [flow run](https://docs.neena.io/#flow-runs) immediately. This seamless process transforms incoming webhooks directly into executable workflows, allowing you to automate responses to events in real time without any manual intervention.

When creating a webhook trigger operation, you must specify a [webhook trigger definition](https://docs.neena.io/#webhook-trigger-definitions), which defines the type of event and the data structure you expect to receive. The `template_body` field allows you to map the incoming webhook payload to a format that can be used within your flows.


## Webhook Trigger Operation Model

| Field                      | Type               | Description                                                                                      |
|----------------------------|--------------------|--------------------------------------------------------------------------------------------------|
| **name**                   | string             | Name of the webhook trigger operation.                                                           |
| **webhook_trigger_definition** | string (UUID)  | ID of the associated webhook trigger definition.                                                 |
| **template_body**          | string             | Template for the webhook payload body.                                                           |
| **status**                 | string             | Status of the webhook trigger operation (e.g., 'active', 'inactive', 'failed').                  |
| **id**                     | string (UUID)      | Unique identifier of the webhook trigger operation.                                              |
| **created_date**           | string (date-time) | ISO 8601 formatted date-time when the operation was created.                                     |
| **modified_date**          | string (date-time) | ISO 8601 formatted date-time when the operation was last modified.                               |
| **created_by_email**       | string             | Email of the user who created the webhook trigger operation.                                     |
| **modified_by_email**      | string             | Email of the user who last modified the webhook trigger operation.                               |
| **organization**           | string (UUID)      | ID of the organization associated with the operation.                                            |

### Webhook Trigger Operation Status Enum

| Value     | Description                                     |
|-----------|-------------------------------------------------|
| **active**   | The webhook trigger operation is active.       |
| **inactive** | The webhook trigger operation is inactive.     |
| **failed**   | The webhook trigger operation has encountered a failure. |

---

## Create Webhook Trigger Operation

```shell
curl -X POST "api.neena.io/webhook_trigger_operations/" \
  -H "api_key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
        "name": "New Issue Created",
        "webhook_trigger_definition": "uuid-of-webhook-trigger-definition",
        "template_body": "{\"issue_id\": \"<{issue_id}>\", \"title\": \"<{title}>\"}",
        "status": "active"
      }'
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
    'Content-Type': 'application/json',
}

data = {
    "name": "New Issue Created",
    "webhook_trigger_definition": "uuid-of-webhook-trigger-definition",
    "template_body": "{\"issue_id\": \"<{issue_id}>\", \"title\": \"<{title}>\"}",
    "status": "active"
}

response = requests.post('api.neena.io/webhook_trigger_operations/', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/webhook_trigger_operations/';

const headers = {
  'api_key': 'YOUR_API_KEY',
  'Content-Type': 'application/json',
};

const body = {
  name: 'New Issue Created',
  webhook_trigger_definition: 'uuid-of-webhook-trigger-definition',
  template_body: '{"issue_id": "<{issue_id}>", "title": "<{title}>"}',
  status: 'active'
};

fetch(url, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(body),
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> The above command returns JSON structured like this:

```json
{
  "name": "New Issue Created",
  "webhook_trigger_definition": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "template_body": "{\"issue_id\": \"<{issue_id}>\", \"title\": \"<{title}>\"}",
  "status": "active",
  "id": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
  "created_date": "2024-09-18T12:00:00Z",
  "modified_date": "2024-09-18T12:00:00Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com",
  "organization": "1a2b3c4d-5e6f-7g8h-9i0j-k1l2m3n4o5p6"
}
```

This endpoint creates a new webhook trigger operation in the system.


The event fields are inserted into the body using `<{event_field}>`, which in turn fetches the item from the event and inserts it into the request.


### HTTP Request

`POST /webhook_trigger_operations/`

### Headers

| Field       | Type   | Required | Description                        |
|-------------|--------|----------|------------------------------------|
| api_key     | string | Yes      | API key `YOUR_API_KEY`             |

### Request Body

| Field                      | Type          | Required | Description                                                      |
|----------------------------|---------------|----------|------------------------------------------------------------------|
| name                       | string        | Yes      | Name of the webhook trigger operation.                           |
| webhook_trigger_definition | string (UUID) | Yes      | ID of the associated webhook trigger definition.                 |
| template_body              | string        | Yes      | Template for the webhook payload body.                           |
| status                     | string        | No       | Status of the webhook trigger operation (default is 'inactive'). |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 422    | Validation Error    |

---

## Read Webhook Trigger Operation

```shell
curl -X GET "api.neena.io/webhook_trigger_operations/?id=uuid-of-webhook-trigger-operation" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-webhook-trigger-operation'
}

response = requests.get('api.neena.io/webhook_trigger_operations/', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/webhook_trigger_operations/?id=uuid-of-webhook-trigger-operation';

fetch(url, {
  headers: {
    'api_key': 'YOUR_API_KEY',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> The above command returns JSON structured like this:

```json
{
  "name": "New Issue Created",
  "webhook_trigger_definition": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "template_body": "{\"issue_id\": \"<{issue_id}>\", \"title\": \"<{title}>\"}",
  "status": "active",
  "id": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
  "created_date": "2024-09-18T12:00:00Z",
  "modified_date": "2024-09-18T12:00:00Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com",
  "organization": "1a2b3c4d-5e6f-7g8h-9i0j-k1l2m3n4o5p6"
}
```

This endpoint retrieves a specific webhook trigger operation by its ID.

### HTTP Request

`GET /webhook_trigger_operations/`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type   | Required | Description                               |
|-------|--------|----------|-------------------------------------------|
| id    | string | Yes      | The ID of the webhook trigger operation.  |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read All Webhook Trigger Operations

```shell
curl -X GET "api.neena.io/webhook_trigger_operations/all?skip=0&limit=100" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'skip': 0,
    'limit': 100
}

response = requests.get('api.neena.io/webhook_trigger_operations/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/webhook_trigger_operations/all?skip=0&limit=100';

fetch(url, {
  headers: {
    'api_key': 'YOUR_API_KEY',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> The above command returns JSON structured like this:

```json
[
  {
    "name": "New Issue Created",
    "webhook_trigger_definition": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "template_body": "{\"issue_id\": \"<{issue_id}>\", \"title\": \"<{title}>\"}",
    "status": "active",
    "id": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
    "created_date": "2024-09-18T12:00:00Z",
    "modified_date": "2024-09-18T12:00:00Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com",
    "organization": "1a2b3c4d-5e6f-7g8h-9i0j-k1l2m3n4o5p6"
  },
  {
    "name": "Comment Added",
    "webhook_trigger_definition": "4ec2d0f1-1234-5678-9abc-def012345678",
    "template_body": "{\"comment_id\": \"{{comment_id}}\", \"content\": \"{{content}}\"}",
    "status": "inactive",
    "id": "7e4b2d1c-3456-789a-bcde-f0123456789a",
    "created_date": "2024-09-18T13:00:00Z",
    "modified_date": "2024-09-18T13:00:00Z",
    "created_by_email": "admin@neena.io",
    "modified_by_email": "admin@neena.io",
    "organization": "1a2b3c4d-5e6f-7g8h-9i0j-k1l2m3n4o5p6"
  }
]
```

This endpoint retrieves all webhook trigger operations associated with the user.

### HTTP Request

`GET /webhook_trigger_operations/all`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type | Required | Description                               |
|-------|------|----------|-------------------------------------------|
| skip  | int  | No       | Number of records to skip for pagination. |
| limit | int  | No       | Maximum number of records to return.      |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 422    | Validation Error    |

---

## Remove Webhook Trigger Operation

```shell
curl -X DELETE "api.neena.io/webhook_trigger_operations/?id=uuid-of-webhook-trigger-operation" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-webhook-trigger-operation'
}

response = requests.delete('api.neena.io/webhook_trigger_operations/', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/webhook_trigger_operations/?id=uuid-of-webhook-trigger-operation';

fetch(url, {
  method: 'DELETE',
  headers: {
    'api_key': 'YOUR_API_KEY',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint deletes a webhook trigger operation by its ID.

### HTTP Request

`DELETE /webhook_trigger_operations/`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type   | Required | Description                               |
|-------|--------|----------|-------------------------------------------|
| id    | string | Yes      | The ID of the webhook trigger operation.  |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Deletion |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |
