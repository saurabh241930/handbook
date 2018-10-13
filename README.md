<h1 style="margin-left:0 auto">My own take on <span style="color:green">NodeJS</span></h1>

**`Please note that this is my personal overview on node ,this maybe not 100% correct`**

Wouldn't it be awesome if you only had to know one programming language for building a full stack application? Ryan Dahl put this thought into action and created Node.js. Node.js is a server side framework that is built upon Chrome's powerful V8 JavaScript engine. Though originally written in C++, it uses JavaScript to be used in applications.

See, problem solved. One language to rule them all. But this also brings you down to the fact that now your whole application is using the same language. That means you must have a sound knowledge of that language.


*If you write front end code, it shouldn’t be news that you could easily write web apps with NodeJs since both rely heavily on JavaScript.*

It’s very important to understand that Node isn’t a silver bullet, it’s not always the best solution for every project. Anybody can start up a server in Node, but it takes a deeper understanding of the language to write web apps that scale.

Recently, I’ve been having a lot of fun with Node and I think I’ve learned enough to share, get feedback and learn more. Generally to cement my knowledge.

<h1>Before Node.js.</h1>

Web applications were written in a client/server model where the client would demand resources from the server and the server would respond with the resources. The server only responded when the client requested and would close the connection after each response.

This pattern is efficient because every request to the server takes time and resources(memory, CPU etc). So, it’s wiser to close a connection after serving the requested asset so the server could respond to other requests too.

So how do servers like these respond to millions of requests coming in at the same time? If you asked this question, you understand that it wouldn’t be nice if requests to servers would be delayed till all other requests were responded to.

Imagine visiting Facebook and you were told to wait for 5 minutes, for thousands of people requesting before you. There has to be a way to run thousands or at least hundred of requests at once. Good news!!. There’s a thing as Threads.

Threads are a way for systems to run multiple operations concurrently. So every request would open a new thread, every thread had all it required to run the same code to completion.

If it sounds strange?. Let’s use this analogy. 







`
Imagine a restaurant with just one person serving food.When the demand for food increases, things would crazily go wrong. People would have to wait till all preceding orders were delivered A way to solve this problem would be to employ more people to serve food right? This way, more customers would be served at once.
`


Each thread is a new employee and the browsers, well, hungry people. I’m sure you can get the point now.

But this system has a downside.**It would get to a point where there’s a lot of requests and starting up new threads would consume a whole lot of memory and system resources.**



NodeJs heavily depends on **First class functions** and **Function expressions**

<h3>First class function:</h3>

*In JavaScript, functions are first-class objects, which means they can be
stored in a variable, object, or array.
passed as an argument to a function.
returned from a function.eg:*

```javascript
const foo = function() {
   console.log("foobar");
}
// Invoke it using the variable
foo();
```



<h3>Function expression:</h3>


*A block of code that results in a value eg.*

```javascript
var getRectArea = function(width, height) {
    return width * height;
}

console.log(getRectArea(3,4));
// expected output: 12

```



<h3>Function constructors:</h3>



*Sometimes we need a "blueprint" for creating many objects of the same "type".
The way to create an "object type", is to use an object constructor function.
In the example above, function Person() is an object constructor function.
Objects of the same type are created by calling the constructor function with the new keyword*

```javascript
function Person(first, last, age, eye) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eye;
}

var myFather = new Person("John", "Doe", 50, "blue");

```

if you log **myFather** 

```
Person {firstName: "John", lastName: "Doe", age: 50, eyeColor: "blue"}
age: 50
eyeColor: "blue"
firstName: "John"
lastName: "Doe"
__proto__:
constructor: ƒ Person(first, last, age, eye)
__proto__: Object
}
```

<h3>Prototype in function : </h3>


*When a function is created in JavaScript, JavaScript engine adds a prototype property to the function. This prototype property is an object (called as prototype object) has a constructor property by default. constructor property points back to the function on which prototype object is a property.*


```javascript
function Person(first, last, age, eye) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eye;
}

Person.prototype.getFullName = function(){
  var fullName = this.firstName + this.lastName
  return fullName;
}

var myFather = new Person("John", "Doe", 50, "blue");

//to call prototype

console.log(myFather.getFullName())    // will give John Doe
```

<h3>Javascript by reference vs. by value : </h3>


*Javascript is always pass by value, but when a variable refers to an object (including arrays), the "value" is a reference to the object.
Changing the value of a variable never changes the underlying primitive or object, it just points the variable to a new primitive or object.
However, changing a property of an object referenced by a variable does change the underlying object
So, to work through some of your examples:*


