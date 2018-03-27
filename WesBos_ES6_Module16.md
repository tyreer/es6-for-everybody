## Module 16 - Generators

#### 55 - Introducing Generators

```js
function* listPeople(){
  yield 'Joe';
  yield 'Abe';
  yield 'Sam';
}

const people = listPeople();

console.log(people.next()); // {value: "Joe", done: false}
```

+ Function with __*__, __yield__ and __next()__ syntax that offers multiple returns
+ __done__ will be _true_ once all the _yield_ cases have been satisfied

```js
const inventors = [
  { first: 'Albert', last: 'Einstein', year: 1879 },
  { first: 'Isaac', last: 'Newton', year: 1643 },
  { first: 'Galileo', last: 'Galilei', year: 1564 },
  { first: 'Marie', last: 'Curie', year: 1867 },
  { first: 'Johannes', last: 'Kepler', year: 1571 },
  { first: 'Nicolaus', last: 'Copernicus', year: 1473 },
  { first: 'Max', last: 'Planck', year: 1858 },
];

function* loop(arr) {
  console.log(inventors);
  for (const item of arr) {
    yield item;
  }
}

const inventorGen = loop(inventors);
```
+ Here the first call of _next()_ would log all the inventors and _return/yield_ the first object ('Einstein')
+ Then we'd need to call _next()_ to step through the array.
  + It's like the _yield_ suspends within the __for..of__ loop until _done_ has a value of true
