## Module 5 - Destructuring
+ Key syntax point is to match the left and right sides:

```js
const { A, B } = { A: 'value1', B: 'value2' };
```

```js
const [ A, B ] = ['value1', 'value2'];
```

In both cases:
```js
console.log(A); //"value1"
console.log(typeof(A)); //string
```

#### 18 - Destructuring Objects

__Object destructuring syntax__
```js
const person = {
  first: 'Wes',
  last: 'Bos',
  country: 'Canada',
  city: 'Hamilton',
  twitter: '@wesbos'
};

const { first, last, twitter } = person;
```

```js
const { } = person;
```
+ The __brackets here are destructuring syntax__, not a block or an object.
  + When destructuring an object, we use curly brackets. But when destructuring an array, we'll use square brackets.
  + A bit like a __type hint__
+ __Top-level variable__

__Destructuring deeply nested data__

```js
const wes = {
  first: 'Wes',
  last: 'Bos',
  links: {
    social: {
      twitter: 'https://twitter.com/wesbos',
      facebook: 'https://facebook.com/wesbos.developer',
    },
    web: {
      blog: 'https://wesbos.com'
    }
  }
};

const { twitter, facebook } = wes.links.social;
```
+ With __nested data__

__Renaming__

```js
const { twitter: tweet, facebook: fb } = wes.links.social;
```
+ Can __rename as you destructure__
+ Otherwise, we're a bit "trapped" because "twitter" is the key we need to use
  + Here the variable names are _tweet_ and _fb_

__Setting defaults__

```js
const settings = { width: 300, color: 'black' }
const { width = 100, height = 100, color = 'blue', fontSize = 25} = settings;
```
+ If the value exists in _settings_ then it will override the default

__Combined example__
```js
// Object destructuring with variable renaming & default values
const { w: width = 400, h: height = 500 } = { w: 800 }
```
+ Not necessarily a common use case, but models all concepts

#### 19 - Destructuring Arrays

__Array destructuring syntax__

```js
const details = ['Wes Bos', 123, 'wesbos.com'];
const [name, id, website] = details;
```
+ Uses __square brackets__, as opposed to curly brackets for objects

```js
const data = 'Basketball,Sports,90210,23,wes,bos,cool';
const [itemName, category, sku, inventory] = data.split(',');
```
+ Creating an array from a string with _split()_, and then immediately destructuring into the four desired variables

```js
const team = ['Wes', 'Harry', 'Sarah', 'Keegan', 'Riker'];
const [captain, assistant, ...players] = team;
console.log(players) // ['Sarah', 'Keegan', 'Riker'] ;
```
+ __Rest operator__ allows us to get all the remaining items

#### 20 - Swapping Variables with Destructuring

```js
let inRing = 'Hulk Hogan';
let onSide = 'The Rock';

console.log(inRing, onSide); // 'Hulk Hogan', 'The Rock'

[inRing, onSide] = [onSide, inRing];

console.log(inRing, onSide); // 'The Rock', 'Hulk Hogan'
```

```js
[inRing, onSide] = [onSide, inRing];
```
+ On the right, creating an array which is immediately destructured into the two assignments on the left

#### 21 - Destructuring Functions - Multiple returns and named defaults

__Multiple returns__
```js
function convertCurrency(amount) {
  const converted = {
    USD: amount * 0.76,
    GPB: amount * 0.53,
    AUD: amount * 1.01,
    MEX: amount * 13.30
  };
  return converted;
}

const { USD, GBP } = convertCurrency(100);
```
+ While we're technically still just returning the single object, this effectively allows __multiple values from a single function return__


__Named defaults__
```js
function tipCalc({ total = 100, tip = 0.15, tax = 0.13 } = {}) {
  return total + (tip * total) + (tax * total);
}

const bill = tipCalc({ tip: 0.20, total: 200 });
```
+ Versatile syntax allowed by passing an object into a function
  + Order of values passed in is not important
  + Don't need to pass in _undefined_ for a parameter value like we did in video 5
  + Empty object is a bit unintuitive as a sort of second-tier default, but needed in case no values are passed in as parameters