```javascript
function f(a,b,c) {
    // Argument a is re-assigned to a new value.
    // The object or primitive referenced by the original a is unchanged.
    a = 3;
    // Calling b.push changes its properties - it adds
    // a new property b[b.length] with the value "foo".
    // So the object referenced by b has been changed.
    b.push("foo");
    // The "first" property of argument c has been changed.
    // So the object referenced by c has been changed (unless c is a primitive)
    c.first = false;
}

var x = 4;
var y = ["eeny", "miny", "mo"];
var z = {first: true};
f(x,y,z);
console.log(x, y, z.first); // 4, ["eeny", "miny", "mo", "foo"], false
Example 2:

var a = ["1", "2", {foo:"bar"}];
var b = a[1]; // b is now "2";
var c = a[2]; // c now references {foo:"bar"}
a[1] = "4";   // a is now ["1", "4", {foo:"bar"}]; b still has the value
              // it had at the time of assignment
a[2] = "5";   // a is now ["1", "4", "5"]; c still has the value
              // it had at the time of assignment, i.e. a reference to
              // the object {foo:"bar"}
console.log(b, c.foo); // "2" "bar
```

<h3>Scope : </h3>


*I think about the best I can do is give you a bunch of examples to study. Javascript programmers are practically ranked by how well they understand scope. It can at times be quite counter-intuitive.*


```javascript
//A globally-scoped variable

// global scope
var a = 1;

function one() {
  alert(a); // alerts '1'
}

//Local scope

// global scope
var a = 1;

function two(a) { // passing (a) makes it local scope
  alert(a); // alerts the given argument, not the global value of '1'
}

// local scope again
function three() {
  var a = 3;
  alert(a); // alerts '3'
}
Intermediate: No such thing as block scope in JavaScript (ES5; ES6 introduces let)

a.

var a = 1;

function four() {
  if (true) {
    var a = 4;
  }

  alert(a); // alerts '4', not the global value of '1'
}
b.

var a = 1;

function one() {
  if (true) {
    let a = 4;
  }

  alert(a); // alerts '1' because the 'let' keyword uses block scoping
}

//Intermediate: Object properties

var a = 1;

function Five() {
  this.a = 5;
}

alert(new Five().a); // alerts '5'
Advanced: Closure

var a = 1;

var six = (function() {
  var a = 6;

  return function() {
    // JavaScript "closure" means I have access to 'a' in here,
    // because it is defined in the function in which I was defined.
    alert(a); // alerts '6'
  };
})();


//Advanced: Prototype-based scope resolution

var a = 1;

function seven() {
  this.a = 7;
}

// [object].prototype.property loses to
// [object].property in the lookup chain. For example...

// Won't get reached, because 'a' is set in the constructor above.
seven.prototype.a = -1;

// Will get reached, even though 'b' is NOT set in the constructor.
seven.prototype.b = 8;

alert(new seven().a); // alerts '7'
alert(new seven().b); // alerts '8'


//Global+Local: An extra complex Case

var x = 5;

(function () {
    console.log(x);
    var x = 10;
    console.log(x); 
})();

//This will print out undefined and 10 rather than 5 and 10 since JavaScript always moves variable declarations (not initializations) to the top of the scope, making the code equivalent to:

var x = 5;

(function () {
    var x;
    console.log(x);
    x = 10;
    console.log(x); 
})();

//Catch clause-scoped variable

var e = 5;
console.log(e);
try {
    throw 6;
} catch (e) {
    console.log(e);
}
console.log(e);

```

<h3>Overiding with call() : </h3>

```javascript
var person = {
    fullName: function(city, country) {
        return this.firstName + " " + this.lastName + "," + city + "," + country;
    }
}
var person1 = {
    firstName:"John",
    lastName: "Doe",
}
person.fullName.call(person1, "Oslo", "Norway");
```

will overide **person1** with *Oslo* and *Norway*

usage of **call(this)** 

```javascript
var util = require('util');

function Person() {
	this.firstname = 'John';
	this.lastname = 'Doe';
}

Person.prototype.greet = function() {
	console.log('Hello ' + this.firstname + ' ' + this.lastname);
}

function Policeman() {
	this.badgenumber = '1234';
}

util.inherits(Policeman, Person);  //inherit all method from Policeman
var officer = new Policeman();
officer.greet();
```

when we run this

```
Hello undefined undefined
```

firstname & lastname are defined because **util.js** just connected prototypes not the property directly attached to object

to solve this we can add

```javascript
function Policeman() {
        Person.call(this);
	this.badgenumber = '1234';
}
```

here **call(this)** will work as a super constructor and it will run **Person** constructor

