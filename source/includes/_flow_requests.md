# Flow Requests

Flow requests are the starting point to get work done. It is the prompt to get your assistant working. 

Based on the flow request your Neena assistant will plan the steps to fulfil your request (i.e., it will generate a Flow) which it can then execute in a Flow Run.

## Flow Request Model

| Field                  | Type               | Required | Description                                      |
|------------------------|--------------------|----------|--------------------------------------------------|
| **id**                 | string (UUID)      | Yes      | Unique identifier of the flow request.           |
| **request_instructions** | string           | Yes      | Instructions or details about the flow request.  |
| **request_metadata**   | array of objects   | No       | Additional metadata or context for the request.  |
| **request_name**       | string             | No       | Optional name of the flow request.               |
| **flow**               | string (UUID)      | No       | Associated flow ID, if applicable.               |
| **organization**       | string (UUID)      | No       | Organization ID linked to the flow request.      |
| **created_date**       | string (date-time) | Yes      | Timestamp when the flow request was created.     |
| **modified_date**      | string (date-time) | Yes      | Timestamp when the flow request was last modified. |
| **created_by_email**   | string             | Yes      | Email of the user who created the flow request.  |
| **modified_by_email**  | string             | Yes      | Email of the user who last modified the request. |

## Create Flow Request

```shell
curl -X POST "api.neena.io/flow_requests" \
  -H "api_key: YOUR_API_KEY" \
  -d '{
        "request_instructions": "Process this request.",
        "request_metadata": [{"key": "value"}],
        "request_name": "Sample Request",
        "flow": "uuid-of-flow",
        "organization": "uuid-of-organization"
      }'
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

data = {
    "request_instructions": "Process this request.",
    "request_metadata": [{"key": "value"}],
    "request_name": "Sample Request",
    "flow": "uuid-of-flow",
    "organization": "uuid-of-organization"
}

response = requests.post('api.neena.io/flow_requests', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_requests';

const headers = {
  'api_key': 'YOUR_API_KEY',
};

const body = {
  request_instructions: 'Process this request.',
  request_metadata: [{"key": "value"}],
  request_name: 'Sample Request',
  flow: 'uuid-of-flow',
  organization: 'uuid-of-organization',
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
  "request_metadata": [{"key": "value"}],
  "request_instructions": "string",
  "request_name": "string",
  "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "created_date": "2024-09-17T15:35:21.360Z",
  "modified_date": "2024-09-17T15:35:21.360Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint creates a new flow request in the system.

### HTTP Request

`POST /flow_requests`

### Headers

| Field     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| api_key | string | Yes      | API key `YOUR_API_KEY` |

### Request Body

| Field                | Type             | Required | Description                        |
|----------------------|------------------|----------|------------------------------------|
| request_instructions | string           | Yes      | Instructions for the flow request. |
| request_metadata     | array of objects | No       | Additional metadata.               |
| request_name         | string           | No       | Name of the flow request.          |
| flow                 | string (UUID)    | No       | ID of the associated flow.         |
| organization         | string (UUID)    | No       | ID of the organization.            |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 422    | Validation Error    |

---

## Update Flow Request

```shell
curl -X PUT "api.neena.io/flow_requests" \
  -H "api_key: YOUR_API_KEY" \
  -d '{
        "request_metadata": [{"key": "new_value"}],
        "request_instructions": "string",
        "request_name": "string",
        "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
      }'
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

