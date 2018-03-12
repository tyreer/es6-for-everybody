# Notes on ES6

Notes as I work through the Wes Bos _ES6 for Everyone_ course.

Check out the course at [https://es6.io/](https://es6.io/)

## Module 1 - New Variables — Creation, Updating and Scoping

#### 01 - var-let-const

__var__
+ __Function scope__: Available inside function where a _var_ is created (local variable)
+ Or globally scoped if not in a function
  + But generally not what we want. To update a variable outside a function we should __return__ from the function
  + __if{} blocks are not functions__ and will leak _var_ variable scope

```js
var age = 100;
if(age > 12) {
  var dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}

console.log(dogYears) //700
```

__let__
+ __Block scope__: Available inside block where a _let_ is created
+ __if{} blocks__ will constrain scope

```js
var age = 100;
if(age > 12) {
  let dogYears = age * 7;
  console.log(`You are ${dogYears} dog years old!`);
}

console.log(dogYears) //undefined
```

#### 02 - let VS const

In contrast to var, _let_ and _const_ __cannot be redeclared__ in the same scope.

```js
let points = 50;
let winner = false;
let points = 40; // error
```

__Object.freeze()__
>The Object.freeze() method freezes an object: that is, prevents new properties from being added to it; prevents existing properties from being removed; and prevents existing properties, or their enumerability, configurability, or writability, from being changed, it also prevents the prototype from being changed.

#### 03 - let and const in the Real World

__IIFE__
+ Thanks to block scope, blocks + let/const constrain scope just like an IIFE

```js
{
  const name = 'wes';
}
```
...can replace...
```js
(function(){
  const name = 'wes';
})()
```

__for loop__
+ The block scope of _let_ prevents the variable in a for loop from escaping into global scope.
+ Also creates nine scopes in the example below that all retain their declared value, in contrast to _var_ which would be overridden and simply output the final value 10 times
```js
for(let i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log('The number is ' + i);
  },1000);
}
```

#### 04 - Temporal Dead Zone
+ _var_ can be accessed prior to being declared, although it's value is undefined
```js
console.log(pizza); //undefined because in the TEMPORAL DEAD ZONE
var pizza = 'Deep Dish 🍕🍕🍕';
```

+ _let/const_ cannot be accessed before being declared
```js
console.log(pizza); //Error
const pizza = 'Deep Dish 🍕🍕🍕';
```

#### 05 - Is var Dead? What should I use?
+ Most familiar to me:

>+ use const by default
+ only use let if rebinding is needed
+ (var shouldn’t be used in ES2015)

https://mathiasbynens.be/notes/es6-const
