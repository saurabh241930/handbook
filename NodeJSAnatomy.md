<h1 style="margin-left:0 auto">My own take on <span style="color:green">NodeJS</span></h1>

## INDEX
[Origin of NodeJS](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#before-nodejs)

[REFRESHMENT](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#refreshment)

[First Class function](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#first-class-function)

[Function expression](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#function-expression)

[Function constructors](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#function-constructors)

[Prototype in function](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#prototype-in-function--)

[Javascript by reference vs. by value](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#javascript-by-reference-vs-by-value--)

[Scope](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#scope--)

[Overiding with call()](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#overiding-with-call--)

[ES6 - class](https://github.com/saurabh241930/handbook/blob/master/NodeJSAnatomy.md#-es6---class-)


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

Node.js is a **single threaded** language which in background uses **multiple threads** to execute **asynchronous** code.

Node.js is non-blocking which means that all functions ( callbacks ) are delegated to the event loop and they are ( or can be ) executed by different threads. That is handled by Node.js **run-time**.

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
 
# `Refreshment`

exploring more **nodejs** in practical way

<h2>How Node Applications Work</h2>

Node.js couples JavaScript with an event loop for quickly dispatching operations
when events occur. Many JavaScript environments use an event loop,
but it is a core feature of Node.js.
Node’s philosophy is to give you low-level access to the event loop and to
system resources. Or, in the words of core committer Felix Geisendörfer, in
Node “everything runs in parallel except your code.”

If this seems a little backwards to you, don’t worry. The following figure shows
how the event loop works


<img src = "https://i.imgur.com/RpeH0SO.png"/>

As long as there’s something left to do, **Node’s event** loop will keep spinning.
Whenever an event occurs, Node invokes any **callbacks (event handlers)** that
are listening for that event.

As a Node developer, your job is to create the callback functions that get
executed in response to events. Any number of callbacks can respond to any
event, but only one **callback** function will ever be executing at any time.
Everything else your program might do—like waiting for data from a file or
an incoming HTTP request—is handled by Node, in parallel, behind the scenes.
Your application code will never be executed at the same time as anything
else. It will always have the full attention of Node’s JavaScript engine while
it’s running.

# Single-Threaded and Highly Parallel

Other systems try to gain parallelism by running lots of code at the same
time, typically by spawning many threads. But not Node.js. For JavaScript,
Node is a single-threaded environment. At most, only one line of your code
will ever be executing at any time.

Node gets away with this by doing most I/O tasks using **nonblocking**
techniques. Rather than waiting line-by-line for an operation to finish, you
create a callback function that will be invoked when the operation eventually
succeeds or fails.
Your code should do what it needs to do, then quickly hand control back over
to the event loop so Node can work on something else. 

If it seems strange to you that Node achieves parallelism by running only one
piece of code at a time, that’s because it is. It’s an example of something I call
a **backwardism.**

<h3>Backwardisms in Node.js</h3>

A backwardism is a concept that’s so bizarre that at first it seems completely
backwards. You’ve probably experienced many backwardisms while learning
to program, whether you noticed them or not.
Take the concept of a variable. In algebra it’s common to see equations like
“7x + 3 = 24.” Here, x is called a variable; it has exactly one value, and your job
is to figure out what that value is.

Then when you start learning how to program, you quickly run into statements
like “x = x + 7.” Now x is still called a variable, but it can have any value that
you assign to it. It can even have different values at different times.
From algebra’s perspective, this is a backwardism. The equation “x = x + 7”
makes no sense at all. The notion of a variable in programming is not just a
little different—it’s 100 percent backwards. But once you understand the
concept of assignment, the programming variable makes perfect sense.
So it is with Node’s single-threaded event loop. From a multithreaded perspective,
running just one piece of code at a time seems silly. But once you
understand event-driven programming—with nonblocking APIs—it becomes
clear.

Programming is chock-full of backwardisms like these, and Node.js is no
exception. Starting out, you’ll frequently run into code that looks like it should
work one way, but it actually does something quite different.
That’s OK! With this book, you’ll learn Node by making compact programs
that interact in useful ways. As we run into more of Node’s backwardisms,
we’ll dive in and explore them.

## Redefining Node.js Event-Driven Architecture idea

Most of Node’s objects — like HTTP requests, responses, and streams — implement the EventEmitter module so they can provide a way to emit and listen to events.

```javascript
const EventEmitter = require('events')
```
The simplest form of the event-driven nature is the callback style of some of the popular Node.js functions — for example, fs.readFile. In this analogy, the event will be fired once (when Node is ready to call the callback) and the callback acts as the event handler.

Let’s explore this basic form first.

<h3>Call me when you’re ready, Node!</h3>

The original way Node handled asynchronous events was with callback. This was a long time ago, before JavaScript had native promises support and the async/await feature.

Callbacks are basically just functions that you pass to other functions. This is possible in JavaScript because functions are first class objects.

It’s important to understand that callbacks do not indicate an asynchronous call in the code. A function can call the callback both synchronously and asynchronously.

For example, here’s a host function fileSize that accepts a callback function cb and can invoke that callback function both synchronously and asynchronously based on a condition:

```javascript
function fileSize (fileName, cb) {
  if (typeof fileName !== 'string') {
    return cb(new TypeError('argument should be string')); // Sync
  }
  fs.stat(fileName, (err, stats) => {
    if (err) { return cb(err); } // Async
    cb(null, stats.size); // Async
  });
}
```

Note that this is a bad practice that leads to unexpected errors. Design host functions to consume callback either always synchronously or always asynchronously.

Let’s explore a simple example of a typical asynchronous Node function that’s written with a callback style

```javascript
const readFileAsArray = function(file, cb) {
  fs.readFile(file, function(err, data) {
    if (err) {
      return cb(err);
    }
    const lines = data.toString().trim().split('\n');
    cb(null, lines);
  });
};
```

readFileAsArray takes a file path and a callback function. It reads the file content, splits it into an array of lines, and calls the callback function with that array.

Here’s an example use for it. Assuming that we have the file numbers.txt in the same directory with content like this:

```
10

11

12

13

14

15
```

If we have a task to count the odd numbers in that file, we can use **readFileAsArray** to simplify the code:

```javascript
readFileAsArray('./numbers.txt', (err, lines) => {
  if (err) throw err;
  const numbers = lines.map(Number);
  const oddNumbers = numbers.filter(n => n%2 === 1);
  console.log('Odd numbers count:', oddNumbers.length);
});
```

The code reads the numbers content into an array of strings, parses them as numbers, and counts the odd ones.

Node’s callback style is used purely here. The callback has an **error-first argument** err that’s nullable and we pass the callback as the last argument for the host function. You should always do that in your functions because users will probably assume that. Make the host function receive the callback as its last argument and make the callback expect an error object as its first argument.

<h3>The modern JavaScript alternative to Callbacks</h3>

In modern JavaScript, we have promise objects. Promises can be an alternative to callbacks for asynchronous APIs. Instead of passing a callback as an argument and handling the error in the same place, a promise object allows us to handle success and error cases separately and it also allows us to chain multiple asynchronous calls instead of nesting them.

If the `readFileAsArray` function supports promises, we can use it as follows:

```javascript
readFileAsArray('./numbers.txt')
  .then(lines => {
    const numbers = lines.map(Number);
    const oddNumbers = numbers.filter(n => n%2 === 1);
    console.log('Odd numbers count:', oddNumbers.length);
  })
  .catch(console.error);
  ```
 Instead of passing in a callback function, we called a .then function on the return value of the host function. This .then function usually gives us access to the same lines array that we get in the callback version, and we can do our processing on it as before. To handle errors, we add a .catch call on the result and that gives us access to an error when it happens.

Making the host function support a promise interface is easier in modern JavaScript thanks to the new Promise object. Here’s the readFileAsArray function modified to support a promise interface in addition to the callback interface it already supports:

```javascript
const readFileAsArray = function(file, cb = () => {}) {
  return new Promise((resolve, reject) => {
    fs.readFile(file, function(err, data) {
      if (err) {
        reject(err);
        return cb(err);
      }
      const lines = data.toString().trim().split('\n');
      resolve(lines);
      cb(null, lines);
    });
  });
};
```

So we make the function return a Promise object, which wraps the **fs.readFile** async call. The promise object exposes two arguments, a resolve function and a reject function.

Whenever we want to invoke the callback with an error we use the promise reject function as well, and whenever we want to invoke the callback with data we use the promise resolve function as well.

The only other thing we needed to do in this case is to have a default value for this callback argument in case the code is being used with the promise interface. We can use a simple, default empty function in the argument for that case: () => {}.

## Consuming promises with async/await

Adding a promise interface makes your code a lot easier to work with when there is a need to loop over an async function. With callbacks, things become messy.

Promises improve that a little bit, and function generators improve on that a little bit more. This said, a more recent alternative to working with async code is to use the async function, which allows us to treat async code as if it was synchronous, making it a lot more readable overall.

Here’s how we can consume the readFileAsArray function with async/await:

```javascript

async function countOdd () {
  try {
    const lines = await readFileAsArray('./numbers');
    const numbers = lines.map(Number);
    const oddCount = numbers.filter(n => n%2 === 1).length;
    console.log('Odd numbers count:', oddCount);
  } catch(err) {
    console.error(err);
  }
}
countOdd();
```

We first create an async function, which is just a normal function with the word async before it. Inside the async function, we call the readFileAsArray function as if it returns the lines variable, and to make that work, we use the keyword await. After that, we continue the code as if the readFileAsArray call was synchronous.

To get things to run, we execute the async function. This is very simple and more readable. To work with errors, we need to wrap the async call in a **try/catch** statement.

With this async/await feature, we did not have to use any special API (like .then and .catch). We just labeled functions differently and used pure JavaScript for the code.

We can use the async/await feature with any function that supports a promise interface. However, we can’t use it with callback-style async functions (like setTimeout for example).


## The EventEmitter Module 

The EventEmitter is a module that facilitates communication between objects in Node. EventEmitter is at the core of Node asynchronous event-driven architecture. Many of Node’s built-in modules inherit from EventEmitter.

The concept is simple: emitter objects emit named events that cause previously registered listeners to be called. So, an emitter object basically has two main features:

Emitting name events.

Registering and unregistering listener functions.

To work with the EventEmitter, we just create a class that extends EventEmitter.

```javascript
class MyEmitter extends EventEmitter {

}
```

Emitter objects are what we instantiate from the EventEmitter-based classes:

```javascript
const myEmitter = new MyEmitter();
```
At any point in the lifecycle of those emitter objects, we can use the emit function to emit any named event we want.


```javascript
myEmitter.emit('something-happened');
```

Emitting an event is the signal that some condition has occurred. This condition is usually about a state change in the emitting object.

We can add listener functions using the on method, and those listener functions will be executed every time the emitter object emits their associated name event.

## Events !== Asynchrony

Let’s take a look at an example:

```javascript
const EventEmitter = require('events');

class WithLog extends EventEmitter {
  execute(taskFunc) {
    console.log('Before executing');
    this.emit('begin');
    taskFunc();
    this.emit('end');
    console.log('After executing');
  }
}

const withLog = new WithLog();

withLog.on('begin', () => console.log('About to execute'));
withLog.on('end', () => console.log('Done with execute'));

withLog.execute(() => console.log('*** Executing task ***'));
```

Class `WithLog` is an event emitter. It defines one instance function execute. This execute function receives one argument, a task function, and wraps its execution with log statements. It fires events before and after the execution.

To see the sequence of what will happen here, we register listeners on both named events and finally execute a sample task to trigger things.

Here’s the output of that:
```
Before executing
About to execute
*** Executing task ***
Done with execute
After executing
```

What I want you to notice about the output above is that it all happens synchronously. There is nothing asynchronous about this code.

We get the “Before executing” line first.

The begin named event then causes the “About to execute” line.

The actual execution line then outputs the “*** Executing task ***” line.

The end named event then causes the “Done with execute” line

We get the “After executing” line last.

Just like plain-old callbacks, do not assume that events mean synchronous or asynchronous code.

This is important, because if we pass an asynchronous taskFunc to execute, the events emitted will no longer be accurate.

We can simulate the case with a setImmediate call:

```javascript
// ...

withLog.execute(() => {
  setImmediate(() => {
    console.log('*** Executing task ***')
  });
});
```

Now the output would be:

```
Before executing
About to execute
Done with execute
After executing
*** Executing task ***
```

This is wrong. The lines after the async call, which were caused the “Done with execute” and “After executing” calls, are not accurate any more.

To emit an event after an asynchronous function is done, we’ll need to combine callbacks (or promises) with this event-based communication. The example below demonstrates that.

One benefit of using events instead of regular callbacks is that we can react to the same signal multiple times by defining multiple listeners. To accomplish the same with callbacks, we have to write more logic inside the single available callback. Events are a great way for applications to allow multiple external plugins to build functionality on top of the application’s core. You can think of them as hook points to allow for customizing the story around a state change.

## Asynchronous Events
*Let’s convert the synchronous sample example into something asynchronous*

```javascript
const fs = require('fs');
const EventEmitter = require('events');

class WithTime extends EventEmitter {
  execute(asyncFunc, ...args) {
    this.emit('begin');
    console.time('execute');
    asyncFunc(...args, (err, data) => {
      if (err) {
        return this.emit('error', err);
      }

      this.emit('data', data);
      console.timeEnd('execute');
      this.emit('end');
    });
  }
}

const withTime = new WithTime();

withTime.on('begin', () => console.log('About to execute'));
withTime.on('end', () => console.log('Done with execute'));

withTime.execute(fs.readFile, __filename);
```
The WithTime class executes an asyncFunc and reports the time that’s taken by that **asyncFunc** using console.time and console.timeEnd calls. It emits the right sequence of events before and after the execution. And also emits error/data events to work with the usual signals of **asynchronous calls**


We test a withTime emitter by passing it an **fs.readFile** call, which is an asynchronous function. Instead of handling file data with a callback, we can now listen to the data event.

When we execute this code , we get the right sequence of events, as expected, and we get a reported time for the execution, which is helpful:

```
About to execute
execute: 4.507ms
Done with execute
```

Note how we needed to combine a callback with an event emitter to accomplish that. If the asynFunc supported promises as well, we could use the **async/await** feature to do the same:

```javascript
class WithTime extends EventEmitter {
  async execute(asyncFunc, ...args) {
    this.emit('begin');
    try {
      console.time('execute');
      const data = await asyncFunc(...args);
      this.emit('data', data);
      console.timeEnd('execute');
      this.emit('end');
    } catch(err) {
      this.emit('error', err);
    }
  }
}
```

I don’t know about you, but this is much more readable to me than the callback-based code or any .then/.catch lines. The **async/await** feature brings us as close as possible to the JavaScript language itself, which I think is a big win.

## Events Arguments and Errors

In the previous example, there were two events that were emitted with extra arguments.

The error event is emitted with an error object.

```javascript
this.emit('error', err);
```
The data event is emitted with a data object.

```javascript
this.emit('data', data);
```
We can use as many arguments as we need after the named event, and all these arguments will be available inside the listener functions we register for these named events.

For example, to work with the data event, the listener function that we register will get access to the data argument that was passed to the emitted event and that data object is exactly what the **asyncFunc** exposes.
```javascript
withTime.on('data', (data) => {
  // do something with data
});
```
The error event is usually a special one. In our callback-based example, if we don’t handle the error event with a listener, the node process will actually exit.

To demonstrate that, make another call to the execute method with a bad argument:

```javascript
class WithTime extends EventEmitter {
  execute(asyncFunc, ...args) {
    console.time('execute');
    asyncFunc(...args, (err, data) => {
      if (err) {
        return this.emit('error', err); // Not Handled
      }

      console.timeEnd('execute');
    });
  }
}



withTime.execute(fs.readFile, ''); // BAD CALL
withTime.execute(fs.readFile, __filename);
```

The first execute call above will trigger an error. The node process is going to crash and exit:

```
events.js:163
      throw er; // Unhandled 'error' event
      ^
Error: ENOENT: no such file or directory, open ''
```

The second execute call will be affected by this crash and will potentially not get executed at all.

If we register a listener for the special error event, the behavior of the node process will change. For example:

```javascript
withTime.on('error', (err) => {
  // do something with err, for example log it somewhere
  console.log(err)
});
```

If we do the above, the error from the first execute call will be reported but the node process will not crash and exit. The other execute call will finish normally:

```
{ Error: ENOENT: no such file or directory, open '' errno: -2, code: 'ENOENT', syscall: 'open', path: '' }
execute: 4.276ms
```

Note that Node currently behaves differently with promise-based functions and just outputs a warning, but that will eventually change:

```
UnhandledPromiseRejectionWarning: Unhandled promise rejection (rejection id: 1): Error: ENOENT: no such file or directory, open ''
DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```

The other way to handle exceptions from emitted errors is to register a listener for the global uncaughtException process event. However, catching errors globally with that event is a bad idea.

The standard advice about uncaughtException is to avoid using it, but if you must do (say to report what happened or do cleanups), you should just let the process exit anyway:

```javascript
process.on('uncaughtException', (err) => {
  // something went unhandled.
  // Do any cleanup and exit anyway!

  console.error(err); // don't do just that.

  // FORCE exit the process too.
  process.exit(1);
});
```

However, imagine that multiple error events happen at the exact same time. This means the uncaughtException listener above will be triggered multiple times, which might be a problem for some cleanup code. An example of this is when multiple calls are made to a database shutdown action.

The EventEmitter module exposes a once method. This method signals to invoke the listener just once, not every time it happens. So, this is a practical use case to use with the uncaughtException because with the first uncaught exception we’ll start doing the cleanup and we know that we’re going to exit the process anyway.

## Order of Listeners

If we register multiple listeners for the same event, the invocation of those listeners will be in order. The first listener that we register is the first listener that gets invoked.

```javascript
// प्रथम
withTime.on('data', (data) => {
  console.log(`Length: ${data.length}`);
});

// दूसरा
withTime.on('data', (data) => {
  console.log(`Characters: ${data.toString().length}`);
});

withTime.execute(fs.readFile, __filename);
```
The above code will cause the “Length” line to be logged before the “Characters” line, because that’s the order in which we defined those listeners.

If you need to define a new listener, but have that listener invoked first, you can use the `prependListener` method:

```javascript
// प्रथम
withTime.on('data', (data) => {
  console.log(`Length: ${data.length}`);
});

// दूसरा
withTime.prependListener('data', (data) => {
  console.log(`Characters: ${data.toString().length}`);
});

withTime.execute(fs.readFile, __filename);
```

The above will cause the “Characters” line to be logged first.

And finally, if you need to remove a listener, you can use the removeListener method.


# Node.js Streams: Everything you need to know

Node.js streams have a reputation for being hard to work with, and even harder to understand. Well I’ve got good news for you — that’s no longer the case.

Over the years, developers created lots of packages out there with the sole purpose of making working with streams easier. But in this article, I’m going to focus on the native [Node.js stream API](https://nodejs.org/api/stream.html).

  <h2>“Streams are Node’s best and most misunderstood idea.”</h2>
  <h3>— Dominic Tarr</h3>
  
  
  ## What exactly are streams?
  
  Streams are collections of data — just like arrays or strings. The difference is that streams might not be available all at once, and `they don’t have to fit in memory`. This makes streams really powerful when working with large amounts of data, or data that’s coming from an `external source one chunk at a time`.

However, streams are not only about working with big data. They also give us the power of composability in our code. Just like we can compose powerful `linux` commands by **piping** other smaller Linux commands, we can do exactly the same in Node with streams.

<img src="https://cdn-images-1.medium.com/max/1000/1*Fp3dyVZckIUjPFOp58x-zQ.png"/>

```javascript
const grep = ... // A stream for the grep output
const wc = ... // A stream for the wc input
grep.pipe(wc)
```

Many of the built-in modules in Node implement the streaming interface:

<img src = "https://cdn-images-1.medium.com/max/1000/1*lhOvZiDrVbzF8_l8QX3ACw.png"/>

The list above has some examples for native Node.js objects that are also readable and writable streams. Some of these objects are both readable and writable streams, like **TCP** sockets, zlib and crypto streams.

Notice that the objects are also closely related. While an HTTP response is a readable stream on the client, it’s a writable stream on the server. This is because in the **HTTP** case, we basically read from one object (http.IncomingMessage) and write to the other (http.ServerResponse).

Also note how the stdio streams **`(stdin, stdout, stderr)`** have the inverse stream types when it comes to child processes. This allows for a really easy way to pipe to and from these streams from the main process **stdio** streams.

## A streams practical example

Theory is great, but often not 100% convincing. Let’s see an example demonstrating the difference streams can make in code when it comes to memory consumption.

Let’s create a big file first:

```javascript
const fs = require('fs');
const file = fs.createWriteStream('./big.file');

for(let i=0; i<= 1e6; i++) {
  file.write('Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.\n');
}

file.end();
```

Look what I used to create that big file. A writable stream!

The fs module can be used to read from and write to files using a stream interface. In the example above, we’re writing to that big.file through a writable stream 1 million lines with a loop.

Running the script above generates a file that’s about ~400 MB.

Here’s a simple Node web server designed to exclusively serve the **big.file:**

```javascript
const fs = require('fs');
const server = require('http').createServer();

server.on('request', (req, res) => {
  fs.readFile('./big.file', (err, data) => {
    if (err) throw err;
  
    res.end(data);
  });
});

server.listen(8000);
```

When the server gets a request, it’ll serve the big file using the **asynchronous** method, `fs.readFile`. But hey, it’s not like we’re **blocking** the event loop or anything. Every thing is great, right? Right?

Well, let’s see what happens when we run the server, connect to it, and monitor the memory while doing so.

When I ran the server, it started out with a normal amount of memory, 8.7 MB:


<img src="https://cdn-images-1.medium.com/max/1000/1*125_8HQ4KzJkeBcj1LcEiQ.png"/>

Then I connected to the server. Note what happened to the memory consumed:

<img src="https://cdn-images-1.medium.com/max/1000/1*SGJw31T5Q9Zfsk24l2yirg.gif"/>

Wow — the memory consumption jumped to 434.8 MB.

We basically put the whole `big.file` content in memory before we wrote it out to the **response** object. This is very inefficient.

The HTTP response object (res in the code above) is also a **writable** stream. This means if we have a **readable** stream that represents the content of `big.file`, we can just pipe those two on each other and achieve mostly the same result without consuming ~400 MB of memory.

Node’s **fs** module can give us a **readable** stream for any file using the **createReadStream** method. We can *pipe that to the response object:*

```javascript
const fs = require('fs');
const server = require('http').createServer();

server.on('request', (req, res) => {
  const src = fs.createReadStream('./big.file');
  src.pipe(res);
});

server.listen(8000);
```

Now when you connect to this server, a magical thing happens (look at the memory consumption):

<img src = "https://cdn-images-1.medium.com/max/1000/1*iWNNIMhF9QmD25Vho6-fRQ.gif"/>

What’s happening?

When a client asks for that big file, we stream it one chunk at a time, which means we don’t buffer it in memory at all. The memory usage grew by about 25 MB and that’s it.

You can push this example to its limits. Regenerate the big.file with five million lines instead of just one million, which would take the file to well over 2 GB, and that’s actually bigger than the default buffer limit in Node.

If you try to serve that file using fs.readFile, you simply can’t, by default (you can change the limits). But with fs.createReadStream, there is no problem at all streaming 2 GB of data to the requester, and best of all, the process memory usage will roughly be the same.


Ready to learn streams now?

## Streams 101

There are four fundamental stream types in Node.js: `Readable, Writable, Duplex`, and `Transform` streams.

A readable stream is an abstraction for a source from which data can be consumed. An example of that is the fs.createReadStream method.
A writable stream is an abstraction for a destination to which data can be written. An example of that is the fs.createWriteStream method.
A duplex streams is both Readable and Writable. An example of that is a TCP socket.
A transform stream is basically a duplex stream that can be used to modify or transform the data as it is written and read. An example of that is the zlib.createGzip stream to compress the data using gzip. You can think of a transform stream as a function where the input is the writable stream part and the output is readable stream part. You might also hear transform streams referred to as “through streams.”
All streams are instances of EventEmitter. They emit events that can be used to read and write data. However, we can consume streams data in a simpler way using the pipe method.


## The pipe method
Here’s the magic line that you need to remember:

```javascript
readableSrc.pipe(writableDest)
```
In this simple line, we’re piping the output of a readable stream — the source of data, as the input of a writable stream — the destination. The source has to be a readable stream and the destination has to be a writable one. Of course, they can both be duplex/transform streams as well. In fact, if we’re piping into a duplex stream, we can chain pipe calls just like we do in Linux:


```javascript
readableSrc
  .pipe(transformStream1)
  .pipe(transformStream2)
  .pipe(finalWrtitableDest)
  ```
  The pipe method returns the destination **stream**, which enabled us to do the chaining above. For `streams a (readable), b and c (duplex), and d (writable)`, we can:
  
  ```
  a.pipe(b).pipe(c).pipe(d)
# Which is equivalent to:
a.pipe(b)
b.pipe(c)
c.pipe(d)
# Which, in Linux, is equivalent to:
$ a | b | c | d
```

The **pipe** method is the easiest way to consume streams. It’s generally recommended to either use the pipe method or consume streams with events, but avoid mixing these two. Usually when you’re using the pipe method you don’t need to use events, but if you need to consume the streams in more custom ways, events would be the way to go.

## Stream events
Beside reading from a readable stream source and writing to a writable destination, the pipe method automatically manages a few things along the way. For example, it handles errors, end-of-files, and the cases when one stream is slower or faster than the other.

However, streams can also be consumed with events directly. Here’s the simplified event-equivalent code of what the pipe method mainly does to read and write data:

```javascript
# readable.pipe(writable)
readable.on('data', (chunk) => {
  writable.write(chunk);
});
readable.on('end', () => {
  writable.end();
});
```

Here’s a list of the important events and functions that can be used with readable and writable streams:

<img src = "https://cdn-images-1.medium.com/max/1000/1*HGXpeiF5-hJrOk_8tT2jFA.png"/>

The events and functions are somehow related because they are usually used together.

* The most important events on a readable stream are:

* The data event, which is emitted whenever the stream passes a chunk of data to the consumer

* The end event, which is emitted when there is no more data to be consumed from the stream.

* The most important events on a writable stream are:

* The drain event, which is a signal that the writable stream can receive more data.

* The finish event, which is emitted when all data has been flushed to the underlying system.

Events and functions can be combined to make for a custom and optimized use of streams. To consume a readable stream, we can use the pipe/unpipe methods, or the **read/unshift/resume methods**. To consume a writable stream, we can make it the destination of **pipe/unpipe**, or just write to it with the **write** method and `call the end method when we’re done`.


## Paused and Flowing Modes of Readable Streams

Readable streams have two main modes that affect the way we can consume them:

* They can be either in the paused mode
* Or in the flowing mode
Those modes are sometimes referred to as pull and push modes.

All readable streams start in the paused mode by default but they can be easily switched to flowing and back to paused when needed. Sometimes, the switching happens automatically.

When a readable stream is in the paused mode, we can use the `read()` method to read from the stream on demand, however, for a readable stream in the flowing mode, the data is continuously flowing and we have to listen to events to consume it.

In the flowing mode, data can actually be lost if no consumers are available to handle it. This is why, when we have a readable stream in `flowing mode`, we need a `data event handler`. In fact, just adding a `data event handler` switches a paused stream into flowing mode and removing the data event handler switches the stream back to paused mode. Some of this is done for backward compatibility with the older Node streams interface.

To manually switch between these two stream modes, you can use the `resume()` and `pause()` methods.

<img src = "https://cdn-images-1.medium.com/max/1000/1*HI-mtispQ13qm8ib5yey3g.png" />

When consuming readable streams using the *pipe* method, 
we don’t have to worry about these modes as pipe manages them automatically.

## Implementing Streams

When we talk about streams in Node.js, there are two main different tasks:

* The task of implementing the streams.

* The task of consuming them.

So far we’ve been talking about only consuming streams. Let’s implement some!

Stream implementers are usually the ones who `require` the stream module.

### Implementing a Writable Stream

To implement a writable stream, we need to to use the Writable constructor from the stream module.

```javascript
const { Writable } = require('stream');
```

We can implement a writable stream in many ways. We can, for example, extend the Writable constructor if we want

```javascript
class myWritableStream extends Writable {
}
```

However, I prefer the simpler `constructor` approach. We just create an `object` from the Writable constructor and pass it a number of options. The only required option is a write function which exposes the chunk of data to be written.

```javascript

const { Writable } = require('stream');
const outStream = new Writable({
  write(chunk, encoding, callback) {
    console.log(chunk.toString());
    callback();
  }
});

process.stdin.pipe(outStream);
```

This write method takes three arguments.

* The chunk is usually a buffer unless we configure the stream differently.
* The encoding argument is needed in that case, but usually we can ignore it.
* The callback is a function that we need to call after we’re done processing the data chunk. It’s what signals whether the write was successful or not. To signal a failure, call the callback with an error object.

In `outStream`, we simply console.log the chunk as a string and call the `callback` after that without an error to indicate success. This is a very simple and probably not so useful echo stream. It will echo back anything it receives.

To consume this stream, we can simply use it with `process.stdin`, which is a readable stream, so we can just pipe `process.stdin` into our `outStream`.

When we run the code above, anything we type into process.stdin will be echoed back using the outStream `console.log` line.

This is not a very useful stream to implement because it’s actually already implemented and built-in. This is very much equivalent to `process.stdout`. We can just pipe `stdin` into `stdout` and we’ll get the exact same echo feature with this single line:

```javascript
process.stdin.pipe(process.stdout);
```
### Implement a Readable Stream

To implement a readable stream, we require the Readable interface, and construct an object from it, and implement a `read()` method in the stream’s configuration parameter:


```javascript
const { Readable } = require('stream');
const inStream = new Readable({
  read() {}
});
```
There is a simple way to implement readable streams. We can just directly `push` the data that we want the consumers to consume.

```javascript
const { Readable } = require('stream'); 
const inStream = new Readable({
  read() {}
});
inStream.push('ABCDEFGHIJKLM');
inStream.push('NOPQRSTUVWXYZ');
inStream.push(null); // No more data
inStream.pipe(process.stdout);
```

When we push a null `object`, that means we want to signal that the stream does not have any more data.

To consume this simple `readable stream`, we can simply pipe it into the writable stream `process.stdout`.

When we run the code above, we’ll be reading all the data from inStream and echoing it to the standard out. Very simple, but also not very efficient.

We’re basically pushing all the data in the stream before piping it to process.stdout. The much better way is to `push` data on demand, when a consumer asks for it. We can do that by implementing the `read()` method in the configuration object:

```javascript
const inStream = new Readable({
  read(size) {
    // there is a demand on the data... Someone wants to read it.
  }
});
```

When the read method is called on a `readable` stream, the implementation can push partial data to the queue. For example, we can push one letter at a time, starting with character code 65 (which represents A), and incrementing that on every push:

```javascript

const inStream = new Readable({
  read(size) {
    this.push(String.fromCharCode(this.currentCharCode++));
    if (this.currentCharCode > 90) {
      this.push(null);
    }
  }
});
inStream.currentCharCode = 65;
inStream.pipe(process.stdout);
```

While the consumer is reading a `readable` stream, the read method will continue to fire, and we’ll push more letters. We need to stop this cycle somewhere, and that’s why an if statement to push null when the currentCharCode is greater than 90 (which represents Z).

This code is equivalent to the simpler one we started with but now we’re pushing data on demand when the consumer asks for it. You should always do that.

### Implementing Duplex/Transform Streams

With Duplex streams, we can implement both readable and writable streams with the same object. It’s as if we inherit from both interfaces.

Here’s an example duplex stream that combines the two writable and readable examples implemented above:

```javascript
const { Duplex } = require('stream');

const inoutStream = new Duplex({
  write(chunk, encoding, callback) {
    console.log(chunk.toString());
    callback();
  },

  read(size) {
    this.push(String.fromCharCode(this.currentCharCode++));
    if (this.currentCharCode > 90) {
      this.push(null);
    }
  }
});

inoutStream.currentCharCode = 65;
process.stdin.pipe(inoutStream).pipe(process.stdout);
```

By combining the methods, we can use this duplex stream to read the letters from A to Z and we can also use it for its echo feature. We pipe the readable stdin stream into this duplex stream to use the echo feature and we `pipe` the `duplex` stream itself into the `writable` stdout stream to see the letters A through Z.

It’s important to understand that the readable and writable sides of a `duplex` stream operate completely independently from one another. This is merely a grouping of two features into an object.

A transform stream is the more interesting duplex stream because its output is computed from its input.

For a transform stream, we don’t have to implement the `read` or write methods, we only need to implement a transform method, which combines both of them. It has the signature of the write method and we can use it to `push` data as well.


Here’s a simple transform stream which echoes back anything you type into it after transforming it to upper case format:

```javascript
const { Transform } = require('stream');

const upperCaseTr = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

process.stdin.pipe(upperCaseTr).pipe(process.stdout);
```

In this transform stream, which we’re consuming exactly like the previous duplex stream example, we only implemented a `transform()` method. In that method, we convert the `chunk` into its upper case version and then push that version as the readable part.

### Streams Object Mode
By default, streams expect Buffer/String values. There is an objectMode flag that we can set to have the stream accept any JavaScript object.

Here’s a simple example to demonstrate that. The following combination of transform streams makes for a feature to map a string of comma-separated values into a JavaScript object. So “a,b,c,d” becomes {a: b, c: d}.













_______________________________________________________________________________________________________________________________________
---------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________
## Wrangling the File System

<h3>Programming for the Node.js Event Loop</h3>

Let’s get started by developing a couple of simple programs that watch files
for changes and read arguments from the command line. Even though they’re
short, these applications offer insights into Node’s event-based architecture.

## Watching a File for Changes

Watching files for changes is a convenient problem to start with because it
demands asynchronous coding while demonstrating important Node concepts.
Taking action whenever a file changes is just plain useful in a number of
cases, ranging from automated deployments to running unit tests.

Open a terminal to begin. On the command line, navigate to an empty directory.
You’ll use this directory for all of the code examples in this chapter.
Once there, use the touch command to create a file called target.txt.

```terminal
$ touch target.txt
```
```javascript
file-system/watcher.js

const fs = require('fs');
fs.watch('target.txt', function() {
console.log("File 'target.txt' just changed!");
});
console.log("Now watching target.txt for changes...");
```

Save this file as watcher.js in the same directory as target.txt. Let’s see how this
program works.

First, notice the const keyword at the top. This JavaScriptism (part of
ECMAScript Harmony) sets up a variable with a constant value. The **require()**
function pulls in a Node module and returns it. 

In our case, we’re callingrequire('fs') to incorporate Node’s built-in file-system module.1
In Node.js, a module is a self-contained bit of JavaScript that provides functionality
to be used elsewhere. The output of require() is usually a plain old
JavaScript object. There’s nothing particularly special about it, aside from
the functionality provided by the module.
Node’s module implementation is based on the CommonJS module specification.2
 Modules can depend on other modules, much like libraries in other
 
 programming environments, which import or #include other libraries.
 
Next we call the fs module’s watch() method, which polls the target file for
changes and invokes the supplied callback function whenever it does.
In JavaScript, functions are first-class citizens. This means they can be
assigned to variables and passed as parameters to other functions. Our callback
function is an anonymous function; it doesn’t have a name.
The callback function calls console.log() to echo a message to standard output
whenever the file changes. Let’s try it out.
Return to the command line and launch the watcher program using node, like so:

```
$ node --harmony watcher.js
Now watching target.txt for changes...
```
The --harmony parameter tells Node to use the latest ECMAScript Harmony
features available. ECMAScript Harmony is the code name for the next version
of ECMAScript, the standard behind the JavaScript language. Not all Harmony
features are ready for prime time, but the ones we’ll use in this book are OK
(except where noted).
After the program starts, Node will patiently wait until the target file is
changed. To trigger a change, open another terminal to the same directory
and touch the file again:

` touch target.txt`

The terminal running watcher.js will output the string File ’target.txt’ just
changed!, and then the program will go back to waiting.

## Visualizing the Event Loop
The program we wrote in the last section is a good example of the Node event
loop at work. Our simple file-watcher program causes Node to go through each
of these steps, one by one.

**To run the program, Node does the following.**
1. It loads the script, running all the way through to the last line, which
produces the Now watching message in the console.

2. It sees that there’s more to do, because of the call to watch().

3. It waits for something to happen, namely for the fs module to observe a
change to the file.

4. It executes our callback function when the change is detected.

5. It determines that the program still has not finished, and resumes waiting.

Node.js programs go through these steps, then the event loop spins until
either there’s nothing left to do or the program exits by some other means.
For example, if an exception is thrown and not caught, the process will exit.
We’ll see how this works next.

## Reading Command-Line Arguments
Now let’s make our program more useful by taking in the file to watch as a
command-line argument. This will introduce the process global object and how
Node deals with exceptions.
Open your editor and enter this:

```javascript
file-system/watcher-argv.js

const fs = require('fs'),
filename = process.argv[2];
if (!filename) {
throw Error("A file to watch must be specified!");
}
fs.watch(filename, function() {
console.log("File " + filename + " just changed!");
});
console.log("Now watching " + filename + " for changes...");
```
Save the file as watcher-argv.js. You can run it like so (note the target.txt argument
at the end):

```
$ node --harmony watcher-argv.js target.txt
Now watching target.txt for changes...
```

You should see the same output and behavior as the first watcher.js program.
After outputting Now watching target.txt for changes... the script will diligently
wait for changes to the target file.
This program uses **process.argv** to access the incoming command-line arguments.
**`argv stands for argument vector;`** it’s an **array** containing node and the full path
to the watcher-argv.js as its first two elements. The third element (that is, at

index  is target.txt, the name of our target file.
( i.e 

process.argv[0] == "node"

process.argv[1] == "myprogram.js"

process.argv[2] == "firstarg"

)
Notice that if a target file name is not provided the program will throw an
exception. You can try that by simply omitting the target.txt parameter:

```
$ node --harmony watcher-argv.js
/full/path/to/script/watcher-argv.js:7
throw Error("A file to watch must be specified!");
^
Error: A file to watch must be specified!
```
Any unhandled exception thrown in Node will halt the process. The exception
output shows the offending file, and the line number and position of the
exception.
Processes are important in Node. It’s pretty common in Node development to
spawn separate processes as a way of breaking up work, rather than putting
everything into one big Node program. In the next section, you’ll learn how
to spawn a process in Node.


## The Child Processes Module

We can easily spin a child process using Node’s child_process module and those child processes can easily communicate with each other with a messaging system.

The child_process module enables us to access Operating System functionalities by running any system command inside a, well, child process.

We can control that child process input stream, and listen to its output stream. We can also control the arguments to be passed to the underlying OS command, and we can do whatever we want with that command’s output. We can, for example, pipe the output of one command as the input to another (just like we do in Linux) as all inputs and outputs of these commands can be presented to us using Node.js streams.

Note that examples I’ll be using in this article are all Linux-based. On Windows, you need to switch the commands I use with their Windows alternatives.

There are four different ways to create a child process in Node: spawn(), fork(), exec(), and execFile().

We’re going to see the differences between these four functions and when to use each.


### Spawning a Child Process

The spawn function launches a command in a new process and we can use it to pass that command any arguments. For example, here’s code to spawn a new process that will execute the pwd command.

```javascript
const { spawn } = require('child_process');
const child = spawn('pwd');
```

We simply destructure the spawn function out of the child_process module and execute it with the OS command as the first argument.

The result of executing the spawn function (the child object above) is a ChildProcess instance, which implements the EventEmitter API. This means we can register handlers for events on this child object directly. For example, we can do something when the child process exits by registering a handler for the exit event:

Let’s enhance our file-watching example program even further by having it
spawn a child process in response to a change. To do this, we’ll bring in Node’s
child-process module and dive into some Node patterns and classes. You’ll also
learn how to use streams to pipe data around.
To keep things simple, we’ll make our script invoke the ls command with the
-lh options. This will give us some information about the target file whenever
it changes. You can use the same technique to spawn other kinds of processes,
as well.
Open your editor and enter this:

```javascript
file-system/watcher-spawn.js
"use strict";
const
fs = require('fs'),
spawn = require('child_process').spawn,
filename = process.argv[2];
if (!filename) {
throw Error("A file to watch must be specified!");
}
fs.watch(filename, function() {
let ls = spawn('ls', ['-lh', filename]);
ls.stdout.pipe(process.stdout);
});
console.log("Now watching " + filename + " for changes...");
```
Save the file as watcher-spawn.js and run it with node as before:

```
$ node --harmony watcher-spawn.js target.txt
Now watching target.txt for changes...

```

If you go to a different console and touch the target file, your Node program
will produce something like this:

`-rw-r--r-- 1 jimbo staff 0B Dec 19 22:45 target.txt-rw-r--r-- 1 jimbo staff 0B Dec 19 22:45 target.txt`

The username, group, and other aspects of the file will be different from the
preceding output, but the format should be the same.

The program we just made begins with the string "use strict" at the top. Strict
mode was introduced in ECMAScript version 5—it disables certain problematic
JavaScript language features and makes others throw exceptions.

Generally speaking, it’s a good idea to use strict mode.

Strict mode is also required to use certain ECMAScript Harmony features in
Node, such as the let keyword. Like const, let declares a variable, but a variable
declared with let can be assigned a value more than once.

Keep in mind that by using Harmony features (like let), your code will require
the **--harmony** flag until these features become enabled by default. For example,
const is already available without the --harmony flag, but not so for let.
Next, notice that we added a new require() at the beginning of the program.
Calling require('child_process') returns the child process module. We’re only
interested in the spawn() method, so we save that to a constant with the same
name and ignore the rest of the module.

```javascript
spawn = require('child_process').spawn,
```
Remember, **functions are first-class citizens in JavaScript**, so we’re free to
assign them directly to variables like we did here.
Next, take a look at the callback function we passed to **fs.watch()**.

```javascript
function() {
let ls = spawn('ls', ['-lh', filename]);
ls.stdout.pipe(process.stdout);
}
```

The first parameter to **spawn()** is the name of the program we wish to execute;
in our case it’s ls. The second parameter is an array of command-line arguments.
It contains the flags and the target file name.
The object returned by **spawn()** is a ChildProcess.

 Its **stdin, stdout,** and **stderr** properties
are **Streams** that can be used to read or write data. We want to send the

standard output from the child process directly to our own standard output
stream. This is what the **pipe()** method does.
Sometimes you’ll want to capture data from a **stream**, rather than just **piping**
it forward. Let’s see how to do that.

## Capturing Data from an EventEmitter

EventEmitter is a very importantIt provides a channel for events
to be dispatched and listeners notified. 

Many objects you’ll encounter in Node inherit from **EventEmitter**, like the Streams we saw in the last section.
Now let’s modify our previous program to capture the child process’s output
by listening for events on the stream. Open an editor to the *watcher-spawn.js* file
from the previous section, then find the call to **fs.watch()**. Replace it with this:

```javascript
file-system/watcher-spawn-parse.js
fs.watch(filename, function() {
let
ls = spawn('ls', ['-lh', filename]),
output = '';
ls.stdout.on('data', function(chunk){
output += chunk.toString();
});
ls.on('close', function(){
let parts = output.split(/\s+/);
console.dir([parts[0], parts[4], parts[8]]);
});
});
```
Save this updated file as watcher-spawn-parse.js. Run it as usual, then touch the
target file in a separate terminal. You should see output something like this:

```
$ node --harmony watcher-spawn-parse.js target.txt
Now watching target.txt for changes...
[ '-rw-r--r--', '0B', 'target.txt' ]
```

The new callback starts out the same as before, creating a child process and
assigning it to a variable called ls. It also creates an output variable, which will
buffer the output coming from the child process.

Next we add event listeners. An event listener is a callback function that is
invoked when an event of a specified type is dispatched. Since the Stream class
inherits from EventEmitter, we can listen for events from the child process’s
standard output stream.

```javascript
ls.stdout.on('data', function(chunk){
output += chunk.toString();
});
```
The **on()** method adds a listener for the specified event type. We listen for data
events because we’re interested in data coming out of the stream.
Events can send along extra information, which arrives in the form of
parameters to the **callbacks**. Data events in particular pass along a **buffer**

 Each time we get a chunk of data, we append it to our output.
A Buffer is Node’s way of representing binary data. It points to a blob of
memory allocated by Node’s native core, outside of the JavaScript engine.
Buffers can’t be resized and they require encoding and decoding to convert
to and from JavaScript strings.

Calling *toString()* explicitly converts the buffer’s contents to a JavaScript string
using Node’s default encoding (UTF-8). This means copying the content into
Node’s heap, which can be a slow operation, relatively speaking. If you can,
it’s better to work with buffers directly, but strings are more convenient.
Like Stream, the **ChildProcess class extends EventEmitter**, so we can add **listeners**
to it, as well.

```javascript
ls.on('close', function(){
let parts = output.split(/\s+/);
console.dir([parts[0], parts[4], parts[8]]);
});
```
After a child process has exited and all its streams have been flushed, it emits
a close event. When the callback printed here is invoked, we parse the output
data by splitting on sequences of one or more whitespace characters (using
the regular expression /\s+/).

Finally, we use **console.dir()** to report on the first,
fifth, and ninth fields (indexes 0, 4, and 8), which correspond to the permissions,
size, and file name, respectively.
We’ve seen a lot of Node’s features in this small problem space of file-watching.
You now know how to use key Node classes, including EventEmitter, Stream,
ChildProcess, and Buffer. You also have firsthand experience writing asynchronous
call-back functions and coding for the event loop.
Let’s expand on these concepts in the next phase of our file-system journey:
reading and writing files.

## Reading and Writing Files Asynchronously

Earlier in this chapter, we wrote a series of Node programs that could watch
files for changes. Now let’s explore Node’s methods for reading and writing
files. Along the way we’ll see two common error-handling patterns in Node:
error events on EventEmitters and err callback arguments.
There are a few different approaches to reading and writing files in Node. The
simplest way is to read in or write out the entire file at once. This technique
works well for small files. Other approaches read and write by creating Streams
or staging content in a buffer. Here’s an example of the whole-file-at-once
approach:


```javascript
file-system/read-simple.js
const fs = require('fs');
fs.readFile('target.txt', function (err, data) {
if (err) {
throw err;
}
console.log(data.toString());
```

Save this file as read-simple.js and run it as usual with node --harmony

`$ node --harmony read-simple.js`

You’ll see the contents of target.txt echoed to the command line. If the file is
empty, all you’ll see is a blank line.

Notice how the first parameter to the readFile() callback handler is err. If readFile()
is successful, then err will be false. Otherwise the err parameter will contain
an Error object. This is a common error-reporting pattern in Node, especially
for built-in modules. In our example’s case, we throw the error if there was
one. Recall that an uncaught exception in Node will halt the program by
escaping the event loop.

The second parameter to our callback, data, is a buffer; the same kind that
was passed to our various callbacks in previous sections.
Writing a file using the whole-file approach is similar. Here’s an example:

```javascript
file-system/write-simple.js
const fs = require('fs');
fs.writeFile('target.txt', 'a witty message', function (err) {
if (err) {
throw err;
}
console.log("File saved!");
});
```

This program writes “a witty message” to target.txt (creating it if it doesn’t exist,
or overwriting it if it does). If for any reason the file couldn’t be written, then
the err parameter will contain an Error object.



























