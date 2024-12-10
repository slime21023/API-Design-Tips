# Cheat Sheet

## Naming Conventions

* General Rules
    * Use **nouns** for resources: `/users`, `/products`
    * Use **camelCase** or **snake_case** for fields: `firstName`, `user_id`
* Endpoints
    * Retrieve all: `GET /resources`
    * Retrieve one: `GET /resources/{id}`
    * Create: `POST /resources`
    * Update: `PUT /resources/{id}` or `PATCH /resources/{id}`
    * Delete: `DELETE /resources/{id}`
* Custom Actions
    * Use verbs for actions on resources
        * `POST /resources/{id}/activate`
        * `POST /resources/{id}/reset-password`

## Filtering & Pagination

* Filtering
    * Use query parameters for filtering: 
        * `GET /users?status=active&role=admin`
    * Use consistent syntax for multiple filters:
        * `GET /products?category=electronics&price[lt]=100`

* Pagination
    * Use `pageSize` and `pageToken` for cursor-based pagination:
        * `GET /users?pageSize=10&pageToken=abc123`
    * Include pagination metadata in th response:
        ```json
        {
            "data": [...],
            "pagination": {
                "nextPageToken": "xyz456",
                "totalCount": 100
            }
        }
        ```


## API Versioning

* Versioning Strategies
    * **URL-based**: Include version in the URL
        * `/v1/resources`
    * **Header-based**: Use headers to specify versions
        * `Accept: application/vnd.api+json;version=2`
* Best Practices
    * Maintain backward compatibility as much as possible
    * Deprecate old versions with clear timelines
    * Document breaking changes thoroughly

## Error Handling

* Standard HTTP Status Codes
    * 200: Successful request
    * 201: Resource created
    * 204: No Content
    * 400: Bad Request
    * 401:  Authentication failed
    * 403: No permission
    * 404: Resource not found
    * 500: Server-side issue

* Error Response Structure
    ```json
    {
        "error": {
            "code": 400,
            "message": "Invalid request payload",
            "details": [
                { "field": "email", "message": "Email is required" },
                { "field": "password", "message": "Password must be at least 8 characters" }
            ]
        }
    }
    ```

## Security Measures

* Authentication
    * Use OAuth 2.0 or OpenID Connect for secure authentication
    * Include `Bearer <token>` in the `Authorization` header
* Rete Limiting
    * Implement limits to prevent abuse
    * Use `X-RateLimit-Limit`, `X-RateLimit-Remaining`, and `X-RateLimit-Reset` headers
        * `X-RateLimit-Limit: 1000`, `X-RateLimit-Remaining: 500`
* Data Protection
    * Use HTTPS for all requests to encrypt data
    * Validate all inputs to prevent injection attacks
* Error Messages
    * Avoid exposing sensitive information in error responses
        * Bad: `Database connection failed: Invalid password`
        * Good: `500 Internal Server Error`

