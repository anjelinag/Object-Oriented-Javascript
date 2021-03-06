# OBJECT ORIENTED JAVASCRIPT

## Lesson 1: Objects in Depth

### 1.1 Create and Modify Properties
#### Creating Objects
Use object literal notation or the object() constructor function to create an object.
The object() method is slower and verbose hence the object literal is the recommended
way to create an object.
```javascript
// Using literal notation:
const myObject = {};
// Using the object constructor function:
const myObject = new Object();
```
#### Modifying Properties
Data within an object are *mutable*. Consider the ```cat``` object:
```javascript
const cat = {
 age: 2,
 name: 'Bailey',
 meow: function () {
   console.log('Meow!');
 },
 greet: function (name) {
   console.log(`Hello ${name}`);
 }
};
```
Let's modify the data a bit!
```javascript
cat.age += 1;
cat.age;
// 3
cat.name = 'Bambi';
cat.name;
// Bambi
```
Our ```cat``` now looks like this:
```javascript
const cat = {
 age: 3,
 name: 'Bambi',
 meow: function () {
   console.log('Meow!');
 },
 greet: function (name) {
   console.log(`Hello ${name}`);
 }
};
```
#### Adding Properties
To add a property, specify the property name and just give it a
value. You can either use dot notation or square brackets to add
properties. We can add a method to an object using the dot notation.
```javascript
const printer = {};

// Use dot notation:
printer.on = true;
printer.mode = 'black and white';

// Use square brackets:
printer['remainingSheets'] = 120;

// Add a method:
printer.print = function () {
  console.log('The printer is printing!');
};
```
Now our ```printer``` object looks like this:
```javascript
{
  on: true,
  mode: 'black and white',
  remainingSheets: 120,
  print: function () {
    console.log('The printer is printing!');
  }
}
```
#### Removing Properties
Since Objects are *mutable* you can not only add/modify properties, but also *delete* them.
We can delete properties from the ```printer``` objects as shown below:
```javascript
delete printer.mod;
// true
printer.mod;
// undefined
```
#### Passing Arguments
###### Passing a Primitive
In JavaScript, a primitive (e.g., a string, number, boolean, etc.) is immutable. In other words, any changes made to an argument inside a function effectively creates a copy local to that function, and does not affect the primitive outside of that function. Check out the following example:
```javascript
function changeToEight(n) {
  n = 8; // whatever n was, it is now 8... but only in this function!
}

let n = 7;

changeToEight(n);

console.log(n);
// 7
```
###### Passing an Object
On the other hand, objects in JavaScript are mutable. If you pass an object into a function, Javascript passes a reference to that object. Let's see what happens if we pass an object into a function and then modify a property:
```javascript
let originalObject = {
  favoriteColor: 'red'
};

function setToBlue(object) {
  object.favoriteColor = 'blue';
}

setToBlue(originalObject);

originalObject.favoriteColor;
// 'blue'
```
The reason why this is the case is because objects in JavaScript are passed by reference. If we make changes to the reference, we're actually directly modifying the original object itself. Therefore, when reassigning an object to a new variable, changing that copy chnages the original object as well.
```javascript
const iceCreamOriginal = {
  Andrew: 3,
  Richard: 15
};

const iceCreamCopy = iceCreamOriginal;

iceCreamCopy.Richard;
// 15
iceCreamCopy.Richard = 99;

iceCreamCopy.Richard;
// 99

iceCreamOriginal.Richard;
// 99
```
###### Comparing an Object with Another Object
On the topic of references, let's see what happens when we compare one object with another object. The following objects, parrot and pigeon, have the same methods and properties:
```javascript
const parrot = {
  group: 'bird',
  feathers: true,
  chirp: function () {
    console.log('Chirp chirp!');
  }
};

const pigeon = {
  group: 'bird',
  feathers: true,
  chirp: function () {
    console.log('Chirp chirp!');
  }
};

parrot == pigeon;
// false
```
The expression will only be true when comparing two references to exactly the same object.
```javascript
const myBird = parrot;
myBird == parrot;
// true
```
### 1.2 Invoking Object methods
#### Functions vs. methods
We can extend functionality to objects by adding methods to them.
```javascript
// Function
function sayHello () {
  console.log("Hi there!");
}

const developer = {
  name: 'Andrew'
};
```
If we want to add the sayHello() function into the ```developer``` object, we can add the same way as we add other new properties: by providing property name, then giving it a value. This time, the value of the property is a function!
```javascript
developer.sayHello = function () {
  console.log('Hi there!');
};
```
This is how the ```developer``` object looks like:
```javascript
{
  name: 'Andrew',
  sayHello: function () {
    console.log('Hi there!');
  }
}
```
#### Calling Methods
We can access a function in an object using the property name. Again, another name for a function property of an object is a method. We can access it the same way that we do with other properties: by using dot notation or square bracket notation. Let's take a look back at the updated developer object above, then invoke its `sayHello()` method:
```javascript
developer.sayHello();
// 'Hi there!'

developer['sayHello']();
// 'Hi there!
```
#### Passing Arguments Into methods
If the method takes arguments, you can proceed the same way, too:
```javascript
const developer = {
  name: 'Andrew',
  sayHello: function () {
    console.log('Hi there!');
  },
  favoriteLanguage: function (language) {
    console.log(`My favorite programming language is ${language}`);
  }
};


developer.favoriteLanguage('JavaScript');
// My favorite programming language is JavaScript'
```
#### Calling Methods by Property name
We've been using anonymous functions (i.e., functions without a name) for object methods. However, naming those functions is still valid JavaScript syntax. Consider the following object, ```greeter```:
```javascript
const greeter = {
  greet: function sayHello() {
    console.log('Hello!');
  }
};

greeter.greet();
// 'Hello!'
```
Named functions are great for a smoother debugging experience, since those functions will have a useful name to display in stack traces. They're completely optional, however, and you'll often read code written by developers who prefer one way or the other.

