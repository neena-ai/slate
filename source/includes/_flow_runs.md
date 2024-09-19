# Flow Runs
Flow runs represent the execution of a [Flow](https://docs.neena.io/#flows) that was generated from a [Flow Request](https://docs.neena.io/#flow-requests). Once the flow is created, a flow run performs the steps defined in the flow, transitioning through stages such as pending, in-progress, and completed.

Flow runs are essential for tracking the real-time progress of the workflow, providing key details about the execution of tasks within the flow, such as status updates, when the run was triggered, and task outcomes. 

## Flow Run Model

| Field           | Type             | Required | Description                          |
|-----------------|------------------|----------|--------------------------------------|
| **flow**            | string (UUID)    | Yes      | ID of the associated flow.           |
| **id**              | string (UUID)    | Yes      | ID of the flow run.                  |
| **flow_request**    | string (UUID)    | Yes      | ID of the associated flow request.   |
| **status**          | string           | Yes      | Status of the flow run.              |
| **approval_status** | string           | No       | Approval status of the flow run.     |
| **triggered_time**  | string (date-time)| No      | Time when the flow run was triggered.|
| **end_time**        | string (date-time)| No      | Time when the flow run ended.        |
| **triggered_by**    | string           | No       | Who triggered the flow run.          |
| **task_runs**       | array of objects | Yes      | List of task runs.                   |

## Task Run Model

| Field                | Type               | Description                                                                                  |
|----------------------|--------------------|----------------------------------------------------------------------------------------------|
| **id**               | string (UUID)      | Unique identifier of the task run.                                                           |
| **task_operation_index** | integer        | Index of the task operation within the flow.                                                 |
| **status**           | string             | Status of the task run (e.g., 'pending', 'in_progress', 'completed', 'failed', 'cancelled'). |
| **start_time**       | string (date-time) | ISO 8601 formatted date-time when the task run started.                                      |
| **end_time**         | string (date-time) | ISO 8601 formatted date-time when the task run ended.                                        |
| **flow_run**         | string (UUID)      | Unique identifier of the associated flow run.                                                |
| **task_prep_prompt** | object             | Messages exchanged during task preparation.                                                  |
| **task_prep_answer** | object             | Parameters provided for the task.                                                            |



## Create Flow Run

```shell
curl -X POST "api.neena.io/flow_runs" \
  -H "api_key: YOUR_API_KEY" \
  -d '{
        "flow": "uuid-of-flow",
        "flow_request": "uuid-of-flow-request",
        "status": "pending",
        "approval_status": null,
        "triggered_time": null,
        "end_time": null,
        "triggered_by": null
      }'
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

data = {
    "flow": "uuid-of-flow",
    "flow_request": "uuid-of-flow-request",
    "status": "pending",
    "approval_status": None,
    "triggered_time": None,
    "end_time": None,
    "triggered_by": None
}

response = requests.post('api.neena.io/flow_runs', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_runs';

const headers = {
  'api_key': 'YOUR_API_KEY',
};

const body = {
  flow: 'uuid-of-flow',
  flow_request: 'uuid-of-flow-request',
  status: 'pending',
  approval_status: null,
  triggered_time: null,
  end_time: null,
  triggered_by: null
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
  "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "flow_request": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "status": "pending",
  "approval_status": null,
  "triggered_time": null,
  "end_time": null,
  "triggered_by": null,
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "task_runs": []
}
```

This endpoint creates a new flow run in the system.

### HTTP Request

`POST /flow_runs`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Request Body

| Field           | Type             | Required | Description                         |
|-----------------|------------------|----------|-------------------------------------|
| flow            | string (UUID)    | Yes      | ID of the associated flow.          |
| flow_request    | string (UUID)    | Yes      | ID of the associated flow request.  |
| status          | string           | Yes      | Status of the flow run (e.g., 'pending'). |
| approval_status | string           | No       | Approval status of the flow run.    |
| triggered_time  | string (date-time)| No      | Time when the flow run was triggered.|
| end_time        | string (date-time)| No      | Time when the flow run ended.        |
| triggered_by    | string           | No       | Who triggered the flow run.         |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read Flow Run

```shell
curl -X GET "api.neena.io/flow_runs/?id=uuid-of-flow-run" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-flow-run'
}

response = requests.get('api.neena.io/flow_runs', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_runs/?id=uuid-of-flow-run';

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
  "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "flow_request": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "status": "pending",
  "approval_status": null,
  "triggered_time": null,
  "end_time": null,
  "triggered_by": null,
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "task_runs": [
    {
      "task_operation_index": 0,
      "status": "completed",
      "start_time": "2024-09-18T10:00:00Z",
      "task_prep_prompt": {
        "messages": [
          {
            "role": "system",
            "content": "Please provide the necessary input parameters."
          },
          {
            "role": "user",
            "content": "What is the target email address?"
          }
        ]
      },
      "task_prep_answer": {
        "parameters": [
          {
            "name": "email_address",
            "value": "user@example.com",
            "explanation": "The email address to which the report will be sent."
          },
          {
            "name": "include_summary",
            "value": true,
            "explanation": "Indicates whether to include a summary in the report."
          }
        ]
      },
      "end_time": "2024-09-18T10:05:00Z",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
    },
    {
      "task_operation_index": 0,
      "status": "completed",
      "start_time": "2024-09-18T10:00:00Z",
      "task_prep_prompt": {
        "messages": [
          {
            "role": "system",
            "content": "Please provide the necessary input parameters."
          },
          {
            "role": "user",
            "content": "What is the target email address?"
          }
        ]
      },
      "task_prep_answer": {
        "parameters": [
          {
            "name": "email_address",
            "value": "user@example.com",
            "explanation": "The email address to which the report will be sent."
          },
          {
            "name": "include_summary",
            "value": true,
            "explanation": "Indicates whether to include a summary in the report."
          }
        ]
      },
      "end_time": "2024-09-18T10:05:00Z",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
    }
  ]
}
```

This endpoint retrieves a single flow run by its ID.

### HTTP Request

`GET /flow_runs`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type   | Required | Description                 |
|-------|--------|----------|-----------------------------|
| id    | string | Yes      | The ID of the flow run.     |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read All Flow Runs

```shell
curl -X GET "api.neena.io/flow_runs/all?skip=0&limit=100" \
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

response = requests.get('api.neena.io/flow_runs/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_runs/all?skip=0&limit=100';

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
    "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "flow_request": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "status": "pending",
    "approval_status": null,
    "triggered_time": null,
    "end_time": null,
    "triggered_by": null,
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "task_runs": [
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      },
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      }
    ]
  },
  {
    "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "flow_request": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "status": "pending",
    "approval_status": null,
    "triggered_time": null,
    "end_time": null,
    "triggered_by": null,
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "task_runs": [
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      },
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      }
    ]
  },
]
```

This endpoint retrieves all flow runs for the user.

### HTTP Request

`GET /flow_runs/all`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type | Required | Description                                  |
|-------|------|----------|----------------------------------------------|
| skip  | int  | No       | Number of records to skip for pagination.    |
| limit | int  | No       | Maximum number of records to return.         |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 422    | Validation Error    |

---

## Approve Flow Run

```shell
curl -X PUT "api.neena.io/flow_runs/approve?flow_run_id=uuid-of-flow-run" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'flow_run_id': 'uuid-of-flow-run'
}

response = requests.put('api.neena.io/flow_runs/approve', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_runs/approve?flow_run_id=uuid-of-flow-run';

fetch(url, {
  method: 'PUT',
  headers: {
    'api_key': 'YOUR_API_KEY',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint approves a specific flow run.

### HTTP Request

`PUT /flow_runs/approve`

### Headers

| Field       | Type   | Required | Description                        |
|-------------|--------|----------|------------------------------------|
| api_key     | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field       | Type   | Required | Description                     |
|-------------|--------|----------|---------------------------------|
| flow_run_id | string | Yes      | The ID of the flow run to approve.|

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read All Flow Runs for a Flow ID

```shell
curl -X GET "api.neena.io/flow_runs/all_for_flow_id?flow_id=uuid-of-flow&skip=0&limit=100" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'flow_id': 'uuid-of-flow',
    'skip': 0,
    'limit': 100
}

