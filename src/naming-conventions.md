# Naming Conventions

Naming conventions are critical in API design as they directly impact the clarity, usability, and consistency of the API. A well-designed naming system makes the API intuitive and easy to adopt for developers, minimizing learning curves and errors.

### Core Principles 
1. **Consistency**
    * Maintain uniform patterns across the API to reduce cognitive load.
    * Follow a single case style (e.g., `camelCase`, `snake_case`) and apply it universally to field names, query parameters, and endpoints.
2. **Expressiveness**
    * Names should convey clear meaning about the resource or action they represent.
    * Use self-explanaory terms; avoid ambiguous or generic names.
3. **Predictability**
    * Naming should align with standard conventions and developer expectations.
    * CRUD operations should map to GET, POST, PUT, DELETE in a predictable manner.
4. **Simplicity**
    * Avoid verbose or redundant terms in resource and parameter names.
    * Keep paths short and meaningful.

### Best Practices
1. Resources
    * Use Nouns: Represent resources with nouns, not verbs, as the resource represents an entity or collection.
        * Good: `/users`, `/orders`
        * Bad: `/getUsers`, `/createOrder`
    * Use plural nouns to represent collections and singular nouns for individual items.
        * Collection: `GET /users`
        * Single resource: `GET /users/{id}`
    * Hierarchical Relationships: Use path hierarchy to indicate relationships between resources.
        * Articles for a user: `GET /users/{id}/articles`
        * Comments for an article: `GET /users/{id}/articles/{article_id}/comments`
2. Actions
    * Use Http Methods Appropriately: Let HTTP methods define actions instead of embedding them in resource names.
        * Good:
            * `GET /users` &rarr; Retrieve a list of users
            * `POST /users` &rarr; Create a new user
        * Bad: `/users/getAll`, `/users/add`
    * Custom Actions: For operations beyond CRUD, append actions to resource paths as sub-resources. Use verbs for clarity.
        * Example: 
            * Activate user: `POST /users/{id}/activate`
            * Reset password: `POST /users/{id}/reset-password`
3. Query Parameters
    * Use Descriptive Parameter Names: Query parameters should clearly describe the filtering or sorting applied.
        * `GET /products?category=electronicss&price[lt]=100`
    * Avoid Embedding Parameters in URLs: Parameters should not be part of the path; use query strings instead.
        * Good: `/users?status=active`
        * Bad: `/users/status/active`
4. Case Conventions
    * **camelCase**: Commom use in JSON and JavaScript APIs for names
    * **snake_case**: Often used in RESTful APIs, especially for query parameters
    * Consistently apply the chosen convention throughout the API.
5. Avoid Redundancy
    * Do not repeat resource names unnecessarily.
        * Good: `/users/{id}`
        * Bad: `/users/user/{id}`

### Common Pitfalls to Avoid
1. Inconsistent Naming Styles
    * Mixing `camelCase`, and `snake_case`, and `PascalCase` leads to confusion.
2. Overly Verbose Names
    * Avoid long and redundant names.
    * Example:
        * Good: `/users/{id}`
        * Bad: `/fetchUserDetailsById/{id}`
3. Ambiguous or Non-Descriptive Names
    * Avoid names like `/data` or `/info` that don't clearly describe the resource.
4. Embedding HTTP Methods in Names
    * Avoid paths like `/users/getAll` or `/orders/delete/{id}`

### Real-World Example
A well-structured API for managing blog posts might look like this:

| Operation | Endpoint | HTTP Method |
|----------|----------|-------------|
|List all posts|	`/posts` |	`GET` |
|Retrieve a single post |	`/posts/{id}` |	`GET` |
|Create a new post|	`/posts` |	`POST` |
|Update a post|	`/posts/{id}`	| `PUT` or `PATCH` |
|Delete a post|	`/posts/{id}`	| `DELETE` |
|List comments on a post|	`/posts/{id}/comments`	| `GET` |
|Add a comment to a post|	`/posts/{id}/comments`	| `POST` |
|Like a post|	`/posts/{id}/like`	| `POST` |