<h3> ES6 - class :</h3>

Class Inheritance. ES6 supports the concept of Inheritance. ... A class inherits from another class using the 'extends' keyword. Child classes inherit all properties and methods except constructors from the parent class.

```javascript
'use strict';

class Person {
	constructor(firstname, lastname) {
		this.firstname = firstname;
		this.lastname = lastname;
	}
	
	greet() {
		console.log('Hello, ' + this.firstname + ' ' + this.lastname);
	}
}


// which is basically same as 


function Person() {
	this.firstname = 'John';
	this.lastname = 'Doe';
}

Person.prototype.greet = function() {
	console.log('Hello ' + this.firstname + ' ' + this.lastname);
}

```
But note that js **class** is just a **syntactic sugar** ,it is not similar classes in **java,C#** or other language

useage of **super** with **class**

```javascript
class Greetr extends EventEmitter {
	constructor() {
		super();
		this.greeting = 'Hello world!';
	}
	
	greet(data) {
		console.log(`${ this.greeting }: ${ data }`);
		this.emit('greet', data);
	}
	
var greeter1 = new Greetr();

greeter1.on('greet', function(data) {
	console.log('Someone greeted!: ' + data);
});

greeter1.greet('Tony');

```

<h2>SYNCHRONOUS vs ASYNCHRONOUS</h2>

**synchronous** : Running a only process at a time

**asynchronous** : Running a multiple process at a time

*asynchronous* is one of the major reason why **NODE** exist , but note that **Javascript** and **V8** both are ***synchronous*** in nature ,since **NodeJS** is running both at same time , thats what make it **asynchronous**





________________________________________________________________________________________________________________________________________


<h2>Strong Foundations of NodeJS</h2>

**`Modules`** : *A reusable block of code whose existence does not accidentally imapct other code*(it was not present in vanilla JS)


# Deep dive into modules and exports



creating our module file **greet.js**

*greet.js*

```javascript
console.log("hello world")
```


creating our main **app.js** file & embeding module into it

```javascript
require("./greet.js")
```

if we run app.js in console we will get 
`hello wolrd` in console

to encapsulate specific function

*greet.js*

```javascript
var greet = function(){
console.log("hello world")
}

module.exports = greet;

```


and in our main **app.js** file & embeding module into it

```javascript
var greet = require("./greet.js")

greet()

```
so what really node do to make **require** work

if you look deep into nodejs source code here's what really when happens

**first it wraps our code**

```javascript
(function(exports , require, module, _filename,_dirname){
var greet = function(){
console.log("hello world")
}

module.exports = greet;

});

```

**then it invoke the function* and make require & module function available**

```javascript
fn(module.exports,require,module,filename,dirname)

//then it returns 

return module.exports
```

initially **exports** is a empty object ,we overide with help of *require*



<h2>Nested require</h2>

Now instead of creating a greet file we will create directory *greet* and put 3 files

```
|--greet
  |--english.js
  |--index.js
  |--spanish.js
```

now when we write `javascript require("greet")` it will look inside greet folder and then **index.js** file

suppose in **english.js**

```javascript
var greet = function(){
console.log("hello world")
}

module.exports = greet;
```
and in **spanish.js**

```javascript
var greet = function(){
console.log("hola!")
}

module.exports = greet;
```

to put both files we have to require both in **index.js**

```javascript

var english = require("./english")
var spanish = require("./spanish")

module.exports = {

     english:english,
     spanish:spanish
     
     }  
```

and in our main **app.js** file 

```javascript
var greet = require("./greet.js")

engilsh.greet()
spanish.greet()

```



<h2>Modules Pattern</h2>


**PATTERN 1**

```javascript

module.exports.greet = function() {
 console.log("hello world")
 }
 
```

**PATTERN 2**

```javascript
module.exports = function() {
	console.log('Hello world');
};
```



**PATTERN 3**

```javascript
function Greetr() {
	this.greeting = 'Hello world!!';
	this.greet = function() {
		console.log(this.greeting);
	}
}

module.exports = new Greetr();

```

**PATTERN 4**

```javascript
function Greetr() {
	this.greeting = 'Hello world!!!';
	this.greet = function() {
		console.log(this.greeting);
	}
}

module.exports = Greetr;

```


**PATTERN 5**

```javascript
var greeting = 'Hello world!!!!';

function greet() {
	console.log(greeting);
}

module.exports = {
	greet: greet
}

```

usage in app.js file