response = requests.get('api.neena.io/flow_runs/all_for_flow_id', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_runs/all_for_flow_id?flow_id=uuid-of-flow&skip=0&limit=100';

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
    "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "flow_request": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "status": "pending",
    "approval_status": null,
    "triggered_time": null,
    "end_time": null,
    "triggered_by": null,
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "task_runs": [
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      },
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      }
    ]
  },
  {
    "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "flow_request": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "status": "pending",
    "approval_status": null,
    "triggered_time": null,
    "end_time": null,
    "triggered_by": null,
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "task_runs": [
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      },
      {
        "task_operation_index": 0,
        "status": "completed",
        "start_time": "2024-09-18T10:00:00Z",
        "task_prep_prompt": {
          "messages": [
            {
              "role": "system",
              "content": "Please provide the necessary input parameters."
            },
            {
              "role": "user",
              "content": "What is the target email address?"
            }
          ]
        },
        "task_prep_answer": {
          "parameters": [
            {
              "name": "email_address",
              "value": "user@example.com",
              "explanation": "The email address to which the report will be sent."
            },
            {
              "name": "include_summary",
              "value": true,
              "explanation": "Indicates whether to include a summary in the report."
            }
          ]
        },
        "end_time": "2024-09-18T10:05:00Z",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
      }
    ]
  },
]
```

This endpoint retrieves all flow runs associated with a specific flow ID.

### HTTP Request

`GET /flow_runs/all_for_flow_id`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field    | Type   | Required | Description                                  |
|----------|--------|----------|----------------------------------------------|
| flow_id  | string | Yes      | The ID of the flow.                          |
| skip     | int    | No       | Number of records to skip for pagination.    |
| limit    | int    | No       | Maximum number of records to return.         |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read Flow Run with Flow Result

```shell
curl -X GET "api.neena.io/flow_runs/with_flow_result?id=uuid-of-flow-run" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-flow-run'
}

