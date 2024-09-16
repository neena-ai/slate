---
title: API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
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
curl "https://your_api_base_url/api/endpoint" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

response = requests.get('https://your_api_base_url/api/endpoint', headers=headers)
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://your_api_base_url/api/endpoint")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: uri.scheme == "https") do |http|
  http.request(request)
end
```

```javascript
const fetch = require('node-fetch');

fetch('https://your_api_base_url/api/endpoint', {
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
curl -X GET "https://your_api_base_url/api/v1/users/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

response = requests.get('https://your_api_base_url/api/v1/users/', headers=headers)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://your_api_base_url/api/v1/users/")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: uri.scheme == "https") do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

fetch('https://your_api_base_url/api/v1/users/', {
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

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Responses

Status | Description
-------|-------------
200    | Successful Response

---

## Create User If Not Exists

```shell
curl -X POST "https://your_api_base_url/api/v1/users/create_if_not_exists" \
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

response = requests.post('https://your_api_base_url/api/v1/users/create_if_not_exists', headers=headers, json=data)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/users/create_if_not_exists")
request = Net::HTTP::Post.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"
request["Content-Type"] = "application/json"
request.body = {
  email: "user@example.com",
  full_name: "John Doe",
  auth0_id: "auth0|123456789"
}.to_json

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: uri.scheme == "https") do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/users/create_if_not_exists';

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

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`
Content-Type   | string | Yes      | Should be `application/json`

### Request Body

Field      | Type   | Required | Description
-----------|--------|----------|------------
email      | string | Yes      | The email of the user.
full_name  | string | No       | The full name of the user.
auth0_id   | string | Yes      | The Auth0 ID of the user.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

<aside class="notice">
Ensure that the email provided matches the email of the authenticated user.
</aside>

---

# Flow Requests

## Create Flow Request

```shell
curl -X POST "https://your_api_base_url/api/v1/flow_requests/" \
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

response = requests.post('https://your_api_base_url/api/v1/flow_requests/', headers=headers, json=data)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/flow_requests/")
request = Net::HTTP::Post.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"
request["Content-Type"] = "application/json"
request.body = {
  request_instructions: "Process this request.",
  request_metadata: [{"key": "value"}],
  request_name: "Sample Request",
  flow: "uuid-of-flow",
  organization: "uuid-of-organization"
}.to_json

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: uri.scheme == "https") do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_requests/';

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

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`
Content-Type   | string | Yes      | Should be `application/json`

### Request Body

Field                 | Type            | Required | Description
----------------------|-----------------|----------|------------
request_instructions  | string          | Yes      | Instructions for the flow request.
request_metadata      | array of object | No       | Additional metadata.
request_name          | string          | No       | Name of the flow request.
flow                  | string (UUID)   | No       | ID of the associated flow.
organization          | string (UUID)   | No       | ID of the organization.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

## Update Flow Request

```shell
curl -X PUT "https://your_api_base_url/api/v1/flow_requests/" \
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
# Similar to the previous example, use requests.put with the updated data
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

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`
Content-Type   | string | Yes      | Should be `application/json`

### Request Body

Field                 | Type            | Required | Description
----------------------|-----------------|----------|------------
id                    | string (UUID)   | Yes      | ID of the flow request to update.
request_instructions  | string          | Yes      | Updated instructions.
request_metadata      | array of object | No       | Updated metadata.
request_name          | string          | No       | Updated name.
flow                  | string (UUID)   | No       | Updated flow ID.
organization          | string (UUID)   | No       | Updated organization ID.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

## Read Flow Request

```shell
curl -X GET "https://your_api_base_url/api/v1/flow_requests/?id=uuid-of-flow-request" \
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

response = requests.get('https://your_api_base_url/api/v1/flow_requests/', headers=headers, params=params)
print(response.json())
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

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type   | Required | Description
----------|--------|----------|------------
id        | string | Yes      | The ID of the flow request to retrieve.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

## Delete Flow Request

```shell
curl -X DELETE "https://your_api_base_url/api/v1/flow_requests/?id=uuid-of-flow-request" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
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

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type   | Required | Description
----------|--------|----------|------------
id        | string | Yes      | The ID of the flow request to delete.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

# Flows

## Create Flow

```shell
curl -X POST "https://your_api_base_url/api/v1/flows/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "name": "Sample Flow",
        "task_operations": [...],
        "dependencies": [...]
      }'
```

> The above command returns JSON structured like this:

```json
{
  "name": "Sample Flow",
  "task_operations": [...],
  "dependencies": [...],
  "id": "uuid-of-flow",
  "created_date": "2023-10-15T12:34:56.789Z",
  "modified_date": "2023-10-15T12:34:56.789Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com"
}
```

This endpoint creates a new flow with the specified details.

### HTTP Request

`POST /api/v1/flows/`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`
Content-Type   | string | Yes      | Should be `application/json`

### Request Body

Field            | Type           | Required | Description
-----------------|----------------|----------|------------
name             | string         | Yes      | Name of the flow.
task_operations  | array of object| Yes      | List of task operations in the flow.
dependencies     | array of object| Yes      | List of dependencies between tasks.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

# Integrations

## Get All Integrations

```shell
curl -X GET "https://your_api_base_url/api/v1/integrations/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100,
}

response = requests.get('https://your_api_base_url/api/v1/integrations/all', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/integrations/all?skip=0&limit=100")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/integrations/all?skip=0&limit=100';

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
[
  {
    "class_name": "IntegrationClassName",
    "name": "Integration Name",
    "short_name": "integration_short_name",
    "auth_type": "oauth2",
    "id": "uuid-of-integration",
    "created_date": "2023-10-15T12:34:56.789Z",
    "modified_date": "2023-10-15T12:34:56.789Z"
  },
  {
    "class_name": "AnotherIntegrationClass",
    "name": "Another Integration",
    "short_name": "another_integration",
    "auth_type": "api_key",
    "id": "uuid-of-another-integration",
    "created_date": "2023-10-14T10:20:30.456Z",
    "modified_date": "2023-10-14T10:20:30.456Z"
  }
]
```

This endpoint retrieves all integrations from the database within the specified range.

### HTTP Request

`GET /api/v1/integrations/all`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type   | Required | Description
----------|--------|----------|------------
skip      | int    | No       | Number of records to skip for pagination (default is 0).
limit     | int    | No       | Maximum number of records to return (default is 100).

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

# Task Definitions

## Get All Task Definitions

```shell
curl -X GET "https://your_api_base_url/api/v1/task_definitions/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100,
}

response = requests.get('https://your_api_base_url/api/v1/task_definitions/all', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/task_definitions/all?skip=0&limit=100")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/task_definitions/all?skip=0&limit=100';

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
[
  {
    "task_name": "Sample Task",
    "integration": "uuid-of-integration",
    "parameters": [
      {
        "name": "param1",
        "data_type": "string",
        "position": 1,
        "doc_string": "First parameter",
        "optional": false
      }
    ],
    "input_type": "JSON",
    "input_yml": "input.yml content",
    "description": "A sample task definition.",
    "python_method_name": "sample_method",
    "output_type": "JSON",
    "output_yml": "output.yml content",
    "output_parameters": [
      {
        "name": "result",
        "data_type": "string",
        "position": 1,
        "doc_string": "Result of the task",
        "optional": false
      }
    ],
    "output_is_list": false,
    "id": "uuid-of-task-definition",
    "created_date": "2023-10-15T12:34:56.789Z",
    "modified_date": "2023-10-15T12:34:56.789Z",
    "created_by_email": "user@example.com",
    "modified_by_email": "user@example.com",
    "deleted_at": null,
    "belongs_to_integration": {
      "class_name": "IntegrationClassName",
      "name": "Integration Name",
      "short_name": "integration_short_name",
      "auth_type": "oauth2",
      "id": "uuid-of-integration",
      "created_date": "2023-10-15T12:34:56.789Z",
      "modified_date": "2023-10-15T12:34:56.789Z"
    }
  }
]
```

This endpoint retrieves a list of all task definitions, with pagination.

### HTTP Request

`GET /api/v1/task_definitions/all`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type   | Required | Description
----------|--------|----------|------------
skip      | int    | No       | Number of records to skip for pagination (default is 0).
limit     | int    | No       | Maximum number of records to return (default is 100).

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

## Get Task Definition by ID

```shell
curl -X GET "https://your_api_base_url/api/v1/task_definitions/?id=uuid-of-task-definition" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-task-definition',
}

response = requests.get('https://your_api_base_url/api/v1/task_definitions/', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/task_definitions/?id=uuid-of-task-definition")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/task_definitions/?id=uuid-of-task-definition';

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
  "task_name": "Sample Task",
  "integration": "uuid-of-integration",
  "parameters": [
    {
      "name": "param1",
      "data_type": "string",
      "position": 1,
      "doc_string": "First parameter",
      "optional": false
    }
  ],
  "input_type": "JSON",
  "input_yml": "input.yml content",
  "description": "A sample task definition.",
  "python_method_name": "sample_method",
  "output_type": "JSON",
  "output_yml": "output.yml content",
  "output_parameters": [
    {
      "name": "result",
      "data_type": "string",
      "position": 1,
      "doc_string": "Result of the task",
      "optional": false
    }
  ],
  "output_is_list": false,
  "id": "uuid-of-task-definition",
  "created_date": "2023-10-15T12:34:56.789Z",
  "modified_date": "2023-10-15T12:34:56.789Z",
  "created_by_email": "user@example.com",
  "modified_by_email": "user@example.com",
  "deleted_at": null,
  "belongs_to_integration": {
    "class_name": "IntegrationClassName",
    "name": "Integration Name",
    "short_name": "integration_short_name",
    "auth_type": "oauth2",
    "id": "uuid-of-integration",
    "created_date": "2023-10-15T12:34:56.789Z",
    "modified_date": "2023-10-15T12:34:56.789Z"
  }
}
```

This endpoint retrieves a specific task definition by its unique ID.

### HTTP Request

`GET /api/v1/task_definitions/`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type   | Required | Description
----------|--------|----------|------------
id        | string | Yes      | The unique identifier (UUID) of the task definition.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

<aside class="notice">
If the task definition is not found, a 404 error is raised.
</aside>

---

## Get All Flow Runs

```shell
curl -X GET "https://your_api_base_url/api/v1/flow_runs/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100,
}

response = requests.get('https://your_api_base_url/api/v1/flow_runs/all', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/flow_runs/all?skip=0&limit=100")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_runs/all?skip=0&limit=100';

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
[
  {
    "flow": "uuid-of-flow",
    "flow_request": "uuid-of-flow-request",
    "status": "completed",
    "approval_status": "approved",
    "triggered_time": "2023-10-15T12:34:56.789Z",
    "end_time": "2023-10-15T13:00:00.000Z",
    "triggered_by": "user@example.com",
    "id": "uuid-of-flow-run",
    "task_runs": [
      {
        "task_operation_index": 1,
        "status": "completed",
        "start_time": "2023-10-15T12:35:00.000Z",
        "result": {"key": "value"},
        "end_time": "2023-10-15T12:40:00.000Z",
        "id": "uuid-of-task-run",
        "flow_run": "uuid-of-flow-run"
      }
    ]
  }
]
```

This endpoint retrieves all flow runs from the system.

### HTTP Request

`GET /api/v1/flow_runs/all`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type | Required | Description
----------|------|----------|------------
skip      | int  | No       | Number of records to skip for pagination (default is 0)
limit     | int  | No       | Maximum number of records to return (default is 100)

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

## Get Flow Run by ID

```shell
curl -X GET "https://your_api_base_url/api/v1/flow_runs/?id=uuid-of-flow-run" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-flow-run',
}

response = requests.get('https://your_api_base_url/api/v1/flow_runs/', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/flow_runs/?id=uuid-of-flow-run")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_runs/?id=uuid-of-flow-run';

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
  "flow": "uuid-of-flow",
  "flow_request": "uuid-of-flow-request",
  "status": "completed",
  "approval_status": "approved",
  "triggered_time": "2023-10-15T12:34:56.789Z",
  "end_time": "2023-10-15T13:00:00.000Z",
  "triggered_by": "user@example.com",
  "id": "uuid-of-flow-run",
  "task_runs": [
    {
      "task_operation_index": 1,
      "status": "completed",
      "start_time": "2023-10-15T12:35:00.000Z",
      "result": {"key": "value"},
      "end_time": "2023-10-15T12:40:00.000Z",
      "id": "uuid-of-task-run",
      "flow_run": "uuid-of-flow-run"
    }
  ]
}
```

This endpoint retrieves a specific flow run by its ID.

### HTTP Request

`GET /api/v1/flow_runs/`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type   | Required | Description
----------|--------|----------|------------
id        | string | Yes      | The ID of the flow run to retrieve.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

<aside class="notice">
If the flow run is not found, a 404 error is raised.
</aside>

---

## Create Flow Run

```shell
curl -X POST "https://your_api_base_url/api/v1/flow_runs/" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "flow": "uuid-of-flow",
        "flow_request": "uuid-of-flow-request",
        "status": "pending",
        "approval_status": null,
        "triggered_time": "2023-10-15T12:34:56.789Z",
        "triggered_by": "user@example.com"
      }'
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json',
}

