class: center,middle
Node Wizardry
====================
_Andrés Ortiz Corrales_ - _@angrykoala_    
![Cool Koala](images/cool_koala.jpg)    
<https://github.com/angrykoala>    
![CC_by_sa](images/cc_license.png)        



---
## Acknowledgments
* **Oficina de Software Libre (UGR)**: <https://github.com/oslugr>

* **Alpha-Bug**: <https://github.com/Alpha-Bug>

* **Beer.js Granada**: <https://github.com/beerjs/granada>

---
## <center> What is Node.js? </center>


???
* JavaScript environment
* Mostly used for servers

* 2 ideas principales:
    * Javascript
    * Asíncrono (no bloqueante)

--
count:false
* ### Javascript
	* Common language client-server
	* Efficient (Google V8)

--
count:false
* ### Asynchronous (a.k.a. callback hell)
	* Non blocking I/O
    * Perfect for real time
    
---
class: center,middle
![Async vs sync](images/sync_async.png)

???
Async vs sync

---
class:center,middle
# Example: MyChat
_Small chat service_    
<https://github.com/angrykoala/node-wizardry/example/mychat>
---
## MyChat
Simple IRC-style chat service on a web platform.

We will develop a simple webpage with _node.js_ and _express_ framework. We will learn to use websockets with the library _socket.io_ and how to handle our project with _npm_.


_Based on **Socket.io** official tutorial: <http://socket.io/get-started/chat>_

---
## First steps
**Installing node (debian/ubuntu)**
* **Old version**
	* `sudo apt-get install nodejs`

* **Version 4 (LTS)**
	* `curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -`    
	* `sudo apt-get install -y nodejs`

**Windows**
Download and execute setup:
* Version 4 (LTS): <https://nodejs.org/en/download>

>More options at <https://nodejs.org/en/download/package-manager>

----

* `node --version` to check version


???

* Package nvm allows to use different node versions

* We will use terminal on both, Linux and Windows

---
## First steps
**Setting up the project**
* `npm init` on the root of your project to generate _package.json_
	* The fields will be automatically filled if possible
	  * **License**: GPL-3.0,ASC,MIT...
	* We can change some fields manually

```
-myChat/
	|-package.json
	|-server.js
	|-README.md
	|-app/
		|-"awesome code"
	|-config/
		|-"configuration files"
	|-node_modules/
		|-"dependencies"
	|-test/
		|-"testing code"
	|-public/
		|-"server resources"
```
_Example of Node.js project_

???
* GitHub repository
* It could be called server.js, app.js, index.js or anything
---
## First steps

```Json
{
  "name": "mychat",
  "version": "0.1.0",
  "description": "This is my description",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
  "author": "angrykoala <angrykoala@outlook.es>",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/angrykoala/mychat.git"
  },
  "bugs": {
    "url": "https://github.com/angrykoala/mychat/issues"
  },
  "homepage": "https://github.com/angrykoala/mychat#readme"
}

```
_package.json example_

???
package.json
---

## First steps: Hello Chat

```Javascript
var express = require("express"); //"loads" express framework library
var app = express(); //new express app (some magic here)
var http = require('http').Server(app); //http server using app

//what happens with a get request to our server?
app.get('/', function(req, res) {
	//this function is a magical callback executed when there is a request to /
	//req is the request info, response is used to send our funny stuff
	res.send("Hello Chat");
});

//server starts listening to port 9090
http.listen(9090, function() {
	//Another magical callback
	console.log("Magic happens in port 9090");
});
```
_server.js_

**express**: Framework for servers with node.js   
** http**: Standard node.js library

¿Where is the express library?

???
* **express**
* Callbacks

---
## First steps: Installing dependencies
With **npm** (_Node Package Manager_) we will manage our project and its dependencies

¿Do we need express? ¿Do we have a _package.json_?
* `npm install --save express`
	* npm install will install express under _node_modules_ automatically
	* The option --save will add the express library to the _package.json_ file as a dependency, which will allow us to install it again just wil `npm install`   
```Json
	 "dependencies": {
        "express": "^4.13.3"
 	 }
```

---
## First steps: Execute the example
* `node server.js` will execute our code:
```
	$ node server.js
	Magic happens in port 9090
```

* From any browser: `localhost:9090`    
	`Hello Chat`

----
Executing `node` will open a node.js interpreter client, useful to try things

