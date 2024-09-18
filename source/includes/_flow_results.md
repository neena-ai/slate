# Flow Results


## Flow Result Model

| Field                 | Type               | Description                                      |
|-----------------------|--------------------|--------------------------------------------------|
| **id**                | string (UUID)      | Unique identifier of the flow result.            |
| **body**              | string             | The content or body of the flow result. It contains the result or output message from the flow execution.         |
| **trigger_run_id**    | string (UUID)      | ID of the associated trigger run.                |
| **integration_entity_id** | string (UUID)  | The integration entity ID (e.g., ticket ID) that is used in the integration platform (e.g. Zendesk) |
| **flow_run_id**       | string (UUID)      | ID of the associated flow run.                   |
| **organization_id**   | string (UUID)      | ID of the associated organization.               |
| **created_date**      | string (date-time) | ISO 8601 formatted date-time when created.       |
| **modified_date**     | string (date-time) | ISO 8601 formatted date-time when modified.      |
| **created_by_email**  | string             | Email of the user who created the flow result.   |
| **modified_by_email** | string             | Email of the user who last modified the flow result.|


**Notes:**
- Timestamps are in ISO 8601 format and represent UTC times.

## Read Latest Flow Result For Integration Entity

```shell
curl -X GET "api.neena.io/flow_results/?integration_entity_id=entity-id&integration_short_name=integration_short_name" \
  -H "api_key: YOUR_API_KEY"
```

```python
import requests

headers = {
    'api_key': 'YOUR_API_KEY',
}

params = {
    'integration_entity_id': 'entity-id',
    'integration_short_name': 'integration_short_name'
}

response = requests.get('api.neena.io/flow_results', headers=headers, params=params)
print(response.json())
```

```javascript
fetch('api.neena.io/flow_results/?integration_entity_id=entity-id&integration_short_name=integration_short_name', {
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
```

This endpoint retrieves the latest flow result for a specific integration entity. In case you have set up a trigger that works on a specific entity, for example Google Mails, you can fetch its result by providing the entity id that is used in the platform, along with the integration short name that is used, like in this case: `gmail`.

### HTTP Request

`GET /flow_results`

### Headers

| Parameter     | Type   | Required | Description                                         |
|---------------|--------|----------|-----------------------------------------------------|
| Authorization | string | Yes      | Bearer token in the format `Bearer YOUR_API_KEY` |

### Query Parameters

| Parameter              | Type   | Required | Description                                |
|------------------------|--------|----------|--------------------------------------------|
| integration_entity_id  | string | Yes      | The integration entity ID (e.g., ticket ID) that is used in the integration platform (e.g. Zendesk)|
| integration_short_name | string | Yes      | The integration's short name               |

### Responses

| Status | Description         |
|--------|---------------------|
| 200    | Successful Response |
| 403    | Authorization Error |
| 404    | Not Found Error     |
| 422    | Validation Error    |

---
