# Personalization Permission

## Endpoint
```
POST /{stage}/registerPersonalization/tenant/{tenant}/key/tenant_change_permission_user-{user_alias_id}
```

### Method - **POST**

## Purpose
This API is used to grant permission to developers, allowing them to access a specific tenant by logging into the console dashboard and entering the tenant name.

In the URL, replace `{tenant}` with `metricrealties` and `{user_alias_id}` with the alias ID of the user to whom access must be granted.

## Request Headers
The following headers must be included in the request:

| Header         | Type   | Description |
|---------------|--------|-------------|
| Authorization | String | Bearer token for authentication |
| User         | String | User alias ID (can be retrieved from console network tabs) |
| Tenant       | String | Must always be `metricrealties` |

## Request Body
The request body should be in JSON format with the following structure:

```json
{
    "tenant_operations_change_allowed": true
}
```

## Response
### Success Response
**Status Code:** `200 OK`

**Response Body:**
```json
{
    "tenant": "metricrealties",
    "key": "tenant_change_permission_user-8a8036219205c64e125fd0a7fd3699ca",
    "stage": "beta",
    "tenant_operations_change_allowed": true
}
```

## Steps to Use the API
1. Ensure you have the necessary authentication credentials (Bearer token).
2. Identify the correct user alias ID from the console network tabs.
3. Replace `{tenant}` with `metricrealties` and `{user_alias_id}` with the appropriate alias ID in the endpoint.
4. Set the required headers (`Authorization`, `User`, and `Tenant`).
5. Send a `POST` request with the required JSON body.
6. Verify the response to confirm permission has been granted successfully.