data = {
    "flow": "uuid-of-flow",
    "flow_request": "uuid-of-flow-request",
    "status": "pending",
    "approval_status": None,
    "triggered_time": "2023-10-15T12:34:56.789Z",
    "triggered_by": "user@example.com"
}

response = requests.post('https://your_api_base_url/api/v1/flow_runs/', headers=headers, json=data)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://your_api_base_url/api/v1/flow_runs/")
request = Net::HTTP::Post.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"
request["Content-Type"] = "application/json"
request.body = {
  flow: "uuid-of-flow",
  flow_request: "uuid-of-flow-request",
  status: "pending",
  approval_status: nil,
  triggered_time: "2023-10-15T12:34:56.789Z",
  triggered_by: "user@example.com"
}.to_json

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_runs/';

const headers = {
  'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
  'Content-Type': 'application/json',
};

const body = {
  flow: 'uuid-of-flow',
  flow_request: 'uuid-of-flow-request',
  status: 'pending',
  approval_status: null,
  triggered_time: '2023-10-15T12:34:56.789Z',
  triggered_by: 'user@example.com',
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
  "flow": "uuid-of-flow",
  "flow_request": "uuid-of-flow-request",
  "status": "pending",
  "approval_status": null,
  "triggered_time": "2023-10-15T12:34:56.789Z",
  "end_time": null,
  "triggered_by": "user@example.com",
  "id": "uuid-of-new-flow-run",
  "task_runs": []
}
```

This endpoint creates a new flow run based on the provided details.

### HTTP Request

`POST /api/v1/flow_runs/`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`
Content-Type   | string | Yes      | Should be `application/json`