```javascript

var greet = require('./greet1');
greet();

var greet2 = require('./greet2').greet;
greet2();

var greet3 = require('./greet3');
greet3.greet();
greet3.greeting = 'Changed hello world!';

var greet3b = require('./greet3');
greet3b.greet();

var Greet4 = require('./greet4');
var grtr = new Greet4();
grtr.greet();

var greet5 = require('./greet5').greet;
greet5();

```

when we run this in console

```

Hello world              greet1.js:2
Hello world!             greet2.js:2
Hello world!!            greet3.js:4
Changed hello world!     greet3.js:4
Hello world!!!           greet4.js:4
Hello world!!!!          greet5.js:4
 
```

**Note that** pattern3 does not re run its module event code even though we call it multiple times,it **caches** the output and use that



<h2>modules.exports vs exports</h2>

when our code run through node ,the exports which is a part of parameter in wrapper

```javascript
fn(module.exports,require,module,filename,dirname)

//then it returns 

return module.exports
```

here **exports** and **module.exports** both pointing to the same object

when we run this

greet.js
```javascript
exports = function() {
	console.log('Hello');
}

console.log(exports);
console.log(module.exports);
```

app.js
```javascript
var greet = require('./greet');

```


we will get

```
[Function]
{}
```
 this is a quirk  in JS ,initially **module.exports** point to object by refrence and so do **exports** but it is set equal to value thats why it created a new object since refrence is broken
 But if we use **require** it will only pass **module.exports**
 
 we can mutate the property by doing this
 
 greet2.js
```javascript
  exports.greet = function() {
	console.log('Hello');
    }

console.log(exports);
console.log(module.exports);
```

app.js
```javascript
var greet = require('./greet');
var greet2 = require('./greet2');
greet2.greet();

```


we will get

```
[Function]
{}
{ greet : [Function] }
{ greet : [Function] }
```

By doing this the reference is not broken

________________________________________________________________________________________________________________________________________

# Event emitter

**Event** : Something that has happened in our app that we can respond to.

Node.js core API is based on asynchronous event-driven architecture in which certain kind of objects called emitters periodically emit events that cause listener objects to be called.**[** official defination on nodeJS site :)**]**

To get in-depth knowledge on lets build a simple event **on** and **emit** event

emitter.js
```javascript

//constructor for list which will have events 
function Emitter() {
	this.events = {};
}

Emitter.prototype.on = function(type, listener) {
	this.events[type] = this.events[type] || [];  //if that property is present in array format OR create it if not
	this.events[type].push(listener);
}

Emitter.prototype.emit = function(type) {
	if (this.events[type]) {
		this.events[type].forEach(function(listener) {
			listener();  //emit each each event present in list
		});
	}
}

module.exports = Emitter;
```

importing it in **app.js**

```javascript
var Emitter = require('./emitter');

var emtr = new Emitter();

emtr.on('greet', function() {
	console.log('Somewhere, someone said hello.');
});

emtr.on('greet', function() {
	console.log('A greeting occurred!');
});

console.log('Hello!');
emtr.emit('greet');

```

when we run this

```
Hello!
Somewhere, someone said hello.
A greeting occurred!
```

In a **nutshell** think of **on** a event a event which is responsible to create a TODO list of event of name greet and **emit** is responsible to excecute each event in that TODO list

----------------------------------------------------------------------------------------------------------------------------------------

# libuv The Event loop & Non-Blocking asynchronous excecution

Imagine we have this multi-threaded server (running Ruby on rails) that reads a file saved on the server and sends it to the requesting browser. It’s important to understand that Ruby won’t read the file for us directly. Ruby will tell the file system to read the file and return the contents . The file system is a program used to store and retrieve data on a computer.

The point is, Ruby just sits there doing nothing till the file system is done. And then Ruby collects the content and sends the contents to the browser.

Here’s where NodeJs comes in. In Node, while the file system is reading the file, Node uses the idle time to handle other requests. When the file system is done, it’s smart enough to tell Node to come take the resource and send to the browser. This is possible because of Node’s **event loop**

The **event loop** is basically a program that waits for events and dispatches them when they happens. One other important fact you might know is that **JavaScript is single thread and so is Node.**

Unlike other languages that require a **new thread** or process for every *single request*, **NodeJs** takes all requests and then delegates most of the work to other system workers. There’s something called a **Libuv** library which handles this work effectively with help from the OS kernel. When the background workers are done doing their work, they emit events to NodeJs **callbacks** registered on that event.

This brings us to **callbacks**. *They are basically functions passed into other functions as arguments and are called when certain conditions occur.*

So what NodeJs developers basically do is write **event handlers** that get called when certain Node events happen.

NodeJs is extreamly faster than mullti-threaded systems. Yea even with a single thread.

