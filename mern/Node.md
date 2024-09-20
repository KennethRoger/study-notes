# DEEP LEARNING OF NODEJS #

## NodeJS ##

Node.js runs the V8 JavaScript engine, the core of Google Chrome, outside of the browser. A Node.js app runs in a single process, without creating a new thread for every request. Node.js provides a set of asynchronous I/O primitives in its standard library that prevent JavaScript code from blocking.
When Node.js performs an I/O operation, like reading from the network, accessing a database or the filesystem, instead of blocking the thread and wasting CPU cycles waiting, Node.js will resume the operations when the response comes back.

### An Example Node.js Application ###

```
const { createServer } = require('node:http');

const hostname = '127.0.0.1';
const port = 3000;

const server = createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
To run this snippet, save it as a server.js file and run node server.js in your terminal.

The **createServer()** method of **http** creates a new HTTP server and returns it.
Whenever a new request is received, the **request** event is called, providing two objects: a request (an **http.IncomingMessage** object) and a response (an **http.ServerResponse** object).
Those 2 objects are essential to handle the HTTP call.
The first provides the *request* details.

The second is used to *return data to the caller*.

Some examples include:
```
res.statusCode = 200;
res.setHeader('Content-Type', 'text/plain');
res.end('Hello World\n');
```

### Differences between Node.js and the Browser ###

In the browser, most of the time what you are doing is interacting with the DOM, or other Web Platform APIs like Cookies. Those do not exist in Node.js, of course. You don't have the **document**, **window** and all the other objects that are provided by the browser.

And in the browser, we don't have all the nice APIs that Node.js provides through its modules, like the filesystem access functionality.

Another big difference is that in Node.js you control the environment.

you can use both **require()** and **import** in Node.js, while you are limited to import in the browser.

### the npm package manager ##

**npm** is the standard package manager for Node.js.
The difference between *devDependencies* and dependencies is that the former contains development tools, like a testing library, while the latter is bundled with the app in production. As for the *optionalDependencies* the difference is that build failure of the dependency will not cause installation to fail. But it is your program's responsibility to handle the lack of the dependency.

### Running Tasks ###

The package.json file supports a format for specifying command line tasks that can be run by using
```
npm run <task-name>
```
It's very common to use this feature to run Webpack:
```
{
  "scripts": {
    "watch": "webpack --watch --progress --colors --config webpack.conf.js",
    "dev": "webpack --progress --colors --config webpack.conf.js",
    "prod": "NODE_ENV=production webpack -p --config webpack.conf.js"
  }
}
```
So instead of typing those long commands, which are easy to forget or mistype, you can run
```
$ npm run watch
$ npm run dev
$ npm run prod
```

### Asynchronous flow control ###

complex operations are made up of many functions:

* initiator style / input
* middleware
* terminator

The "initiator style / input" is the first function in the sequence. This function will accept the original input, if any, for the operation. The operation is an executable series of functions, and the original input will primarily be:

  1. variables in a global environment
  2. direct invocation with or without arguments
  3. values obtained by file system or network requests

Network requests can be incoming requests initiated by a foreign network, by another application on the same network, or by the app itself on the same or foreign network.

A middleware function will return another function, and a terminator function will invoke the callback. The following illustrates the flow to network or file system requests. Here the latency is 0 because all these values are available in memory.

```
function final(someInput, callback) {
  callback(`${someInput} and terminated by executing callback `);
}

function middleware(someInput, callback) {
  return final(`${someInput} touched by middleware `, callback);
}

function initiate() {
  const someInput = 'hello this is a function ';
  middleware(someInput, function (result) {
    console.log(result);
    // requires callback to `return` result
  });
}

initiate();
```
## State management ##

Functions may or may not be state dependent. State dependency arises when the input or other variable of a function relies on an outside function.

In this way there are two primary strategies for state management:

  1. passing in variables directly to a function, and
  2. acquiring a variable value from a cache, session, file, database, network, or other outside source.

## Control flow ##

If an object is available in memory, iteration is possible, and there will 
not be a change to control flow:
```
function getSong() {
  let _song = '';
  let i = 100;
  for (i; i > 0; i -= 1) {
    _song += `${i} beers on the wall, you take one down and pass it around, ${
      i - 1
    } bottles of beer on the wall\n`;
    if (i === 1) {
      _song += "Hey let's get some more beer";
    }
  }

  return _song;
}