### Request Body

Field           | Type           | Required | Description
----------------|----------------|----------|------------
flow            | string (UUID)  | Yes      | The ID of the flow.
flow_request    | string (UUID)  | Yes      | The ID of the flow request.
status          | string         | No       | Status of the flow run (default is "pending").
approval_status | string         | No       | Approval status ("approved" or "rejected").
triggered_time  | string (date-time)| No    | The time when the flow run was triggered.
end_time        | string (date-time)| No     | The time when the flow run ended.
triggered_by    | string         | No       | Who triggered the flow run.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

## Approve Flow Run

```shell
curl -X PUT "https://your_api_base_url/api/v1/flow_runs/approve?flow_run_id=uuid-of-flow-run" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'flow_run_id': 'uuid-of-flow-run',
}

response = requests.put('https://your_api_base_url/api/v1/flow_runs/approve', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://your_api_base_url/api/v1/flow_runs/approve?flow_run_id=uuid-of-flow-run")
request = Net::HTTP::Put.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_runs/approve?flow_run_id=uuid-of-flow-run';

fetch(url, {
  method: 'PUT',
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
  "flow": "uuid-of-flow",
  "flow_request": "uuid-of-flow-request",
  "status": "in_progress",
  "approval_status": "approved",
  "triggered_time": "2023-10-15T12:34:56.789Z",
  "end_time": null,
  "triggered_by": "user@example.com",
  "id": "uuid-of-flow-run",
  "task_runs": [...]
}
```

