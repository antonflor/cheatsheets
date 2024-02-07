# RESTful API Cheat Sheet

RESTful APIs (Representational State Transfer) are an architectural style for designing networked applications. They use HTTP requests to access and use data. RESTful APIs are stateless, meaning that each request from a client contains all the information needed to process the request.

- **Key Operations (HTTP Methods)**:
  - GET: Retrieve data from a server.
  - POST: Send data to a server.
  - PUT: Update data on a server.
  - DELETE: Remove data from a server.

## Basic Components

- **Endpoints**: URLs where API operations are accessed.
- **HTTP Methods**: Define action types (GET, POST, PUT, DELETE).
- **Headers**: Convey metadata for the HTTP request and response.
- **Body**: Contains data sent to or received from the server (typically in JSON format).
- **Status Codes**: Indicate the result of the HTTP request (e.g., 200 OK, 404 Not Found).

## Common HTTP Methods

- **GET**
  - Used to retrieve data from a server.
  - Example: `GET /users` retrieves a list of users.

- **POST**
  - Used to send data to a server.
  - Example: `POST /users` with a user object in the request body to create a new user.

- **PUT**
  - Used to update existing data on a server.
  - Example: `PUT /users/123` with updated user data in the request body.

- **DELETE**
  - Used to delete data from a server.
  - Example: `DELETE /users/123` deletes the user with ID 123.

## RESTful API Design Best Practices

- **API Gateway Usage**: Use an API gateway for management, authentication, and analytics.
- **Be Consistent**: Use a consistent naming convention and format.
- **Cache Data to Improve Performance**: Utilize HTTP caching mechanisms for better performance.
- **Consistent Responses**: Use a consistent format for all API responses.
- **Content Negotiation**: Support multiple media types for requests and responses (e.g., application/json, application/xml).
- **Cross-Origin Resource Sharing (CORS)**: Properly configure CORS if your API is to be accessed from different domains.
- **Deprecation Policy**: Clearly communicate any deprecations in API functionality.
- **Documentation**: Provide clear and comprehensive documentation for your API.
- **Environment-Based Configuration**: Separate configuration from code, especially for different environments (development, staging, production).
- **Error Handling Standardization**: Standardize the structure of error messages.
- **Filtering, Sorting, and Searching**: Allow users to filter, sort, and search data through query parameters.
- **Follow REST Constraints**: Adhere to REST constraints such as client-server architecture, statelessness, and cacheability.
- **Handle Errors Gracefully**: Provide clear error messages and appropriate HTTP status codes.
- **Implement ETags for Optimistic Concurrency Control**: Utilize ETags for managing simultaneous updates.
- **Internationalization and Localization**: Consider supporting multiple languages and regional data formats.
- **Limit Resource Nesting**: Avoid deeply nested resources. Aim for a maximum of three levels.
- **Monitoring and Logging**: Implement monitoring and logging to track API usage and errors.
- **Pagination**: Implement pagination for responses with large data sets.
- **Partial Responses**: Allow clients to request only the fields they need.
- **Rate Limiting**: Implement rate limiting to prevent abuse and maintain service availability.
- **Resource Identification in Requests**: Ensure resources are clearly and uniquely identified by their URIs.
- **Respect Privacy and Data Regulations**: Comply with data protection regulations like GDPR.
- **Security Audits and Updates**: Regularly audit and update security measures.
- **Security**: Implement authentication, authorization, and data encryption.
- **Standardize Timestamps and Time Zones**: Use a consistent format for timestamps and consider time zone implications.
- **Statelessness**: Ensure that each API request can be processed independently.
- **Support for HEAD and OPTIONS Methods**: Implement these methods for resource metadata and communication options.
- **Use HATEOAS (Hypertext As The Engine Of Application State)**: Allow navigation through the API via hyperlinks.
- **Use HTTP Methods Appropriately**: Align actions with the correct HTTP methods.
- **Use Nouns in Endpoint Paths**: Prefer `/users` over `/getUsers`.
- **Use of HTTP Status Codes**: Appropriately use HTTP status codes to indicate the outcome of API requests.
- **Use of Query Parameters for Optional Features**: Utilize query parameters for sorting, filtering, and pagination.
- **Use Plural Nouns for Consistency**: Prefer `/items` over `/item` for resource names.
- **Use SSL/TLS for Secure Communication**: Always use HTTPS to secure data in transit.
- **Use Sub-Resources for Relations**: For hierarchical data, use sub-resources (e.g., `/users/{id}/posts`).
- **Use Webhooks for Event Notifications**: Implement webhooks to notify clients of events.
- **Versioning**: Version your API to manage changes and maintain backward compatibility.

## Response Status Codes

- **200 OK**: The request has succeeded.
- **201 Created**: A new resource has been created.
- **400 Bad Request**: The server cannot process the request due to a client error.
- **401 Unauthorized**: Authentication is required and has failed or not been provided.
- **403 Forbidden**: The client does not have permission to access the requested resource.
- **404 Not Found**: The requested resource was not found.
- **500 Internal Server Error**: An unexpected condition was encountered on the server.

## Sample RESTful API Request

- **GET Request Example**
```
  GET /api/users HTTP/1.1
  Host: example.com
  Accept: application/json
```

- **POST Request Example**
```
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john@example.com"
}
```


#### Tools for Testing RESTful APIs
- **Postman**: A popular tool for testing API endpoints.
- **Curl**: A command-line tool for sending HTTP requests.
- **Swagger**: Useful for API documentation and testing.