function singSong(_song) {
  if (!_song) throw new Error("song is '' empty, FEED ME A SONG!");
  console.log(_song);
}

const song = getSong();
// this will work
singSong(song);
```
However, if the data exists outside of memory the iteration will no longer work:
```
function getSong() {
  let _song = '';
  let i = 100;
  for (i; i > 0; i -= 1) {
    /* eslint-disable no-loop-func */
    setTimeout(function () {
      _song += `${i} beers on the wall, you take one down and pass it around, ${
        i - 1
      } bottles of beer on the wall\n`;
      if (i === 1) {
        _song += "Hey let's get some more beer";
      }
    }, 0);
    /* eslint-enable no-loop-func */
  }

  return _song;
}

function singSong(_song) {
  if (!_song) throw new Error("song is '' empty, FEED ME A SONG!");
  console.log(_song);
}

const song = getSong('beer');
// this will not work
singSong(song);
// Uncaught Error: song is '' empty, FEED ME A SONG!
```
**setTimeout** instructs the CPU to store the instructions elsewhere on the bus, and instructs that the data is scheduled for pickup at a later time. Thousands of CPU cycles pass before the function hits again at the 0 millisecond mark, the CPU fetches the instructions from the bus and executes them. The only problem is that song ('') was returned thousands of cycles prior.

The same situation arises in dealing with file systems and network requests. The main thread simply cannot be blocked for an indeterminate period of time-- therefore, we use callbacks to schedule the execution of code in time in a controlled manner.

You will be able to perform almost all of your operations with the following 3 patterns:

1. In series: functions will be executed in a strict sequential order, this one is most similar to for loops.

```
// operations defined elsewhere and ready to execute
const operations = [
  { func: function1, args: args1 },
  { func: function2, args: args2 },
  { func: function3, args: args3 },
];

function executeFunctionWithArgs(operation, callback) {
  // executes function
  const { args, func } = operation;
  func(args, callback);
}

function serialProcedure(operation) {
  if (!operation) process.exit(0); // finished
  executeFunctionWithArgs(operation, function (result) {
    // continue AFTER callback
    serialProcedure(operations.shift());
  });
}

serialProcedure(operations.shift());
```

2. Full parallel: when ordering is not an issue, such as emailing a list of 1,000,000 email recipients.

```
let count = 0;
let success = 0;
const failed = [];
const recipients = [
  { name: 'Bart', email: 'bart@tld' },
  { name: 'Marge', email: 'marge@tld' },
  { name: 'Homer', email: 'homer@tld' },
  { name: 'Lisa', email: 'lisa@tld' },
  { name: 'Maggie', email: 'maggie@tld' },
];
function dispatch(recipient, callback) {
  // `sendEmail` is a hypothetical SMTP client
  sendMail(
    {
      subject: 'Dinner tonight',
      message: 'We have lots of cabbage on the plate. You coming?',
      smtp: recipient.email,
    },
    callback
  );
}
function final(result) {
  console.log(`Result: ${result.count} attempts \
      & ${result.success} succeeded emails`);
  if (result.failed.length)
    console.log(`Failed to send to: \
        \n${result.failed.join('\n')}\n`);
}
recipients.forEach(function (recipient) {
  dispatch(recipient, function (err) {
    if (!err) {
      success += 1;
    } else {
      failed.push(recipient.name);
    }
    count += 1;
    if (count === recipients.length) {
      final({
        count,
        success,
        failed,
      });
    }
  });
});
```

3. Limited parallel: parallel with limit, such as successfully emailing 1,000,000 recipients from a list of 10 million users.

```
let successCount = 0;
function final() {
  console.log(`dispatched ${successCount} emails`);
  console.log('finished');
}
function dispatch(recipient, callback) {
  // `sendEmail` is a hypothetical SMTP client
  sendMail(
    {
      subject: 'Dinner tonight',
      message: 'We have lots of cabbage on the plate. You coming?',
      smtp: recipient.email,
    },
    callback
  );
}
function sendOneMillionEmailsOnly() {
  getListOfTenMillionGreatEmails(function (err, bigList) {
    if (err) throw err;
    function serial(recipient) {
      if (!recipient || successCount >= 1000000) return final();
      dispatch(recipient, function (_err) {
        if (!_err) successCount += 1;
        serial(bigList.pop());
      });
    }
    serial(bigList.pop());
  });
}
sendOneMillionEmailsOnly();
```

Each has its own use cases, benefits, and issues you can experiment and read about in more detail. Most importantly, remember to modularize your operations and use callbacks! If you feel any doubt, treat everything as if it were middleware!

## Middleware ##

In web development, particularly in the context of frameworks like Express.js, **middleware** refers to functions that sit in the middle of the request-response cycle. Middleware functions have access to the `request` object (`req`), the `response` object (`res`), and the next middleware function in the application’s request-response cycle. These functions can perform various tasks before passing control to the next middleware function or sending a response to the client.

**Types of Middleware:**

* Application-level Middleware: Bound to an instance of app using app.use() or app.METHOD().
* Router-level Middleware: Bound to an instance of express.Router() using router.use() or router.METHOD().
* Built-in Middleware: Provided by Express, such as express.static for serving static files or express.json() for parsing JSON request bodies.
* Third-party Middleware: Middleware provided by third-party libraries, such as body-parser for parsing request bodies or cors for enabling Cross-Origin Resource Sharing.

**Common Use Cases:**

* Logging requests.
* Parsing request bodies (e.g., JSON, URL-encoded data).
* Implementing authentication and authorization.
* Handling errors.
* Serving static files.

Example with express:

```
const express = require('express');
const app = express();

