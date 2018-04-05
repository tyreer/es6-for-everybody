## Module 6 - Iterables and Looping

__Built-in JS iterables__
>String, Array, TypedArray, Map and Set are all built-in iterables, because their prototype objects all have a Symbol.iterator method.

#### 22 - The for of loop

__for-of loop__

+ Can be used with any kind of data except objects
+ Allows _break_ and _continue_

```js
const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];

for (const cut of cuts) {
  if(cut === 'Brisket') {
    continue;
  }
  console.log(cut);
}
```

+ Existing methods of iterating and their drawbacks
  + _for_ loop
    + Scope issue with _var_, slightly chunky syntax
  + _forEach_
    + No way to abort a loop with something like _break_
    + No way to skip over with _continue_
  + _for-in_ loop
    + Weirdly, will also iterate over anything added to the prototype, not only the items in the iterable collection
    + 3rd party code might extend prototype (MooTools)

#### 23 - The for-of Loop in Action

```js
for (const [i, cut] of cuts.entries()) {
  console.log(`${cut} is the ${i + 1} item`);
}
```
+ __entries__ is immediately destructured into two variables

>Array.prototype.entries(): The entries() method returns a new Array Iterator object that contains the key/value pairs for each index in the array.

```js
function addUpNumbers() {
  let total = 0;
  for (const num of arguments) {
    total += num;
  }
  return total;
}

addUpNumbers(10,23,52,34,12,13,123);
```
+ __for-of loop with _arguments___ allows us to accept any number of parameters
  + Best practice would be to convert _arguments_ to an array, but if just iterating over then not a problem to forgo conversion

```js
const ps = document.querySelectorAll('p');
for (const paragraph of ps) {
  paragraph.addEventListener('click', function() {
    console.log(this.textContent);
  });
}
```
+ I'd typically use _forEach_ to add event listeners, but _for-of_ might be useful if you wanted to use _break_ or _continue_ to more selectively apply event handlers

#### 24 - Using for-of with Objects

+ __Object.entries()__ is standard in ES2017
  + Subject of video 77

```js
const apple = {
  color: 'Red',
  size: 'Medium',
  weight: 50,
  sugar: 10
};

for (const prop in apple) {
  const value = apple[prop];
}
```
+ Short of _Object.entries()_ Bos suggests just sticking with a _for-in_ loop