#### A Method Can Access the Object it was Called On
Using this, methods can directly access the object that it is called on. Consider the following object, `triangle`:
```javascript
const triangle = {
  type: 'scalene',
  identify: function () {
    console.log(`This is a ${this.type} triangle.`);
  }
};

triangle.identify();

// 'This is a scalene triangle.
```
### 1.3 Beware of Globals
#### ```this``` and Function Invocation
Let's compare the code from the chameleon object with the ```whoThis()``` code.
```javascript
const chameleon = {
  eyes: 2,
  lookAround: function () {
     console.log(`I see you with my ${this.eyes} eyes!`);
  }
};

chameleon.lookAround();
// I see you with my 2 eyes!

function whoThis () {
  this.trickyish = true
};

whoThis();
```
#### ```this``` in the Function/ Method
```JavaScript
// from the chameleon code:
console.log(`I see you with my ${this.eyes} eyes!`);

// from the whoThis() code:
this.trickyish = true
```
For our purposes of discovering the value of this, it does not matter that in the chameleon code, we're using this to retrieve a property, while in the whoThis() code, we're using this to set a property. So, in both of these cases, the use of this is virtually identical.

#### Compares the Structures of the Function/ Method
The lookAround() code is a method because it belongs to an object. Since it's a method, it's invoked as a property on the chameleon object:
```JavaScript
chameleon.lookAround();
```
`whoThis()` is not a method; it's a plain, old, regular function. And look at how the `whoThis()` function is invoked:
```javascript
whoThis();
```

#### ```this``` and Invocation
How the function is invoked determines the value of `this` inside the function.

Because `.lookAround()` is invoked as a method, the value of this inside of `.lookAround()` is whatever is left of the dot at invocation.

When a regular function is invoked, the value of `this` is the global window object.

#### The `window` Object
This window object has access to a ton of information about the page itself, including:
- The page's URL (`window.location;`)
- The vertical scroll position of the page (`window.scrollY'`)
- Scrolling to a new location (`window.scroll(0, window.scrollY + 200`); to scroll 200 pixels down from the current location)
- Opening a new web page (`window.open("https://www.udacity.com/");`)