data = {
    "request_metadata": [{"key": "new_value"}],
    "request_instructions": "string",
    "request_name": "string",
    "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}

response = requests.put('api.neena.io/flow_requests', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_requests';

const headers = {
  'api_key': 'YOUR_API_KEY',
  
};

const body = {
  id: 'uuid-of-flow-request',
  request_instructions: 'Updated instructions.',
  request_metadata: [{"key": "new_value"}],
  request_name: 'Updated Request',
  flow: 'uuid-of-new-flow',
  organization: 'uuid-of-organization',
};

fetch(url, {
  method: 'PUT',
  headers: headers,
  body: JSON.stringify(body),
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> The above command returns JSON structured like this:

```json
{
  "request_metadata": [{"key": "new_value"}],
  "request_instructions": "string",
  "request_name": "string",
  "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "created_date": "2024-09-17T15:35:21.360Z",
  "modified_date": "2024-09-17T15:35:21.360Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint updates an existing flow request.

### HTTP Request

`PUT /flow_requests/`

### Headers

| Field     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| api_key | string | Yes      | API key `YOUR_API_KEY` |

### Request Body

| Field                | Type             | Required | Description                 |
|----------------------|------------------|----------|-----------------------------|
| id                   | string (UUID)    | Yes      | ID of the flow request.     |
| request_instructions | string           | Yes      | Updated instructions.       |
| request_metadata     | array of objects | No       | Updated metadata.           |
| request_name         | string           | No       | Updated name.               |
| flow                 | string (UUID)    | No       | Updated flow ID.            |
| organization         | string (UUID)    | No       | Updated organization ID.    |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read Flow Request

```shell
curl -X GET "api.neena.io/flow_requests/?id=uuid-of-flow-request" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-flow-request'
}

response = requests.get('api.neena.io/flow_requests', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_requests/?id=uuid-of-flow-request';

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
  "request_metadata": [{"key": "new_value"}],
  "request_instructions": "string",
  "request_name": "string",
  "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "created_date": "2024-09-17T15:35:21.360Z",
  "modified_date": "2024-09-17T15:35:21.360Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint retrieves a single flow request by its ID.

### HTTP Request

`GET /flow_requests`


### Headers

| Field     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| api_key | string | Yes      | API key `YOUR_API_KEY` |

### Query Parameters

| Field | Type   | Required | Description                 |
|-----------|--------|----------|-----------------------------|
| id        | string | Yes      | The ID of the flow request. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |


---

## Remove Flow Request

```shell
curl -X DELETE "api.neena.io/flow_requests/?id=uuid-of-flow-request" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'id': 'uuid-of-flow-request'
}

response = requests.delete('api.neena.io/flow_requests', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_requests/?id=uuid-of-flow-request';

fetch(url, {
  method: 'DELETE',
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
  "request_metadata": [{"key": "new_value"}],
  "request_instructions": "string",
  "request_name": "string",
  "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "created_date": "2024-09-17T15:35:21.360Z",
  "modified_date": "2024-09-17T15:35:21.360Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint deletes a flow request by its ID.

### HTTP Request

`DELETE /flow_requests/`

### Headers

| Field     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| api_key | string | Yes      | API key `YOUR_API_KEY` |

### Query Parameters

| Field | Type   | Required | Description                 |
|-----------|--------|----------|-----------------------------|
| id        | string | Yes      | The ID of the flow request. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---

## Read All Flow Requests

```shell
curl -X GET "api.neena.io/flow_requests/all?skip=0&limit=100" \
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

response = requests.get('api.neena.io/flow_requests/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/flow_requests/all?skip=0&limit=100';

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
    "request_metadata": [{"key": "new_value"}],
    "request_instructions": "string",
    "request_name": "string",
    "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "created_date": "2024-09-17T15:35:21.360Z",
    "modified_date": "2024-09-17T15:35:21.360Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com"
  },
  {
    "request_metadata": [{"key": "new_value"}],
    "request_instructions": "string",
    "request_name": "string",
    "flow": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "organization": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "created_date": "2024-09-17T15:35:21.360Z",
    "modified_date": "2024-09-17T15:35:21.360Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com"
  },
]
```

This endpoint retrieves all flow requests for the user.

### HTTP Request

`GET /flow_requests/all`

### Headers


| Field     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| api_key | string | Yes      | API key `YOUR_API_KEY` |

### Query Parameters

| Field | Type | Required | Description                                  |
|-----------|------|----------|----------------------------------------------|
| skip      | int  | No       | Number of records to skip for pagination.    |
| limit     | int  | No       | Maximum number of records to return.         |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 422    | Validation Error    |

---