// Application-level middleware
app.use((req, res, next) => {
    console.log('Request URL:', req.url);
    next(); // Pass control to the next middleware function
});

// Route-specific middleware
app.get('/user/:id', (req, res, next) => {
    console.log('Request Type:', req.method);
    next();
}, (req, res, next) => {
    res.send('USER');
});

// Error-handling middleware
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something broke!');
});

// Start the server
app.listen(3000, () => {
    console.log('Server is running on port 3000');
});
```
Next:

```
const express = require('express');
const app = express();

// Simple middleware function
const loggerMiddleware = (req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next(); // Pass control to the next middleware function or route handler
};

// Use the middleware in the application
app.use(loggerMiddleware);

// Route handler for the home page
app.get('/', (req, res) => {
    res.send('Hello, World!');
});

// Start the server
app.listen(3000, () => {
    console.log('Server is running on port 3000');
});
```
**"I/O" refers primarily to interaction with the system's disk and network supported by libuv**

## Blocking ##

**Blocking** is when the execution of additional JavaScript in the Node.js process must wait until a non-JavaScript operation completes. This happens because the event loop is unable to continue running JavaScript while a **blocking** operation is occurring.

In Node.js, JavaScript that exhibits poor performance due to being CPU intensive rather than waiting on a non-JavaScript operation, such as I/O, isn't typically referred to as **blocking**. Synchronous methods in the Node.js standard library that use libuv are the most commonly used **blocking** operations. Native modules may also have **blocking** methods.

All of the I/O methods in the Node.js standard library provide asynchronous versions, which are **non-blocking**, and accept callback functions. Some methods also have **blocking** counterparts, which have names that end with **Sync**

The Node.js fs module provides an API to interact with the file system. It offers both synchronous and asynchronous methods for performing file operations like reading, writing, and manipulating files and directories.

 list of common file system operations in Node.js, with both their synchronous and asynchronous versions side by side:

1. ### Reading a File ###

* **Asynchronous (fs.readFile)**

```
const fs = require('fs');
fs.readFile('example.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log(data);
});
```

* **Synchronous (fs.readFileSync)**

```
const fs = require('fs');
const data = fs.readFileSync('example.txt', 'utf8');
console.log(data);
```

2. ### Writing to a File ###

* **Asynchronous (fs.writeFile)**

```
const fs = require('fs');
fs.writeFile('example.txt', 'Hello, world!', (err) => {
    if (err) throw err;
    console.log('File written successfully');
});
```

* **Synchronous (fs.writeFileSync)**

```
const fs = require('fs');
fs.writeFileSync('example.txt', 'Hello, world!');
console.log('File written successfully');
```

3. ### Appending to a File ###

* **Asynchronous (fs.appendFile)**

```
const fs = require('fs');
fs.appendFile('example.txt', ' This is appended text.', (err) => {
    if (err) throw err;
    console.log('Text appended successfully');
});
```

* **Synchronous (fs.appendFileSync)**

```
const fs = require('fs');
fs.appendFileSync('example.txt', ' This is appended text.');
console.log('Text appended successfully');
```

4. ### Deleting a File ###

* **Asynchronous (fs.unlink)**

```
const fs = require('fs');
fs.unlink('example.txt', (err) => {
    if (err) throw err;
    console.log('File deleted');
});
```

* **Synchronous (fs.unlinkSync)**

```
const fs = require('fs');
fs.unlinkSync('example.txt');
console.log('File deleted');
```

5. ### Creating a Directory ###

* **Asynchronous (fs.mkdir)**

```
const fs = require('fs');
fs.mkdir('newDir', (err) => {
    if (err) throw err;
    console.log('Directory created');
});
```

* **Synchronous (fs.mkdirSync)**

```
const fs = require('fs');
fs.mkdirSync('newDir');
console.log('Directory created');
```

6. ### Removing a Directory ###

* **Asynchronous (fs.rmdir)**

```
const fs = require('fs');
fs.rmdir('newDir', (err) => {
    if (err) throw err;
    console.log('Directory removed');
});
```

* **Synchronous (fs.rmdirSync)**

```
const fs = require('fs');
fs.rmdirSync('newDir');
console.log('Directory removed');
```

7. ### Checking if a File Exists ###

* **Asynchronous (fs.access)**

```
const fs = require('fs');
fs.access('example.txt', fs.constants.F_OK, (err) => {
    console.log(err ? 'File does not exist' : 'File exists');
});
```

* **Synchronous (fs.existsSync)**

```
const fs = require('fs');
const exists = fs.existsSync('example.txt');
console.log(exists ? 'File exists' : 'File does not exist');
```

8. ### Renaming a File or Directory ###

* **Asynchronous (fs.rename)**

```
const fs = require('fs');
fs.rename('oldName.txt', 'newName.txt', (err) => {
    if (err) throw err;
    console.log('File renamed');
});
```

* **Synchronous (fs.renameSync)**

```
const fs = require('fs');
fs.renameSync('oldName.txt', 'newName.txt');
console.log('File renamed');
```

9. ### Reading a Directory ###

* **Asynchronous (fs.readdir)**

```
const fs = require('fs');
fs.readdir('newDir', (err, files) => {
    if (err) throw err;
    console.log(files);
});
```

* **Synchronous (fs.readdirSync)**

```
const fs = require('fs');
const files = fs.readdirSync('newDir');
console.log(files);
```

10. ### Watching a File for Changes ###

* **Asynchronous (fs.watch)**

```
const fs = require('fs');
fs.watch('example.txt', (eventType, filename) => {
    console.log(`File ${filename} changed with event type ${eventType}`);
});
```

## Concurrency and Throughput ##

JavaScript execution in Node.js is single threaded, so concurrency refers to the event loop's capacity to execute JavaScript callback functions after completing other work. Any code that is expected to run in a concurrent manner must allow the event loop to continue running as non-JavaScript operations, like I/O, are occurring.

The event loop is different than models in many other languages where additional threads may be created to handle concurrent work.

## Handling errors in callbacks ##

One very common strategy is to use what Node.js adopted: the first parameter in any callback function is the error object: error-first callbacks

If there is no error, the object is null. If there is an error, it contains some description of the error and other information.

## What is the Event Loop? ##

The event loop is what allows Node.js to perform non-blocking I/O operations — despite the fact that a single JavaScript thread is used by default — by offloading operations to the system kernel whenever possible.

Since most modern kernels are multi-threaded, they can handle multiple operations executing in the background. When one of these operations completes, the kernel tells Node.js so that the appropriate callback may be added to the **poll** queue to eventually be executed.

#Cluster#

cluster is a node js module used to create multiple instances of the node js process for parrallel execution. When a server needs to handle thousands of requests cluster is used

#Child Porcess#

Child process is also a node js module that of course creates multiple instance of the node js process but it is used to run other scripts or program from outside of the node js process.

#Worker Threads#

Worker threads does not make new instances but runs javascript in multiple threads in the same node js process
