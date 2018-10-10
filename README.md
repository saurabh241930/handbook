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









 
 










