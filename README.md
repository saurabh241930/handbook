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






________________________________________________________________________________________________________________________________________


<h2>Strong Foundations of NodeJS</h2>

**`Modules`** : *A reusable block of code whose existence does not accidentally imapct other code*(it was not present in vanilla JS)


# lets Build a module



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










