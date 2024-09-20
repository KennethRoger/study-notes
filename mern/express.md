# ExpressJS #

For quick reference: [https://expressjs.com/en/4x/api.html#express]()

Express, often referred to as Express.js, is a minimal and flexible web application framework for Node.js. It provides a robust set of features to build web and mobile applications. As the de facto standard for server development in Node.js, Express is known for its simplicity, ease of use, and a large ecosystem of middleware to handle various web development tasks such as routing, session management, and rendering views.

## Key Features of Express ##

1. ### Middleware: ### Express has a powerful middleware system, allowing you to handle HTTP requests and responses efficiently. Middleware functions have access to the request object (req), the response object (res), and the next middleware function in the application’s request-response cycle.

2. ### Routing: ### Express provides a straightforward and robust routing mechanism. You can define routes for different HTTP methods (GET, POST, PUT, DELETE) and set up dynamic routes using URL parameters.

3. ### Template Engines: ### It supports various template engines like Pug, EJS, and Handlebars, enabling server-side rendering of dynamic web pages.

4. ### Error Handling: ### Express has a built-in mechanism for error handling, making it easier to catch and manage errors across your application.

5. ### Extensibility: ### Due to its unopinionated nature, Express is highly extensible and can be customized using various third-party packages available on npm (Node Package Manager).

## Use Cases ##

* RESTful APIs: Express is widely used to build RESTful APIs, thanks to its lightweight and modular architecture.
* Single Page Applications (SPAs): It can serve as a backend server for SPAs created with frameworks like React, Angular, or Vue.
* Full-stack Applications: Combining Express with databases like MongoDB, developers can create robust full-stack application.


```
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

This app starts a server and listens on port 3000 for connections. The app responds with “Hello World!” for requests to the root URL (/) or route. For every other path, it will respond with a 404 Not Found.

## URI(Uniform Resource Identifier) ##

A URI is a technical term that refers to the names of all resources connected to the World Wide Web. It is a string that identifies a resource, regardless of its type, location, or protocol. URIs are used to locate and address resources on the internet.

### Key characteristics: ###

* A URI is an identifier, not a locator. It does not specify how to access the resource, only that it exists.
* URIs can be used to identify any type of resource, not just those on the internet.
* There are two main types of URIs:
	i. Uniform Resource Locators (URLs): specify the location of a resource on the internet, including protocol, domain, path, and query.
	ii. Uniform Resource Names (URNs): provide a persistent and location-independent identifier for a resource, often used in contexts like library catalogs.

Example: `https://www.example.com/images/logo.png` is a URL (locator), while `urn:isbn:978-0-1234567890` is a URN (identifier).

In summary, a URI is a unique string that identifies a resource, and it serves as an umbrella term for both URLs and URNs.

## Routing ##
 <Refer the official site>
[https://expressjs.com/en/guide/routing.html]()


### Route Paths ###

In web development, especially when using frameworks like Express.js or similar, routing is the mechanism that allows you to map HTTP requests to specific handlers. Route paths are used to define the endpoints at which these handlers will respond to requests. There are several types of route paths you can define:

1. ### Static Routes ###
Static routes are straightforward and are defined by a fixed URL path. These routes respond to specific URLs exactly as they are defined.

Example:

js
```
app.get('/home', (req, res) => {
  res.send('Home Page');
});
```
Route Path: /home
Matches: /home
Does Not Match: /home/, /home/user

2. ### Dynamic Routes ###
Dynamic routes include parameters that act as placeholders, which can match different values. Parameters are indicated by a colon (:) followed by the parameter name.

Example:

```
app.get('/user/:id', (req, res) => {
  res.send(`User ID is ${req.params.id}`);
});
```
Route Path: /user/:id
Matches: /user/123, /user/abc
Does Not Match: /user/, /user/123/profile

3. ### Optional Parameters ###
Optional parameters in routes are denoted by appending a question mark (?) to the parameter name. This allows the route to match whether the parameter is present or not.

Example:

```
app.get('/user/:id?', (req, res) => {
  if (req.params.id) {
    res.send(`User ID is ${req.params.id}`);
  } else {
    res.send('User ID is not provided');
  }
});
```
Route Path: /user/:id?
Matches: /user/123, /user/
Does Not Match: /user/123/profile

4. ### Wildcard Routes ###

Wildcard routes use an asterisk (*) to match any part of the path. They can be used to create a catch-all route that matches any unspecified paths or a sub-path.

Example:

```
app.get('/files/*', (req, res) => {
  res.send('File path is ' + req.params[0]);
});
```

Route Path: /files/*
Matches: /files/, /files/any/number/of/segments
Does Not Match: Anything outside /files/ path.

5. ### Regular Expression Routes ###

Routes can also use regular expressions to match paths more flexibly. This allows for complex matching patterns beyond simple static or dynamic segments.

Example:

```
app.get(/.*fly$/, (req, res) => {
  res.send('Path ends with "fly"');
});
```
Route Path: /.*fly$/
Matches: /butterfly, /dragonfly
Does Not Match: /flyby, /insect

6. ### Multi-Parameter Routes ###

You can define routes with multiple parameters, which allows matching more complex patterns with several variables in the URL.

Example:

```
app.get('/user/:userId/book/:bookId', (req, res) => {
  res.send(`User ID: ${req.params.userId}, Book ID: ${req.params.bookId}`);
});
```
Route Path: /user/:userId/book/:bookId
Matches: /user/123/book/456, /user/john/book/789
Does Not Match: /user/123, /book/456

7. ### Named Groups with Regular Expressions ###
Express allows named capture groups in regular expressions, providing more readable routes when using regex.

Example:

```
app.get(/\/user\/(?<userId>\d+)\/book\/(?<bookId>\d+)/, (req, res) => {
  const { userId, bookId } = req.params;
  res.send(`User ID: ${userId}, Book ID: ${bookId}`);
});
```
Route Path: /user/(?<userId>\d+)/book/(?<bookId>\d+)
Matches: /user/123/book/456
Does Not Match: /user/abc/book/def

8. ### Subdomains or Hostname-based Routes ###

Some routing systems can use subdomains or specific hostnames for routing, though this is less common in Express and might require middleware or specific server setups.

## Query strings are not part of the route path. ##

Query strings are not part of the route path because they serve a different purpose in the URL structure. Here's why:

1. Separation of Concerns:

* Route Paths define the specific endpoints of a web application that respond to requests. They determine which handler or controller should be invoked.
* Query Strings provide additional information or parameters for the server to process, often modifying the response based on the given data.

2. Structure:

* The route path is the fixed part of the URL that defines the resource being accessed (e.g., /search).
* The query string starts after a question mark (?) and is composed of key-value pairs (e.g., ?term=express), allowing for flexible and optional parameters that do not change the endpoint itself.

3. Functionality:

* Route paths are used to match specific requests to specific functions or actions.
* Query strings allow for variable data input without altering the route or endpoint itself, making the endpoint reusable with different parameters.a

## Middleware ##

<Refer>: [https://expressjs.com/en/guide/writing-middleware.html]()
then:
<Refer>: [https://expressjs.com/en/guide/using-middleware.html]()

# API (Application Programming Interface)

* They are a set of rules and protocols when building and interacting with what software application you need to implement on your project maybe (like getting google map's' api on your delivary project)
* this allows different software systems to communicate with each other.

## Types of API's ##
    * **Rest**: Uses HTTP requests (HTTP methods) to interact with resources.
    * **Soap**: Uses XML-based messages for communications.
    * **GraphQL**: Allows clients to request specific data.

# Cookies (with Sessions) #

* cookies are a browser storage that has 4kb of data storage where the server can keep their datas who can have differnet functionalitites for visiting that server

They are:
    i. **session cookies** which temporarly stores data for a single session like user login status or form inputs. They are deleted when browser is closed

    ii. **persistant cookies** which store data across sessions like user preferences for a website or their login credentials. They have an expiration date or remain until that date or until deleted manually deleted.

    iii. **third-party cookies** which are set by the domains than the one you're visiting like third party ads often for tracking and advertising purposes

    iv. **secure-cookies** which are transimitted only over https, ensuring they are encrypted during transmission. Used to enchance security by protecting sensitive data.

    v. **httpOnlyCookies** which are not accessible via JS which reduces the risk of cross-site scripting (XSS) attacks. This is used to store session tokens securly

## Purpose of the Secret Key ##

The secret key is used to sign the session ID cookie that is sent to the client (user's browser). Here's what happens in detail:

    1. **Session ID Generation and Signing:**

        * When a user initiates a session (e.g., by logging in), the server generates a unique session ID.

        * This session ID is then signed with the secret key using a hash function. This process creates a hash (a unique string) that represents the session ID but is secure and can't be easily tampered with.

        * The signed session ID is then stored in a cookie (usually named connect.sid) and sent to the client.

    2. **Client Stores the Cookie:**

        * The client’s browser stores the session cookie and sends it back to the server with every subsequent request.

    3. **Server Validates Session:**
        * On each request, the server checks the session cookie sent by the client.
        * It verifies the signature of the session ID using the same secret key. If the session ID has been altered in any way, the hash will not match, indicating that the session is invalid or has been tampered with.

        * If the hash matches, the server considers the session valid and retrieves the corresponding session data from the session store (e.g., MongoDB, Redis, in-memory, etc.).

## Local Storage and Session storage ##

* Both storages can have 5-10 Mb of storage which depends on the browser.

* In **local storage** data remains even after the browser is closed. This is used to store user preferences or settings that should persist across sessions.

* In **session storage** the data is cleared when the tab or window is closed. This is used to store temporary data like the cart details you add when you browse through an ecommerce site (like the item added in cart shouldn't be removed when the user browses through the site), similarly for form inputs like the user shouldn't need to verify each time they acquire a new url or for the UI state.
 
