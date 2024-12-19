# Overview of HTTP Communication

HTTP (HyperText Transfer Protocol) is the foundation of communication between clients (like browsers or apps) and servers. A typical HTTP communication involves requests and responses:

- **Client (e.g., browser)** sends an HTTP request to the server.
- **Server** processes the request and sends an HTTP response back.

## 1. Components of an HTTP Request

An HTTP request consists of multiple parts, each serving a specific purpose:

### A. HTTP Request Line

The request line contains:

1. HTTP Method: Specifies the type of operation. Common methods:
  - **GET:** Fetch data.
  - **POST:** Send data.
  - **PUT:** Update data.
  - **DELETE:** Remove data.

2. URL: The resource you want to access, e.g., /users.

3. HTTP Version: Version of the protocol (e.g., HTTP/1.1, HTTP/2).

Example:

```
GET /users HTTP/1.1
```

### B. HTTP Headers

Headers provide additional information about the request or response. They are key-value pairs.

**Request Headers:**
  - Authorization: Used for authentication (e.g., Bearer <JWT_TOKEN>).
  - Content-Type: Type of data sent (e.g., application/json for JSON payloads).
  - User-Agent: Info about the client making the request (e.g., Chrome).
  - Cookie: Cookies sent by the client.

**Response Headers:**
  - Content-Type: Type of data returned (e.g., text/html, application/json).
  - Set-Cookie: Sends a cookie to the client.

Example Request Header:

```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### C. Query Parameters

Found in the URL after a `?` and used to send data in a `GET` request.
Example: `/users?role=admin&page=2`
They are key-value pairs:
  - `role=admin`
  - `page=2`

### D. Request Body

The body contains the data you want to send to the server, typically with `POST` or `PUT` requests.

  - Example (JSON Body):

```
{
  "name": "Kenneth",
  "email": "kenneth@example.com"
}
```

### E. Cookies

Cookies are small pieces of data sent by the server and stored on the client. They are:

  - Sent to the server with each request (via Cookie header).
  - Used for:
      * Session management (e.g., user login).
      * Storing preferences.
      * Authentication (e.g., refresh tokens).

Example Cookie Header:

```
Cookie: jwtToken=abcd1234; theme=dark
```

## 2. Components of an HTTP Response

The server’s response to the client also consists of specific parts:

### A. Status Line

Indicates the status of the request. Consists of:

  - HTTP Version
  - Status Code: Indicates the result (e.g., 200 OK, 404 Not Found, 500 Internal Server Error).
  - Status Message: A human-readable message.

Example:

```
HTTP/1.1 200 OK

```

### B. Response Headers

Headers with metadata about the response.

    Example:

Content-Type: application/json
Set-Cookie: refreshToken=xyz123; HttpOnly

### C. Response Body

The actual data sent back to the client.

    Example JSON Response Body:

{
  "message": "Login successful",
  "data": { "id": 1, "name": "Kenneth" }
}

## 4. Key Networking Concepts

### A. Bearer Token

 - A type of token used for authentication.
 - Sent in the Authorization header:

```
Authorization: Bearer <your_access_token>
```

### B. Headers vs Cookies vs Body vs Params

| Type |Purpose | When to Use |
| ---- | ------ | ----------- |
| Headers | Meta-information about the request (e.g., authentication, content type). |	Use for authentication tokens, content type, and additional information. |
| Cookies | Small pieces of data stored on the client, automatically sent with every request.| Use for session tokens, preferences, and state management. |
| Request Body | Data payload sent with POST, PUT, and similar requests.| Use for sending large amounts of structured data (e.g., JSON payload). |
| Query Params | Key-value pairs in the URL, used to filter or query resources (e.g., /users?role=admin). | Use for sending lightweight, non-sensitive data in GET requests. |

## 5. HTTP Status Codes

Status codes provide feedback about the request:
1xx (Informational)

    100 Continue: Initial part of a request received; continue.

2xx (Success)

    200 OK: Request was successful.
    201 Created: A new resource was created.

3xx (Redirection)

    301 Moved Permanently: Resource moved to a new URL.
    304 Not Modified: Cached resource is up to date.

4xx (Client Error)

    400 Bad Request: Invalid request.
    401 Unauthorized: Authentication required.
    403 Forbidden: Access denied.
    404 Not Found: Resource not found.

5xx (Server Error)

    500 Internal Server Error: Generic error.
    502 Bad Gateway: Server received an invalid response.
    503 Service Unavailable: Server is down.

## 6. Networking Lifecycle for Backend Development

    Request Creation: Client sends an HTTP request.
    Request Handling: Backend server processes headers, body, and params.
    Authentication: Validates tokens/cookies for protected routes.
    Business Logic: Executes logic based on the request.
    Response Creation: Backend sends HTTP status, headers, and body back.

## 7. Tools for Learning and Testing

    Postman/Insomnia:
        Test API requests (e.g., headers, body, params).
    Browser DevTools:
        Inspect Network tab to see headers, cookies, and responses.
    Mock Servers:
        Create fake APIs to practice sending and receiving requests.
