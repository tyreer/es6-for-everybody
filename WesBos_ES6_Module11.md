## Module 11 - Symbols

#### 38 - All About Symbols
+ __7th primitive__ type and a new primitive in JS
+ Previous 6: _number, string, object, boolean, null, undefined_
+ Can help avoid naming conflicts, but not really a highly demanded addition
+ Could be useful if you want to create a property in a unique way

  ```js
  const wes = Symbol('Wes');
  const person = Symbol('Wes');
  console.log(wes === person) // false;
  ```
+ Pass in a __descriptor__ (here _'Wes'_)
+ Symbol is a unique identifier
+ Actual symbol is something like a 40-character random string, and the descriptor just represents it for ease of reading

```js
  const classRoom = {
    [Symbol('Mark')] : { grade: 50, gender: 'male' },
    [Symbol('Olivia')]: { grade: 80, gender: 'female' },
    [Symbol('Olivia')]: { grade: 83, gender: 'female' },
  };

  for (const person in classRoom) {
    console.log(person); //Nothing!
  }
```
+ Symbols __cannot be iterated__ over
+ Good example since people in a group need a unique identifier but could have the same name
  + Ensures keys in an object are absolutely unique
+ Potentially useful for storing private data?


```js
const syms = Object.getOwnPropertySymbols(classRoom);
const data = syms.map(sym => classRoom[sym]);
console.log(data);
```
+ __Object.getOwnPropertySymbols()__ pass in object and returns array of symbols, which can then be iterated over
  + Weird work around