![](http://31.media.tumblr.com/fdb485348996efd348858229e1acdb78/tumblr_inline_nfi38oxoA81t547dw.png)

---
## First steps
### Npm lil' reference
* `npm init`: Generates package.json 
* `npm install`: Installs all package.json dependencies
	* `npm install --save <package>`: Installs package and saves it as a dependency
	* `npm install -g <package>`: Installs a package globally
* `npm run <script>`: Executes the given script defined in package.json
* `npm start`: Executes "start" script in in package.json
* `npm test`: Executes script "test"
* `npm remove <package>`: Removes installed package
	* Options -g and --save also valid for remove

-----
Don't worry, this will not be on the final exam


---
## Serving a HTML file
**Lets make a real webpage**     
Under `public/` we will create an _index.html_ (remember that here we will add our resources)    

```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>MyChat</title>
</head>
<body>
    <center> <!-- Not good, i know, use css for this!! -->
    <h1>My Chat</h1>
    <p>Node Wizardry</p>
    </center>
</body>
</html>
```
_index.html_

???
```
-myChat/
	|-package.json
	|-server.js
	|-node_modules/
		|-express/
	|-public/
		|-index.html
```

---
## Serving a HTML file
* To serve all the files under public folder with express, simply add:
```javascript
app.use(express.static('public'));
```
* In the browser: `localhost:9090/index.html` will return our html page
* In the callback _app.get_ we could redirect to our page with `res.redirect('/index.html')`

```javascript
//requires and stuff
app.use(express.static('public')); //makes public folder as root of resources

app.get('/', function(req, res) {
	res.redirect('/index.html'); //redirects to index.html
});
//more code and stuff here
```
_server.js_

----
~~`res.sendFile(...)`~~: Allows us to send a file directly, however, we won't hace access to any script, image or css file without a public folder

???
* Now `localhost:9090/`  will take us to`localhost:9090/index.html`
* Try now

---
class:center
Now we have a web server, but, How do we build a chat service?

![Scared Koala Here](images/scared_koala.png)

---
## Websockets using Socket.io
Websockets allow us to send bidirectional messages in real time between client and server more efficiently that plain http request

<center>
<img src="images/websocket.png" alt="Websocket diagram"></br>
WebSocket diagram (www.pubnub.com)
</center>

???
Usa estándares y puerto 80 (bueno para evitar firewalls)

---
## Socket.io
Based on events, with two main methods:

* **On**: Will assign a callback to a certain event, the callback will be executed when we received the event (message)
	* `socket.on('my_message',callback)`


* **Emit**: Will send a message (event), we can also add a content (string,json,...)
	* `socket.emit('mi_message','content')`
	* With `socket.send('content')` we will send an event _'message'_ automatically

----
Socket.io allows one-to-one messages and broadcasting among other options

???
* Namespaces, rooms
* Message with ACK

---

## Socket.io - Events
We can assign any event to our messages, but socket.io provides certain events by default:

* **message:** Default event for our messages with `socket.send`


* **connection/connect:** Automatically fired when a socket connects
	* In the client always use **connect**

* **disconnect:** Fired when a socket disconnects

----
There are more events such as `reconnect` or `error` among others

---
## Server-side Socket.io

First, we need 'socket.io' library, so we repeat the same steps as with express:
1. `$ npm install --save socket.io` to download and install socket.io
```js
	  "dependencies": {
      "express": "^4.13.3",
      "socket.io": "^1.3.7"
	  }
```
2. `var io=require('socket.io')(http)` at the beginning of _server.js_, to assign websockets to our http

```js
var express = require("express"); //"loads" express framework library
var app = express(); //new express app (some magic here)
var http = require('http').Server(app); //http server using app
var io = require('socket.io')(http); //attach out socket.io to our server
```
---
## Server-side Socket.io

```js
//When a socket connects
io.on('connection', function (socket) { //the callback arg is the connected socket
 	console.log("User connected");

	//attach an event listener (msg) to connected socket
	socket.on('msg', function (content) {
		console.log("Received:"+content);
  	});
	//will fire when socket disconnects
	socket.on('disconnect', function () {
		console.log('User disconnected');
	});
});
```
_Basic Socket.io setup_

???
* Important! });

---
## Server-side Socket.io

1. `io.on('connection',function(socket)...` This event will fire with any socket connection. The callback receives the socket as argument. The rest of events are attached to given socket inside the callback

2. `socket.on('msg',function(content)...` Socket fired when an event `msg` is received. 
	* 'msg' is a custom event, it will only fired with messages labeled that way!

3. `socket.on('disconnect', function ()...` Event fired when socket disconnects. It will allow us to update any necessary information

???
Javascript scopes

---
## Client-side Socket.io
We will need to connect our fancy sockets to something (a client)

Luckily, out web client also runs with _Javascript_ so programming with Socket.io is similar

* First (again), is getting the library, luckily, our server is already configured by default to provide this client by adding to the index.html:
```html
</body>
<script src="/socket.io/socket.io.js"></script>
</html>
```
_index.html_


???
* It is also possible to provide the client manually or downloading from any other repository

There are also a lot of socket-io and websockets libraries for Java, Android etc... fully compatible

---
## Client-side Socket.io
Now lets code our client, lets keep everything tidy by creating a new file **chat.js** under _public/js/_ and loading in index.html:

```html
<!-- body code-->
</body>
<script src="/socket.io/socket.io.js"></script>
<script src="/js/chat.js"></script>
</html>
```
_index.html_

