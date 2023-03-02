# 30 Days of N(c)ode

I will be learning and coding NodeJS for 30 days and I will be updating daily about my progress and understanding.

# Resources

I'll be posting more and more resources I encounter in my journey.

For now, these are the resources I'm following:

- [Roadmap](https://roadmap.sh/backend)

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

