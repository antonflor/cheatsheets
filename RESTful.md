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

- **Use Nouns in Endpoint Paths**: Prefer `/users` over `/getUsers`.
- **Be Consistent**: Use a consistent naming convention and format.
- **Use HTTP Methods Appropriately**: Align actions with the correct HTTP methods.
- **Handle Errors Gracefully**: Provide clear error messages and appropriate HTTP status codes.
- **Statelessness**: Ensure that each API request can be processed independently.
- **Versioning**: Version your API to manage changes and maintain backward compatibility.
- **Security**: Implement authentication, authorization, and data encryption.

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
