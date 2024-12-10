# Filtering & Pagination

Filtering and pagination are crucial for APIs that handle large datasets. They allow clients to retrieve only the data they need, improving performance and reducing bandwidth usage. Properly designed filtering and pagination make APIs more efficient, scalable, and user-friendly.

### Filtering

**Purpose**

Filtering enables clients to refine their data requests by specifying conditions. It reduces the size of responses by returning only the relevant data.

**Best Practices**

1. Use Query Parameters
    * Define filters as query parameters in the URL.
        * Example: `GET /users?status=active`
2. Support Multiple Filters
    * Allow combining filters to refine results further.
        * Example: `GET /products?category=electronics&price[lt]=100`
3. Use Standard Operators
    * Support common operators for comparison, such as:
        * `eq` (equals): `/users?age[eq]=30`
        * `lt` (less than): `/products?price[lt]=100`
        * `gt` (greater than): `/products?price[gt]=50`
        * `in` (in list): `/users?role[in]=admin,editor`
4. Ensure Consistent Syntax
    * Follow a consistent convention for naming and structuring filters.
5. Document Available Filters
    * Clearly specify all supported filters in the API documentation.
6. Secure Filtering
    * Validate and sanitize filter inputs to prevent SQL injection or other security risks.

### Pagination

**Purpose**

Pagination splits large datasets into smaller, manageable chunks, allowing clients to retrieve data incrementally.

**Types of Pagination**
1. Offset-Based Pagination
    * Uses `offset` and `limit` query parameters
        * `GET /users?offset=10&limit=20`
    * Advantages: Simple to implement and widely supported.
    * Disadvantages: Inefficient for large datasets due to database scanning.
2. Cursor-Based Pagination
    * Uses a `pageToken` or `cursor` for navigating through data.
        * `GET /users?pageSize=10&pageToken=abc123`
    * Advantages: More efficient for large datasets.
    * Disadvantages: Slightly more complex to implement.
3. Keyset Pagination
    * Relies on sorted unique keys (e.g., `id`) to retrieve data incrementally
        * GET /users?lastSeenId=50&limit=20
    * Advantages: Highly performant for real-time data retrieval.

### Best Practices

1. Return Pagination Metadata
    * Include metadata like nextPageToken, totalCount, and pageSize in the response.
        ```json
        {
            "data": [...],
            "pagination": {
                "nextPageToken": "xyz456",
                "totalCount": 500,
                "pageSize": 20
            }
        }
        ```
2. Provide Flexible Parameters
    * Allow clients to specify `pageSize` (number of items per page)
        * Default: Provide a sensible default (e.g., `20`).
        * Limit: Enforce a maximum (e.g., `100`) to prevent excessive data loads.
3. Use Stable Sorting
    * Ensure consistent results by sorting data by unique keys (e.g., `id`)
4. Handle Empty Pages Gracefully
    * Return an empty data array when no resulrs exist:
        ```json
        {
            "data": [],
            "pagination": {
                "nextPageToken": null,
                "totalCount": 0,
                "pageSize": 20
            }
        }
        ```
5. Optimize Backend Queries
    * Use database indexes for faster filtering and pagination.
6. Document Pagination Parameters
    * Clearly explain the supported pagination methods in the API documentation.

### Common Pitfalls
1. Relying Solely on Offset-Based Pagination
    * Causes performance issues for large datasets.
    * Prefer cursor-based or keyset pagination for scalability.
2. Lack of Pagination Metadata
    * Failing to include `nextPageToken` or `totalCount` makes client-side navigation difficult.
3. Ignoring Security
    * Unsanitized filters can lead to injection attacks. Always validate inputs.
4. Inconsistent Syntax
    * Mixing pagination and filtering conventions confuses users. Ensure clarity and consistency.
    