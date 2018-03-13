## Module 2 - Function Improvements: Arrows and Default Arguments

#### 06 - Arrow functions introduction

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

#### 07 - More arrow function examples
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

#### 08 - Arrow functions and 'this'
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