This is because programs usually don’t only consist of numeric, arithmetic and logic computations that take much time. In fact, a lot of times they merely write something to the file system, do network requests or access peripheries such as the console or an external device. Node excels in situations like this. When it experiences them, it quickly delegates the work to someone else and tackles other incoming **requests.**





If you’ve been following along strictly, you might also have speculated that NodeJs doesn’t excel at operations that consume CPU. *CPU intensive operations overload the main thread (Only thread). When using single threaded systems, it’s ideal to avoid such operations so the main thread can do other things.*


<img src="https://cdn-images-1.medium.com/max/2000/1*evOcy9n3vslkDt0Mj8mBYw.jpeg"/>


**It’s also important to know that everything in JavaScript runs parallel except your code.** 

Yea your **code** executes **`one`** thing at a time even when other workers are doing their jobs **concurrently.**

consider this **eg.**

```
Imagine a king that has thousands of other servants, the king writes a list of things he wants the servant to do (a long list), 

a servant takes it and delegates work to all other servants. When one is done, he takes his result to the king. The king is always busy

making other lists while the servants were working, so he collects his result and gives him another work to do.

```
*The point is, the king does one thing at a time even if alot of things are running parallel. In this anology, the king is your code and the servants are NodeJs background workers. Everything is happening parallel except your code.*



<img src="https://i.imgur.com/djsZCUb.png"/>

until now we have seen custom events(**event emitter**) now we will explore its cause i.e **system events** which is handled by library **libuv** .

**LIBUV** is library written to deal with things specifically lower level to **V8** and connect to our request

**`libuv`** -------REQUEST---------> **OS**

*libuv* internally have **queue** of events that have completed this happens simultaneously while **V8** is running and inside libuv there's most imp component exist **event loop** thats a loop which libuv constantly checks events in *queue*

<img src="https://i.imgur.com/18e4PVD.png"/>

this is **non-blocking** cycle visually

**NON-BLOCKING** : *Doing other things without stopping your program from running*

<h3>streams and buffer</h3>

**Streams** are objects that let you read data from a source or write data to a destination in continuous fashion. 

In Node.js, there are five types of **streams** −

**Readable** − Stream which is used for read operation.

**Writable** − Stream which is used for write operation.

**Duplex** − Stream which can be used for both read and write operation.

**Transform** − A type of duplex stream where the output is computed based on input.

**Passthrough** − A type of duplex stream where the output is computed based on input.

*Note that* stream is built on top of **event emitter** ,hence its have acccess to all the method of that and it is **abstract** in class( we have to inherit to use it)


<h3>fs</h3>
nodejs  **fs**  is used to read file

```
|--app.js
|--greet.txt
```

to read file **greet.txt** using fs


```javascript
var fs = require('fs');

var greet = fs.readFileSync(__dirname + '/greet.txt', 'utf8');
console.log(greet);

var greet2 = fs.readFile(__dirname + '/greet.txt', 'utf8', function(err, data) {
	console.log(data);
});

console.log('Done!');
```

here **readFileSync** is sychronous we can make it **asychronous** by passing callback

Now let's use **fs** to copy data from one file to one another

suppose we have a file of name **someText.txt**  contains

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris sodales iaculis diam. Phasellus ac pellentesque velit, sed pellentesque nulla. Vestibulum molestie facilisis diam et viverra. Cras aliquet maximus dui, vel porttitor velit fermentum et. Maecenas non ante eu ante mollis lacinia. Suspendisse molestie felis vitae est sollicitudin accumsan. Pellentesque maximus maximus lacus at fermentum. Sed mollis, velit vitae pellentesque porttitor, nibh lectus feugiat augue, aliquam dignissim nibh enim quis augue. Curabitur vel leo id erat consequat egestas. Aliquam erat volutpat..........about 10000 words

```
now to we have to copy this file into **someTextCopy.txt**  but we also have to keep in mind that it does have large amount of data , so we have to minimize the memory consumption

So,rather than sending all data in big **buffer** file we can break into samll **chunks**

```javascript
var fs = require('fs');

var readable = fs.createReadStream(__dirname + '/greet.txt', { encoding: 'utf8', highWaterMark: 16 * 1024 });
//breaking each buffer from 64kb to 16kb

var writable = fs.createWriteStream(__dirname + '/greetcopy.txt');

