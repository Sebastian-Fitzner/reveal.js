# MODULES IN ES6

a "brief" introduction

--

>In JavaScript, the word "modules" refers to small units of independent, reusable code

--

That means a bunch of files each containing one functionality

--

Something like:

``` bash
user.module.js
```

that it is supposed to do just user related things

--

Till ES6 the module functionality was always emulated using libraries, see **CommonJS** and **RequireJS**.

--
You often see CommonJS (in node.js), and it looks like:

_lib.js_

``` js
function sayYes(){ return 'Yes' }
function sayNo(){ return 'No' }

module.exports = {
	roger: sayYes,
	sayNo // here is a bonus es6 shortcut for you...
};
```

_main.js_

``` js
var ofCourse = require('lib').roger;
var nee = require('lib').sayNo;

console.log(ofCourse(), nee()); // "yes", "No"
```
--
Those libraries works because of the **_[Module pattern](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript)_**.

--
Now the module pattern is "somehow" part of the language!
--

That means you can now do the same just by:

_lib.js_
``` js
export function sayYes(){ return 'Yes' }
export function sayNo(){ return 'No' }
```

_main.js_
``` js
import { sayYes as ofCourse, sayNo as nee } from './lib';

console.log(ofCourse(), nee()); // "yes", "No"
```

