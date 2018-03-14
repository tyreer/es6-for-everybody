## Module 2 - Function Improvements: Arrows and Default Arguments

#### 06 - Arrow Functions Introduction

Some benefits of arrow functions
+ Concise
+ Implicit returns
```js
const fullNames4 = names.map(name => `${name} bos`);
```
vs
```js
const fullNames3 = names.map(name => {
  return `${name} bos`;
});
```
+ Doesn't __rebind__ the value of _this_

+ Arrow functions are always __anonymous functions__, but they can be declared in a variable.
```js
const sayMyName = (name) => {
  alert(`Hello ${name}!`)
}
```
One benefit of a named function is __tracking errors__ in a stack trace

#### 07 - More Arrow Function Examples
+ Implicit return with object literal
  + Removing function block braces works for other types of returned values, but gets weird with an object literal, which has its own braces

Solution: __parenthesis around object literal__:
```js
const win = winners.map((winner, i) => ({name: winner, race, place: i + 1}));
```

Nice example of implicit return of a boolean
```js
const ages = [23,62,45,234,2,62,234,62,34];
const old = ages.filter(age => age >= 60);
```

#### 08 - Arrow Functions and 'this'
+ When using an arrow function, the value of this is not rebound, so can end up with parent scope on a click event handler.

```js
const box = document.querySelector('.box');
box.addEventListener('click', () => console.log(this)) //Window NOT box!
```

+ Generally good then to have a non-arrow function at the top-level of an event handler because we want _this_ to establish a specific context relative to the element associated with triggering the event.

```js
const box = document.querySelector('.box');
box.addEventListener('click', function() {console.log(this);}) // Box!
```

+ Arrow function inherits value of _this_ from parent function, which is useful inside a callback.

```js
const box = document.querySelector('.box');
box.addEventListener('click', function() {

  this.classList.toggle('opening');
  setTimeout(() => {
    console.log(this); // Still box, but would be Window if non-arrow function! bc new context unbound to box would be created by non-arrow function
    this.classList.toggle('open');
  }, 500);
});
```

Inheriting _this_ via arrow functions allows you to achieve the same thing as
```js
var that = this;
```

#### 09 - Default Function Arguments

```js
function calculateBill(total, tax = 0.13, tip = 0.15) {
  return total + (total * tax) + (total * tip);
}

const totalBill = calculateBill(100, undefined, 0.25);
```
+ _undefined_ is still needed as a parameter in the function invocation, which seems odd to me

Can replace:

```js
tax = tax || 0.13;
```

#### 10 - When NOT to Use an Arrow Function

__(1)__ __Top level of an event handler__ callback where _'this'_ is needed

✅
```js
const button = document.querySelector('#pushy');
button.addEventListener('click', function() {
  console.log(this); //button
  this.classList.toggle('on');
});
```
⛔
```js
const button = document.querySelector('#pushy');
button.addEventListener('click', () => {
  console.log(this); //Oh no, Window!
  this.classList.toggle('on');
});
```

__(2)__ When you need a __method to bind to an object__

✅
```js
const person = {
  points: 23,
  score() {
    console.log(this); // person {...}
    this.points++; // Increments object attribute
  }
  // ^  SAME AS  ^
  // score: function() {}
}
```
⛔
```js
const person = {
  points: 23,
  score: () => {
    console.log(this); // Oh no, Window!
    this.points++; // Undefined
  }
}
```

__(3)__ When you need to add a __prototype method__

✅
```js
class Car {
  constructor(make, colour) {
    this.make = make;
    this.colour = colour;
  }
}

const beemer = new Car('bmw', 'blue');
const subie = new Car('Subaru', 'white');

Car.prototype.summarize = function() {
   return `This car is a ${this.make} in the colour ${this.colour}`;
};

beemer.summarize() //"This car is a bmw in the colour blue"
```

⛔
```js
class Car {
  constructor(make, colour) {
    this.make = make;
    this.colour = colour;
  }
}

const beemer = new Car('bmw', 'blue');
const subie = new Car('Subaru', 'white');

Car.prototype.summarize = () => {
   return `This car is a ${this.make} in the colour ${this.colour}`;
};

beemer.summarize() // "This car is a undefined in the colour undefined"
```

__(4)__ When you need __arguments object__

✅
```js
const orderChildren = function() {
  // orderChildren('beyonce', 'solange')
  console.log(arguments); // ["beyonce", "solange"]
  const children = Array.from(arguments);
  return children.map((child, i) => {
    return `${child} was child #${i + 1}`;
  })
}
```

⛔
```js
const orderChildren = () => {
  // orderChildren('beyonce', 'solange')
  console.log(arguments); // Uncaught ReferenceError: arguments is not defined
  const children = Array.from(arguments);
  return children.map((child, i) => {
    return `${child} was child #${i + 1}`;
  })
}
```
