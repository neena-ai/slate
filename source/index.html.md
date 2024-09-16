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