--
![nice?](https://media.giphy.com/media/dD26OUpjq8vdu/giphy.gif)

I suppose it is better, ...right?
--
**Right!**

_Because..._
--
Not only you have **short and nice syntax** but you also get
--
declarations (variables, functions) **local to the module**!
--
and you decide what it is accessible from the outside.
--
_lib.js_
``` js
export function sayYes(){ return 'Yes' }
export function sayNo(){ return 'No' }

// this is... PRIVATE!
function sayWhatIReallyThink () { return 'laz* *u***r'}
```
--
This approach to modules avoids global variables

---
## Exports
--
There are two kind:
--
* **named exports** (several per module)
--
* **default exports** (one per module)
--
_it is possible use both at the same time_
--
## Exports - Named exports

A module can export multiple things by prefixing its declarations with the keyword export.
--
We have seen this already:

_lib.js_
``` js
export function sayYes(){ return 'Yes' }
export function sayNo(){ return 'No' }
```
_main.js_
``` js
import { sayYes, sayNo} from './lib';
console.log(sayYes(), sayNo()); // "yes", "No"
```
--
## Exports - Default exports

They are **one per module**
--
export a default function:

myFunc.js
``` js
export default function () {}
```

main.js
``` js
import myFunc from 'myFunc';
myFunc();
```
--
or a default class:

MyClass.js
``` js
export default class {}
```

main.js
``` js
import MyClass from 'MyClass';
const inst = new MyClass();
```
--
wait a minute...

![what?](https://media.giphy.com/media/9ohlKnRDAmotG/giphy.gif)
--
there was no class name...
--
again
--
``` js
export default class {}
```

![a trap](https://media.giphy.com/media/r35VO5DnOV8ty/giphy.gif)
--
no, it's all fine because...
--
exports are the only place where JavaScript has _anonymous function declarations_ and _anonymous class declarations_
--
brr...
--
anyway, let's finish the export discussion
--
by saying that you can also list everything you want to export at the end


like:

``` javascript
function sayYes(){ return 'Yes' }
function sayNo(){ return 'No' }

export { sayYes: sayYes, sayNo: sayNo };
// export { sayYes, sayNo }; bonus shortcut!
```

---
## Imports
--
Imports are really easy too
--
Remember this?

``` js
import { sayYes, sayNo } from './lib';
console.log(sayYes(), sayNo()); // "yes", "No"
```
--
Now let's have a closer look
--
That syntax "`{ sayYes, sayNo }`" ~~is using~~ looks like an ES6 feature called [Destructuring_assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
--
so _destructuring_ example goes like:

``` js
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true
```
--
said that, you can't deeply destructuring inside an import declaration, like:

``` js
import foo, { bar: { a } } from 'my-module';
// If you try to do that, babel will tell you:
// ES2015 named imports do not destructure.
// Use another statement for destructuring after the import.
```

--
So even if they look very similar, destructuring assignment is not what's happening in your inport declaration, because:

> import statement brings in a binding, and not a reference or a value

--
whatever... just think that with import statements you are pulling out public things from an object in another file
--
And you guessed already:

> a module **is just another Object**

--
If you want to, you can also import the whole module and refer to its named exports via property notation:

_main.js_
``` js
import * as talkToMe from './lib';

console.log(talkToMe.sayYes(), talkToMe.sayNo());
```
--
Basically do what you want
--
but be aware that:
--
There are only two ways to combine these styles and the order in which they appear is fixed; the default export always comes first.
``` js
// Combining a default import with a namespace import:
import theDefault, * as my_lib from 'src/my_lib';

// Combining a default import with named imports
import theDefault, { name1, name2 } from 'src/my_lib';

```
---
## Recap!

![the old days](https://media.giphy.com/media/SJAt3WDv9t5yE/giphy.gif)

--

Using this new syntax is really easy and straightforward.

--

You have:

## exports

and

## imports

declarations

--

| Exports       	| Imports          |
| ----------------|------------------|
| named exports   | default import   |
| default exports | namespace import |
|  								| named imports    |
|  								| empty import 		 |

--

## exports

``` js
// named exports
export function sayYes(){ return 'Yes' }

// default exports
export default function sayYes() {}
```

## Imports

``` js
// Default import
import localName from 'src/my_lib';

// Namespace import
import * as my_lib from 'src/my_lib';

// Named imports
import { name1, name2 } from 'src/my_lib';

// Empty import
import 'src/my_lib';
```
--

Basically ES6 Modules are
--
## Referenced objects

--
with compact and declarative syntax!

![squeeze](http://gifimage.net/wp-content/uploads/2017/10/bud-spencer-gif-6.gif)

---
## Fun facts
--
you didn't really believe it, right?
--
no fun for you.
--

* Imports and exports must be at the top level

* Imports are hoisted

--
And now we have some time for questions
--
![waiting](https://media.giphy.com/media/tXL4FHPSnVJ0A/giphy.gif)
--
I knew that, so **I** prepared some questions for **you**!
--
Can I import a module conditionally?
--
![no](https://media.giphy.com/media/kQbMO5X7UA1C8/giphy.gif)
--
>Import statements must always be at the top level of modules. That means that you can’t nest them inside if statements, functions, etc.
--
Can I use variables in an import statement?
--
![no](https://media.giphy.com/media/hDYdeQVIllABi/giphy.gif)
--
what is imported must not depend on anything that is computed at runtime. Therefore:

```
// Illegal syntax:
import foo from 'some_module'+SUFFIX;
```
--
But maybe you can...

![boh](https://media.giphy.com/media/Q9CzYe4DkNKec/giphy.gif)
--
well, kind of...

![forget it](https://media.giphy.com/media/UeCVnJcAw2yPK/giphy.gif)
--
I'm tired, are we done here?
--
Yes
--
But no
--
![yes but](https://media.giphy.com/media/IAvLGRTZ7LBjW/giphy.gif)
--
sorry, not yet done with you:
---
# Exercise

![exercises](https://media.giphy.com/media/hI8wKII0K8DAc/giphy.gif)
--
Create a little implementation where you have one module called `greetings.module.js`.

This module contains just 2 functions and 1 variable. The variable will host a static string like "ciao" and will live in a local scope.

One of the two functions will be public and will call the other one (that is private) that will return the variable value.

In the end, import what you need in another file called `index.js` and play around. Try all type of imports and exports.

--
And to finish here you get [the link](http://exploringjs.com/es6/ch_modules.html#sec_overview-modules) from where I basically copied half of this presentation!

---

# Variables and scoping
another _**"brief"**_ introduction
--
With ES6 we have new way to handle variables.
--
You already heard of _let_ and _const_, right?
--
Good
--
Actually there's not much to say
--
* _let_ is block scoped and you can change the value


* _const_ is like _let_ but you can't change the value

--
And that's it!
--
Thanks for your attention

![ciao](https://media.giphy.com/media/3orieOPmJGswsJ7Ily/giphy.gif)



---
# Questions?
--
What happened to _var_?
--
Nothing, it is still there and you can use it.
--
I mean, don't. Never.
--
Got it, but what's the difference between _var_ and _let_?
--
_var_ is **function-scoped** while _let_ is **block-scoped**
--
**block**-scoped ?
--
I _forgot_ it (ja ja), can you please tell me again what do you mean by **block**?
--
Officially:

> A block statement is used to group zero or more statements

--
That it is just another way to say...
--
whatever you wrap in __{}__
--
_var_
``` js
var x = 1;
{
  var x = 2; // var does not care about blocks
}
console.log(x); // logs 2
```

_let_
``` js
let x = 1;
{
  let x = 2; // let (and const too) do have block scope
}
console.log(x); // logs 1
```
--
Again, look at the differences between _function_ and _block_ scope:
--
``` js
function showMeWhatUGot () {
  var iDontCareAboutBlocks = 'nasty';
  let imTheGoodOne = 'iDontPollute';
  if(true){
    var iDontCareAboutBlocks = 'stillNotGivingADamn';
    let imTheGoodOne = 'becauseICareAboutBlocks';
  }
  console.log(iDontCareAboutBlocks); // "stillNotGivingADamn"
  console.log(imTheGoodOne); // "iDontPollute"

}
showMeWhatUGot ();
```
--
Ok, all clear so far.

What about _const_? Can you please tell me more about that?
--
Well, const works like let, but...

> the variable you declare must be **immediately initialized**, with a value that **can’t be changed afterwards**.

--
You have two takeaway here:

_***immediately initialized***_
``` js
const foo; // SyntaxError: missing = in const declaration
```

_***can’t be changed afterwards***_
``` js
const bar = 123;
bar = 456; // TypeError: `bar` is read-only
```
--
Basically _const_ creates **immutable** variables
--
All easy, right?
--
well...
--
there is a very important **pitfall** here
--

> const does not make the value immutable

--
now be careful and read slowly
--

> Variables that are assigned a non-primitive value are given a reference to that value. That reference points to the object’s location in memory. The variables don’t actually contain the value.

--
The takeaway is:

> reference values are stored as pointers in the variable

--
Of course you knew it...
--
Just stressing how important this is, specially when we say that _const_ creates **immutable** variables
--
so we all agree that:

``` js
const ciao = 'ciao';
```

Is pretty much very different from:
``` js
const ciao = {};
// or
const ciao = [];
// or
const ciao = function ciao() {};
```
--
and we all agree that:

> Javascript has 3 data types that are passed by reference: Array, Function, and Object. These are all technically Objects.

--
therefore no issues with that:

``` js
var arrRef = ['easyPeasy'];
var arrRef2 = arrRef;
console.log(arrRef === arrRef2); // -> true
```
--
and with that too:

``` js
var arr1 = ['crystalClear'];
var arr2 = ['crystalClear'];
console.log(arr1 === arr2); // -> false
```
--
Lovely! now we can go out of the "question" area :-)
---
# In depth
(just a bit, I promise)
--
First let's clarify what **Hoisting** means
--

> Hoisting was thought up as a general way of thinking about how execution contexts (specifically the creation and execution phases) work in JavaScript

--
whatever, right?
--
no
--
sorry

--
we need to get it right
--
both examples are working! that is hoisting in action.

execute after function declaration
``` js
function computerSay(what) {
  console.log("I say " + what);
}
computerSay("Mee"); --> "I say Mee"
```

execute **before** function declaration
``` js
computerSay("Mee"); --> "I say Mee"
function computerSay(what) {
  console.log("I say " + what);
}
```
--
You can picture this by thinking that javascript moves things around before executing.
--
not really, but just pretend again

![forget it](https://media.giphy.com/media/UeCVnJcAw2yPK/giphy.gif)
--
Basically javascript looks immediately for your _declarations_ (variable and functions) and reserve the right space in memory.
--
Again: **Only declarations are hoisted**
--
JavaScript only hoists declarations, **not initializations**.
--
If a variable is declared and initialized after using it, the value will be undefined. For example:

``` js
console.log(num); // Returns undefined
var num;
num = 6;
```

but if you declare the variable after it is used, but initialize it beforehand, it will return the value:

``` js
num = 6;
console.log(num); // returns 6
var num;
```
--
![puke](https://media.giphy.com/media/qTpK7CsOq6T84/giphy.gif)
--
Oh, I don't like this either...
--
enter...
--
#### The temporal dead zone!
![really?](https://media.giphy.com/media/CggoHW4h87Ktq/giphy.gif)
--

> A variable declared by let or const has a so-called temporal dead zone (TDZ): When entering its scope, it can’t be accessed (got or set) until execution reaches the declaration.

--
_var_ don’t have a temporal dead zone

``` js
console.log(num); // --> undefined
num = 6; // call initializer again but with a new assignment
console.log(num); // --> 6
var num; // create AND immediately initialized to undefined
```

So they are created and immediately initialized.
--
_let_ and _const_ do have the TDZ, therefore:

``` js
console.log(num); // Execution stops here!
num = 6;
console.log(num);
let num; // create but not initialize
```

That means you will get a `ReferenceError` if you try to access it before the initialization
``` js
Uncaught ReferenceError: num is not defined
```
---
## The global object
--
We all know by now that the global object is like an annoying relative:

you can't get rid of, just need to learn how to live with and avoid interaction as much as possible
--
ES6 helps with that
--
Remember modules?

* top level variables are local to the module itself
* the value of _this_ at top level is _undefined_

--
But also _let_, _const_ and _Class_ don't pollute the global object

---
## Recap!

![the old days](https://media.giphy.com/media/SJAt3WDv9t5yE/giphy.gif)
--
_let_ and _const_ are **block-scoped** variables
--
_let_ creates **mutable** variables
--
_const_ creates **immutable** variables
--
when working with _const_ always keep in mind the difference between **primitive values** and **reference values**!
--
_let_ and _const_ are less ambiguous because of a strict hoisting (TDZ)
--
_let_ and _const_ do not create **global properties**
--

| Type     | Hoisting           | Scope         | Creates global properties |
|----------|--------------------|---------------|---------------------------|
| var      | Declaration        | Function      | Yes                       |
| let      | Temporal dead zone | Block         | No                        |
| const    | Temporal dead zone | Block         | No                        |
| function | Complete           | Block         | Yes                       |
| class    | No                 | Block         | No                        |
| import   | Complete           | Module-global | No                        |
--

As always, you can find all proper informations here:
http://exploringjs.com/es6/ch_variables.html

---

# Template literals
--
You saw them already, they look like
--
``` js
  const firstName = 'Beppe';
  console.log(` Hello ${firstName}! `);

// Hello Beppe!
```
--
The syntax goes that the entire literal is delimited by backticks (\`)
--
and the interpolated expressions inside with `${` and `}`
--

> Template literals are string literals with support for interpolation and multiple lines

--
support for interpolation

``` js
const food = 'toteoma';
console.log(`
  mmmm,
  ${(food === 'pizza') ? pizza : 'spaghetti'}!
  `);
// mmm,
// spaghetti!
```
--
multiple lines

``` js
let bestQuote =` - What a filthy job.
                 - Could be worse.
                 - How?
                 - Could be raining.
                `;
```

instead of

``` js
let bestQuote = " - What a filthy job. " +
                " - Could be worse." +
                " - How?" +
                " - Could be raining."
            ;
```
--

so far, so good

--

but wait, there's more to cover...

--

template literals could also look like

--
``` js
let ciao = sayHello`Hello ${person}!
                    How are you
                    today?`;
```

![really?](https://media.giphy.com/media/NITFX5emjpMQ0/giphy.gif)
--
yep, they are called
--
### tagged template literals
--

> Tagged template literals are function calls whose parameters are provided via template literals.

--
``` js
function sayHello(strings, person, when){
  let firstPart   = strings[0],
      secondPart  = strings[1],
      thirdPart   = strings[2];
  return firstPart + person + secondPart + when + thirdPart;
}

let person = "Beppe";
let when = "today";
let ciao = sayHello`  Hello ${person}
                      How are you ${when}?`;
console.log(ciao);
// Hello Beppe
// How are you today?
```
--
that is like saying

``` js

sayHello(['Hello ', 'How are you', '?'], person, when)

```
--
ok nice, but what can I do with that?
--
apparently a lot of things!
--
* localization

``` js

msg`Welcome to ${site}, you are visitor number ${vNumber}:d!`

// en ->  Welcome to {0}, you are visitor number {1}!
// de ->  Besucher Nr. {1}, willkommen bei {0}!
// tlh -> majQa' {0}, visitor mI' {1}!
```
--
* shell commands

``` js

var proc = sh`ps ax | grep ${pid}`;


```
--
* dom selectors

``` js

let elements = query`.${className}`;


```
--
* fancy frameworks

``` js

Tea = Relay.createContainer(Tea, {
  fragments: {
    tea: () => Relay.QL`
      fragment on Tea {
        name,
        steepingTime,
      }
    `,
  },
});

```
--
you can even use them to create templates

``` js
var data = [
    { first: 'Werner', last: 'Herzog' },
    { first: 'Arthur C.', last: 'Clarke' },
];
const tmpl = besties => `
    <ul>
    ${besties.map(pal => `
        <li>${pal.first} ${pal.last}</li>
    `).join('')}
    </ul>
`;
console.log(tmpl(data));

//<ul>
//    <li>Werner Herzog</li>
//    <li>Arthur C. Clarke</li>
//</ul>

```


---
## Recap!

![the old days](https://media.giphy.com/media/SJAt3WDv9t5yE/giphy.gif)
--
There are two kind of literals:

* template literals
> Template literals are string literals that can stretch across multiple lines and include interpolated expressions

* tagged template literals
> Tagged template literals are function calls whose parameters are provided via template literals

--
As always, you can find all proper informations here:
http://exploringjs.com/es6/ch_template-literals.html

---

## Exercise

Rewrite your greetings module to use template literals. 

Return a whole sentence with a variable in there. 

This variable needs to be passed as parameter to the exported function.

---

# Classes

--

1. Definition
    - Declaration
    - Expression
1. Body and Methods
    - Constructor
    - Prototype Methods
    - Getter and Setter
    - Static Methods
    - Instance Properties
1. Sub-Classing
    - `extends`
    - `super`
1. Browser Support
    
---

## Definition

ES6 Classes formalize the common JavaScript pattern of simulating class-like inheritance hierarchies using functions and prototypes. 

--

They are effectively simple sugaring over prototype-based OO.

--

Classes support prototype-based inheritance, constructors, super calls, instance and static methods.

--

### Declaration

One way to define a class is using a class declaration. 

To declare a class, you use the class keyword with the name of the class.

``` js 
class MyCustomClass {}
```

--

### Expression 

A class expression is another way to define a class, they can be named or unnamed.

``` js
const MyClass = class {} // unnamed
const MyNextClass = class MyCustomClass {} // named
```

---

## Body & Methods

The body of a class is the part that is in curly brackets {}. 

This is where you define class members, such as methods or constructor.

--

### Constructor

The constructor method is a special method for creating and initializing an object created with a class. 

There can only be one special method with the name "constructor" in a class.

A constructor can use the `super` keyword to call the constructor of the super class (That is a class from which you extend).

--

``` js 
class Person {
    constructor (firstname, lastname) {
        this.firstName = firstname;
        this.lastName = lastname;
    }
}
```

--

## Prototype Methods

Prototype methods are functions which are available after a class is instantiated. 

``` js
class Person {
    constructor (firstname, lastname) {
        this.firstName = firstname;
        this.lastName = lastname;
    }
    
    fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
}
```

--

## Getter & Setter 

But we are able to improve the above even more with getters and setters. 

``` js
class Person {
    constructor (firstname, lastname) {
        this.firstName = firstname;
        this.lastName = lastname;
    }
    
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
    
    set fullName(name) {
        const names = name.toString().split(' ');
        this.firstName = words[0] || '';
        this.lastName = words[1] || '';
    }
}
```

-- 

With that you can do the following: 

``` js
class Person {
	constructor(firstname, lastname) {
		this.firstName = firstname;
		this.lastName = lastname;
	}

	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	}

	set fullName(name) {
		const names = name.toString().split(' ');
		this.firstName = words[ 0 ] || '';
		this.lastName = words[ 1 ] || '';
	}
}

const person = new Persion();

person.fullName = 'Jack Black';

console.log(person.fullName);
```

-- 

Means a getter and setter is handled like reading/assigning a variable. 

--

## Static Methods

Static methods are called without instantiating their class and cannot be called through a class instance. 

Static methods are often used to create utility functions for an application.

Since these methods operate on the class instead of instances of the class, they are called on the class.

--

There are two ways to call static methods:

``` js
Foo.methodName() 
// calling it explicitly on the Class name
// this would give you the actual static value. 

this.constructor.methodName() 
// calling it on the constructor property of the class
// this might change since it refers to the class of the current instance, where the static property could be overridden
```

--

Personally, I use the static keyword to indicate to the developer that this method doesn't use information from the current instance.

--

## Instance Properties by using class field declarations (Stage 3)

There is a proposal to support Class field declarations for JavaScript (https://github.com/tc39/proposal-class-fields).

Personally I really like that and it also makes you component more readable. 

--

Let's say you have the following: 

``` js 
class Counter extends HTMLElement {
	constructor() {
		super();
		this.onclick = this.clicked.bind(this);
		this.x = 0; // instance property
	}

	clicked() {
		this.x++;
		window.requestAnimationFrame(this.render.bind(this));
	}

	connectedCallback() {
		this.render();
	}

	render() {
		this.textContent = this.x.toString();
	}
}

window.customElements.define('num-counter', Counter);
``` 
 
--

To define a field (`x`) you have to save it on `this` in a method or constructor. Let's rewrite it: 

``` js

class Counter extends HTMLElement {
	x = 0; // class field 

	constructor() {
		super();
		this.onclick = this.clicked.bind(this);
	}

	clicked() {
		this.x++;
		window.requestAnimationFrame(this.render.bind(this));
	}

	connectedCallback() {
		this.render();
	}

	render() {
		this.textContent = this.x.toString();
	}
}

window.customElements.define('num-counter', Counter);
```

--

![You know what](https://media.giphy.com/media/l3V0Gp1JYxMeegIda/giphy.gif "You know what?")

--

You can also use private fields: 

``` js

class Counter extends HTMLElement {
	#x = 0; // private class field

	constructor() {
		super();
		this.onclick = this.clicked.bind(this);
	}

	clicked() {
		this.#x++;
		window.requestAnimationFrame(this.render.bind(this));
	}

	connectedCallback() { 
		this.render(); 
	}

	render() {
		this.textContent = this.#x.toString();
	}
}
window.customElements.define('num-counter', Counter);
```

--

But ...

--

You cannot access these values in your constructor. The order of exectution is:

1. constructor
1. every function called in constructor
1. class field declarations

---

## Sub-Classing 

- `extends`
- `super`

--

### Extending Classes

The extends keyword is used in class declarations or class expressions to create a class as a child of another class.

We take the previous person class:

``` js
class Person {
	constructor(firstname, lastname) {
		this.firstName = firstname;
		this.lastName = lastname;
	}

	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	}

	set fullName(name) {
		const names = name.toString().split(' ');
		this.firstName = words[ 0 ] || '';
		this.lastName = words[ 1 ] || '';
	}
}
```

--

And extend it:

```js 
class Teacher extends Person {
    constructor(field, firstname, lastname) {
        super(firstname, lastname);
        
        this.field = field;
    }
    
    printInfo() {
        return `The main field of ${this.fullName} is ${this.field}`;
    }
}

```

--

The order of the execution in the constructor is `super` important!

 

-- 

A lot of things are happening here: 

1. We extend the base class Person by using `extends`.
1. We provide all necessary parameters and call the constructor of the base class by using `super`.
1. We create a new prototype method (`printInfo()`) and call the getter of `Person`. 


---

## Browser Support

--

You can use ES6 in every modern browser, but ...

--

... do not use any staged proposals.  

--

... be sure to use only the latest browser versions. 

--

So, how can be make it compatible with older browsers?

--

By using Babel as compiler. 

--

And that's almost it. 

---

## Recap

--

Classes are sugar syntax for Prototypes. 

--

Classes support prototype-based inheritance, constructors, super calls, instance and static methods.

--

Constructors are necessary.

--

Babel is the tool to compile your ESNext stuff to ES5 (browser compatibility).

--

``` js

'use strict';

/**
 * Shape class.
 * 
 * @constructor
 * @param {String} id - The id.
 * @param {Number} x  - The x coordinate.
 * @param {Number} y  - The y coordinate.
 */
function Shape(id, x, y) {
    this.id = id;
    this.setLocation(x, y);
}

/**
 * Set shape location.
 * 
 * @param {Number} - The x coordinate.
 * @param {Number} - The y coordinate.
 */
Shape.prototype.setLocation = function(x, y) {
    this.x = x;
    this.y = y;
};

/**
 * Get shape location.
 * 
 * @return {Object}
 */
Shape.prototype.getLocation = function() {
    return {
        x: this.x,
        y: this.y
    };
};

/**
 * Get shape description.
 * 
 * @return {String}
 */
Shape.prototype.toString = function() {
    return 'Shape("' + this.id + '")';
};

/**
 * Circle class.
 * 
 * @constructor
 * @param {String} id     - The id.
 * @param {Number} x      - The x coordinate.
 * @param {Number} y      - The y coordinate.
 * @param {Number} radius - The radius.
 */
function Circle(id, x, y, radius) {
    Shape.call(this, id, x, y);
    this.radius = radius;
}
Circle.prototype = Object.create(Shape.prototype);
Circle.prototype.constructor = Circle;

/**
 * Get circle description.
 * 
 * @return {String}
 */
Circle.prototype.toString = function() {
    return 'Circle > ' + Shape.prototype.toString.call(this);
};

// test the classes
var myCircle = new Circle('mycircleid', 100, 200, 50); // create new instance
console.log(myCircle.toString()); // Circle > Shape("mycircleid")
console.log(myCircle.getLocation()); // { x: 100, y: 200 }
```

--

``` js

'use strict';

class Shape {
    
    /**
     * Shape class.
     * 
     * @constructor
     * @param {String} id - The id.
     * @param {Number} x  - The x coordinate.
     * @param {Number} y  - The y coordinate.
     */
    constructor(id, x, y) { // constructor syntactic sugar
        this.id = id;
        this.setLocation(x, y);
    }
    
    /**
     * Set shape location.
     * 
     * @param {Number} - The x coordinate.
     * @param {Number} - The y coordinate.
     */
    setLocation(x, y) { // prototype function
        this.x = x;
        this.y = y;
    }
    
    /**
     * Get shape location.
     * 
     * @return {Object}
     */
    getLocation() {
        return {
            x: this.x,
            y: this.y
        };
    }
    
    /**
     * Get shape description.
     * 
     * @return {String}
     */
    toString() {
        return `Shape("${this.id}")`;
    }
}

/**
 * Circle class.
 * 
 * @constructor
 * @param {String} id     - The id.
 * @param {Number} x      - The x coordinate.
 * @param {Number} y      - The y coordinate.
 * @param {Number} radius - The radius.
 */
class Circle extends Shape {
    constructor(id, x, y, radius) {
        super(id, x, y); // call Shape's constructor via super
        this.radius = radius;
    }
    
    /**
     * Get circle description.
     * 
     * @return {String}
     */
    toString() { // override Shape's toString
        return `Circle > ${super.toString()}`; // call `super` instead of `this` to access parent
    }
}

// test the classes
var myCircle = new Circle('mycircleid', 100, 200, 50); // create new instance
console.log(myCircle.toString()); // Circle > Shape("mycircleid")
console.log(myCircle.getLocation()); // { x: 100, y: 200 }
```


---

## Exercise

--

### No 1

Create a base component class in its own file which does the following:

1. Select the DOM element which is provided by a parameter.
1. Save the element as property. 
1. Add a prototype method to add a class to the element.
1. Execute the method in constructor.
1. Create one instance to make sure everything is working.


--

### No 2

Extend the imported base component class:

1. By adding a render method which appends HTML to the element.
1. Create one instance to make sure everything is working.

---

# Arrow functions

These are like JavaScript candy. 

--

### Definition

An arrow function expression has a shorter syntax than a function expression and does not have its own this, arguments, super, or new.target.

--

### Syntax 

In general it looks like that:

``` js
(parameters) => { statements } 
```

--

Let's make it more clear within an example:

``` js
function funcName(params) {
   return params + 2;
}

funcName(2);
// 4
```

``` js
var funcName = (params) => params + 2;

funcName(2);
// 4
```

--

#### Syntax Breakdown

Lets see it in detail ... 

--

**No parameters**

``` js
() => { statements }
```

--

**One parameters**

Here you see, you can even delete the brackets when only one parameter is provided.

``` js
parameter => { statements }
```

--

**Return expression**

If you do the same stuff for your expression, you return that automatically.

This: 

``` js
parameter => expression
```

... is the same like this: 

``` js
function (parameters){
  return expression;
}
```

---

### Benefits

- No binding of `this`
- Shorter syntax
 
---

## Exercise

1. Bind a click handler to your component element.
1. Execute an arrow function as callback function. 
1. Toggle a class in that callback function. 

---

# Asynchronous Handling in JS

**Single VS multi thread**

**Events**

-- 

**Promises**
    - Definition
    - API
    - Promise in Detail
    - Chaining
    - Resolve multiple Promises
    
--

**Async & Await**
    - Definition
    - API
    - Try/Catch
    - Chaining
    - Resolve multiple async methods

--

## Single Thread vs multi thread

--

### Multi Thread

As a human being, you're multithreaded. 
At the same time you can: 

1. type with multiple fingers, 
2. drive and 
3. hold a conversation. 

-- 

The only _blocking function_ we have to deal with is sneezing, where all current activity must be suspended for the duration of the sneeze. 

-- 

That's pretty annoying, especially when you're driving and trying to hold a conversation.

--

### Single Thread

JavaScript works on a single thread, means it cannot execute two bits of code at the same time.

--

Furthermore JavaScript needs to share its resources in the browser. It is in the same queue as painting, updating styles, and handling user actions. 

Activity in one of these things delays the others.

--

### Events

To get around this you use `events` and `callbacks`. 

--

**Example**

A click action is an asynchronous event which is handled by a callback function and looks like this:

``` js
var el = document.querySelector('.cta');

el.addEventListener('click', function() {
  // argh everything's broken
});

```

--

But events aren't always the best

--

The code above is asynchronous, but in this case only the action is asynchronous. Therefore events are really great.

--

Let's take another example into consideration:

--

**Example**

``` js
var img1 = document.querySelector('.img-1');

img1.addEventListener('load', function() {
  // woo yey image loaded
});

img1.addEventListener('error', function() {
  // argh everything's broken
});
```

--

What is wrong with this example?

--

The events can happened before we started listening for them, so we need to work around that using the "complete" property of images. 

--

``` js
var img1 = document.querySelector('.img-1');

function loaded() {
  // woo yey image loaded
}

if (img1.complete) {
  loaded();
}
else {
  img1.addEventListener('load', loaded);
}

img1.addEventListener('error', function() {
  // argh everything's broken
});
```

--

But this does not catch images which errored before we attached the event listener. 

And we are talking only about one image, what would you do if you have multiple images?

---

## Exercise: Events & Callbacks

### Generic `loadImage()` function

Clean up the code and create a generic `loadImage()` function which does the following: 

1. The function expects 2 parameters
    - Image URL
    - Callback function
    
--

1. Create an image by using the native `Image()` functionality.
1. Change the `src` of the image to the URL you have passed as parameter.

-- 

1. Add check statements: 
    - Check if image is already loaded with `naturalWidth`
        - If so call the callback function and hand over the image object
    - If image is not loaded attach the event listeners and execute the callback as well.
1. Test your function by loading an image from the web.

--

### Load multiple images

Now try to load multiple images. 

Make sure that you execute a callback function after **all** images are loaded. 

---

## Solution

### Solve our image event code

``` js
// Let's define a promise function
function loadImage(url, callback) {
    let img = new Image(); // Create an image 
    img.src = url;
		
		if (img.naturalWidth) {
            // If the browser can determine the naturalWidth the
            // image is already loaded successfully
            callback(img);
        } else {
            // Use the event listener like before, but resolve the promise after loaded ... 
            img.addEventListener('load', e => callback(img));
            		
            // or reject the promise
            img.addEventListener('error', () => {
                callback(null, new Error(`Failed to load image's URL: ${url}`));
            });
        }
	});
}

loadImage('https://placeimg.com/640/480/any', () => {
    loadImage('https://placeimg.com/720/320/any', () => {
        loadImage('https://placeimg.com/1920/1080/any', () => {
            console.log('All images loaded!');
        })
    })
})
```

### Strength and weakness of Events

Events are great for things that can happen multiple times on the same object — keyup, touchstart etc. 

With those events you don't really care about what happened before you attached the listener.

But when it comes to async success/failure, you need something else (Promises).

--

## Pyramid of Doom 

What you can see, that it is no other way around to use nested callbacks for the resolve process of all images.

That makes your code ugly and harder to maintain.  

---

## Promises

--

### Definition 

A promise to either return a result at some point in the future or a promise to reject with an error at some point in the future. 

When a promise knows the return result, the promise resolves with the return result. 

When a promise fails because of an error, the promise rejects with the reason why the promise failed.

--

At their most basic, promises are a bit like event listeners except:

--

A promise can only succeed or fail once. It cannot succeed or fail twice, neither can it switch from success to failure or vice versa.

--

If a promise has succeeded or failed and you later add a success/failure callback, the correct callback will be called, even though the event took place earlier.

--

This is extremely useful for async success/failure, because you're less interested in the exact time something became available, and more interested in reacting to the outcome.

--

Promises were designed to make it easier to write sequential asynchronous code easier. 

--

Most of the time you use an API that returns a Promise (like `fetch()`). 

---

## Promise API

The API of a Promise is pretty simple. Call the promise, 
1. `then()` handle the resolved one or
2. `catch()` the rejected one

-- 

### Use fetch()

Let's use the Promise `fetch()` which is provided by the latest browsers.

--

``` js
fetch('https://api.chucknorris.io/jokes/PFRkezc3Q4SBB_bhY53iBw')
    .then((data) => {
        console.log('data', data.json());
    })
    .catch(e => e);
```

--

### Exercise

Use `fetch()` to retrieve some data from an open API like (https://api.chucknorris.io/). 

Attach the retrieved data value to the DOM.

--

The API is straight forward, easy to remember and has some benefits over callbacks. 

---

## Chaining 

What if we want to chain them together to retrieve more data sequentially? 

--

Easy to do, just return a new Promise out of another one: 

``` js 
const jokes = [];

fetch('https://api.chucknorris.io/jokes/PFRkezc3Q4SBB_bhY53iBw')
    .then((data) => {
        jokes.push(data.json().value);
        
        return fetch('https://api.chucknorris.io/jokes/ZbmOzXiNRbGKGxCcufpPHg')
    })
    .then((data) => {
        jokes.push(data.json().value);
        
        return jokes;
    })
    .then(allJokes => {
        console.log(allJokes);
    })
    .catch(e => e);

```

--

You see what is happening? We solve the issue with the pyramid of doom. 

No nesting anymore. 

--

![yeah](https://media.giphy.com/media/RrVzUOXldFe8M/giphy.gif)

--

### Exercise

Create a sequence of calls against an API. 

Save the data and attach it to the DOM. 

---

## Resolve all at once

But perhaps you do not want to resolve it sequentially and speed up the process? 

Therefore Promise provides further functionality. 

--

### Promise.all()

`Promise.all()` expects an array of Promises. It waits until all promises are resolved ... 

``` js
Promise.all([
    fetch('https://api.chucknorris.io/jokes/PFRkezc3Q4SBB_bhY53iBw'),
    fetch('https://api.chucknorris.io/jokes/ZbmOzXiNRbGKGxCcufpPHg')
])
    .then((dataset) => {
        console.log(dataset.json())
    })
    .catch(e => e);
```

--

That makes it even more easier to maintain your code.

--

![yeah](https://media.giphy.com/media/yoJC2GnSClbPOkV0eA/giphy.gif)

--

### Exercise

Create multiple Promises which needs to get resolved all together and attach the value to the DOM. 

---

## How to write your own Promise

--

We are talking all the time about resolving or rejecting something. 

Here we see how it works under the hood.

``` js
let myPromise = new Promise((resolve, reject) => { /* code */ });
```

--

Means it uses callback functions ... 

--

## Resolve

`Resolve` is executed when the Promise is fulfilled/resolved. 

--

## Reject 

`Reject` is executed when the Promise throws an error. 

--

### Exercise

Re-write your callback function to a custom Promise.

--

### Solution 

``` js 
function loadImage(url) {
    let img = new Image(); // Create an image 
    img.src = url;
    
    // Easy peasy return a promise
	return new Promise((resolve, reject) => { // Ah, arrow function - nice!
		
		if (img.naturalWidth) {
            // If the browser can determine the naturalWidth the
            // image is already loaded successfully
            resolve(img);
        } else {
            // Use the event listener like before, but resolve the promise after loaded ... 
            img.addEventListener('load', e => resolve(img));
            		
            // or reject the promise
            img.addEventListener('error', () => {
                reject(new Error(`Failed to load image's URL: ${url}`));
            });
        }
	});
}
```

--

Now you can use it like so:

``` js
// load the image, and append it to the element id="image-holder"
loadImage('http://thecatapi.com/api/images/get?format=src&type=jpg&size=small')
	.then(img => document.getElementById('image-holder').appendChild(img))
	.catch(error => console.error(error));
```

--

Can you see already the power?! 

![power](https://media.giphy.com/media/3oKIPjzfv0sI2p7fDW/source.gif)

---

## Await & Async 

--





 
