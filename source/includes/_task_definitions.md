# Task Definitions

Task definitions are the blueprints for task operations, which in turn are the core components of a [Flow](https://docs.neena.io/#flows, 'Flow Model') in Neena. They define specific operations or actions within a flow, detailing the necessary inputs and expected outputs. These tasks integrate with external systems and services.

Creating, updating, or deleting task definitions is not possible through the API; they are maintained by the platform but can be viewed and utilized in your flows.

Understanding task definitions helps you identify possible actions for your flows. For instance, when setting up a flow that includes the "Create Trello Card" task, knowing the required parameters (`board_id`, `list_id`, `card_name`) ensures you gather or generate these values beforehand.

<aside class="notice">
Task definitions depend on integrations. Make sure you have the appropriate credentials before using a task. For example, to use the "Create Trello Card" task, authorize Neena to access your Trello account via the [application](https://app.neena.io/integrations).
</aside>

If you would like to see certain tasks on the platform, [talk to founder](mailto:rauf@neena.io) to get support.

## Task Definition Model

| Field                  | Type               | Description                                                                                 |
|------------------------|--------------------|---------------------------------------------------------------------------------------------|
| **task_name**          | string             | Name of the task.                                                                           |
| **integration**        | string (UUID)      | ID of the associated integration.                                                           |
| **parameters**         | array of objects   | List of input parameters required for the task.                                             |
| **input_type**         | string             | Type of input expected by the task.                                                         |
| **input_yml**          | string             | YAML representation of the input parameters.                                                |
| **description**        | string             | Description of the task's functionality.                                                    |
| **python_method_name** | string             | Internal Python method name implementing the task.                                          |
| **output_type**        | string             | Type of output produced by the task.                                                        |
| **output_yml**         | string             | YAML representation of the output parameters.                                               |
| **output_parameters**  | array of objects   | List of output parameters provided by the task.                                             |
| **output_is_list**     | boolean            | Indicates whether the output is a list.                                                     |
| **id**                 | string (UUID)      | Unique identifier of the task definition.                                                   |
| **created_date**       | string (date-time) | ISO 8601 formatted date-time when the task was created.                                     |
| **modified_date**      | string (date-time) | ISO 8601 formatted date-time when the task was last modified.                               |
| **created_by_email**   | string (email)     | Email of the user who created the task definition.                                          |
| **modified_by_email**  | string (email)     | Email of the user who last modified the task definition.                                    |
| **deleted_at**         | string (date-time) | ISO 8601 formatted date-time when the task was deleted, if applicable.                      |
| **belongs_to_integration** | object         | The integration object to which this task definition belongs.                               |

### Task Parameter Model

| Field         | Type    | Description                                        |
|---------------|---------|----------------------------------------------------|
| **name**      | string  | Name of the parameter.                             |
| **data_type** | string  | Data type of the parameter (e.g., 'string', 'int').|
| **position**  | integer | Position of the parameter in the parameter list.   |
| **doc_string**| string  | Description of the parameter.                      |
| **optional**  | boolean | Indicates whether the parameter is optional.       |

---

## Read All Task Definitions

```shell
curl -X GET "api.neena.io/task_definitions/all?skip=0&limit=100" \
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

response = requests.get('api.neena.io/task_definitions/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/task_definitions/all?skip=0&limit=100';

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
    "task_name": "Send Email",
    "integration": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "parameters": [
      {
        "name": "email_address",
        "data_type": "string",
        "position": 1,
        "doc_string": "The email address to send to.",
        "optional": false
      },
      {
        "name": "subject",
        "data_type": "string",
        "position": 2,
        "doc_string": "The subject of the email.",
        "optional": false
      },
      {
        "name": "body",
        "data_type": "string",
        "position": 3,
        "doc_string": "The body content of the email.",
        "optional": false
      }
    ],
    "input_type": "EmailInput",
    "input_yml": "parameters:\n  - name: email_address\n    data_type: string\n    position: 1\n    doc_string: 'The email address to send to.'\n    optional: false\n  - name: subject\n    data_type: string\n    position: 2\n    doc_string: 'The subject of the email.'\n    optional: false\n  - name: body\n    data_type: string\n    position: 3\n    doc_string: 'The body content of the email.'\n    optional: false",
    "description": "Sends an email using the specified integration.",
    "python_method_name": "send_email",
    "output_type": "EmailOutput",
    "output_yml": "parameters:\n  - name: status\n    data_type: string\n    position: 1\n    doc_string: 'Status of the email sending operation.'\n    optional: false",
    "output_parameters": [
      {
        "name": "status",
        "data_type": "string",
        "position": 1,
        "doc_string": "Status of the email sending operation.",
        "optional": false
      }
    ],
    "output_is_list": false,
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "created_date": "2024-09-18T10:00:00Z",
    "modified_date": "2024-09-18T10:00:00Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com",
    "deleted_at": null,
    "belongs_to_integration": {
      "class_name": "EmailIntegration",
      "name": "Email",
      "short_name": "email",
      "auth_type": "api_key",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "created_date": "2024-09-18T09:00:00Z",
      "modified_date": "2024-09-18T09:00:00Z"
    }
  },
  {
    "task_name": "Create Trello Card",
    "integration": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
    "parameters": [
      {
        "name": "board_id",
        "data_type": "string",
        "position": 1,
        "doc_string": "The ID of the Trello board.",
        "optional": false
      },
      {
        "name": "list_id",
        "data_type": "string",
        "position": 2,
        "doc_string": "The ID of the list on the board.",
        "optional": false
      },
      {
        "name": "card_name",
        "data_type": "string",
        "position": 3,
        "doc_string": "The name of the new card.",
        "optional": false
      }
    ],
    "input_type": "TrelloCardInput",
    "input_yml": "parameters:\n  - name: board_id\n    data_type: string\n    position: 1\n    doc_string: 'The ID of the Trello board.'\n    optional: false\n  - name: list_id\n    data_type: string\n    position: 2\n    doc_string: 'The ID of the list on the board.'\n    optional: false\n  - name: card_name\n    data_type: string\n    position: 3\n    doc_string: 'The name of the new card.'\n    optional: false",
    "description": "Creates a new card in a specified Trello board and list.",
    "python_method_name": "create_trello_card",
    "output_type": "TrelloCardOutput",
    "output_yml": "parameters:\n  - name: card_id\n    data_type: string\n    position: 1\n    doc_string: 'The ID of the created card.'\n    optional: false",
    "output_parameters": [
      {
        "name": "card_id",
        "data_type": "string",
        "position": 1,
        "doc_string": "The ID of the created card.",
        "optional": false
      }
    ],
    "output_is_list": false,
    "id": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
    "created_date": "2024-09-18T11:00:00Z",
    "modified_date": "2024-09-18T11:00:00Z",
    "created_by_email": "admin@neena.io",
    "modified_by_email": "admin@neena.io",
    "deleted_at": null,
    "belongs_to_integration": {
      "class_name": "TrelloIntegration",
      "name": "Trello",
      "short_name": "trello",
      "auth_type": "oauth2",
      "id": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
      "created_date": "2024-09-18T10:30:00Z",
      "modified_date": "2024-09-18T10:30:00Z"
    }
  }
  // Additional task definitions...
]
```

This endpoint retrieves all task definitions available in the system.

### HTTP Request

`GET /task_definitions/all`

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

## Read Task Definition

```shell
curl -X GET "api.neena.io/task_definitions/?id=uuid-of-task-definition" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-task-definition'
}

response = requests.get('api.neena.io/task_definitions/', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/task_definitions/?id=uuid-of-task-definition';

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
  "task_name": "Create Trello Card",
  "integration": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
  "parameters": [
    {
      "name": "board_id",
      "data_type": "string",
      "position": 1,
      "doc_string": "The ID of the Trello board.",
      "optional": false
    },
    {
      "name": "list_id",
      "data_type": "string",
      "position": 2,
      "doc_string": "The ID of the list on the board.",
      "optional": false
    },
    {
      "name": "card_name",
      "data_type": "string",
      "position": 3,
      "doc_string": "The name of the new card.",
      "optional": false
    }
  ],
  "input_type": "TrelloCardInput",
  "input_yml": "parameters:\n  - name: board_id\n    data_type: string\n    position: 1\n    doc_string: 'The ID of the Trello board.'\n    optional: false\n  - name: list_id\n    data_type: string\n    position: 2\n    doc_string: 'The ID of the list on the board.'\n    optional: false\n  - name: card_name\n    data_type: string\n    position: 3\n    doc_string: 'The name of the new card.'\n    optional: false",
  "description": "Creates a new card in a specified Trello board and list.",
  "python_method_name": "create_trello_card",
  "output_type": "TrelloCardOutput",
  "output_yml": "parameters:\n  - name: card_id\n    data_type: string\n    position: 1\n    doc_string: 'The ID of the created card.'\n    optional: false",
  "output_parameters": [
    {
      "name": "card_id",
      "data_type": "string",
      "position": 1,
      "doc_string": "The ID of the created card.",
      "optional": false
    }
  ],
  "output_is_list": false,
  "id": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
  "created_date": "2024-09-18T11:00:00Z",
  "modified_date": "2024-09-18T11:00:00Z",
  "created_by_email": "admin@neena.io",
  "modified_by_email": "admin@neena.io",
  "deleted_at": null,
  "belongs_to_integration": {
    "class_name": "TrelloIntegration",
    "name": "Trello",
    "short_name": "trello",
    "auth_type": "oauth2",
    "id": "9b74c989-7b8f-4e4a-8a3e-0b6c0d2e8f06",
    "created_date": "2024-09-18T10:30:00Z",
    "modified_date": "2024-09-18T10:30:00Z"
  }
}
```

This endpoint retrieves a single task definition by its ID.

### HTTP Request

`GET /task_definitions/`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type   | Required | Description                       |
|-------|--------|----------|-----------------------------------|
| id    | string | Yes      | The ID of the task definition.    |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |
