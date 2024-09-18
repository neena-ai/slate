# Webhook Trigger Definitions

Webhook trigger definitions specify the types of external events that can initiate workflows in Neena. They define the structure and data expected from incoming webhooks, enabling you to integrate with various external systems and services.

Webhook trigger definitions are directly linked to [webhook trigger operations](https://docs.neena.io/#webhook-trigger-operations). When you create a webhook trigger operation, you must specify the webhook trigger definition it is based on. This ensures that the incoming webhook data is correctly interpreted and processed according to the defined event fields.


When an external event occurs and a webhook is sent to Neena, a **trigger run** is created based on the matching webhook trigger definition. This trigger run effectively becomes a [flow request](https://docs.neena.io/#flow-requests), which then generates a [flow](https://docs.neena.io/#flows) and starts a [flow run](https://docs.neena.io/#flow-runs) immediately. This process allows you to automate responses to events in real time, transforming incoming webhooks into executable workflows without manual intervention.

When creating a webhook trigger operation, you must specify a webhook trigger definition, which defines the type of event and the data structure you expect to receive. The `event_fields` field allows you to map the incoming webhook payload to a format that can be used within your flows.



## Webhook Trigger Definition Model

| Field                   | Type               | Description                                                                                 |
|-------------------------|--------------------|---------------------------------------------------------------------------------------------|
| **trigger_name**        | string             | Name of the webhook trigger definition.                                                     |
| **integration**         | string (UUID)      | ID of the associated integration.                                                           |
| **python_method_name**  | string             | Internal Python method name handling the webhook logic.                                     |
| **event_fields**        | array of objects   | List of event fields expected in the webhook payload.                                       |
| **description**         | string             | Description of the webhook trigger definition.                                              |
| **id**                  | string (UUID)      | Unique identifier of the webhook trigger definition.                                        |
| **created_date**        | string (date-time) | ISO 8601 formatted date-time when the definition was created.                               |
| **modified_date**       | string (date-time) | ISO 8601 formatted date-time when the definition was last modified.                         |
| **created_by_email**    | string             | Email of the user who created the webhook trigger definition.                               |
| **modified_by_email**   | string             | Email of the user who last modified the webhook trigger definition.                         |
| **deleted_at**          | string (date-time) | ISO 8601 formatted date-time when the definition was deleted, if applicable.                |

### Event Field Model

| Field        | Type    | Description                                              |
|--------------|---------|----------------------------------------------------------|
| **name**     | string  | Name of the event field.                                 |
| **json_path**| string  | JSON path to extract the field from the webhook payload. |
| **data_type**| string  | Data type of the field (e.g., 'string', 'int').          |
| **doc_string**| string | Description of the event field.                          |
| **optional** | boolean | Indicates whether the field is optional.                 |

---

## Read All Webhook Trigger Definitions

```shell
curl -X GET "api.neena.io/webhook_trigger_definitions/all?skip=0&limit=100" \
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

response = requests.get('api.neena.io/webhook_trigger_definitions/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/webhook_trigger_definitions/all?skip=0&limit=100';

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
    "trigger_name": "New Issue Created",
    "integration": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
    "python_method_name": "handle_new_issue",
    "event_fields": [
      {
        "name": "issue_id",
        "json_path": "$.issue.id",
        "data_type": "string",
        "doc_string": "The unique ID of the issue.",
        "optional": false
      },
      {
        "name": "title",
        "json_path": "$.issue.title",
        "data_type": "string",
        "doc_string": "The title of the issue.",
        "optional": false
      },
      {
        "name": "description",
        "json_path": "$.issue.description",
        "data_type": "string",
        "doc_string": "The description of the issue.",
        "optional": true
      }
    ],
    "description": "Triggered when a new issue is created in the issue tracking system.",
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "created_date": "2024-09-18T10:00:00Z",
    "modified_date": "2024-09-18T10:00:00Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com",
    "deleted_at": null
  },
  {
    "trigger_name": "Comment Added",
    "integration": "4ec2d0f1-1234-5678-9abc-def012345678",
    "python_method_name": "handle_comment_added",
    "event_fields": [
      {
        "name": "comment_id",
        "json_path": "$.comment.id",
        "data_type": "string",
        "doc_string": "The unique ID of the comment.",
        "optional": false
      },
      {
        "name": "content",
        "json_path": "$.comment.content",
        "data_type": "string",
        "doc_string": "Content of the comment.",
        "optional": false
      }
    ],
    "description": "Triggered when a new comment is added to an issue.",
    "id": "7e4b2d1c-3456-789a-bcde-f0123456789a",
    "created_date": "2024-09-18T11:00:00Z",
    "modified_date": "2024-09-18T11:00:00Z",
    "created_by_email": "admin@neena.io",
    "modified_by_email": "admin@neena.io",
    "deleted_at": null
  }
]
```

This endpoint retrieves all webhook trigger definitions available in the system.

### HTTP Request

`GET /webhook_trigger_definitions/all`

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

## Read Webhook Trigger Definition

```shell
curl -X GET "api.neena.io/webhook_trigger_definitions/?id=uuid-of-webhook-trigger-definition" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-webhook-trigger-definition'
}

response = requests.get('api.neena.io/webhook_trigger_definitions/', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/webhook_trigger_definitions/?id=uuid-of-webhook-trigger-definition';

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
  "trigger_name": "New Issue Created",
  "integration": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
  "python_method_name": "handle_new_issue",
  "event_fields": [
    {
      "name": "issue_id",
      "json_path": "$.issue.id",
      "data_type": "string",
      "doc_string": "The unique ID of the issue.",
      "optional": false
    },
    {
      "name": "title",
      "json_path": "$.issue.title",
      "data_type": "string",
      "doc_string": "The title of the issue.",
      "optional": false
    },
    {
      "name": "description",
      "json_path": "$.issue.description",
      "data_type": "string",
      "doc_string": "The description of the issue.",
      "optional": true
    }
  ],
  "description": "Triggered when a new issue is created in the issue tracking system.",
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "created_date": "2024-09-18T10:00:00Z",
  "modified_date": "2024-09-18T10:00:00Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com",
  "deleted_at": null
}
```

This endpoint retrieves a specific webhook trigger definition by its ID.

### HTTP Request

`GET /webhook_trigger_definitions/`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type   | Required | Description                               |
|-------|--------|----------|-------------------------------------------|
| id    | string | Yes      | The ID of the webhook trigger definition. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