readable.on('data', function(chunk) {
	console.log(chunk);
	writable.write(chunk);
});
```

two streams are connect by **pipe** .eg. In node you pipe from **Readable** stream to **Writable** stream

<img src="https://i.imgur.com/7brXE7d.png" />

----------------------------------------------------------------------------------------------------------------------------------------



# HTTP in NodeJS

**HTTP** is a protocol which allows the fetching of resources, such as HTML documents. It is the foundation of any data exchange on the Web and a client-server protocol, which means requests are initiated by the recipient, usually the Web browser. A complete document is reconstructed from the different sub-documents fetched, for instance text, layout description, images, videos, scripts, and more.

[Read more about HTTP here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

```javascript
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.write('Hello World!');
  res.end();
}).listen(8080,'127.0.0.1');
```
 now when we run this this code will succesfully sits there in **node** engine wait for **request** to emit **response**
 
 best way to request is use our browser

[http://localhost:8080](http://localhost:8080) will get **response** *Hello World!*


<h3>Outputing HTML and templates</h3>

```javascript
var http = require('http');
var fs = require('fs');

http.createServer(function(req, res) {
    
    res.writeHead(200, { 'Content-Type': 'text/html' });
    var html = fs.readFileSync(__dirname + '/index.htm', 'utf8');
    var message = 'Hello world...';
    html = html.replace('{Message}', message);
    res.end(html);
    
}).listen(1337, '127.0.0.1');

```

```html
<html>
	<head></head>
	<body>
		<h1>{Message}</h1>
	</body>
</html>
```

note that this method can lead to **latency** while  outputing response when you are delivering large amount of data, instead use **pipe** to output send chunks of data  via **response** stream 

```javascript
var http = require('http');
var fs = require('fs');

http.createServer(function(req, res) {
    
    res.writeHead(200, { 'Content-Type': 'text/html' });
    fs.createReadStream(__dirname + '/index.htm').pipe(res);
    
}).listen(1337, '127.0.0.1');
```


# Routing

A server stores lots of files, when browsers request, they tell the server what they are looking for. The server responds accordingly by giving them files they ask for. This is called **Routing.**

In NodeJs, we need to manually define our routes. It’s no big deal, here’s how to do a basic one.

```javascript
//server.js
const http = require('http'),
      url = require('url'),
 
makeServer = function (request,response){
   let path = url.parse(request.url).pathname;
   console.log(path);
   if(path === '/'){
      response.writeHead(200,{'Content-Type':'text/plain'});
      response.write('Hello world');
   }
   else if(path === '/about'){
     response.writeHead(200,{'Content-Type':'text/plain'});
     response.write('About page');
   }
   else if(path === '/blog'){
     response.writeHead(200,{'Content-Type':'text/plain'});
     response.write('Blog page');
   }
   else{
     response.writeHead(404,{'Content-Type':'text/plain'});
     response.write('Error page');
   }
   response.end();
 },
server = http.createServer(makeServer);
server.listen(3000,()=>{
 console.log('Node server created at port 3000');
});

```
Paste this code in your **server.js** file, navigate to it, run it with node server.js head to your browser hit localhost:3000 and localhost:3000/about,. Try doing it with something like **localhost:3000/somethingelse.** Our error page right?.

While this may satisfy our immediate need of starting a server, it’s crazy to do this for every web page on your server. Nobody does this actually, but it’s just to give you the idea of how things work.

If you observed, we required another module; url. It provides us with an easy way to work with urls.

The **`.parse()`** method takes a url as argument and breaks it into protocol , **host**, **path** and **querystringetc.** If you don’t get that part, it’s alright.

Let’s take for example the url.

<img src="https://cdn-images-1.medium.com/max/1000/1*W6g3mdceuhhwSrDShWW91A.png"/>

So when we do **url.parse(request.url).pathname** , it gives us the pathname or the url which is primarily what we use to **route requests.** But things can get easier though.


<h3> Routing with express </h3>

If you’ve done some underground research, you must have heard about **Express.** It’s a NodeJs framework for easily building web apps and **APIs**. Since you want to write NodeJs apps, you’ll need Express too. It makes everything easier. You’ll see what I mean.

```javascript
//server.js
const express = require('express'),
      server = express();

server.set('port', process.env.PORT || 3000);

//Basic routes
server.get('/', (request,response)=>{
   response.send('Home page');
});

server.get('/about',(request,response)=>{
   response.send('About page');
});

//Express error handling middleware
server.use((request,response)=>{
   response.type('text/plain');
   response.status(505);
   response.send('Error page');
});

