---
title: API Reference

language_tabs:
  - shell
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://neena.io'>Visit our Landing page</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Neena API
---

# Introduction

Welcome to the Neena API! With the Neena platform API, you can send requests for your Neena agents to automate. Our API provides easy access to Neena’s features, enabling seamless integration into your existing systems.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can pass the correct header with each request
curl "https://api.neena.io/api/endpoint" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

response = requests.get('https://api.neena.io/api/endpoint', headers=headers)
```

```javascript
fetch('https://api.neena.io/api/endpoint', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> Make sure to replace `YOUR_ACCESS_TOKEN` with your API key.

The Neena API uses bearer tokens for authentication. You must include the `Authorization` header with the value `Bearer YOUR_ACCESS_TOKEN` in all API requests.

<aside class="notice">
You must replace <code>YOUR_ACCESS_TOKEN</code> with your personal API key.
</aside>

# Users

## Get Current User

```shell
curl -X GET "https://api.neena.io/api/v1/users/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

response = requests.get('https://api.neena.io/api/v1/users/', headers=headers)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/users/', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> The above command returns JSON structured like this:

```json
{
  "email": "user@example.com",
  "full_name": "John Doe",
  "auth0_id": "auth0|123456789",
  "id": "uuid-of-the-user",
  "permissions": ["read", "write"]
}
```

This endpoint retrieves the current authenticated user's information.

### HTTP Request

`GET /api/v1/users/`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |

---

## Create User If Not Exists

```shell
curl -X POST "https://api.neena.io/api/v1/users/create_if_not_exists" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "email": "user@example.com",
        "full_name": "John Doe",
        "auth0_id": "auth0|123456789"
      }'
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
}

data = {
    "email": "user@example.com",
    "full_name": "John Doe",
    "auth0_id": "auth0|123456789"
}

response = requests.post('https://api.neena.io/api/v1/users/create_if_not_exists', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/users/create_if_not_exists';

const headers = {
  'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  'Content-Type': 'application/json',
};

const body = {
  email: 'user@example.com',
  full_name: 'John Doe',
  auth0_id: 'auth0|123456789',
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
  "email": "user@example.com",
  "full_name": "John Doe",
  "auth0_id": "auth0|123456789",
  "id": "uuid-of-the-user",
  "permissions": null
}
```

This endpoint creates a new user in the database if it does not exist. You can only create a user with the same email as the authenticated user.

### HTTP Request

`POST /api/v1/users/create_if_not_exists`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |
| Content-Type  | string | Yes      | Should be `application/json`                       |

### Request Body

| Field     | Type   | Required | Description                |
|-----------|--------|----------|----------------------------|
| email     | string | Yes      | The email of the user.     |
| full_name | string | No       | The full name of the user. |
| auth0_id  | string | Yes      | The Auth0 ID of the user.  |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

<aside class="notice">
Ensure that the email provided matches the email of the authenticated user.
</aside>

---

# Flow Requests

## Create Flow Request

```shell
curl -X POST "https://api.neena.io/api/v1/flow_requests/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
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
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
}

data = {
    "request_instructions": "Process this request.",
    "request_metadata": [{"key": "value"}],
    "request_name": "Sample Request",
    "flow": "uuid-of-flow",
    "organization": "uuid-of-organization"
}