#### Global Variables are Properties on `window`
Since the `window` object is at the highest (i.e., global) level, an interesting thing happens with global variable declarations. Every variable declaration that is made at the global level (outside of a function) automatically becomes a property on the `window` object!
```javascript
var currentlyEating = 'ice cream';

window.currentlyEating === currentlyEating
// true
```
###### Globals and `var`, `let`, and `const`
Only declaring variables with the `var` keyword will add them to the `window` object. If you declare a variable outside of a function with either `let` or `const`, it will not be added as a property to the `window` object.
```javascript
let currentlyEating = 'ice cream';

window.currentlyEating === currentlyEating
// false!
```
#### Global Functions are Methods on `window`
Similarly to how global variables are accessible as properties on the `window` object, any global function declarations are accessible on the `window` object as methods:
```javascript
function learnSomethingNew() {
  window.open('https://www.udacity.com/');
}

window.learnSomethingNew === learnSomethingNew
// true
```
#### Avoid Globals
There are a number of reasons why global variables are bot ideal:
- **Tight coupling** - Code is too dependent on the details of each other
- **Name collisions** - Occurs when two (or more) functions depend on a variable with the same name. A major problem with this is that both functions will try to update the variable and or set the variable, but these changes are overridden by each other.

```javascript
let counter = 1;

function addDivToHeader () {
  const newDiv = document.createElement('div');
  newDiv.textContent = 'div number ' + counter;

  counter = counter + 1;

  const headerSection = document.querySelector('header');
  headerSection.appendChild(newDiv)
}

function addDivToFooter() {
  const newDiv = document.createElement('div');
  newDiv.textContent = 'div number ' + counter;

  counter = counter + 1;

  const headerSection = document.querySelector('footer');
  headerSection.appendChild(newDiv)
}
```
### 1.4 Extracting Properties and Values
#### Object methods
```javascript
const myNewFancyObject = new Object();
```
The `Object()` function actually includes a few methods of its own to aid in the development of your applications. These methods are:
- `Object.keys()`
- `Object.values()`
#### Object.keys() and Object.values()

```javascript
const dictionary = {
  car: 'automobile',
  apple: 'healthy snack',
  cat: 'cute furry animal',
  dog: 'best friend'
};

Object.keys(dictionary);

// [car, apple, 'cat', 'dog']

Object.values(dictionary);

// ['automobile', 'healthy snack', 'cute furry animal', 'best friend']
```
The methods return the keys and values of an object respectively, in an array.
## Lesson 2: Functions at Runtime
### 2.1 First-Class Functions
n JavaScript, functions are first-class functions. This means that you can do with a function just about anything that you can do with other elements, such as numbers, strings, objects, arrays, etc. JavaScript functions can:
 - Be stored in variables
 - Be returned from a function.
 - Be passed as arguments into another function.

#### Functions can Return functions

```javascript
function alertThenReturn() {
  alert('Message 1!');

  return function () {
    alert('Message 2!');
  };
}
```
If `alertThenReturn()` is invoked in a browser, we'll first see an alert message that says `'Message 1!'`, followed by the `alertThenReturn()` function returning an anonymous function. However, we don't actually see an alert that says `'Message 2!'`, since none of the code from the inner function is executed. How do we go about executing the returned function?

Since `alertThenReturn()` returns that inner function, we can assign a variable to that return value:
```javascript
const innerFunction = alertThenReturn();

innerFunction();

// alerts 'Message 2!'
```
The `innerFunction` is used like any other function.

Likewise, this function can be invoked immediately without being stored in a variable. We'll still get the same outcome if we simply add another set of parentheses to the expression `alertThenReturn();`:
```javascript
alertThenReturn()();

// alerts 'Message 1!' then alerts 'Message 2!'
```
### 2.2 Callbacks
#### Callback functions
A function that takes other functions as arguments (and/or returns a function) is known as a **higher-order function**. A function that is passed as an argument into another function is called a **callback function**.

```javascript
function callAndAdd(n, callbackFunction){
  return n + callbackFunction();
}

function returnsThree(){
  return 3;
}

callAndAdd(2, returnsThree);
// 5
```
#### Array Methods
- `forEach()`
- `map()`
- `filter`