//Binding to a port
server.listen(3000, ()=>{
  console.log('Express server started at port 3000');
});
```

Now this looks clean right? I believe you might be getting tuned already. After importing the express module, we called it as a function. This begins our server journey.

Next, we try to set the port with **server.set()**. **process.env.PORT** gets the environment port the app is running on and somehow, if it’s not available, we default it to 3000.

If you observed the code above, routing in Express follows a pattern.

**`server.VERB('route',callback);`**

**VERB** here is any of the **GET, POST** etc, pathname is a string that gets appended to the domain .And the **callback** is any function we want to fire when the requests come in.

There’s one more thing.

**`server.use(callback);`**

Whenever you see a **.use()** method in express, it’s called a **middleware.** Middlewares are functions that do some http heavy lifting for us. In the code above, the middleware is an error handling one.

Finally, we do our normal *server.listen()* remember?

This is what routing looks like in NodeJs applications

# Building RESTful APIs with Node and Express.

APIs are a way for web applications to send data to one another. Have you ever been to a site where you make use of your facebook account to sign in or sign up. Facebook gives out a part fo their functionality for other applications to use. That’s what an API looks like.

A **REST**ful API is one that the server or client has no idea of the state of each other. By using a REST interface, different clients hit the same REST endpoints, perform the same actions, and receive the same responses without minding the state of each other.

**An API end point is a single function in an API that returns data.**

Creating a RESTful API involves sending data in JSON or XML format. Let’s try to make one in NodeJs. We’ll make one that returns dummy json data when a client requests via Ajax. It’s not a fancy API, but it’ll help us understand how things work in Node. So…

```javascript

//users.js
module.exports.users = [
 {
  name: 'Mark',
  age : 19,
  occupation: 'Lawyer',
  married : true,
  children : ['John','Edson','ruby']
 },
  
 {
  name: 'Richard',
  age : 27,
  occupation: 'Pilot',
  married : false,
  children : ['Abel']
 },
  
 {
  name: 'Levine',
  age : 34,
  occupation: 'Singer',
  married : false,
  children : ['John','Promise']
 },
  
 {
  name: 'Endurance',
  age : 45,
  occupation: 'Business man',
  married : true,
  children : ['Mary']
 },
]
```
This is the data we want to share to other applications. We export it so every program can easily make use of it. That’s the idea. We store the users array in the **modules.exports** object.

```javascript

//server.js
const express = require('express'),
      server = express(),
      users = require('./users');

//setting the port.
server.set('port', process.env.PORT || 3000);

//Adding routes
server.get('/',(request,response)=>{
 response.sendFile(__dirname + '/index.html');
});

server.get('/users',(request,response)=>{
 response.json(users);
});

//Binding to localhost://3000
server.listen(3000,()=>{
 console.log('Express server started at port 3000');
});
```
Here, we require('express) and create our server with express(). If you watch closely, you’ll also see that we’re requiring something else. That’s our **users.js**, remember we stored the data we’re sharing? We need it for the program to work.

Express has some methods that help us send certain content to the browser. response.sendFile() searches for a file and sends it to the browser. Here, we use a __dirname to get the root folder where our server is running from, then we add + 'index.js' to make sure we target the right file.

The **response.json()** sends json content to requesting websites. We pass it an argument, users array which is what we’re actually sharing. The rest should look familiar to you I guess.

```html
//index.html

<!DOCTYPE html>
<html>
<head>
 <meta charset="utf-8">
 <title>Home page</title>
</head>
<body>
 <button>Get data</button>
<script src="https://code.jquery.com/jquery-3.2.1.min.js" integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4=" crossorigin="anonymous"></script>
 
  <script type="text/javascript">
  
    const btn = document.querySelector('button');
    btn.addEventListener('click',getData);
    function getData(e){
        $.ajax({
        url : '/users',
        method : 'GET',
        success : function(data){
           console.log(data);
        },
      
        error: function(err){
          console.log('Failed');
        }
   });
  } 
 </script>
</body>
</html>
```


Right in the root folder, Hit **node server.js**. Now, open your browser. Hit localhost:3000, press the button and open your browser console.

<img src = "https://cdn-images-1.medium.com/max/1000/1*pNPba0k7HMSminNUlRDgrQ.gif"/>
 
 
 btn.addEventListent('click',getData); The getData makes a ‘GET’ ajax request. It contains a **$.ajax({properties})** function that sets certain parameters likeurl, success and error etc.

In real world systems, you won’t just be reading JSON files. You would want to Create, Read, Update and Delete data. Express allows these these through htttp verbs. **POST, GET, PUT and Delete** respectively.

# Networking with NodeJs sockets.

A computer network is a connection of computers to share and receive information. To do networking in NodeJs, we require the **net** module.

`javascript const net = require('net'); `

In **Transmission Control Protocol (TCP)** , there must be two endpoints; one endpoint(computer) binds to a numbered port while the other connects to the port.

If you don’t understand it, take this analogy.

```
Imagine your cell phone. Once you buy a sim, you get assigned a number 

