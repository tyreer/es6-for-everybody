## Module 18 - Sets and Weak Sets

#### 61 - Sets

```js
const people = new Set();
people.add('Wes');
people.add('Snickers');
people.add('Kait');

console.log(people.size); // 3
people.delete('Wes') ;
people.clear();
```

+ New primitive. Like an array, but __no indexes__ and each item is unique

>Set objects are collections of values. You can iterate through the elements of a Set in insertion order. A value in the Set may only occur once; it is unique in the Set's collection.

```js  
for (const person of people) {
    console.log(person);
  }
```
+ Iterate over a set with ___for...of___
+ __values()__ will return a _SetIterator_ which has the same API as a generator, so can iterate with ___.next()___

```js
const students = new Set(['Wes', 'Kara', 'Tony']);

const dogs = ['Snickers', 'Sunny'];
const dogSet = new Set(dogs);
```
+ Can create a Set by passing in an array

#### 62 - Understanding Sets with Brunch

```js
const brunch = new Set();
brunch.add('Wes');
brunch.add('Sarah');
brunch.add('Simone');
const line = brunch.values(); //Returns a generator
console.log(line.next().value); //Steps through the generator while logging the name at the current position, which is here Wes
console.log(line.next().value); // Sarah
console.log(brunch); //Set(3) {"Wes", "Sarah", "Simone"}
console.log(line); //SetIterator {"Simone"}
````
+ When you call __next()__ against a SetIterator the value at the current position __will remove itself__ (mutation by default, weird)

#### 63 - WeakSets

+ A __WeakSet can only ever contain objects__
+ Cannot loop over a weak set (no iterator)
+ Cannot use __clear()__

```js
let dog1 = { name: 'Snickers', age: 3 };
let dog2 = { name: 'Sunny', age: 1 };

const weakSauce = new WeakSet([dog1, dog2]);

console.log(weakSauce);
dog1 = null;
console.log(weakSauce); //After waiting a few seconds, the weak set should have been garbage collected
```

+ Weak sets clean themselves up through __garbage collection__
  + By setting _dog1_ to _null_ we've given the weak set a green light to __garbage collect__ and automatically delete that value
  + A __memory leak__ is when you reference something that should not be able to be referenced but hasn't yet been garbage collected
  + Hard to say exactly when garbage collection will occur because it varies between browsers and cannot be forced
  + This may be useful in the case of removing DOM nodes that are no longer needed

> The WeakSet is weak: References to objects in the collection are held weakly. If there is no other reference to an object stored in the WeakSet, they can be garbage collected. That also means that there is no list of current objects stored in the collection. WeakSets are not enumerable.