###### forEach()
Array's `forEach()` method takes in a callback function and invokes that function for each element in the array. In other words, `forEach()` allows you to iterate (i.e., loop) through an array, similar to using a `for` loop. Check out its signature:
```javascript
array.forEach(function callback(currentValue, index, array) {
    // function code here
});
```
The callback function itself receives the arguments: the current array element, its index, and the entire array itself.

```javascript
[1,5,2,4,6,3].forEach(function logIfOdd(n) {
  if (n % 2 !== 0){
    console.log(n)
  }
});
```
###### map()
Array's `map()` method is similar to `forEach()` in that it invokes a callback function for each element in an array. However, `map()` returns a new array based on what's returned from the callback function. Check out the following:
```javascript
const names = ['David', 'Richard', 'Veronika'];

const nameLengths = names.map(function(name) {
  return name.length;
});
```
### 2.3 Scope
There is block and function scope. These determine where a variable can be seen in some code. There is also **runtime scope**. This scope represents the *context* of the function, specially, the set of variables available for the function to use.

The code inside a function has access to:
- The function's arguments.
- Local variables declared within the function.
- Variables from its parent function's Scope.
- Global variables.

```javascript
const myName = 'Andrew';
// Global variable

function introduceMyself() {

  const you = 'student';
  // Variable declared where introduce() is defined
  // (i.e., within introduce()'s parent function, introduceMyself())

  function introduce() {
    console.log(`Hello, ${you}, I'm ${myName}!`);
  }

  return introduce();
}
```
##### Scope Chaining
When it comes to accessing variables, the JavaScript engine will traverse the scope chain, first looking at the innermost level (e.g., a function's local variables), then to outer scopes, eventually reaching the global scope if necessary.

##### Variable Shadowing
What happens when you create a variable with the same name as another variable somewhere in the scope chain?

JavaScript won't throw an error or otherwise prevent you from creating that extra variable. In fact, the variable with local scope will just temporarily "shadow" the variable in the outer scope. This is called variable shadowing. Consider the following example:
```javascript
const symbol = '¥';

function displayPrice(price) {
  const symbol = '$';
  console.log(symbol + price);
}

displayPrice('80');
// '$80'
```

### 2.4 Closures
A closure refers to the combination of a function and the lexical environment in which that function was declared. Every time a function is defined, closure is created for that function. This is especially especially powerful in situations where a function is defined within another function, allowing the nested function to access variables outside of it. Functions also keep a link to its parent's scope even if the parent has returned. This prevents data in its parents from being garbage collected.

### Immediately-Invoked Function Expressions (IIFE)
An immediately-invoked function expression, or IIFE (pronounced iffy), is a function that is called immediately after it is defined.
```javascript
(function sayHi(){
    alert('Hi there!');
  }
)();

// alerts 'Hi there!'
```
##### Passing Arguments into IIFE's
```javascript
(function (name){
    alert(`Hi, ${name}`);
  }
)('Andrew');

// alerts 'Hi, Andrew'
```

## Lesson 3: Classes and Objects
### 3.1 Constructor Functions
To instantiate (i.e., create) a new object, we use the new operator to invoke the function:
```javascript
new SoftwareDeveloper();
```
What does make `SoftwareDeveloper` a constructor function are:
- The use of the `new` operator to invoke the function
- How the function is coded internally (which we'll look at right now!)

##### Constructor Functions Structure and Syntax
```javascript
function SoftwareDeveloper() {
  this.favoriteLanguage = 'JavaScript';
}
```
First, rather than declaring local variables, constructor functions persist data with the `this` keyword. The above function will add a `favoriteLanguage` property to any object that it creates, and assigns it a default value of `'JavaScript'`. Don't worry too much about `this` in a constructor function for now; just know that `this` refers to the `new` object that was created by using the `new` keyword in front of the constructor function.

##### Creating a New Object
Let's use the `new` operator to create a new object:
```javascript
let developr = new SoftwareDeveloper();

console.log(developer);
\\ SoftwareDeveloper{favoriteLanguage: "JavaScript"}
```
We've saved the return value of this invocation to the variable `developer`.

##### Creating Multiple Objects
Let's invoke the same `SoftwareDeveloper()` constructor two more times to instantiate two additional objects: `engineer` and `programmer`.
```javascript
let engineer = new SoftwareDeveloper();
let programmer = new SoftwareDeveloper();

console.log(engineer);
// SoftwareDeveloper { favoriteLanguage: 'JavaScript' }

console.log(programmer);
// SoftwareDeveloper { favoriteLanguage: 'JavaScript' }
```

##### Constructor Functions Can Have Parameters
```javascript
function SoftwareDeveloper(name) {
  this.favoriteLanguage = 'JavaScript';
  this.name = name;
}

let instructor = new SoftwareDeveloper('Andrew');

console.log(instructor);
// SoftwareDeveloper { favoriteLanguage: 'JavaScript', name: 'Andrew' }

```
### 3.2 The `this` Keyword
##### `this` in Constructor Functions
```javascript
function Cat(name) {
 this.name = name;
 this.sayName = function () {
   console.log(`Meow! My name is ${this.name}`);
 };
}

const bailey = new Cat('Bailey');
```
As it turns out, when invoking a constructor function with the `new` operator, `this` gets set to the newly-created object!

##### What Does `this` Get Set To?
|**Call Style**| `new` | method | function |
|--------------|-------|--------|----------|
|**`this`**    | {}    |object itsels| global object |
|**Example**|new Cat()|bailey.sayName()|introduce()|

### 3.3 Setting Our Own `this`
##### More Ways to Invoke Functions
We've seen various ways to invoke functions, each with their own implications regarding the value of `this`. There are yet two more ways to invoke a function: either using the `call()` or the `apply()` methods.

##### call()
`call()` is a method directly invoked onto a function. We first pass into it in a single value to set as the value of `this`; then we pass in any of the receiving function's arguments one-by-one, separated by commas.

Consider the following function, `multiply()`, which simply returns the product of its two arguments:
```javascript
function multiply(n1, n2) {
  return n1 * n2;
}

multiply(3, 4);
// 12
```
No surprises here! But now -- let's use the `call()` method to invoke the same function:
```javascript
multiply.call(window, 3, 4);
// 12
```
Another example, consider the `mockingbird` object:
```javascript
const mockingbird = {
  title: 'To Kill a Mockingbird',
  describe: function () {
    console.log(`${this.title} is a classic novel`);
  }
};

mockingbird.describe();
// 'To Kill a Mockingbird is a classic novel'

const pride = {
  title: 'Pride and Prejudice'
};

mockingbird.describe.call(pride);
// 'Pride and Prejudice is a classic novel'
```
In the first call of `mockingbird.describe` `this` is set to the newly created mockingbird object. In the second call, `this` is set to the `pride` object.

##### apply()
The difference between call and apply is that after entering the `this` parameter call takes all other parameters separated by a comma while apply takes a sing array of all other parameters

### 3.4 Prototypal Inheritance
To save memory and keep things DRY, we can add methods to the constructor function's `prototype`'s property. The prototype is just an object, and all objects created by a constructor function keep a reference to the `prototype`. Those objects can even use the prototype's properties as their own!

JavaScript leverages this secret link -- between an object and its prototype -- to implement inheritance.

Recall that each function has a `prototype` property, which is really just an object. When this function is invoked as a constructor using the `new `operator, it creates and returns a new object. This object is secretly linked to its constructor's `prototype`, and this secret link allows the object to access the `prototype`'s properties and methods as if it were its own!

Since we know that the `prototype` property just points to a regular object, that object itself also has a secret link to its `prototype`. And that prototype object also has reference to its own `prototype` -- and so on. This is how the **prototype chain** is formed.

### 3.5 Prototypal Inheritance: Subclasses
One of the benefits of implementing inheritance is that it allows you to reuse existing code. By establishing inheritance, we can **subclass**, that is, have a "child" object take on most or all of a "parent" object's properties while retaining unique properties of its own.

Let's say we have a parent `Animal` object, which contains properties like `age` and `weight`. That same Animal object can also access methods like `eat` and `sleep`.

Now, let's also say that we want to create a `Cat` child object. Just like you can with other animals, you can also describe a cat by its *age* or *weight*, and you can also be certain that the cat *eats* and *sleeps* as well. When creating that `Cat` object, then, we can simply re-write and re-implement all those methods and properties from `Animal` -- or, we can save some time and prevent repeated code by having `Cat` inherit those existing properties and methods from `Animal`!

Not only can `Cat` take on properties and methods of `Animal`, we can also give `Cat` its own unique properties and methods as well! Perhaps a `Cat` has a unique lives property of `9`, or it has a specialized `meow()` method that no `Animal` has.

By using prototypal inheritance, `Cat` only needs to implement `Cat`-specific functionality, and just reuse `Animal`'s existing functionality.

#### Inheritance vs Prototypes
When calling any property on any object, the JavaScript engine will first look for the property in the object itself (i.e., the object's own, non-inherited properties). If the property is not found, JavaScript will then look at the object's prototype. If the property still isn't found in the object's prototype, JavaScript will continue the search up the prototype chain.

Again, inheritance in JavaScript is all about setting up this chain!

#### The Secret Link
As you know, an object's constructor function's prototype is first place searched when the JavaScript engine tries to access a property that doesn't exist in the object itself. Consider the following `bear` object with two properties, `claws` and `diet`:
```javascript
const bear = {
  claws: true,
  diet: 'carnivore'
};
```
We'll assign the following PolarBear() constructor function's prototype property to bear:
```javascript
function PolarBear() {
  // ...
}

PolarBear.prototype = bear;
```
Let's now call the `PolarBear()` constructor to create a new object, then give it two properties:
```javascript
const snowball = new PolarBear();

snowball.color = 'white';
snowball.favoriteDrink = 'cola';
```
This is how the snowball object looks at this point:
```javascript
{
  color: 'white',
  favoriteDrink: 'cola'
}
```
Note that snowball has just two properties of its own: `color` and `favoriteDrink`. However, snowball also has access to properties that don't exist inside it: `claws` and `diet`:
```javascript
console.log(snowball.claws);
// true

console.log(snowball.diet);
// 'carnivore'
```
Since `claws` and `diet` both exist as properties in the prototype object, they are looked up because objects are secretly linked to their constructor's prototype property.

#### Object.create()
At this point, we've reached a few roadblocks when it comes to inheritance. First, even though `__proto__` can access the prototype of the object it is called on, using it in any code you write is not good practice.

What's more: we also shouldn't inherit only the prototype; this doesn't set up the prototype chain, and any changes that we made to a child object will also be reflected in a parent object.

So how should we move forward?

There's actually a way for us to set up the prototype of an object ourselves: using `Object.create()`. And best of all, this approach lets us manage inheritance without altering the prototype!

  Object.create() takes in a single object as an argument, and returns a new object with its `__proto__` property set to what argument is passed into it. From that point, you simply set the returned object to be the prototype of the child object's constructor function. Let's check out an example!

First, let's say we have a `mammal` object with two properties: `vertebrate` and `earBones`:
```javascript
const mammal = {
  vertebrate: true,
  earBones: 3
};
```
Recall that `Object.create()` takes in a single object as an argument, and returns a new object. That new object's `__proto__` property is set to whatever was originally passed into `Object.create()`. Let's save that returned value to a variable, `rabbit`:
```javascript
const rabbit = Object.create(mammal);
```
We expect the new `rabbit` object to be blank, with no properties of its own:
```javascript
console.log(rabbit);
// {}
```
However, `rabbit` should now be secretly linked to `mammal`. That is, its `__proto__` property should point to `mammal`:
```javascript
console.log(rabbit.__proto__ === mammal);

// true
```
Great! This means that now, `rabbit` extends `mammal` (i.e., `rabbit` inherits from `mammal`). As a result, `rabbit` can access `mammal`'s properties as if it were its own!
```javascript
console.log(rabbit.vertebrate);
// true

console.log(rabbit.earBones);
// 3
```
`Object.create()` gives us a clean method of establishing prototypal inheritance in JavaScript. We can easily extend the prototype chain this way, and we can have objects inherit from just about any object we want! 