and you bind to that number. When your friends want to call you, they 

connect to your number. You are one endpoint and your caller is another.
```

<h3>TCP & IP </h3>

TCP (Transmission Control Protocol) is a standard that defines how to establish and maintain a network conversation via which application programs can exchange data. TCP works with the Internet Protocol (IP), which defines how computers send packets of data to each other.


<img src = "https://i.imgur.com/lEEZtLo.png"/>

To cement this knowledge, we’re going to make a program that watches a file and informs connected clients when the file changes. Let’s dive right in.

Create a folder, call it **/node-network** .
Create three files, **filewatcher.js** , **subject.txt** , and **client.js**. Put this into **filewatcher.js** .

```javascript


//filewatcher.js

const net = require('net'),
   fs = require('fs'),
   filename = process.argv[2],
      
server = net.createServer((connection)=>{
 console.log('Subscriber connected');
 connection.write(`watching ${filename} for changes`);
  
let watcher = fs.watch(filename,(err,data)=>{
  connection.write(`${filename} has changed`);
 });
  
connection.on('close',()=>{
  console.log('Subscriber disconnected');
  watcher.close();
 });
  
});
server.listen(3000,()=>console.log('listening for subscribers'));
```

 Next we should provide a file to watch. Put this into **subject.txt** and be sure to save.
 
 `Hello world, I'm gonna change`
 
  Next, lets create our client. Put this into **client.js**
  
  ```javascript
  const net = require('net');
let client = net.connect({port:3000});
client.on('data',(data)=>{
 console.log(data.toString());
});.
```

We’ll need two terminals. In the first one, we run the **filename.js** with the name of the file to watch like so.

```
//the subject.txt will get stored in our filename variable

node filewatcher.js subject.txt

//listening for subscribers
```

On the other terminal, we’ll run **client.js** .

```
node client.js

//watching subject.txt for file changes.
```

Now go ahead and change subject.txt . Make sure to save the changes. Take a look at the client.js terminal and notice the additional line.

`//subject.txt has changed.`

One major characteristics of a network is that many clients can connect at the same time. Start up another command line, navigate to client.js run node client.js and change the subject.txt file again (don’t forget to save).

**What are we doing?**

Don’t worry if you don’t understand, let’s go over it together.

Our **filewatcher.js** basically does three things

creates a server to send messages to many clients. **net.createServer()**
Tell the server that a client connected, also tells the client that there’s a file being watched. console.log() and connection.write() .
Finally watches the file with thewatcher variable and closes it when the client disconnects. Here’s **filewatcher.js** again.


```javascript

//filewatcher.js

const net = require('net'),
   fs = require('fs'),
   filename = process.argv[2],
      
server = net.createServer((connection)=>{
 console.log('Subscriber connected');
 connection.write(`watching ${filename} for changes`);
  
let watcher = fs.watch(filename,(err,data)=>{
  connection.write(`${filename} has changed`);
 });
  
connection.on('close',()=>{
  console.log('Subscriber disconnected');
  watcher.close();
 });
  
});
server.listen(3000,()=>console.log('listening for subscribers'));
```

We require two modules, **fs** and **net** for reading and writing files and doing network connections relatively. If you watch closely, you’ll notice the **process.argv[2]** , **process** is a global variable that provides vital information about the **NodeJs** code running. **argv[]** is an array of arguments. When we say **argv[2]** , we refer to the third argument used to run the NodeJs code. In the command line, we would enter the name of the file we want to watch as third argument **argv[2]** . Remember this?

`node filewatcher.js subject.txt`

We also have something pretty familiar; the **net.createServer()** this function fires whenever a *client connects to the port*, it accepts a **callback*. The callback takes only one parameter which is the object that communicates to connecting clients.

The **connection.write()** method sends data to any client connecting to the port 3000. In this case, our connection object. In this case, we tell the client that a file is being **watched.**

The watcher contains a function that sends information to the client that the watched file has changed. And when the client closes the connection by exiting the **process**, the onclose event fires and the event handler passes message to the server and closes the **watcher.**

```javascript
//client.js
const net = require('net'),
      client = net.connect({port:3000});
client.on('data',(data)=>{
  console.log(data.toString());
});
```

**client.js** is pretty straightforward, we require the net module and connect it to port 3000. We then listen for the data event and logs it to the console.

You should understand that whenever our filewatcher.js does a **connection.write()** , our client gets a *data* event.

This is just a scratch of the surface of how networks work. Primarily, one endpoint broadcasts messages when certain events occur and every client connected receives it as a data event.
 