This endpoint approves a specific flow run.

### HTTP Request

`PUT /api/v1/flow_runs/approve`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter    | Type   | Required | Description
-------------|--------|----------|------------
flow_run_id  | string | Yes      | The ID of the flow run to approve.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

<aside class="notice">
If the flow run does not exist or cannot be approved, an HTTPException is raised.
</aside>

---

## Get Flow Run with Flow Result

```shell
curl -X GET "https://your_api_base_url/api/v1/flow_runs/with_flow_result?id=uuid-of-flow-run" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'id': 'uuid-of-flow-run',
}

response = requests.get('https://your_api_base_url/api/v1/flow_runs/with_flow_result', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://your_api_base_url/api/v1/flow_runs/with_flow_result?id=uuid-of-flow-run")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_runs/with_flow_result?id=uuid-of-flow-run';

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
  "flow": "uuid-of-flow",
  "flow_request": "uuid-of-flow-request",
  "status": "completed",
  "approval_status": "approved",
  "triggered_time": "2023-10-15T12:34:56.789Z",
  "end_time": "2023-10-15T13:00:00.000Z",
  "triggered_by": "user@example.com",
  "id": "uuid-of-flow-run",
  "task_runs": [...],
  "flow_result": {
    "body": "Result body content",
    "trigger_run_id": null,
    "flow_run_id": "uuid-of-flow-run",
    "organization_id": "uuid-of-organization",
    "id": "uuid-of-flow-result",
    "created_date": "2023-10-15T13:00:00.000Z",
    "modified_date": null,
    "created_by_email": null,
    "modified_by_email": null,
    "deleted_at": null
  }
}
```

