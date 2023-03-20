# 30 Days of N(c)ode

I will be learning and coding NodeJS for 30 days and I will be updating daily about my progress and understanding.

# Resources

I'll be posting more and more resources I encounter in my journey. Also I've placed reference links in the documentation too so that you can visit there for a deeper understanding.

For now, these are the resources I'm following:

- [Roadmap](https://roadmap.sh/backend)
- [Youtube Playlist](https://www.youtube.com/watch?v=yo8xV6izqIk&list=PLd7dW_Jxkr_ZSPvEj297MGOU42KSRvml_)

You can manually scroll to check my progress or click these links directly to navigate:

- [Day 1](#Day-1)
- [Day 2](#Day-2)
- [Day 3](#Day-3)
- [Day 4](#Day-4)
- [Day 5](#Day-5)
- [Day 6](#Day-6)
- [Day 7](#Day-7)
- [Day 8](#Day-8)
- [Day 9](#Day-9)
- [Day 10](#Day-10)
- [Day 11](#Day-11)
- [Day 12](#Day-12)
- [Day 13](#Day-13)
- [Day 14](#Day-14)
- [Day 15](#Day-15)
- [Day 16](#Day-16)
- [Day 17](#Day-17)
- [Day 18](#Day-18)
- [Day 19](#Day-19)

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
# Day 6

## REST API

REST API (Representational State Transfer Application Programming Interface) is an architectural style that enables communication between different systems and applications via HTTP requests. It is a popular approach for building web services that can be used by various clients, including web browsers, mobile apps, and other systems.

REST API uses HTTP methods such as GET, POST, PUT, PATCH, and DELETE to perform operations on resources represented by URLs. It relies on stateless, client-server communication and the use of standard data formats such as JSON or XML for data exchange.

![REST API](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture3.PNG)

While designing a REST API it should align to the following six REST design principles ( architectural constraints ):
- __Uniform Interface :__ All API requests for the same resource should look the same, no matter where the request comes from. The REST API should ensure that the same piece of data, such as the name or email address of a user, belongs to only one uniform resource identifier (URI). Resources shouldn’t be too large but should contain every piece of information that the client might need.

- __Client-server decoupling :__ In REST API design, client and server applications must be completely independent of each other. The only information the client application should know is the URI of the requested resource; it can't interact with the server application in any other ways. Similarly, a server application shouldn't modify the client application other than passing it to the requested data via HTTP.

- __Statelessness :__ REST APIs are stateless, meaning that each request needs to include all the information necessary for processing it. In other words, REST APIs do not require any server-side sessions. Server applications aren’t allowed to store any data related to a client request.

- __Cacheability :__ When possible, resources should be cacheable on the client or server side. Server responses also need to contain information about whether caching is allowed for the delivered resource. The goal is to improve performance on the client side, while increasing scalability on the server side.

- __Layered system architecture :__ In REST APIs, the calls and responses go through different layers. As a rule of thumb, don’t assume that the client and server applications connect directly to each other. There may be a number of different intermediaries in the communication loop. REST APIs need to be designed so that neither the client nor the server can tell whether it communicates with the end application or an intermediary.

- __Code on demand (optional) :__ REST APIs usually send static resources, but in certain cases, responses can also contain executable code (such as Java applets). In these cases, the code should only run on-demand.

References taken from IBM ( [here](https://www.ibm.com/topics/rest-apis) ).

## Middlewares

Middleware functions are functions that have access to the request object (req), the response object (res), and the next function in the application’s request-response cycle. The next function is a function in the Express router which, when invoked, executes the middleware succeeding the current middleware.

Middleware functions can perform the following tasks:
- Execute any code
- Make changes to the request and the response objects
- End the request-response cycle
- Call the next middleware in the stack

Diagramatic representation of request response cycle in express is shown below: 

![request response cycle](https://iq.opengenus.org/content/images/2019/08/Add-a-subheading--2-.png)

Note that if the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging.

# Day 7

## Environmental Variables

The variable which exists outside your code is a part of your server environment and can help you by both streamlining and making more secure the process of running your script and applications and they are called environmental variables.

During the application initialization, these are loaded into the `process.env` and accessed by suffixing the name of the variable.

We can set the environmental variables by setting the value before running the script. For example, to set the NODE_ENV variable to development we can run the command:
```
NODE_ENV=development nodemon server.js
```
We can also setup a new file to store all the values of the environmental variables. You can create a file with `.env` extension. In this case we create a file named `config.env`

We can then store all the variables. For e.g.
```
NODE_ENV=development
PORT=8000
USER=jonas
PASSWORD=123456
```
We can install the `dotenv` package by writing the command:
```
npm install dotenv
```
Now, to access the config file, we can import it by storing in a constant, in this case `dotenv`
```
const dotenv = require('dotenv')
```
we then set the path of the dotenv config to the file where we have stored the environmental variables
```
dotenv.config({path: './config.env' })
```
Note that these have to be done before importing the app module.

## Mongo DB

MongoDB is an open-source document-oriented database that is designed to store a large scale of data and also allows you to work with that data very efficiently. It is categorized under the NoSQL (Not only SQL) database because the storage and retrieval of data in the MongoDB are not in the form of tables. 

- __Document Model :__  It is a document-oriented database, which means that data is stored as documents, and documents are grouped in collections.

- __Sharding :__ Sharding is the process of splitting larger datasets across multiple distributed instances, or “shards.” When applied to particularly large datasets, sharding helps the database distribute and better execute what might otherwise be problematic and cumbersome queries.

- __Replication :__ Replication allows you to deploy multiple servers for disaster recovery and backup.

- __Authentication :__ Authentication ensures that only authorized users can access the database. MongoDB provides a number of authentication mechanisms for users to access the database.

- __Indexing :__ Without the right indexes, a database is forced to scan documents one by one to identify the ones that match the query statement.

- __Load Balancing :__ MongoDB supports large-scale load balancing via horizontal scaling features like replication and sharding.

You can visit the mongodb official website for more indepth insights or simply click [here](https://www.mongodb.com/features).

## Getting started with MongoDB

To use a collection ( creates a collection if it already doesn't exist ) in this case testdb you can write this command in mongosh (mongoshell) :
```
use testdb
```
To insert a document in the collection you can write:
```
db.user.insertOne({ name:"Peter",age:21,height:5.67 })
```

![Insert Document](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture4.PNG)

To check if it has been added to the db you can write:
```
db.user.find()
```

![Find Document](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture5.PNG)

To add multiple documents into the collection you can simply write:
```
db.user.insertMany([{ name:"Mary",age:19,height:5.32 },{ name:"Steve",age:24,height:5.69,gender:"Male"}])
```

![Insert Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture6.PNG)

To find the documents with age = 19, We can do it by :
```
db.user.find({age:19})
```

![Find Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture7.PNG)

To find the documents with age > 19 :
```
db.user.find({age:{$gt: 19}})
```

![Find Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture8.PNG)

To find the documents with age <= 21 and height > 5.50 :
```
db.user.find({age:{$lte: 21},height:{$gt:5.50}})
```

![Find Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture9.PNG)

To find the documents with either age <= 21 or height > 5.50 :
```
db.user.find({$or: [{age:{$lte: 21}},{height:{$gt:5.50}}]})
```

![Find Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture10.PNG)

# Day 8

## Getting started with MongoDB (continued...)

To update a single document, you can use `db.collection.updateOne()` and `db.collection.updateMany()` to update multiple documents. 

For e.g, to update the age of the document with name peter to 21 you can simply write:

```
db.user.updateOne({name:'Peter'},{$set:{age:21}})
```

![Update Document](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture11.PNG)

To update the age of the documents with height >=5.67 to 23 we can write:
```
db.user.updateMany({height:{$gte:5.67}},{$set:{age:23}})
```

![Update Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture12.PNG)

To delete a single document you can use `db.collection.deleteOne()` and `db.collection.deleteMany()` to delete multiple documents.

To delete a document with name peter you can write:
```
db.user.deleteOne({name:'Peter'})
```

![Delete Document](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture13.PNG)

To delete multiple documents with age >=19 you can write:
```
db.user.deleteMany({age:{$gte:19}})
```

![Delete Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture14.PNG)

You can also pass empty object to delete all documents:
```
db.user.deleteMany({})
```

![Delete Documents](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture15.PNG)

## Getting familiar with the tools

These are the tools we'll be using to work with MongoDB:

- __MongoDB Compass :__ MongoDB Atlas is a cloud-based database service provided by MongoDB Inc. It allows users to easily deploy and manage MongoDB databases on various cloud platforms such as AWS, Google Cloud Platform, and Microsoft Azure. MongoDB Atlas offers a range of features such as automated backups, auto-scaling, and global distribution for high availability and low latency.

- __MongoDB Atlas :__ MongoDB Atlas is a cloud-based database service provided by MongoDB Inc. It allows users to easily deploy and manage MongoDB databases on various cloud platforms such as AWS, Google Cloud Platform, and Microsoft Azure. MongoDB Atlas offers a range of features such as automated backups, auto-scaling, and global distribution for high availability and low latency.

- __Mongoose :__ Mongoose is an Object-Document Mapping (ODM) library for Node.js and MongoDB. It allows developers to interact with MongoDB using a simple and consistent API, and provides features such as schema definition, validation, and middleware. Mongoose simplifies the process of defining data models and querying the database, and is widely used in Node.js applications that use MongoDB as their data store.

## Model and schema in Mongoose

In Mongoose, a schema is a blueprint for defining the structure and data types of documents (records) that will be stored in a MongoDB database collection. It defines the fields and their properties, such as data type, validation rules, default values, and indexes.

A schema is typically defined using the Mongoose Schema class, which allows you to define fields using JavaScript data types such as String, Number, Date, Boolean, and more. You can also define sub-documents and arrays of sub-documents using nested schemas.

Here is an example of a simple Mongoose schema for a user information:
```
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true,'A user should have a name']
  },
  age: {
    type: Number,
    required: [true,'A user should have an age']
  }
  isVeg: {
    type: Boolean,
    default: false
  }
});
```

In Mongoose, a model is a constructor function that is compiled from a schema definition and represents a collection in MongoDB. A model provides an interface for querying and manipulating documents that belong to a specific collection.

You can create a model in Mongoose by calling the mongoose.model() method and passing in the name of the model, the schema definition, and optionally a collection name (if you want to use a different name than the default, which is based on the model name).

Here is an example of creating a Mongoose model for the user schema we defined earlier:
```
const User = mongoose.model('User',userSchema)
```

We can then further create a document using the model. For example:
```
const testUser = new User({
  name: 'Peter',
  age: 19,
  isVeg: false
});
```
To save the document in the data base we can use `testUser.save()`. Since it returns a promise we can write:

```
testUser
  .save()
  .then((doc) => {
    console.log(doc);
  })
  .catch((err) => {
    console.log('Error : ', err);
  });
```

# Day 9

## MVC Architecture

MVC stands for Model-View-Controller and is a software architectural pattern used in designing applications. It separates an application into three interconnected components: the __Model, View, and Controller.__

- __Model :__ The backend that contains all the data logic.
- __View :__ The frontend or graphical user interface (GUI)
- __Controller :__ The brains of the application that controls how data is displayed

![MVC Architecture](https://www.freecodecamp.org/news/content/images/size/w1600/2021/04/MVC3.png)

The key benefit of the MVC architecture is that it separates concerns and allows for the individual components to be developed, tested, and maintained independently. This makes the code more modular, easier to understand, and easier to maintain over time. 

### Features of MVC architecture:

- __Modularity__: Promotes a modular approach to application development.
- __Separation of concerns__: Ensures each component has a distinct responsibility.
- __Testability__: Makes it easier to test individual components.
- __Flexibility__: Allows for flexibility in application design.
- __Code reusability__: Allows for code reuse across different applications.
- __Scalability__: Can be scaled to meet the needs of larger applications.
- __Maintenance__: Makes it easier to maintain an application over time.

## Implementing MVC architecture :

The Model is responsible for representing the data and the rules that govern how the data can be accessed and manipulated. It contains the business logic, which defines the operations and processes that the application must perform to implement the business rules.

The Controller acts as an intermediary between the View and the Model components. It receives input from the user via the View, processes the input, and updates the Model accordingly. It also contains the application logic, which defines the behavior of the application and determines how it responds to user input.

Let's create a tour model using mongoose and export it :
```
const mongoose = require('mongoose');

const tourSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'A tour must have a name'],
    unique: true,
    trim: true,
  },
  duration: {
    type: Number,
    required: [true, 'A tour must have a duration'],
  },
  maxGroupSize: {
    type: Number,
    required: [true, 'A tour must have a group size'],
  },
  difficulty: {
    type: String,
    required: [true, 'A tour must have a difficulty'],
  },
});

const Tour = mongoose.model('Tour', tourSchema);

module.exports = Tour;
```

We now create a controller to create a new tour and store in the database by referring to the tour model:
```
const Tour = require('../models/tourModel');

exports.createTour = async (req, res) => {
  try {
    const newTour = await Tour.create(req.body);
    res.status(201).json({
      staus: 'success',
      data: {
        tour: newTour,
      },
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: 'Invalid data sent',
    });
  }
};
```

We store these in different folders; `controllers` and `models`. 

To read, update and delete documents, you can add these in the controller:
```
exports.getAllTours = async (req, res) => {
  try {
    const tours = await Tour.find();
    res.status(200).json({
      status: 'success',
      results: tours.length,
      data: { tours },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};

exports.getTour = async (req, res) => {
  try {
    const tour = await Tour.findById(req.params.id);
    res.status(200).json({
      status: 'success',
      data: { tour },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }

exports.updateTour = async (req, res) => {
  try {
    const tour = await Tour.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
      runValidators: true,
    });
    res.status(200).json({
      status: 'success',
      data: {
        tour,
      },
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: 'Invalid data sent',
    });
  }
};

exports.deleteTour = async (req, res) => {
  try {
    await Tour.findByIdAndDelete(req.params.id);
    res.status(204).json({
      status: 'success',
      data: null,
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: 'Invalid data sent',
    });
  }
};
```

# Day 10

## Script to read data from json file and upload to database

Here's the script to read data from json file and upload to database:
```
const fs = require('fs');
const mongoose = require('mongoose');
const dotenv = require('dotenv');
const Tour = require('../../models/tourModel');

dotenv.config({ path: `${__dirname}/../../config.env` });

const DB = process.env.DATABASE.replace(
  '<PASSWORD>',
  process.env.DATABASE_PASSWORD
);

mongoose.connect(DB).then(() => {
  console.log('DB connection succesful');
});

//READ JSON FILE
const tours = JSON.parse(
  fs.readFileSync(`${__dirname}/tours-simple.json`, 'utf-8')
);

//IMPORT DATA INTO DATABASE
const importData = async () => {
  try {
    await Tour.create(tours);
    console.log('Data succesfully loaded');
    process.exit();
  } catch (err) {
    console.log(err);
  }
};

const deleteData = async () => {
  try {
    await Tour.deleteMany();
    console.log('Data succesfully deleted!');
    process.exit();
  } catch (err) {
    console.log(err);
  }
};

if (process.argv[2] === '--import') importData();
if (process.argv[2] === '--delete') deleteData();

console.log(process.argv);
```
Use `node script.js --delete` to delete the data and `node script.js --import` to import the data.

# Day 11

## Handling Queries

Handling queries is an important aspect of designing and implementing an API. Queries allow API users to specify the specific data they need to retrieve from the API, which can help reduce the amount of data transmitted over the network and improve performance.

Importance of handling queries are:

- __Efficiency:__ Queries can help reduce the amount of data returned by the API, which can improve the efficiency and speed of the API. By allowing users to specify only the data they need, we can reduce the amount of data transferred over the network and minimize server load.

- __Flexibility:__ Query parameters can provide users with the flexibility to customize their requests and retrieve data based on specific criteria. This can be useful in cases where the user only needs a subset of the available data, or where the data needs to be filtered or sorted in a particular way.

- __Usability:__ By providing query parameters, we can make our API more user-friendly and easier to use. Query parameters can provide users with a more intuitive way to interact with the API, as they can specify their requests using familiar URL syntax.

- __Compatibility:__ Query parameters are a widely accepted standard in web development, and are compatible with a variety of programming languages and platforms. By implementing query parameters in your API, you can make it easier for developers to integrate your API into their applications.

## Handling queries with mongoose

We first create a copy of the `req.query` using the spread operator:
```
const queryObj = { ...req.query };
```
We now filter `sort`, `page`, `limit` and `field` so that we can handle the specific queries separate and treat sorting and other stuffs independently
```
const excludedFields = ['page', 'sort', 'limit', 'fields'];
excludedFields.forEach((element) => delete queryObj[element]);
```
We can then use advanced filtering to filter out the results using gte,gt,lte and lt
```
let queryStr = JSON.stringify(queryObj);
queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, (match) => `$${match}`);
let query = Tour.find(JSON.parse(queryStr));
```
### Sorting in Mongoose

Sorting is done by using the `query.sort()` function
```
if (req.query.sort) {
  const sortBy = req.query.sort.split(',').join(' ');
  query = query.sort(sortBy);
} else {
  query = query.sort('-createdAt');
}
```
Here `split()` and `join()` are used to sort queries based on multiple parameters.

# Day 12

## Handling Queries (continued...)

### Field Limiting 

Field limiting in queries means specifying which fields in a document should be returned by a query. It helps to reduce query result size, improve query performance, and increase security by preventing unauthorized access to sensitive data.

To do field limiting in mongoose:
```
if (req.query.fields) {
  const fields = req.query.fields.split(',').join(' ');
  query = query.select(fields);
} else {
  query = query.select('-__v');
}
```

### Pagination

Pagination refers to the process of dividing large sets of database records into smaller, more manageable chunks called "pages." Pagination allows you to retrieve only a subset of the data at a time, rather than loading all the records at once.

In Mongoose, pagination can be implemented using the `skip()` and `limit()` methods. The `skip()` method skips a specified number of records in the result set, while the `limit()` method specifies the maximum number of records to return in each page.

To achieve pagination in mongoose:
```
const page = req.query.page * 1 || 1;
const limit = req.query.limit * 1 || 100;
const skip = (page - 1) * limit;
query = query.skip(skip).limit(limit);
```
We can also throw back an error if we skip more documents than the total number of documents available. We can do this by:
```
if (req.query.page) {
  const numTours = await Tour.countDocuments();
  if (skip >= numTours) throw new Error('This page does not exist');
}
```

### Executing the query

Finally after modifying the query according to our demands we then retrieve the data. We can do this using `await`:
```
const tours = await query;
```

## Refactoring Queries

Although the above methods work fine but it is not reusable and is hard to read so we refactor our code to make it more reusable and easy to read and work with.

In order to do that we create a class and apply various methods:
```
class APIfeatures {
  constructor(query, queryString) {
    this.query = query;
    this.queryString = queryString;
  }

  filter() {
    //BUILD QUERY
    const queryObj = { ...this.queryString };
    const excludedFields = ['page', 'sort', 'limit', 'fields'];
    excludedFields.forEach((element) => delete queryObj[element]);

    //ADVANCED FILTERING
    let queryStr = JSON.stringify(queryObj);
    queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, (match) => `$${match}`);

    this.query = this.query.find(JSON.parse(queryStr));

    return this;
  }

  sort() {
    //SORTING
    if (this.queryString.sort) {
      const sortBy = this.queryString.sort.split(',').join(' ');
      this.query = this.query.sort(sortBy);
    } else {
      this.query = this.query.sort('-createdAt');
    }

    return this;
  }

  limitFields() {
    //FIELDLIMITING
    if (this.queryString.fields) {
      const fields = this.queryString.fields.split(',').join(' ');
      this.query = this.query.select(fields);
    } else {
      this.query = this.query.select('-__v');
    }

    return this;
  }

  paginate() {
    //PAGINATION
    const page = this.queryString.page * 1 || 1;
    const limit = this.queryString.limit * 1 || 100;
    const skip = (page - 1) * limit;
    this.query = this.query.skip(skip).limit(limit);

    return this;
  }
}
```
We can then store this in a separate file and then export by writing:
```
module.exports = APIfeatures;
```

To execute the query we can simply write :
```
//EXECUTE QUERY
const features = new APIfeatures(Tour.find(), req.query)
  .filter()
  .sort()
  .limitFields()
  .paginate();
const tours = await features.query;
```
This way the code is more organized, reusable and easy to read

## Introduction to Aggregation Pipeline

Aggregation Pipeline is a powerful feature of MongoDB that enables data processing and transformation of documents in a collection. It is a framework for performing data aggregation operations on a collection in MongoDB, similar to SQL's GROUP BY clause. It allows you to perform a series of data processing steps on the input documents to produce a final output.

The aggregation pipeline in MongoDB is a sequence of stages, where each stage is a transformation operation on the input documents. Each stage takes the output of the previous stage and passes it to the next stage. The pipeline supports a wide range of data manipulation operations, such as filtering, grouping, sorting, projecting, and aggregating.

![Aggregation Pipeline](https://www.c-sharpcorner.com/article/aggregation-pipeline-in-mongodb/Images/image001.gif)

# Day 13

## Aggregation Pipeline

An aggregation pipeline consists of one or more stages that process documents:

- Each stage performs an operation on the input documents. For example, a stage can filter documents, group documents, and calculate values.

- The documents that are output from a stage are passed to the next stage.

- An aggregation pipeline can return results for groups of documents. For example, return the total, average, maximum, and minimum values.

### Using Aggregation pipelines

Suppose we have a database storing the tours. We will be using aggregation pipeline to group the tours on the basis of difficulty and calculate the number of tours, number of ratings, average ratings, average price, minimum and maximum price of the tours of that difficulty.

To do that we use `Tour.aggregate()` function where `Tour` is the model of the document.

```
exports.getTourStats = async (req, res) => {
  try {
    const stats = await Tour.aggregate([
      {
      //To filter the data with average rating greater or equal to 4.5

        $match: { ratingsAverage: { $gte: 4.5 } },
      },
      {
        $group: {
          //Grouping the data based on difficulty
          _id: { $toUpper: '$difficulty' },

          //Adding 1 for each document passed
          numTours: { $sum: 1 },

          //Using the inbuilt keywords to calculate sum, average, maximum and minimum
          numRatings: { $sum: '$ratingsQuantity' },
          avgRating: { $avg: '$ratingsAverage' },
          avgPrice: { $avg: '$price' },
          minPrice: { $min: '$price' },
          maxPrice: { $max: '$price' },
        },
      },
      {
        //To sort the data on the basis of average price (ascending)
        $sort: { avgPrice: 1 },
      },
    ]);

    //Sending back the response of the data received from aggregation pipeline
    res.status(200).json({
      status: 'success',
      data: {
        stats,
      },
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: 'Invalid data sent',
    });
  }
};
```

We have exported the async function so that we can route it in the routes file. In order to do so:
```
const express = require('express');
const tourController = require('../controllers/tourController');

const router = express.Router();

router.route('/tour-stats').get(tourController.getTourStats);
```

Let us use aggrregation pipeline to group and get the data on the basis of months it will start and in a specific year:

For this, let us create a route:
```
router.route('/monthly-plan/:year').get(tourController.getMonthlyPlan);
```
then we create the async function to handle the request using aggregation:
```
exports.getMonthlyPlan = async (req, res) => {
  try {
    const year = req.params.year * 1;
    const plan = await Tour.aggregate([
      {

//It deconstruct an array field in a document and create separate output documents for each item in the array
        $unwind: '$startDates',
      },
      {
        $match: {
          startDates: {
            $gte: new Date(`${year}-01-01`),
            $lte: new Date(`${year}-12-31`),
          },
        },
      },
      {
        $group: {
          _id: { $month: '$startDates' },
          numTourStarts: { $sum: 1 },
          tours: { $push: '$name' },
        },
      },
      {

        //Create a field month with same value of _id
        $addFields: { month: '$_id' },
      },
      {
        $project: {

          //Hides the id from the output
          _id: 0,
        },
      },
      {

        //Sorts the data according to num of tours in a month (descending)
        $sort: { numTourStarts: -1 },
      },
      {

        //Limits the output to 12 items (not necessary)
        $limit: 12,
      },
    ]);
    res.status(200).json({
      status: 'success',
      data: {
        plan,
      },
    });
  } catch {
    res.status(400).json({
      status: 'fail',
      message: 'Invalid data sent',
    });
  }
};
```

You can check more on aggregation pipeline in MongoDB's official documentation or simply click [here](https://www.mongodb.com/docs/manual/core/aggregation-pipeline/).

# Day 14

## Aggregation Pipeline Operators

Aggregation pipeline operators are tools used in MongoDB, a popular NoSQL database, to manipulate and process data in complex ways. These operators allow you to perform operations such as filtering, grouping, sorting, transforming, and combining data, using a pipeline of stages to progressively modify the data.

The pipeline consists of a series of stages, where each stage performs a specific operation on the data and passes the results to the next stage in the pipeline. The operators used in each stage determine the operation performed on the data. Some examples of aggregation pipeline operators include $match, $group, $sort, $project, $limit, $skip, $unwind, $lookup, $facet, and many others.

By using these operators in combination, you can create powerful and flexible queries that can handle a wide range of data processing tasks, from simple filtering and sorting to complex data transformations and aggregations.

### Expression Operators :
These expression operators are available to construct expressions for use in the aggregation pipeline stages.

Operator expressions are similar to functions that take arguments. In general, these expressions take an array of arguments and have the following form:
```
{ <operator>: [ <argument1>, <argument2> ... ] }
```
The types of expression operators are:

  - Arithmetic Expression Operators
  - Array Expression Operators
  - Boolean Expression Operators
  - Comparison Expression Operators
  - Conditional Expression Operators
  - Custom Aggregation Expression Operators
  - Data Size Operators
  - Date Expression Operators
  - Literal Expression Operators
  - Miscellaneous Operators
  - Object Expression Operators
  - Set Expression Operators
  - String Expression Operators
  - Test Expression Operators
  - Timestamp Expression Operators
  - Trigonometry Expression Operators
  - Type Expression Operators

To reduce the volume of the documentation and avoid just copy pasting everything from the official documentation I haven't documented the details, functions and examples of these operators.

You can manually open the official documentation of MongoDB or simply click [here](https://www.mongodb.com/docs/manual/reference/operator/aggregation/).

# Day 15

## Aggregation Pipeline Operators (cont...)

There are other type of Aggregation Pipeline Operators:

- __Accumulators:__ An accumulator is an operator that is used to aggregate data and compute aggregate values such as sum, count, average, and more.

  Accumulators are used in conjunction with the `$group` stage of the aggregation pipeline to `group` the documents by a specified field or set of fields, and then compute an aggregate value for each `group`. The accumulator operators take a field as input and produce a single output value for the `group`.

- __Window Operators:__  A window operator is an operator that is used to perform calculations on a subset of documents within a `group`.

  Window functions in MongoDB were introduced in version 3.6 of the MongoDB server. They operate on a set of documents within a specified window or frame, which is defined by an aggregation stage, and can be used to compute rankings, running totals, moving averages, and more.

## Aggregation Pipeline Optimization

Aggregation pipeline operations have an optimization phase which attempts to reshape the pipeline for improved performance.

### Projection Optimization:

Aggregation pipeline operations have an optimization phase which attempts to reshape the pipeline for improved performance.

When you use a `$project` operator it is usually written at the last stage of the pipeline. Using it at the beginning or in the middle is not helpful to improve performance, since the database performs this optimization automatically.

### Pipeline Sequence Optimization:

When using various stages in an aggregation, the aggregation pipeline will determine which stage should preceed the another and in times might destructure the stages to use it in an optimal sequence.

Some of the sequence optimizations in MongoDB are:

- __(`$project` or `$unset` or `$addFields` or `$set`) + `$match` Sequence Optimization:__

  Whenever there are these stages (projection stages) MongoDB moves the filter of the `$match` which do not depend on the computed value of the projection before the computation so that the documents in which it has to compute on is significantly reduced. 

- __``$sort`` + `$match` Sequence Optimization:__

  When you have a sequence with `$sort` followed by a `$match`, the `$match` moves before the `$sort` to minimize the number of objects to `$sort`.

- __`$redact`+`$match` Sequence Optimization:__

  When possible, when the pipeline has the $redact stage immediately followed by the `$match` stage, the aggregation can sometimes add a portion of the `$match` stage before the $redact stage. If the added `$match` stage is at the start of a pipeline, the aggregation can use an index as well as query the collection to limit the number of documents that enter the pipeline.

- __`$project`/`$unset` + `$skip` Sequence Optimization:__

  When you have a sequence with $project or $unset followed by $skip, the $skip moves before $project.

### Pipeline Coalescence Optimization:

When possible, the optimization phase coalesces(merges) a pipeline stage into its predecessor.

- __`$sort` + `$limit` Coalescence:__

  When a `$sort` precedes a `$limit`, the optimizer can coalesce the `$limit` into the `$sort` if no intervening stages modify the number of documents (e.g. `$unwind`, `$group`).

- __`$limit` + `$limit` Coalescence:__

  When a `$limit` immediately follows another `$limit`, the two stages can coalesce into a single `$limit` where the limit amount is the smaller of the two initial limit amounts.

- __`$skip` + `$skip` Coalescence:__

  When a `$skip` immediately follows another `$skip`, the two stages can coalesce into a single `$skip` where the skip amount is the sum of the two initial skip amounts

- __`$match` + `$match` Coalescence:__ 

  When a `$match` immediately follows another `$match`, the two stages can coalesce into a single `$match` combining the conditions with an `$and`.

- __`$lookup` + `$unwind` Coalescence:__ 

  When a `$unwind` immediately follows another `$lookup`, and the `$unwind` operates on the as field of the `$lookup`, the optimizer can coalesce the `$unwind` into the `$lookup` stage. This avoids creating large intermediate documents.

### Improve Performance with Indexes and Document Filters:

-  __Indexes:__

    An aggregation pipeline can use indexes from the input    collection to improve performance. Using an index limits the  amount of documents a stage processes. Ideally, an index can cover the stage query. A covered query has especiallly high performance, since the index returns all matching documents.

    For example, a pipeline that consists of `$match`, `$sort`, `$group` can benefit from indexes at every stage:
    - An index on the `$match` query field can efficiently identify the relevant data
    - An index on the sorting field can return data in sorted order for the `$sort` stage

    - An index on the grouping field that matches the `$sort` order can return all of the field values needed to execute the `$group` stage (a covered query)

- __Document Filters:__

  If your aggregation operation requires only a subset of the documents in a collection, filter the documents first:

    - Use the `$match`, `$limit`, and `$skip` stages to restrict the documents that enter the pipeline.

    - When possible, put `$match` at the beginning of the pipeline to use indexes that scan the matching documents in a collection.

    - `$match` followed by `$sort` at the start of the pipeline is equivalent to a single query with a sort, and can use an index.

To have deeper insights, you can visit MongoDB official documentation or simply click [here](https://www.mongodb.com/docs/manual/core/aggregation-pipeline-optimization/)

# Day 16

## Document Middleware:

Document middleware is supported for the following document functions.

- validate
- save
- remove
- updateOne
- deleteOne
- init

In document middlewares, `this` refers to the document.
Example of a document middleware is:
```
tourSchema.pre('save', function (next) {
  this.slug = slugify(this.name, { lower: true });
  next();
});
```
This runs before the save function this middleware adds slug field to the document using the `this` keyword.

Similarly, we can write a middleware for `model.post()`

## Query Middleware

Document middleware is supported for the following document functions.

- count
- countDocuments
- deleteMany
- deleteOne
- estimatedDocumentCount
- find
- findOne
- findOneAndDelete
- findOneAndRemove
- findOneAndReplace
- findOneAndUpdate
- remove
- replaceOne
- update
- updateOne
- updateMany
- validate

Query middleware executes when you call exec() or then() on a Query object, or await on a Query object. In query middleware functions, this refers to the query.

Example of query middleware:
```
tourSchema.pre(/^find/, function (next) {
  this.find({ secretTour: { $ne: true } });
  next();
});
```
Here, we used the regex expression `/^find/` instead of `find` so that the middleware for all find methods of mongoose. Here, the function chains the `.find()` method so that the document having the secretTour set to true is filtered out in the result.

Similarly, we can write a middleware for `model.post()`

## Aggregation Middleware

Aggregate middleware is for `MyModel.aggregate()`. Aggregate middleware executes when you call `exec()` on an aggregate object. In aggregate middleware, this refers to the aggregation object.

Example of aggregation middleware:
```
tourSchema.pre('aggregate', function (next) {
  this.pipeline().unshift({ $match: { secretTour: { $ne: true } } });
  next();
});
```
This adds yet another filter to the aggregation pipeline so that the secretTour data is not aggregated.

Similarly, we can write a middleware for `MyModel.post()`

# Day 17

## Validators

Validators in MongoDB and Mongoose are functions that are used to enforce certain constraints on the data that is stored in a database. Validators can be defined at the schema level, and can be used to ensure that the data that is stored in a particular field or document meets certain requirements.

### Built-In Validators

They are the validators that comes predefined with mongoose. We can simply specify the inbuilt validators like type, required, maxLength, etc. within the fieldname.

For Example:
```
 name: {
      type: String,
      required: [true, 'A tour must have a name'],
      maxLength: [40, 'A tour name must have less or equal than 40 characters'],
```

### Custom Validators

They are the validators that we define ourself or the validators that we import from external modules. We use the `validate:` field to define custom validators.

For Example:
```
    priceDiscount: {
      type: Number,
      validate: {
        validator: function (val) {
          // this only points to current doc on NEW document creation
          return val < this.price;
        },
        message: 'Discount price ({VALUE}) should be below regular price',
      },
    },
```

## NDB 

ndb is an open-source Node.js debugger built on top of Chrome DevTools. It provides a graphical user interface (GUI) that allows developers to inspect and debug Node.js applications in a browser-like environment. Some of the features of "ndb" include:

- __Breakpoints:__ Developers can set breakpoints in their code and step through it line-by-line to see how it executes.

- __Console:__ "ndb" provides a console where developers can interact with their code and run arbitrary JavaScript commands.

- __Call stack:__ The call stack shows the current state of the code and allows developers to see how functions are called and in what order.

- __Memory profiling:__ "ndb" includes a memory profiler that helps developers identify memory leaks and other performance issues in their code.

Overall, ndb is a powerful tool that makes it easier for developers to debug Node.js applications and find and fix bugs more efficiently.

# Day 18

## Class Validators

Class validators are functions or methods that validate the input data of a class or object. These validators are typically used to ensure that the input data meets certain requirements or constraints before it is processed by the class or object.

For example, consider a class that represents a user profile. This class may have properties such as "username", "email", and "password". In order to ensure that these properties are valid, the class may define validator methods for each property. These validators can check if the input data is of the correct type, length, or format.
```
class UserProfile {
  constructor(username, password) {
    this.username = username;
    this.password = password;
  }

  validateUsername() {
    if (typeof this.username !== 'string' || this.username.length < 3) {
      throw new Error('Invalid username. Must be a string of at least 3 characters.');
    }
  }

  validatePassword() {
    if (typeof this.password !== 'string' || this.password.length < 8) {
      throw new Error('Invalid password. Must be a string of at least 8 characters.');
    }
  }

  validateAll() {
    this.validateUsername();
    this.validatePassword();
  }
}

const user = new UserProfile('peter', 'mypassword');
user.validateAll();
```

## Class Transformers

Class transformers are functions or methods that transform the input data of a class or object. These transformers are used to convert data from one format to another, or to perform some other operation on the input data before it is processed by the class or object.

Class transformers can be used for a wide range of tasks, such as data normalization, data cleaning, or data formatting. They are often used in conjunction with class validators to ensure that the input data is in the correct format before it is transformed.

```
class UserProfile {
  constructor(username, password) {
    this.username = username;
    this.password = password;
  }

  transformUsername() {
    // Transform username to lowercase
    this.username = this.username.toLowerCase();
  }

  transformPassword() {
    // Hash password
    this.password = hash(this.password);
  }

  transformAll() {
    this.transformUsername();
    this.transformPassword();
  }
}

const user = new UserProfile("Peter", "mypassword");
user.transformAll(); // Transforms all properties of the user profile
```

## YUP

Yup is a JavaScript schema validation library that allows you to define a schema, or set of rules, for validating JavaScript objects. Yup makes it easy to define and enforce data validation rules, such as required fields, string length limits, and format validation.

Yup is often used in web development, where it can be used to validate form data before it is submitted to a server. It can also be used in other contexts where data validation is important, such as in data processing pipelines or API input validation.

Here is an example of how Yup can be used to define a schema and validate an object:

```
import * as yup from 'yup';

const schema = yup.object().shape({
  name: yup.string().required(),
  email: yup.string().email().required(),
  age: yup.number().positive().integer().required(),
});

const data = {
  name: 'Peter Lee',
  email: 'peter@example.com',
  age: 21,
};

schema.validate(data)
  .then(valid => console.log(valid))
  .catch(err => console.log(err));
```

Also, I have made a form verification using YUP in react.

You can check it out by clicking [here](https://github.com/probablysamir/practiceForm-react).

# Day 19

## Basic ndb usage

- Install ndb using the following command in the terminal:
```
npm install -g ndb
```
- Next, navigate to the directory where your Node.js application is located.

- To start the ndb debugger with the GUI interface, run the following command for the file you want to debug; in this case server.js
```
ndb server.js
```

- Once your application is running, you can use the GUI interface to set breakpoints, step through your code, and inspect variables.

  - To set a breakpoint, simply click on the line number in your source code where you want the breakpoint to be placed.

  - To start debugging your application, click the "play" button in the GUI interface.

  - When your application reaches a breakpoint, the GUI interface will pause execution and allow you to step through your code, inspect variables, and evaluate expressions.

  - To step through your code, use the "step over", "step into", and "step out" buttons in the GUI interface.

  - To inspect variables, click on the "Scope" tab in the GUI interface and select the scope where the variable is defined. You can then view the values of variables in that scope and evaluate expressions.

- When you have finished debugging your application, you can exit ndb by closing the GUI interface or using the "exit" button.

To help you navigate through the ndb when you first see it, I have included a screenshot of ndb GUI with some basic functions:

![ndb](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture17.jpg)

## Error Handling

When operational errors occur in our code, it is very important that we give meaningful error statement to the user. In order to do that I'll be adding error handlers in my code and refactor the code.

## Creating the global error handler

To handle all the errors we create a global error handler that handles all the errors so we don't have to go through the hectic process of rewriting the error handlers everywhere.

```
module.exports = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';
  res.status(err.statusCode).json({
    status: err.status,
    message: err.message,
  });
};
```

## Sending various errors

We can create an AppError class that inherits the properties of the class Error.

To do that:
```
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);

    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith(4) ? 'fail' : 'error';
    this.isOperational = true;

    Error.captureStackTrace(this, this.constructor);
  }
}

module.exports = AppError;
```

Let us assume we got a 404 error, let us handle it.
```
const globalErrorHandler = require('./controllers/errorController');

app.all('*', (req, res, next) => {
  next(new AppError(`Can't find ${req.originalUrl} on this server!`, 404));
});

app.use(globalErrorHandler);
```

Note that we use `app.all` below all the routes so that no any routes above it is matched it sends the statusCode (404) and errorMessage along with the mismatched url to the error handler which then sends back the error response to the user.

Here's an another example where we handle an error where the user tries to get the data.
```
const catchAsync = (fn) => {
  return (req, res, next) => {
    fn(req, res, next).catch(next);
  };
};

exports.getTour = catchAsync(async (req, res, next) => {
  const tour = await Tour.findById(req.params.id);
  res.status(200).json({
    status: 'success',
    data: { tour },
  });
});
```
We can also store the catchAsync function in the util folder but for the sake of simplicity I have written this in the same tourController file.

The catchAsync function in reusable and we can use it for creating, deleting and updating the resources too. Making catchAsync lets us to remove the catch block in the code, thus making the code shorter and makes it easy to handle the error.

# Day 20

## Error Handling (cont...)

### Separating Operational and Programming Errors

You can separate the operational and programming errors by:
```
const sendErrorDev = (err, res) => {
  res.status(err.statusCode).json({
    status: err.status,
    error: err,
    message: err.message,
    stack: err.stack,
  });
};

const sendErrorProd = (err, res) => {
  //Operational, trusted error: send message to client
  if (err.isOperational) {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.message,
    });
  }

  //Programming or other unkonwn error: don't leak any error details
  else {
    // 1) Log error
    console.error('Error 💥', err);

    // 2) Send generic message
    res.status(500).json({
      status: 'error',
      message: 'Something went wrong!',
    });
  }
};

module.exports = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';
  console.log(process.env.NODE_ENV);

  if (process.env.NODE_ENV === 'development') {
    sendErrorDev(err, res);
  } else if (process.env.NODE_ENV === 'production') {
      err = handleValidationErrorDB(err);
    sendErrorProd(err, res);
  }
};
```

Sometimes, we forget to handle rejected promises (for e.g. forgetting to add a catch statement). To handle these errors, we can add a safety net:
```
process.on('unhandledRejection', (err) => {
  console.log('UNHANDLED REJECTION!! 💥 Shutting down....');
  console.log(err.name, err.message);
  server.close(() => {
    process.exit(1);
  });
});
```
To handle synchronous errors (unhandled exceptions):
```
process.on('unhandledRejection', (err) => {
  console.log('UNHANDLED REJECTION!! 💥 Shutting down....');
  console.log(err.name, err.message);
  server.close(() => {
    process.exit(1);
  });
});
```

## Fixed a bug 

I discussed about ndb earlier but today I'll be sharing my simple experience about how `console.log()` couldn't help me much and ndb was a life savior.

`TLDR (Too Long Didn't Read) at the end`

_(The code is just above this in today's day)_

__Scenario:__ When I was separating operational and programming errors, I was unable to see why my production code was unable to handle the errors.  For e.g. When I sent a get request with mistyped URL postman was giving me the following response:

![bug](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture18.PNG)

I knew that the request-response cycle was stuck somewhere so I looked whether I forgot to send response somewhere but I had sent the response in the code. I then thought about checking the NODE_ENV variable value via `console.log()` to confirm that the production value was set and yes it returned production. I then tried to log something within `sendErrorProd` function and then I found out that the function itself was not called and thus stuck the req-res cycle.

I now started to get frustrated because I know that the simple if-else statement is not working and I can't figure out why. Also, `console.log()` was giving the response in the condition. I then thought about checking the NODE_ENV variable value from ndb.

This is what I got:
![ndb ss](https://raw.githubusercontent.com/probablysamir/30daysofnode/main/File_dumps/Capture19.PNG)

_(Scope->Global->process->env->NODE_ENV)_

You can see the bug right? Even if you cannot, let me tell you that the extra space after the production was ruining everything. Yes, it was not much of a big bug but I invested a lot of time fixing this by modifying code here and there. If I had used ndb for looking at this value earlier it would have been easier to fix. 

__Source of the bug:__ 
```
  "scripts": {
    "start:prod": "SET NODE_ENV=production & nodemon server.js",
  },
```
__Solution:__
```
  "scripts": {
    "start:prod": "SET NODE_ENV=production&nodemon server.js",
  },
```
__TLDR:__ ndb helped me to find the bug right below my nose and helped me fix it under 5 minutes where normal console logs would have just wasted my time further and shifted my focus somewhere else and mess up even more.
