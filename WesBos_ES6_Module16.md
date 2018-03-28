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

+ __A Generator__ has __*__, __yield__ and __next()__ syntax, which offers multiple returns
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
  + It's like the _yield_ suspends within the __for...of__ loop until _done_ has a value of true

#### 56 - Using Generators for Ajax Flow Control

+ One case for generators is ability to do waterfall-like Ajax requests

```js
function ajax(url) {
  fetch(url).then(data => data.json()).then(data => dataGen.next(data))
}

function* steps() {
  console.log('fetching beers');
  const beers = yield ajax('http://api.react.beer/v2/search?q=hops&type=beer');
  console.log(beers);

  console.log('fetching wes');
  const wes = yield ajax('https://api.github.com/users/wesbos');
  console.log(wes);

  console.log('fetching fat joe');
  const fatJoe = yield ajax('https://api.discogs.com/artists/51988');
  console.log(fatJoe);
}

const dataGen = steps();
dataGen.next(); // kick it off
```

```js
.then(data => dataGen.next(data))
```
+ Calling __next()__ here automatically sets up the next fetch
  + So, you could include something returned from the first fetch in the query to a subsequent fetch assured that the first request would have returned before the second is issued

#### 57 - Looping generators with for...of

```js
function* lyrics() {
  yield `But don't tell my heart`;
  yield `My achy breaky heart`;
  yield `I just don't think he'd understand`;
}

const achy = lyrics();

for (const line of achy) {
  console.log(line);
}
```
+ __for...of loop__ allows iteration through a generator and doesn't require _.next()_