response = requests.post('https://api.neena.io/api/v1/flow_requests/', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/flow_requests/';

const headers = {
  'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  'Content-Type': 'application/json',
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
  "request_instructions": "Process this request.",
  "id": "uuid-of-flow-request",
  "created_date": "2023-10-15T12:34:56.789Z",
  "modified_date": "2023-10-15T12:34:56.789Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint creates a new flow request in the system.

### HTTP Request

`POST /api/v1/flow_requests/`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |
| Content-Type  | string | Yes      | Should be `application/json`                       |

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
| 422    | Validation Error    |

---

## Update Flow Request

```shell
curl -X PUT "https://api.neena.io/api/v1/flow_requests/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "id": "uuid-of-flow-request",
        "request_instructions": "Updated instructions.",
        "request_metadata": [{"key": "new_value"}],
        "request_name": "Updated Request",
        "flow": "uuid-of-new-flow",
        "organization": "uuid-of-organization"
      }'
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
}

data = {
    "id": "uuid-of-flow-request",
    "request_instructions": "Updated instructions.",
    "request_metadata": [{"key": "new_value"}],
    "request_name": "Updated Request",
    "flow": "uuid-of-new-flow",
    "organization": "uuid-of-organization"
}

response = requests.put('https://api.neena.io/api/v1/flow_requests/', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/flow_requests/';

const headers = {
  'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  'Content-Type': 'application/json',
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
  "request_instructions": "Updated instructions.",
  "id": "uuid-of-flow-request",
  "created_date": "2023-10-15T12:34:56.789Z",
  "modified_date": "2023-10-16T14:20:00.000Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint updates an existing flow request.

### HTTP Request

`PUT /api/v1/flow_requests/`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |
| Content-Type  | string | Yes      | Should be `application/json`                       |

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
| 422    | Validation Error    |

---

## Read Flow Request

```shell
curl -X GET "https://api.neena.io/api/v1/flow_requests/?id=uuid-of-flow-request" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-flow-request'
}

response = requests.get('https://api.neena.io/api/v1/flow_requests/', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/flow_requests/?id=uuid-of-flow-request';

fetch(url, {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> The above command returns JSON structured like this:

```json
{
  "request_instructions": "Updated instructions.",
  "id": "uuid-of-flow-request",
  "created_date": "2023-10-15T12:34:56.789Z",
  "modified_date": "2023-10-16T14:20:00.000Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint retrieves a single flow request by its ID.

### HTTP Request

`GET /api/v1/flow_requests/`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type   | Required | Description                 |
|-----------|--------|----------|-----------------------------|
| id        | string | Yes      | The ID of the flow request. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Remove Flow Request

```shell
curl -X DELETE "https://api.neena.io/api/v1/flow_requests/?id=uuid-of-flow-request" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-flow-request'
}

response = requests.delete('https://api.neena.io/api/v1/flow_requests/', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/flow_requests/?id=uuid-of-flow-request';

fetch(url, {
  method: 'DELETE',
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

> The above command returns JSON structured like this:

```json
{
  "request_instructions": "Updated instructions.",
  "id": "uuid-of-flow-request",
  "created_date": "2023-10-15T12:34:56.789Z",
  "modified_date": "2023-10-16T14:20:00.000Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint deletes a flow request by its ID.

### HTTP Request

`DELETE /api/v1/flow_requests/`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type   | Required | Description                 |
|-----------|--------|----------|-----------------------------|
| id        | string | Yes      | The ID of the flow request. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Read All Flow Requests

```shell
curl -X GET "https://api.neena.io/api/v1/flow_requests/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100
}

response = requests.get('https://api.neena.io/api/v1/flow_requests/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/flow_requests/all?skip=0&limit=100';

fetch(url, {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint retrieves all flow requests from the database.

### HTTP Request

`GET /api/v1/flow_requests/all`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type | Required | Description                                  |
|-----------|------|----------|----------------------------------------------|
| skip      | int  | No       | Number of records to skip for pagination.    |
| limit     | int  | No       | Maximum number of records to return.         |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

# Task Definitions

## Read All Task Definitions

```shell
curl -X GET "https://api.neena.io/api/v1/task_definitions/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100
}

response = requests.get('https://api.neena.io/api/v1/task_definitions/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/task_definitions/all?skip=0&limit=100';

fetch(url, {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint retrieves a list of all task definitions, with pagination.

### HTTP Request

`GET /api/v1/task_definitions/all`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type | Required | Description                               |
|-----------|------|----------|-------------------------------------------|
| skip      | int  | No       | Number of records to skip for pagination. |
| limit     | int  | No       | Maximum number of records to return.      |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Read Task Definition

```shell
curl -X GET "https://api.neena.io/api/v1/task_definitions/?id=uuid-of-task-definition" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-task-definition'
}

response = requests.get('https://api.neena.io/api/v1/task_definitions/', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/task_definitions/?id=uuid-of-task-definition';

fetch(url, {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint retrieves a specific task definition by its unique ID.

### HTTP Request

`GET /api/v1/task_definitions/`

### Headers

| Parameter     | Type   | Required | Description                                        |
|---------------|--------|----------|----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type   | Required | Description                     |
|-----------|--------|----------|---------------------------------|
| id        | string | Yes      | The unique ID of the task definition. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |



# Flows

## Create Flow

```shell
curl -X POST "https://api.neena.io/api/v1/flows/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "name": "Sample Flow",
        "task_operations": [...],
        "dependencies": [...]
      }'
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
}

data = {
    "name": "Sample Flow",
    "task_operations": [...],
    "dependencies": [...]
}

response = requests.post('https://api.neena.io/api/v1/flows/', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/flows/';

const headers = {
  'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  'Content-Type': 'application/json',
};

const body = {
  name: 'Sample Flow',
  task_operations: [...],
  dependencies: [...],
};

fetch(url, {
  method: 'POST',
  headers: headers,
  body: JSON.stringify(body),
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint creates a new flow with the specified details.

### HTTP Request

`POST /api/v1/flows/`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |
| Content-Type  | string | Yes      | Should be `application/json`                        |

### Request Body

| Field           | Type             | Required | Description                        |
|-----------------|------------------|----------|------------------------------------|
| name            | string           | Yes      | Name of the flow.                  |
| task_operations | array of objects | Yes      | List of task operations in the flow. |
| dependencies    | array of objects | Yes      | List of dependencies between tasks. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Update Flow

```shell
curl -X PUT "https://api.neena.io/api/v1/flows/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "id": "uuid-of-flow",
        "name": "Updated Flow Name",
        "task_operations": [...],
        "dependencies": [...]
      }'
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
}

data = {
    "id": "uuid-of-flow",
    "name": "Updated Flow Name",
    "task_operations": [...],
    "dependencies": [...]
}

response = requests.put('https://api.neena.io/api/v1/flows/', headers=headers, json=data)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'https://api.neena.io/api/v1/flows/';

const headers = {
  'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  'Content-Type': 'application/json',
};

const body = {
  id: 'uuid-of-flow',
  name: 'Updated Flow Name',
  task_operations: [...],
  dependencies: [...],
};

fetch(url, {
  method: 'PUT',
  headers: headers,
  body: JSON.stringify(body),
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint updates an existing flow with new details.

### HTTP Request

`PUT /api/v1/flows/`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |
| Content-Type  | string | Yes      | Should be `application/json`                        |

### Request Body

| Field           | Type             | Required | Description                        |
|-----------------|------------------|----------|------------------------------------|
| id              | string (UUID)    | Yes      | Unique identifier of the flow.     |
| name            | string           | Yes      | New name for the flow.             |
| task_operations | array of objects | Yes      | Updated list of task operations.   |
| dependencies    | array of objects | Yes      | Updated list of dependencies.      |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Read Flow

```shell
curl -X GET "https://api.neena.io/api/v1/flows/?id=uuid-of-flow" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-flow'
}

response = requests.get('https://api.neena.io/api/v1/flows/', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/flows/?id=uuid-of-flow', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint retrieves a single flow by its ID.

### HTTP Request

`GET /api/v1/flows/`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type   | Required | Description                    |
|-----------|--------|----------|--------------------------------|
| id        | string | Yes      | Unique identifier of the flow. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Remove Flow

```shell
curl -X DELETE "https://api.neena.io/api/v1/flows/?id=uuid-of-flow" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-flow'
}

response = requests.delete('https://api.neena.io/api/v1/flows/', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/flows/?id=uuid-of-flow', {
  method: 'DELETE',
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint deletes a flow by its ID.

### HTTP Request

`DELETE /api/v1/flows/`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type   | Required | Description                    |
|-----------|--------|----------|--------------------------------|
| id        | string | Yes      | Unique identifier of the flow. |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Read All Flows

```shell
curl -X GET "https://api.neena.io/api/v1/flows/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100
}

response = requests.get('https://api.neena.io/api/v1/flows/all', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/flows/all?skip=0&limit=100', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint retrieves a list of flows, optionally paginated.

### HTTP Request

`GET /api/v1/flows/all`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type | Required | Description                               |
|-----------|------|----------|-------------------------------------------|
| skip      | int  | No       | Number of items to skip (default is 0).   |
| limit     | int  | No       | Maximum number of items to return (default is 100). |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Generate Flow

```shell
curl -X GET "https://api.neena.io/api/v1/flows/generate?flow_request_id=uuid-of-flow-request" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'flow_request_id': 'uuid-of-flow-request'
}

response = requests.get('https://api.neena.io/api/v1/flows/generate', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/flows/generate?flow_request_id=uuid-of-flow-request', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint generates a flow from an existing Flow Request ID.

### HTTP Request

`GET /api/v1/flows/generate`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter       | Type   | Required | Description                           |
|-----------------|--------|----------|---------------------------------------|
| flow_request_id | string | Yes      | Flow Request ID to generate flow from.|

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

## Execute Flow

```shell
curl -X POST "https://api.neena.io/api/v1/flows/execute?flow_id=uuid-of-flow&flow_request_id=uuid-of-flow-request" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'flow_id': 'uuid-of-flow',
    'flow_request_id': 'uuid-of-flow-request'
}

response = requests.post('https://api.neena.io/api/v1/flows/execute', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/flows/execute?flow_id=uuid-of-flow&flow_request_id=uuid-of-flow-request', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint executes a flow using the specified Flow ID and Flow Request ID.

### HTTP Request

`POST /api/v1/flows/execute`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter       | Type   | Required | Description                     |
|-----------------|--------|----------|---------------------------------|
| flow_id         | string | Yes      | Flow ID to execute.             |
| flow_request_id | string | Yes      | Flow Request ID to execute.     |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

# Flow Runs

## Read All Flow Runs

```shell
curl -X GET "https://api.neena.io/api/v1/flow_runs/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100
}

response = requests.get('https://api.neena.io/api/v1/flow_runs/all', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/flow_runs/all?skip=0&limit=100', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint retrieves all flow runs from the system.

### HTTP Request

`GET /api/v1/flow_runs/all`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter | Type | Required | Description                               |
|-----------|------|----------|-------------------------------------------|
| skip      | int  | No       | Number of records to skip for pagination. |
| limit     | int  | No       | Maximum number of records to return.      |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

[Additional endpoints continue in the same format, including the following sections:

- Read Flow Run
- Read Flow Run With Flow Result
- Read All Flow Runs For Flow ID
- Approve Flow Run
- Read Flow Runs (Preview of All Flow Runs)
- Integrations
  - Read All Integrations
- Integration Credentials
  - Create or Update Integration Credential
  - Remove Integration Credential
  - Read All Integration Credentials
  - Read OAuth2 Authorize URL
  - Send OAuth2 Code
- Webhook Trigger Definitions
  - Read All Webhook Trigger Definitions
  - Read Webhook Trigger Definition
- Webhook Trigger Operations
  - Read All Webhook Trigger Operations
  - Read Webhook Trigger Operation
  - Create Webhook Trigger Operation
  - Remove Webhook Trigger Operation
- Flow Results
  - Read Latest Flow Result For Integration Entity

Each endpoint includes code samples in shell, Python, and JavaScript, along with detailed documentation of HTTP requests, headers, parameters, and responses.]

---

# Flow Results

## Read Latest Flow Result For Integration Entity

```shell
curl -X GET "https://api.neena.io/api/v1/flow_results/?integration_entity_id=entity-id&integration_short_name=integration_short_name" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'integration_entity_id': 'entity-id',
    'integration_short_name': 'integration_short_name'
}

response = requests.get('https://api.neena.io/api/v1/flow_results/', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/flow_results/?integration_entity_id=entity-id&integration_short_name=integration_short_name', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  },
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint retrieves the latest flow result for a specific integration entity.

### HTTP Request

`GET /api/v1/flow_results/`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |

### Query Parameters

| Parameter              | Type   | Required | Description                                |
|------------------------|--------|----------|--------------------------------------------|
| integration_entity_id  | string | Yes      | The integration entity ID (e.g., ticket ID)|
| integration_short_name | string | Yes      | The integration's short name               |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 404    | Flow Result Not Found |

---

# Webhook Trigger Operations

## Create Webhook Trigger Operation

```shell
curl -X POST "https://api.neena.io/api/v1/webhook_trigger_operations/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "name": "Sample Webhook Operation",
        "webhook_trigger_definition": "uuid-of-trigger-definition",
        "template_body": "Template content",
        "status": "active"
      }'
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
}

data = {
    "name": "Sample Webhook Operation",
    "webhook_trigger_definition": "uuid-of-trigger-definition",
    "template_body": "Template content",
    "status": "active"
}

response = requests.post('https://api.neena.io/api/v1/webhook_trigger_operations/', headers=headers, json=data)
print(response.json())
```

```javascript
fetch('https://api.neena.io/api/v1/webhook_trigger_operations/', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'Sample Webhook Operation',
    webhook_trigger_definition: 'uuid-of-trigger-definition',
    template_body: 'Template content',
    status: 'active',
  }),
})
  .then(response => response.json())
  .then(data => console.log(data));
```

This endpoint creates a new webhook trigger operation.

### HTTP Request

`POST /api/v1/webhook_trigger_operations/`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN` |
| Content-Type  | string | Yes      | Should be `application/json`                        |

### Request Body

| Field                      | Type          | Required | Description                             |
|----------------------------|---------------|----------|-----------------------------------------|
| name                       | string        | Yes      | Name of the webhook operation.          |
| webhook_trigger_definition | string (UUID) | Yes      | ID of the webhook trigger definition.   |
| template_body              | string        | Yes      | Template content for the operation.     |
| status                     | string        | No       | Status of the operation (default: "inactive"). |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 422    | Validation Error    |

---

[Continue adding the remaining endpoints in the same format.]

# Shortcuts

Please ensure to replace placeholders like `YOUR_ACCESS_TOKEN`, `uuid-of-flow`, `integration_short_name`, etc., with your actual data when making API requests.