response = requests.get('api.neena.io/flow_runs/with_flow_result', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_runs/with_flow_result?id=uuid-of-flow-run';

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
  "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "flow_request": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "status": "completed",
  "approval_status": null,
  "triggered_time": null,
  "end_time": null,
  "triggered_by": null,
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "task_runs": [
    {
      "task_operation_index": 0,
      "status": "completed",
      "start_time": "2024-09-18T10:00:00Z",
      "task_prep_prompt": {
        "messages": [
          {
            "role": "system",
            "content": "Please provide the necessary input parameters."
          },
          {
            "role": "user",
            "content": "What is the target email address?"
          }
        ]
      },
      "task_prep_answer": {
        "parameters": [
          {
            "name": "email_address",
            "value": "user@example.com",
            "explanation": "The email address to which the report will be sent."
          },
          {
            "name": "include_summary",
            "value": true,
            "explanation": "Indicates whether to include a summary in the report."
          }
        ]
      },
      "end_time": "2024-09-18T10:05:00Z",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
    },
    {
      "task_operation_index": 0,
      "status": "completed",
      "start_time": "2024-09-18T10:00:00Z",
      "task_prep_prompt": {
        "messages": [
          {
            "role": "system",
            "content": "Please provide the necessary input parameters."
          },
          {
            "role": "user",
            "content": "What is the target email address?"
          }
        ]
      },
      "task_prep_answer": {
        "parameters": [
          {
            "name": "email_address",
            "value": "user@example.com",
            "explanation": "The email address to which the report will be sent."
          },
          {
            "name": "include_summary",
            "value": true,
            "explanation": "Indicates whether to include a summary in the report."
          }
        ]
      },
      "end_time": "2024-09-18T10:05:00Z",
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "flow_run": "4ec2d0f1-1234-5678-9abc-def012345678"
    }
  ],
  "result": {
    "body": "string",
    "trigger_run_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "integration_entity_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "flow_run_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "organization_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "created_date": "2024-09-18T10:27:57.149Z",
    "modified_date": "2024-09-18T10:27:57.149Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com",
  }
},
```

This endpoint retrieves a specific flow run by its ID along with its flow result.

### HTTP Request

`GET /flow_runs/with_flow_result`

### Headers

| Field     | Type   | Required | Description                        |
|-----------|--------|----------|------------------------------------|
| api_key   | string | Yes      | API key `YOUR_API_KEY`             |

### Query Parameters

| Field | Type   | Required | Description                 |
|-------|--------|----------|-----------------------------|
| id    | string | Yes      | The ID of the flow run.     |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---