This endpoint retrieves a specific flow run by its ID along with its flow result.

### HTTP Request

`GET /api/v1/flow_runs/with_flow_result`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type   | Required | Description
----------|--------|----------|------------
id        | string | Yes      | The ID of the flow run to retrieve.

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

<aside class="notice">
If the flow run is not found, a 404 error is raised.
</aside>

---

## Get All Flow Runs for a Specific Flow ID

```shell
curl -X GET "https://your_api_base_url/api/v1/flow_runs/all_for_flow_id?flow_id=uuid-of-flow&skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'flow_id': 'uuid-of-flow',
    'skip': 0,
    'limit': 100,
}

response = requests.get('https://your_api_base_url/api/v1/flow_runs/all_for_flow_id', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://your_api_base_url/api/v1/flow_runs/all_for_flow_id?flow_id=uuid-of-flow&skip=0&limit=100")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_runs/all_for_flow_id?flow_id=uuid-of-flow&skip=0&limit=100';

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
[
  {
    "flow": "uuid-of-flow",
    "flow_request": "uuid-of-flow-request",
    "status": "completed",
    "approval_status": "approved",
    "triggered_time": "2023-10-15T12:34:56.789Z",
    "end_time": "2023-10-15T13:00:00.000Z",
    "triggered_by": "user@example.com",
    "id": "uuid-of-flow-run",
    "task_runs": [...]
  }
]
```

This endpoint retrieves all flow runs associated with a specific flow ID.

### HTTP Request

`GET /api/v1/flow_runs/all_for_flow_id`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type | Required | Description
----------|------|----------|------------
flow_id   | string | Yes    | The ID of the flow.
skip      | int    | No     | Number of records to skip (default is 0).
limit     | int    | No     | Maximum number of records to return (default is 100).

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

## Get Preview of All Flow Runs

```shell
curl -X GET "https://your_api_base_url/api/v1/flow_runs/preview/all?skip=0&limit=100" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
}

params = {
    'skip': 0,
    'limit': 100,
}

response = requests.get('https://your_api_base_url/api/v1/flow_runs/preview/all', headers=headers, params=params)
print(response.json())
```

```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://your_api_base_url/api/v1/flow_runs/preview/all?skip=0&limit=100")
request = Net::HTTP::Get.new(uri)
request["Authorization"] = "Bearer YOUR_ACCESS_TOKEN"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```

```javascript
const fetch = require('node-fetch');

const url = 'https://your_api_base_url/api/v1/flow_runs/preview/all?skip=0&limit=100';

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
[
  {
    "id": "uuid-of-flow-run",
    "flow_name": "Sample Flow",
    "flow_request_instructions": "Process this request.",
    "approval_status": "approved",
    "start_time": "2023-10-15T12:34:56.789Z",
    "status": "completed",
    "end_time": "2023-10-15T13:00:00.000Z"
  }
]
```

This endpoint retrieves a preview of all flow runs, with limited information.

### HTTP Request

`GET /api/v1/flow_runs/preview/all`

### Headers

Parameter      | Type   | Required | Description
---------------|--------|----------|------------
Authorization  | string | Yes      | Bearer token in the format `Bearer YOUR_ACCESS_TOKEN`

### Query Parameters

Parameter | Type | Required | Description
----------|------|----------|------------
skip      | int  | No       | Number of records to skip (default is 0).
limit     | int  | No       | Maximum number of records to return (default is 100).

### Responses

Status | Description
-------|-------------
200    | Successful Response
422    | Validation Error

---

# Conclusion

The above documentation provides detailed information about the **Flow Runs** endpoints in your API, including code examples, descriptions, HTTP requests, headers, parameters, and responses. You can continue to expand your documentation by adding any additional endpoints or sections as needed.

Remember to replace placeholders like `your_api_base_url` and `YOUR_ACCESS_TOKEN` with actual values when deploying the documentation.
