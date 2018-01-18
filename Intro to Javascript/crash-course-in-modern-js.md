# Crash Course in Modern JS
## ECMAScript History
1. “Old School Javascript” aka ES5
    1. Last major update to JS
    1. Standardized in 2009
1. ES6/ES2015 and future releases
    1. In 2015, ECMAScript decided to start providing annual updates to the language rather than monolithic releases like ES edition 5
    1. Naming convention has flip flopped a bit since the decision to release annually, hence ES6 and ES2015 refer to the same specification.
    1. Annual releases have allowed JS to become a first class language.
    1. ES7 released mid 2016, ES8 (ES2017) released June 2017
## Notable ES6/7 Features
### Arrows
Arrows are a function shorthand using the `=>` syntax.  They are syntactically similar to the related feature in C#, Java 8 and CoffeeScript.  They support both statement block bodies as well as expression bodies which return the value of the expression.  Unlike functions, arrows share the same lexical `this` as their surrounding code.

```JavaScript
//ES6 shorthand
v => v + 1

//ES5 function
function(v) { return v + 1; }

// Lexical this
// ES6
this.nums.forEach((v) => {
    if (v % 5 === 0)
        this.fives.push(v)
})

//ES5
var self = this;
this.nums.forEach(function (v) {
    if (v % 5 === 0)
        self.fives.push(v);
});
```
More info: [MDN Arrow Functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)



### Let + Const
Block-scoped binding constructs.  `let` is the new `var`.  `const` is single-assignment (i.e. immutable).  Static restrictions prevent use before assignment.


```JavaScript
function f() {
  {
    let x;
    {
      // okay, block scoped name
      const x = "sneaky";
      // error, const
      x = "foo";
    }
    // error, already declared in block
    let x = "inner";
  }
}
```

More MDN info: [let statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [const statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)



### Rest parameter
Aggregation of remaining arguments into single parameter of variadic functions. Rest replaces the need for `arguments` and addresses common cases more directly.

```JavaScript
//ES6
function f(x, ...y) {
  // y is an Array
  return x * y.length
}
f(3, "hello", true) == 6

//ES5
function f (x) {
    var y = Array.prototype.slice.call(arguments, 1);
    return x * y.length;
};
f(3, "hello", true) == 6;
```

More MDN info: [Rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)



### Spread operator
Spreading of elements of an iterable collection (like an array or even a string) into both literal elements and individual function parameters.
```JavaScript 
//ES6
var params = [ "hello", true, 7 ]
var other = [ 1, 2, ...params ] // [ 1, 2, "hello", true, 7 ]

//ES5
var params = [ "hello", true, 7 ];
var other = [ 1, 2 ].concat(params); // [ 1, 2, "hello", true, 7 ]
```
```JavaScript 
//ES6
var str = "foo"
var chars = [ ...str ] // [ "f", "o", "o" ]

//ES5
var str = "foo";
var chars = str.split(""); // [ "f", "o", "o" ]
```

More MDN info: [Spread Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)



### Promises
Promises are a library for asynchronous programming.  Promises are a first class representation of a value that may be made available in the future.

```JavaScript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000)
  .then(() => { return timeout(2000) })
  .then(() => { throw new Error("hmm") })
  .catch(err => { return err })

console.log(p) //returns "hmm" after both timeout Promises have been resolved.
```

More info: [MDN Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)



### Classes
#### Class Definition
ES6 classes are a simple sugar over the prototype-based OO pattern.  Having a single convenient declarative form makes class patterns easier to use, and encourages interoperability. The class syntax is **NOT** introducing a new object-oriented inheritance model to JavaScript.

```JavaScript
//ES6 Class Definition
class Shape {
  constructor (x, y) {
    this.move(x, y)
  }
  move (x, y) {
    this.x = x
    this.y = y
  }
}

//ES5 Prototype Definition
var Shape = function (x, y) {
    this.move(x, y);
};
Shape.prototype.move = function (x, y) {
    this.x = x;
    this.y = y;
};
```

#### Class Inheritance
JavaScript classes provide a much simpler and clearer syntax to create objects and deal with inheritance. Classes support prototype-based inheritance, super calls, instance and static methods and constructors.
```Javascript
//ES6 Class Inheritance
class Rectangle extends Shape {
    constructor (x, y, width, height) {
        super(x, y);
        this.width  = width;
        this.height = height;
    }
}


//ES5 Prototype Inheritance
var Rectangle = function (x, y, width, height) {
    Shape.call(this, x, y);
    this.width  = width;
    this.height = height;
};
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;
```

More info: [MDN Classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)



### Modules
Language-level support for modules for component definition. Runtime behavior defined by a host-defined default loader. Implicitly async model – no code executes until requested modules are available and processed (prevents global namespace pollution).

```Javascript
//  lib/math.js
export function sum (x, y) { return x + y }
export var pi = 3.141593

//  someApp.js
import * as math from "lib/math"
console.log("2π = " + math.sum(math.pi, math.pi))

//  otherApp.js
import { sum, pi } from "lib/math"
console.log("2π = " + sum(pi, pi))
```

More MDN info: [import statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), [export statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)