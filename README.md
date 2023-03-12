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
