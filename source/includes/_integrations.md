# Integrations

Integrations represent the external services or platforms that can be connected to your workflows in Neena. By setting up integrations, you can automate tasks across different systems, allowing seamless data flow and process automation between Neena and other applications.

Integrations are essential for extending the capabilities of your workflows, enabling tasks like sending emails, creating tickets, updating records, and more in external systems directly from your flows.

If you would like to see certain integrations on the platform, please contact [one of our founders](mailto:rauf@neena.io) to get support

## Integration Model

| Field          | Type             | Required | Description                             |
|----------------|------------------|----------|-----------------------------------------|
| **id**             | string (UUID)    | Yes      | Unique identifier of the integration.    |
| **name**           | string           | Yes      | Name of the integration.                 |
| **short_name**     | string           | Yes      | Short name used to reference the integration. |
| **class_name**     | string           | Yes      | Class name used internally.              |
| **auth_type**      | string           | Yes      | Authentication type (e.g., 'oauth2').    |
| **created_date**   | string (date-time)| Yes     | When the integration was created.        |
| **modified_date**  | string (date-time)| Yes     | When the integration was last modified.  |

---

## Read All Integrations

```shell
curl -X GET "api.neena.io/integrations/all?skip=0&limit=100" \
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

response = requests.get('api.neena.io/integrations/all', headers=headers, params=params)
print(response.json())
```

```javascript
const fetch = require('node-fetch');

const url = 'api.neena.io/integrations/all?skip=0&limit=100';

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
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "Slack",
    "short_name": "slack",
    "class_name": "SlackIntegration",
    "auth_type": "oauth2",
    "created_date": "2024-09-18T10:27:57.149Z",
    "modified_date": "2024-09-18T10:27:57.149Z"
  },
  {
    "id": "4ec2d0f1-1234-5678-9abc-def012345678",
    "name": "Perplexity",
    "short_name": "perplexity",
    "class_name": "PerplexityIntegration",
    "auth_type": "api_key",
    "created_date": "2024-09-18T10:27:57.149Z",
    "modified_date": "2024-09-18T10:27:57.149Z"
  }
]
```

This endpoint retrieves all available integrations.

### HTTP Request

`GET /integrations/all`

### Headers

| Field    | Type   | Required | Description            |
|----------|--------|----------|------------------------|
| api_key  | string | Yes      | API key `YOUR_API_KEY` |

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