## Module 19 - Map and Weak Map

#### 64 - Maps
+ Just as __Sets__ are similar to arrays but with a twist, __Maps__ are similar to objects (but with a twist)

```js
const dogs = new Map();

dogs.set('Snickers', 3);
dogs.set('Sunny', 2);
dogs.set('Hugo', 10);

console.log(dogs.has('Snickers')); //true
console.log(dogs.get('Snickers')); //3
```
+ Simple API
  + _has()_, _get()_

```js
dogs.forEach((val, key) => console.log(val, key));

for (const [key, val] of dogs) {
  console.log(key, val);
}
```
+ Two ways to iterate: __forEach__ + __for...of__

```js
for (const [key, val] of dogs) {
```
+ Because each element in a Map returns an _array_ with the _key_ and _value_, we can use __destructuring__ signalled by the square brackets here to declare these as variables being pulled out of the array

#### 64 - Map Metadata with DOM Node Keys

__Use case__ here is __storing metadata about an object__, but avoiding putting the data on the object itself, which is a bit _dirty_ and means all that data is erased if the object is erased. The metadata here is how many times the button has been clicked

```js
const clickCounts = new Map();
const buttons = document.querySelectorAll('button');

buttons.forEach(button => {
  clickCounts.set(button, 0);
  button.addEventListener('click', function() {
    const val = clickCounts.get(this);
    clickCounts.set(this, val + 1);
  });
});
```
+ The __keys on a Map can be objects__, in this case a DOM node
  + The enhanced feature over an object literal is that the __key isn't limited to being a string__

```js
clickCounts.set(button, 0);
button.addEventListener('click', function() {
  const val = clickCounts.get(this);
  clickCounts.set(this, val + 1);
```
+ __get()__ and __set()__ used this way still feels quite weird to me
+ Because _this_ refers to __both the DOM node__ registering the click event _and_ __the Map key__, it can be seen as an accurate identifier


#### 65 - WeakMap and Garbage Collection

```js
let dog1 = { name: 'Snickers' };
let dog2 = { name: 'Sunny' };

const strong = new Map();
const weak = new WeakMap();

strong.set(dog1, 'Snickers is the best!');
weak.set(dog2, 'Sunny is the 2nd best!');

dog1 = null;
dog2 = null;
```

+ The __WeakMap__ here allows the deleted _dog2_ variable to be __garbage collected__
  + This prevents a __memory leak__, where _dog2_ might be referenced even though it's been removed from our application. The "_strong_" __Map__, on the other hand, could expose the app to such a potential memory leak
  + Bos says WeakMap is useful when you __don't want to "babysit"__ what is included in your Map
  + https://stackoverflow.com/questions/29413222/what-are-the-actual-uses-of-es6-weakmap
  + How does the browser know that a given object or DOM node __has no references__ to it and can therefor be garbage collected?