```javascript
var serviceURL = document.URL; //Url from document source
var socket = io.connect(serviceURL); //new socket connected to serviceURL

//On socket conenction
socket.on('connect', function(){
	alert("Connected"); //alert when connected
});
```
_chat.js_

???
* socket on(connect vs connection)

---
## Client-side Socket.io
**Lets try!**
1. `node server.js` to start our server
2. `localhost:9090` in our browser.

We will see our web and an alert in our browser saying "Connected".
In the server:
```
Magic happens in port 9090
User connected
```

----
If we reload the page or open another, other "user" will connect, if we shut the server (Ctrl+C) and connect again, the user will connect automatically and another alert will appear.

???
* Automatic reconnection
* We don't need to restart the client when modifying the client (it's a resource)

---
## Client-side Socket.io
We can connect now!!, we need to make this web to look like a chat application:
```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>MyChat</title>
</head>
<body>
  <center> <!-- Not good, I know -->
  <h1>My Chat</h1>
  <p>Node Wizardry</p>
  <p id="chat"></p> <!-- Chat messages are shown here -->
     <!-- form to write our messages -->
  <form action="javascript:void(0);" onsubmit="javascript:send();" id="chatform">
    <input type="text" id="userInput" size="30" /></br>
    <input type="submit" value="Send" /> <!-- when input call send function -->
  </form>
  </center>
</body>
<script src="/socket.io/socket.io.js"></script>
<script src="/js/chat.js"></script>
</html>
```
_index.html_

---
## Client-side Socket.io

<center>
    <img src="images/mychat_screen.png" alt="MyChat Screenshot">

</center>
_MyChat screenshot_

---
## Client-side Socket.io
The layout is ready, now we will code the event handling and interactions in chat.js:
* **Receive message**: We create a new event (e.g. _receive_) for when receiving a message, this event will contain the message, which we will display on our html page (using DOM)
```javascript
//On message receive (custom event)
socket.on('receive', function(msg) { //msg is the content
    var chatobj=document.getElementById("chat"); //gets chat DOM object
    //appends msg to content
    chatobj.innerHTML=chatobj.innerHTML+'</br>'+msg;
});
```
* **Disconnection:** We will alert the user that he has disconnected from the chat
```javascript
//On disconnection
socket.on('disconnect', function() {
    var chatobj=document.getElementById("chat"); //gets chat DOM object
	chatobj.innerHTML=chatobj.innerHTML+'</br>'+"Server connection lost";
});
```

???
What is DOM    

---
## Client-side Socket.io

* **Connection**: We will change the way to notify a new connection, similar to disconnection:
```javascript
socket.on('connect', function(){
    var chatobj=document.getElementById("chat"); //gets chat DOM object
	chatobj.innerHTML=chatobj.innerHTML+'</br>'+"connected";
});
```

* **Send message**: When the user submits their message, the function `send()` will be executed (see index.html), we must call `socket.emit` to send the message to the server:
```javascript
//sends message
function send() {
    //reads userInput value
	var input = document.getElementById("userInput").value;
    //erases userInput value in html
	document.getElementById("userInput").value="";
    //if input longer than 0, emits message
	if(input.trim().length>0)	socket.emit('msg',input.trim());
}
```

---
## Message boradcasting
Lets try again our chat

When entering the web, we can see "_connected_", now lets try typing something and pressing enter...

--
count: false
The input field erases like we said, however, nothing else happens...

Lets check the server console...

```
Magic happens in port 9090
User connected
Received:hello
```

The server receives the message successfuly, but it never sends it to the other users    

--
count:false
<center>
<img src="images/scream.png"></img>
</center>

---
## Message boradcasting
Lets go back to our server (server.js), and we can see that it does what we asked
 (`console.log("Received:"+content)`)

Now, we sould change the callback, so it will send the event _receive_ to all sockets...

```javascript
socket.on('msg', function (content) {
    //console.log("Received:"+content);
    io.emit('receive', content); //note that is io.emit, not socket
});
```
_msg event in server.js_

----

Note that we use `io.emit` instead of socket.emit, thats so the event is sent to all sockets

---
## Message boradcasting

Lets try (again) and... success!! our messages reach to every user (Wizardry!!)

Now we could try different things like connecting and disconnecting users, shutting down the server or connecting from different computers. As we can see, Socket.io and node.js are robust and fast for this kind of tasks.

---
## What now?
¿Improve the web with CSS? ¿Android app? ¿Add user registration? ¿Grab a beer?

--
count:false

<center>
Come and see the world of Node.js</br>
<img src="images/smash.jpg"></img>
</center>

* NPM modules to explore: <https://www.npmjs.com>
* Check these slides: <https://github.com/angrykoala/node-wizardry>

---
class: middle,center
# The End
![Guinness Koala](images/guinness.jpg)
