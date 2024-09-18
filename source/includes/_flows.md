# Flows

Flows represent the blueprint of a workflow, defining the sequence of task operations and their dependencies. They are generated from a [Flow Request](https://docs.neena.io/#flow-requests, 'Flow Request Model') and can be executed to perform the specified tasks in order.

Flows are essential for structuring complex workflows, allowing you to define tasks, set dependencies, and manage the overall process. Once a flow is executed, it creates a [Flow Run](https://docs.neena.io/#flow-runs, 'Flow Run Model') that performs the tasks as defined.

## Flow Model

| Field             | Type                | Required | Description                                    |
|-------------------|---------------------|----------|------------------------------------------------|
| **name**              | string              | Yes      | Name of the flow.                              |
| **task_operations**   | array of objects     | Yes      | List of task operations in the flow.           |
| **dependencies**      | array of objects     | Yes      | List of dependencies between task operations.  |
| **id**                | string (UUID)        | Yes      | Unique identifier of the flow.                 |
| **created_date**      | string (date-time)   | Yes      | When the flow was created.                     |
| **modified_date**     | string (date-time)   | Yes      | When the flow was last modified.               |
| **created_by_email**  | string (email)       | Yes      | Email of the creator.                          |
| **modified_by_email** | string (email)       | Yes      | Email of the last modifier.                    |

## Task Operation Model

| Field               | Type             | Description                                            |
|---------------------|------------------|--------------------------------------------------------|
| **name**            | string           | Name of the task operation.                            |
| **task_definition** | string (UUID)    | ID of the associated task definition.                  |
| **instruction**     | string           | Instructions for the task operation.                   |
| **x**               | number           | X-coordinate for UI representation.                    |
| **y**               | number           | Y-coordinate for UI representation.                    |
| **index**           | integer          | Index of the task operation in the flow.               |
| **id**              | string (UUID)    | Unique identifier of the task operation.               |
| **flow**            | string (UUID)    | ID of the associated flow.                             |

## Dependency Model

| Field                   | Type             | Description                                              |
|-------------------------|------------------|----------------------------------------------------------|
| **instruction**         | string           | Instructions for the dependency.                         |
| **source_task_operation** | integer        | Index of the source task operation.                      |
| **target_task_operation** | integer        | Index of the target task operation.                      |
| **id**                  | string (UUID)    | Unique identifier of the dependency.                     |
| **flow**                | string (UUID)    | ID of the associated flow.                               |

---

## Create Flow

```shell
curl -X POST "api.neena.io/flows" \
  -H "api_key: YOUR_API_KEY" \
  -d '{
        "name": "Example Flow",
        "task_operations": [
          {
            "name": "Task 1",
            "task_definition": "uuid-of-task-definition-1",
            "instruction": "Perform task 1",
            "index": 0
          },
          {
            "name": "Task 2",
            "task_definition": "uuid-of-task-definition-2",
            "instruction": "Perform task 2",
            "index": 1
          }
        ],
        "dependencies": [
          {
            "source_task_operation": 0,
            "target_task_operation": 1,
            "instruction": "Task 2 depends on Task 1"
          }
        ]
      }'
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

data = {
    "name": "Example Flow",
    "task_operations": [
        {
            "name": "Task 1",
            "task_definition": "uuid-of-task-definition-1",
            "instruction": "Perform task 1",
            "index": 0
        },
        {
            "name": "Task 2",
            "task_definition": "uuid-of-task-definition-2",
            "instruction": "Perform task 2",
            "index": 1
        }
    ],
    "dependencies": [
        {
            "source_task_operation": 0,
            "target_task_operation": 1,
            "instruction": "Task 2 depends on Task 1"
        }
    ]
}

response = requests.post('api.neena.io/flows', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flows';

const headers = {
  'api_key': 'YOUR_API_KEY',
};

const body = {
  name: 'Example Flow',
  task_operations: [
    {
      name: 'Task 1',
      task_definition: 'uuid-of-task-definition-1',
      instruction: 'Perform task 1',
      index: 0
    },
    {
      name: 'Task 2',
      task_definition: 'uuid-of-task-definition-2',
      instruction: 'Perform task 2',
      index: 1
    }
  ],
  dependencies: [
    {
      source_task_operation: 0,
      target_task_operation: 1,
      instruction: 'Task 2 depends on Task 1'
    }
  ]
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
  "name": "Example Flow",
  "task_operations": [
    {
      "name": "Task 1",
      "task_definition": "uuid-of-task-definition-1",
      "instruction": "Perform task 1",
      "x": null,
      "y": null,
      "index": 0,
      "sorted_index": null,
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "flow": "uuid-of-flow",
      "created_date": "2024-09-18T10:27:57.149Z",
      "modified_date": "2024-09-18T10:27:57.149Z",
      "created_by_email": "user@example.com",
      "modified_by_email": "user@example.com"
    },
    {
      "name": "Task 2",
      "task_definition": "uuid-of-task-definition-2",
      "instruction": "Perform task 2",
      "x": null,
      "y": null,
      "index": 1,
      "sorted_index": null,
      "id": "4ec2d0f1-1234-5678-9abc-def012345678",
      "flow": "uuid-of-flow",
      "created_date": "2024-09-18T10:27:57.149Z",
      "modified_date": "2024-09-18T10:27:57.149Z",
      "created_by_email": "user@example.com",
      "modified_by_email": "user@example.com"
    }
  ],
  "dependencies": [
    {
      "instruction": "Task 2 depends on Task 1",
      "source_task_operation": 0,
      "target_task_operation": 1,
      "id": "5fd7a7d2-9876-5432-1abc-def012345678",
      "flow": "uuid-of-flow",
      "created_date": "2024-09-18T10:27:57.149Z",
      "modified_date": "2024-09-18T10:27:57.149Z",
      "created_by_email": "user@example.com",
      "modified_by_email": "user@example.com"
    }
  ],
  "id": "uuid-of-flow",
  "created_date": "2024-09-18T10:27:57.149Z",
  "modified_date": "2024-09-18T10:27:57.149Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint creates a new flow in the system.

### HTTP Request

`POST /flows`

### Headers

| Field    | Type   | Required | Description           |
|----------|--------|----------|-----------------------|
| api_key  | string | Yes      | API key `YOUR_API_KEY`|

### Request Body

| Field             | Type                | Required | Description                                    |
|-------------------|---------------------|----------|------------------------------------------------|
| name              | string              | Yes      | Name of the flow.                              |
| task_operations   | array of objects    | Yes      | List of task operations in the flow.           |
| dependencies      | array of objects    | Yes      | List of dependencies between task operations.  |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 422    | Validation Error    |

---

## Read Flow

```shell
curl -X GET "api.neena.io/flows?id=uuid-of-flow" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-flow'
}

response = requests.get('api.neena.io/flows', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flows?id=uuid-of-flow';

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
  "name": "Example Flow",
  "task_operations": [
    {
      "name": "Task 1",
      "task_definition": "uuid-of-task-definition-1",
      "instruction": "Perform task 1",
      "x": null,
      "y": null,
      "index": 0,
      "sorted_index": null,
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "flow": "uuid-of-flow",
      "created_date": "2024-09-18T10:27:57.149Z",
      "modified_date": "2024-09-18T10:27:57.149Z",
      "created_by_email": "user@example.com",
      "modified_by_email": "user@example.com"
    },
    {
      "name": "Task 2",
      "task_definition": "uuid-of-task-definition-2",
      "instruction": "Perform task 2",
      "x": null,
      "y": null,
      "index": 1,
      "sorted_index": null,
      "id": "4ec2d0f1-1234-5678-9abc-def012345678",
      "flow": "uuid-of-flow",
      "created_date": "2024-09-18T10:27:57.149Z",
      "modified_date": "2024-09-18T10:27:57.149Z",
      "created_by_email": "user@example.com",
      "modified_by_email": "user@example.com"
    }
  ],
  "dependencies": [
    {
      "instruction": "Task 2 depends on Task 1",
      "source_task_operation": 0,
      "target_task_operation": 1,
      "id": "5fd7a7d2-9876-5432-1abc-def012345678",
      "flow": "uuid-of-flow",
      "created_date": "2024-09-18T10:27:57.149Z",
      "modified_date": "2024-09-18T10:27:57.149Z",
      "created_by_email": "user@example.com",
      "modified_by_email": "user@example.com"
    }
  ],
  "id": "uuid-of-flow",
  "created_date": "2024-09-18T10:27:57.149Z",
  "modified_date": "2024-09-18T10:27:57.149Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint retrieves a specific flow by its ID.

### HTTP Request

`GET /flows`

### Headers

| Field    | Type   | Required | Description           |
|----------|--------|----------|-----------------------|
| api_key  | string | Yes      | API key `YOUR_API_KEY`|

### Query Parameters

| Field | Type   | Required | Description           |
|-------|--------|----------|-----------------------|
| id    | string | Yes      | The ID of the flow.   |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read All Flows

```shell
curl -X GET "api.neena.io/flows/all?skip=0&limit=100" \
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

response = requests.get('api.neena.io/flows/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flows/all?skip=0&limit=100';

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
    "name": "Example Flow 1",
    "task_operations": [
      {
        "name": "Task A",
        "task_definition": "uuid-of-task-definition-A",
        "instruction": "Perform task A",
        "x": null,
        "y": null,
        "index": 0,
        "sorted_index": null,
        "id": "task-operation-id-A",
        "flow": "uuid-of-flow-1",
        "created_date": "2024-09-18T10:27:57.149Z",
        "modified_date": "2024-09-18T10:27:57.149Z",
        "created_by_email": "user@example.com",
        "modified_by_email": "user@example.com"
      }
    ],
    "dependencies": [],
    "id": "uuid-of-flow-1",
    "created_date": "2024-09-18T10:27:57.149Z",
    "modified_date": "2024-09-18T10:27:57.149Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com"
  },
  {
    "name": "Example Flow 2",
    "task_operations": [
      {
        "name": "Task B",
        "task_definition": "uuid-of-task-definition-B",
        "instruction": "Perform task B",
        "x": null,
        "y": null,
        "index": 0,
        "sorted_index": null,
        "id": "task-operation-id-B",
        "flow": "uuid-of-flow-2",
        "created_date": "2024-09-18T10:27:57.149Z",
        "modified_date": "2024-09-18T10:27:57.149Z",
        "created_by_email": "user@example.com",
        "modified_by_email": "user@example.com"
      }
    ],
    "dependencies": [],
    "id": "uuid-of-flow-2",
    "created_date": "2024-09-18T10:27:57.149Z",
    "modified_date": "2024-09-18T10:27:57.149Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com"
  }
]
```

This endpoint retrieves all flows available to the user.

### HTTP Request

`GET /flows/all`

### Headers

| Field    | Type   | Required | Description           |
|----------|--------|----------|-----------------------|
| api_key  | string | Yes      | API key `YOUR_API_KEY`|

### Query Parameters

| Field | Type | Required | Description                                |
|-------|------|----------|--------------------------------------------|
| skip  | int  | No       | Number of records to skip for pagination.  |
| limit | int  | No       | Maximum number of records to return.       |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 422    | Validation Error    |

---

## Delete Flow

```shell
curl -X DELETE "api.neena.io/flows?id=uuid-of-flow" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-flow'
}

response = requests.delete('api.neena.io/flows', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flows?id=uuid-of-flow';

fetch(url, {
  method: 'DELETE',
  headers: {
    'api_key': 'YOUR_API_KEY',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint deletes a specific flow by its ID.

### HTTP Request

`DELETE /flows`

### Headers

| Field    | Type   | Required | Description           |
|----------|--------|----------|-----------------------|
| api_key  | string | Yes      | API key `YOUR_API_KEY`|

### Query Parameters

| Field | Type   | Required | Description           |
|-------|--------|----------|-----------------------|
| id    | string | Yes      | The ID of the flow.   |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Execute Flow

```shell
curl -X POST "api.neena.io/flows/execute?flow_id=uuid-of-flow&flow_request_id=uuid-of-flow-request" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'flow_id': 'uuid-of-flow',
    'flow_request_id': 'uuid-of-flow-request'
}

response = requests.post('api.neena.io/flows/execute', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flows/execute?flow_id=uuid-of-flow&flow_request_id=uuid-of-flow-request';

fetch(url, {
  method: 'POST',
  headers: {
    'api_key': 'YOUR_API_KEY',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint executes a flow by its ID, creating a new flow run.

### HTTP Request

`POST /flows/execute`

### Headers

| Field    | Type   | Required | Description           |
|----------|--------|----------|-----------------------|
| api_key  | string | Yes      | API key `YOUR_API_KEY`|

### Query Parameters

| Field            | Type   | Required | Description                           |
|------------------|--------|----------|---------------------------------------|
| flow_id          | string | Yes      | The ID of the flow to execute.        |
| flow_request_id  | string | Yes      | The ID of the associated flow request.|

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Generate Flow from Flow Request

```shell
curl -X GET "api.neena.io/flows/generate?flow_request_id=uuid-of-flow-request" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'flow_request_id': 'uuid-of-flow-request'
}

response = requests.get('api.neena.io/flows/generate', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flows/generate?flow_request_id=uuid-of-flow-request';

fetch(url, {
  headers: {
    'api_key': 'YOUR_API_KEY',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint generates a new flow based on an existing flow request.

### HTTP Request

`GET /flows/generate`

### Headers

| Field    | Type   | Required | Description           |
|----------|--------|----------|-----------------------|
| api_key  | string | Yes      | API key `YOUR_API_KEY`|

### Query Parameters

| Field            | Type   | Required | Description                           |
|------------------|--------|----------|---------------------------------------|
| flow_request_id  | string | Yes      | The ID of the flow request.           |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |
