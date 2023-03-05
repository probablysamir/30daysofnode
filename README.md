# 30 Days of N(c)ode

I will be learning and coding NodeJS for 30 days and I will be updating daily about my progress and understanding.

# Resources

I'll be posting more and more resources I encounter in my journey.

For now, these are the resources I'm following:

- [Roadmap](https://roadmap.sh/backend)
- [Youtube Playlist](https://www.youtube.com/watch?v=yo8xV6izqIk&list=PLd7dW_Jxkr_ZSPvEj297MGOU42KSRvml_)

You can manually scroll to check my progress or click these links directly to navigate:

- [Day 1](#Day-1)
- [Day 2](#Day-2)
- [Day 3](#Day-3)
- [Day 4](#Day-4)
- [Day 5](#Day-5)

# Day 1

### Server

Server is a software or hardware device that accepts and responds made over a network. The device that makes the request, and receives a response from the server, is called a client.

![Server responding to a request](https://th.bing.com/th/id/R.38c844f53fedb82d2952867258a5929d?rik=K1gigEKclUNHGg&riu=http%3a%2f%2fbytesofgigabytes.com%2fIMAGES%2fNetworking%2fHTTPcommuncation%2fhttp+communication.png&ehk=q2XcnKljyMTZbxn2qZeyxcET1Low9B4JZTlpnP%2f%2fb34%3d&risl=&pid=ImgRaw&r=0)

The client requests the server for some data and the server responds to the request.

**Advantages of using a server**

- Efficient storage and delivery of information

- Customized user experience

- Controlled access to content

- Notifications and communications

### HTTP

HTTP ( **Hypertext** Transfer Protocol ) is an application layer protocol in the Internet protocol suite model for distributed, collaborative, hypermedia information systems. It is used to send and receive webpages and files on the internet.

It was initially made to set certain rules and regulations for the transfer of HTML ( **Hypertext** Markup Language ) files.

# Day 2

### Creating a simple server using Node.js

Let us create some constants first to import http and fs.

``` 
const http = require('http');
const fs = require('fs');

```
To create a server, we create a constant which uses createServer function from http to create a server. The `createServer()` takes two arguments: `req` and `res`. `req` is the request made and `res` is the response sent from the server.
```
const server = http.createServer((req,res)=>{
    console.log('I am called everytime the browser makes a request');
    res.setHeader('Content-Type','text/html'); // informs the browser that the response is a html file.
    res.write('<h1>Hi there!!<h1>'); // this is the response text
    res.end();
});
```
Now we make sure that the server is listening to the desired port.

`server.listen` takes three arguments `portnumber`, `host` and `callback function`

```
server.listen(3000, "localhost", () => console.log("server is loading on port 3000"));
```

To run the server, store the code in a js file ( in my case `server.js` ). You can then run it using :
```
node server.js
```

![node-run](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Day02_dumps/Capture.PNG)

or you can run it using **nodemon**.

*nodemon is a module that develop node.js based applications by automatically restarting the node application when file changes in the directory are detected. To run the file using nodemon, you have to first install it if you haven't.*

To install nodemon:
```
npm install -g nodemon
```
you can then run the server.js by:
```
nodemon server.js
```
 
 ### Displaying a html file

 In order to display a html instead of just plain html line using `res.write()` we read the file first using `fs.readfile()`

 To read a file using `fs.readfile()`:

 ```
let path = './pages/index.html'

 fs.readFile(path, (err, fileData) => {
    if (err) console.log(err);
    else {
        res.write(fileData);
        res.end();
     });
 ```

![index-page](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Day02_dumps/Capture1.PNG)

we can replace `res.write(fileData)` and `res.end()` with just `res.end(fileData)` if we only have to write one response.

So alternatively the above can be shortened as:
```
let path = './pages/index.html'

fs.readFile(path, (err, fileData) => {
    err ? console.log(err) : res.end(fileData);
  });
```
We can also serve other various types content types. 
To serve a JSON file we can do:
```
  let jsonResponse = {
    status: 200,
    message: "successful",
    result: ["sunday", "monday", "tuesday", "wednesday", "thursday", "friday", "saturday"],
    code: 2000,
  };
  res.write(JSON.stringify(jsonResponse));
  res.end();
  ```

  To serve an audio file we can write: 
  ```
  res.setHeader("Content-Type", "audio/mp3");
  let rStream = fs.createReadStream("./Day02_dumps/music.mp3");
  rStream.pipe(res);
  ```
  Here we created a readable stream to read the file and then pipe it to the response because `res.write()` has to load the whole file before sending it to client but by using a readable stream and piping it to the response, you can avoid loading the entire file into memory and instead stream it to the client in chunks, which is more memory-efficient.

  ![audio-file](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Day02_dumps/Capture2.PNG)

  The audio file after served looks like this. Note that you should not use `res.end` when your'e using readable stream.

# Day 3
## Status Codes

Whenever a server gets a request and sends back the response, it sends back a status code to inform if the HTTP request was successfully completed.

The status code responses are grouped into five groups:

- Informational responses (`100`-`199`)
- Successful responses (`200`-`299`)
- Redirection responses (`300`-`399`)
- Client error responses (`400`-`499`)
- Server error responses (`500`-`599`)

The most commonly used status codes are:
- `200 OK` - The Request succeeded

- `301 Moved Permanently` - The URL of the requested resource has been changed permanently. The new URL is given in the response.

- `307 Temporary Redirect` - The server sends this response to direct the client to get the requested resource at another URI with the same method that was used in the prior request

- `304 Permanent Redirect` - This means that the resource is now permanently located at another URI, specified by the `Location:` HTTP Response header.

- `400 Bad Request` - The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).

- `401 Unauthorized` - The client must authenticate itself to get the requested response.

- `403 Forbidden` - The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike `401 Unauthorized`, the client's identity is known to the server.

- `404 Not Found` - The server cannot find the requested resource. Most likely, the user has entered an incorrect URL.

- `500 Internal Server Error` - The server has encountered a situation it does not know how to handle.

- `502 Bad Gateway` - This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.

- `503 Service Unavailable` - This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.

We can send back a status code with a response by simply assigning value to  `res.statusCode` 

You can know more about status codes by checking mdn docs or by simply clicking [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

## Creating different paths for different pages

We can create different paths for different pages by using conditional statement ( switch-case here ).
```
let path = "./pages";
switch (req.url) {
  case "/":
    path += "/index.html";
    res.statusCode = 200;
    break;
  case "/about-us":
    path += "/about.html";
    res.statusCode = 200;
    break;
  default:
    path += "/404.html";
    res.statusCode = 404;
}
```
Note that we also implemented the status code here. Since `.pages/index.html` and `.pages/about.html` are valid paths so we set it's status code to `200` which means that the request succeeded but if the entered url is something else then we send back the `404.html` that says the URL is incorrect and we set it's status code to `404`.

Sometimes we need to change the url of a page and to ensure that the client doesn't get a `404 error` when they enter the old URL, we create redirects. In the above scenario let us suppose that `localhost:3000/about-us` is changed to `localhost:3000/about` we then create the redirection by changing the initial case and adding another one. Let us first change the initial case to:
```
case "/about":
      path += "/about.html";
      res.statusCode = 200;
      break;
```
we then add the case for the old URL:
```
case "/about-me":
  res.statusCode = 301;
  res.setHeader("Location", "/about");
  res.end();
```
We set the respose header `Location:` to the new location i.e. `/about` and then send a status code of `301 Moved Permanently` to inform that the URL of the requested resource has been changed permanently and he new URL is given in the response. 

# Day 4

## The Node Architecture

Node.JS runtime has several dependencies but the most important ones are Javscript V8 engine and libuv. It also has other dependecies like http-parser, c-ares, openSSL, zlib but V8 engine and libuv are the core dependencies of Node.JS.

![Node Architecture](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/NodeJsArchitecture.png)

V8 engine helps to write javascript code in Node.

libuv gives node access to the underlying computer operating system, file system, networking and more. It also implements two of the most important features of Node.JS:
- Event Loop
- Thread Pool

Node.JS is single threaded by nature. This means that our code will be executed in the same thread. Which means if we have some synchronous ( blocking code ) intensive task in the thread, then the thread will be unavailable for the rest of the code until it is completed which makes it a huge problem when there are multiple users requesting access to the thread. 

Node.JS, however, has solved this problem with the help of thread pool. Whenever there is an intensive task that is to be performed, it is offloaded to the thread pool for processing and thus doesn't block the operations of the main thread.

![Thread Pool](https://devopedia.org/images/article/131/2362.1540794088.jpg)

### Event Loop
The event loop is what allows Node.js to perform non-blocking I/O operations — despite the fact that JavaScript is single-threaded — by offloading operations to the system kernel whenever possible.

![Event Loop](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture1.PNG)

Event loop processes the phases in the above order.

The phases are briefly discussed here:

- __timers :__ this phase executes callbacks scheduled by `setTimeout()` and `setInterval()`

- __pending callbacks :__ executes I/O callbacks deferred to the next loop iteration.

- __idle, prepare :__ only used internally.

- __poll :__  retrieve new I/O events; execute I/O related callbacks (almost all with the exception of close callbacks, the ones scheduled by timers, and `setImmediate()`); node will block here when appropriate.

- __check :__  `setImmediate()` callbacks are invoked here.

- __close callbacks :__ some close callbacks, e.g. `socket.on('close', ...)`.

To know about the event loop in detail you can visit the Node.js documentation or simply click [here](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/).

### Thread Pool:

A thread pool in Node.js is a mechanism that allows multiple tasks to be executed concurrently in separate threads, without blocking the main thread. When Node.js receives a request that requires an intensive operation, such as reading or writing to a file, it can offload that task to a worker thread in the thread pool. The worker thread performs the task in the background while the main thread continues to handle other requests.

The size of the thread pool is configurable in Node.js, and it can be adjusted based on the workload and the number of available processors on the system. By default, the thread pool size in Node.js is equal to the number of available CPUs on the system.

Using a thread pool in Node.js can significantly improve the performance and scalability of the application, especially when dealing with I/O-bound operations. It enables Node.js to handle multiple requests concurrently, without causing the main thread to block, resulting in a more responsive and efficient application.

Some of the intensive tasks that are offloaded to the thread pool are:
- File System APIs
- Cryptography
- Compression
- DNS Lookups

## Event Driven Architecture

Event-driven architecture is a programming paradigm in which software systems respond to events by triggering appropriate actions. In Node.js, event-driven architecture is achieved through the use of the Event Loop and the EventEmitter module.

In Event driven architecture, there are event emitters, event listeners and event handlers. Event emitter is responsible for emitting events that occur within the application or system. Other parts of the application can register themselves as listeners for specific events emitted by the event emitter and handle the event.

![Event Driven Architecture](https://i0.wp.com/www.tutorialspoint.com/nodejs/images/event_loop.jpg)

Let us create an event emitter, event listener, and an event listener to see event driven architecture in action.

Firstly, let's import the events module as EventEmitter.
```
const EventEmitter = require("events");
```
Now we will create a class `MyEmitter` that inherits the properties of `EventEmitter`
```
class MyEmitter extends EventEmitter {
  constructor() {
    super();
  }
}
```
We will now create an object of the class `MyEmitter`.
```
const myEmitter = new MyEmitter();
```
To set an event listener and event handler:
```
myEmitter.on("requestfile", () => {
  console.log("A file has been requested");
});
```
Here, we create an event listener for the named event `requestfile`. The attached callback function is the event handler of the event.

We then create an event emitter to invoke the event listeners and event handlers.
```
myEmitter.emit("requestfile");
```
Now, whenever this line of code is run i.e event is emitted, the event listeners will register the event and fire up the callback function which is the event handler of the event.


In a nutshell, an event emitter emits events, an event listeners listen to that event and the event handlers perform specific actions assigned to them when the event listeners register an event.

# Day 5

## Introduction to Express 

Express is a small framework that sits on top of Node.js’s web server functionality to simplify its APIs and add helpful new features.It makes it easier to organize your application’s functionality with middle ware and routing.

### Why Express Framework?

- It is time efficient.
- It is fast.
- It is economical.
- It is easy to learn.
- It is asynchronous ( non-blocking ) in nature

### Features of Express Framework

- Fast Server-Side Development
- Middleware
- Routing
- Templating
- Debugging

References taken from simplilearn and to know more about it you can click [here](https://www.simplilearn.com/tutorials/nodejs-tutorial/what-is-express-js).

## Express installation

To install express in your project you simply need to write `npm install express` in your terminal.

## Starting a simple server with Express

First you need to import the express module
```
const express = require('express')
const app = express();
```
then initialize the listener

`app.listen('3000')`

Now for a get request on the home page you can write:
```
app.get("/", (req, res) => {
  res.send(<h1>Hello there!!</h1>);
});
```

and that's it!! You made a simple server using express.

Now there are some points to be noted here. Firstly, we don't need to configure the localhost and we don't even need to specify the content type in the response header while using express. Also the code looks cleaner and much readable than writing it in Node.js

### Displaying a HTML file using Express

You can display a html file by writing:
```
app.get("/", (req, res) => {
  res.sendFile("./pages/index.html", { root: __dirname });
});
```
It sends back a file instead of a string. Note that there is another argument `{root:__dirname}` this is used to set the root directory as the curresnt directory and provide relative address for the file.

This argument is optional if you choose to give an absolute path ( *not recommended* )

You can set as many different html paths you like using the same way. Let us add another path that displays the about page.
```
app.get("/about", (req, res) => {
  res.sendFile("./pages/about.html", { root: __dirname });
});
```
Suppose this page's route was changed from `/about-us` to `/about`. We can add a redirect in the following way:
```
app.get("/about-us", (req, res) => {
  res.redirect("/about");
});
```
Note that we did not specify the status code. It's because express automatically sets up a status code for most of the responses.

Now for a 404 page we use `app.use` to send the 404 page if none of the path matches above.
```
app.use((req, res) => {
  res.status(404).sendFile("./pages/404.html", { root: __dirname });
});

```

Altogether the code looks like:
```
const express = require("express");

const app = express();

app.listen(3000);

app.get("/", (req, res) => {
  res.sendFile("./pages/index.html", { root: __dirname });
});
app.get("/about", (req, res) => {
  res.sendFile("./views/about.html", { root: __dirname });
});

//Redirect

app.get("/about-us", (req, res) => {
  res.redirect("/about");
});

//404 Page

app.use((req, res) => {
  res.status(404).sendFile("./views/404.html", { root: __dirname });
});

```

## HTTP Methods

HTTP defines a set of request methods to indicate the desired action to be performed for a given resource. 

Some of the commonly used methods are:

- `GET` - As the name suggests, it is used to get or retrieve data from the server.

- `POST` - It submits an entity to the specified resource. Simply, it used to add data

- `PUT` - It replaces all current representations of the target resource with the request payload.

- `PATCH` - It applies partial modifications to a resource. Simply, it is used to update data.

- `DELETE` - It deletes the specified resource.

You can learn about more HTTP methods from the MDN docs or by simply clicking [here](https://developer.mozilla.org/en-US/docs/web/http/methods).

### Postman

Postman is an API platform for building and using APIs which simplifies each step of the API lifecycle and streamlines collaboration so you can create better APIs—faster.

![postman-ui](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture.PNG)

You can specify the type of request in postman to get desired results

![postman-methods](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture(1).PNG)

Here's a code to read, add, update and delete a user information using post, patch and delete request respectively

```
//GET request
app.get("/user", (req, res) => {
  res.send(users);
});

//POST request
app.post("/user", (req, res) => {
  console.log(req.body);
  users = req.body;
  res.json({
    message: "data received succesfully",
    user: req.body,
  });
});

//PATCH request
app.patch("/user", (req, res) => {
  console.log("req-body", req.body);
  //update data in users obj
  let dataToBeUpdated = req.body;
  for (key in dataToBeUpdated) {
    users[key] = dataToBeUpdated[key];
  }
  res.json({
    message: "data updated succesfully",
    user: req.body,
  });
});

//DELETE request
app.delete("/user", (req, res) => {
  users = {};
  res.json({
    message: "Users deleted succesfully",
  });
});
```
