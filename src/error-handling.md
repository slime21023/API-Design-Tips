# Error Handling

Error handling is critical for building user-friendly and reliable APIs. A well-structured error-handling strategy helps API consumers diagnose and fix issues efficiently while maintaining security and clarity.

### Goals of Error Handling
* **Clarity**: Errors should provide enough information for users to understand what went wrong.
* **Consistency**: Use a standardized format for error responses across the API.
* **Security**: Avoid exposing sensitive internal details in error messages.
* **Actionability**: Enable consumers to take corrective actions based on the error response.

### Best Practices for Error Handling

**Use Standard HTTP Status Codes**

HTTP status codes should reflect the nature of the error:
* **2xx - Success**
    - `200 OK`: The request was successful.
    - `201 Created`: A resource was created successfully.
* **4xx - Client Errors**
    - `400 Bad Request`: The request is malformed or contains invalid data.
    - `401 Unauthorized`: Authentication is required or failed.
    - `403 Forbidden`: The client does not have permission.
    - `404 Not Found`: The resource does not exist.
    - `409 Conflict`: There is a conflict with the current state of the resource.
* **5xx - Server Errors**
    - `500 Internal Server Error`: A generic server-side issue.
    - `503 Service Unavailable`: The server is temporarily unavailable.

**Provide a Consistent Error Response Format**

Error responses should follow a predictable and standardized structure:
```json
{
  "error": {
    "code": 400,
    "message": "Invalid request payload",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters"
      }
    ]
  }
}
```

* Fields in the Error Response:
    * `code`: The HTTP status code of the error.
    * `message`: A human-readable description of the error.
    * `details`: (Optional) An array of specific issues for granular feedback.

**Differentiate Between Client and Server Errors**

* Client errors (`4xx`) should indicate that the problem lies with the request.
* Server errors (`5xx`) should indicate that the issue is on the server-side.

**Avoid Exposing Internal Details**

* Do Not: Leak stack traces, database errors, or sensitive configurations.
* Do: Use generic messages like `"An internal error occurred. Please try again later."`

**Include Error Codes for Machine Parsing**

* Use error codes for programmatic identification and handling.
  ```json
  {
    "error": {
        "code": 400,
        "errorCode": "INVALID_INPUT",
        "message": "The 'email' field is required."
    }
  }
  ```

### Common Scenarios and How to Handle Them

**Validation Errors**
* Use Case: Invalid input data.
* HTTP Code: `400 Bad Request`
* Response:
  ```json
  {
    "error": {
        "code": 400,
        "message": "Invalid input data",
        "details": [
            { "field": "email", "message": "Email is required" },
            { "field": "password", "message": "Password is too short" }
        ]
    }
  }
  ```

**Authentication Errors**
* Use Case: Missing or invalid authentication credentials.
* HTTP Code: 
    * `401 Unauthorized` for authentication issues.
    * `403 Forbidden` for permission issues.
* Response:
  ```json
  {
    "error": {
        "code": 401,
        "message": "Authentication failed. Token is invalid or expired."
    }
  }
  ```

**Resource Not Found**
* Use Case: The requested resource does not exist.
* HTTP Code: `404 Not Found`
* Response:
  ```json
  {
    "error": {
        "code": 404,
        "message": "Resource not found: User with ID 123 does not exist."
    }
  }
  ```

**Conflict Errors**
* Use Case: State conflicts, such as duplicate data or resource locking.
* HTTP Code: `409 Conflict`
* Response:
  ```json
  {
    "error": {
        "code": 409,
        "message": "A user with this email address already exists."
    }
  }
  ```

**Rate Limiting**
* Use Case: Too many requests from a client.
* HTTP Code: `429 Too Many Requests`
* Response:
  ```json
  {
    "error": {
        "code": 429,
        "message": "Too many requests. Please try again later."
    }
  }
  ```

**Internal Server Errors**
* Use Case: Unexpected issues on the server.
* HTTP Code: `500 Internal Server Error`
* Response:
  ```json
  {
    "error": {
        "code": 500,
        "message": "An internal server error occurred. Please contact support."
    }
  }
  ```

### Advanced Error Handling Techniques

**Error Localization**
* Include localized messages for multi-language support.
  ```json
  {
    "error": {
        "code": 400,
        "message": "Invalid input data",
        "localizedMessage": {
            "en": "Invalid input data",
            "es": "Datos de entrada no válidos"
        }
    }
  }
  ```

**Correlation IDs**
* Include a unique identifier for debugging server-side errors
  ```json
  {
    "error": {
        "code": 500,
        "message": "An internal server error occurred.",
        "correlationId": "1234567890"
    }
  }
  ```

**Error Logging and Monitoring**
* Log errors with detailed context for debugging.
* Use monitoring tools like Sentry or Datadog to track error trends.


### Common Pitfalls 
1. Generic Error Messages: Avoid vague messages like `"Something went wrong."` Be specific and actionable.
2. Inconsistent Error Structures: Ensure all endpoints return errors in the same format.
3. Overexposing Sensitive Data: Do not include internal stack traces, or database details in error messages.
